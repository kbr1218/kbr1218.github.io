---
layout: post
title:  "[RAG] Cross-Encoder를 이용한 Re-ranking 구현과 원리 (RAG 성능 최적화)"
author: me
categories: [RAG]
date: 2025-12-19 20:00:00
image: assets/images/20251219/Gemini_Generated_Image_csh5pncsh5pncsh5.png
---

####  RAG 파이프라인에서의 Context 문제

<p style="margin-top: 2.3em;"></p>

RAG 파이프라인을 구축할 때, LLM에게 전달할 참고 문서 <small style="color:gray">(컨텍스트; context)</small>의 수는 적을수록 좋다.  

문서가 많아질수록(==컨텍스트가 길어질 수록) 다음과 같은 문제가 발생하기 때문.  

<p style="margin-top: 2.3em;"></p>

1. **품질 저하 문제**  <small style="color:gray">(Lost in the Middle & Hallucination)</small>
    
    가장 큰 문제는 LLM이 참고할 정보가 많아질수록 **정답을 놓치거나 잘못된 정보를 생성**할 확률이 커진다는 것이다.  
    
    - **Lost in the Middle**: LLM은 문서가 너무 길면 중간에 있는 핵심 내용을 무시하거나 잊어버리는 경향이 있다.
    - **노이즈** <small style="color:gray">(Noise)</small>**로 인한 환각** <small style="color:gray">(Hallucination)</small>: 질문과 무관한 정보가 많이 섞일수록 LLM이 정답을 찾는데 방해를 받아 이상한 대답을 할 확률이 높아진다.

    <p style="margin-top: 2.3em;"></p>

2. **효율성 및 비용 문제**  <small style="color:gray">(Token & sLM의 한계)</small>
    
    문서 수가 증가하면 효율성 면에서도 문제가 생긴다.
    
    - 입력 토큰 수 증가로 인한 **비용 상승**
    - 특히 SLM <small style="color:gray">(소형 언어 모델)</small>의 경우 **프롬프트 처리 한계** <small style="color:gray">(Context Window)</small> 초과

하지만 그렇다고 문서 수를 무조건 줄이면 **정답이 포함된 핵심 문서**까지 잘려 나갈 위험이 커진다.  

그래서 **Retrieval** <small style="color:gray">(검색)</small> 단계에서는 문서를 넉넉히 가져오면서, LLM에게 넘겨주기 전에 **가져온 문서들 중 가장 적절하고 정확도가 높은 것만 주자** 라는 전략이 필요하다.  

이때 사용하는 것이 바로 **Cross-Encoder!**

<br>

---

<p style="margin-top: 2.3em;"></p>

#### RAG 파이프라인에서의 Cross-Encoder

Cross-Encoder는 전체 파이프라인 중 **Re-ranking** <small style="color:gray">(재순위화)</small> 단계에서 사용한다.  

<p style="margin-top: 2.3em;"></p>

1. **Retrieval**  <small style="color:gray">(벡터 검색)</small>: 빠르게 후보 문서를 가져옴 (정확도보단 속도와 재현율 중심)  

2. **Reranking**  <small style="color:gray">(재순위)</small>: **Cross-Encoder**로 후보 문서들과의 유사도 Top K를 선정 (정밀도)  

3. **Generation**  <small style="color:gray">(생성)</small>: 선정된 Top K만 LLM에게 전달하여 답변 생성


<br>

---

<p style="margin-top: 2.3em;"></p>

#### LLM 평가를 하지 않는 이유

Cross-Encoder는 기본적인 RAG 구조에 **재순위라는 한 단계를 추가**한 것이다.  

따라서 이 단계에서는 **정확도를 높이면서도 속도를 유지**하는 것이 중요하다.  

<p style="margin-top: 2.3em;"></p>

물론 또 다른 LLM을 사용해서 문서를 평가할 수는 있겠지만, LLM 기반 평가는 **가성비와 속도** 측면에서 여러 단점이 있다.

비교를 해보자면

<style>
  .styled-table {
    border-collapse: collapse;
    width: 100%;
    margin: 20px 0;
  }
  .styled-table th, .styled-table td {
    border: 1px solid #dddddd; /* 테두리 색상 */
    padding: 8px;
    text-align: left;
  }
  .styled-table th {
    background-color: #f2f2f2; /* 헤더 배경색 (연한 회색) */
    font-weight: bold;
  }
</style>

| 구분 | Cross-Encoder (re-ranker) | LLM eval |
| :--- | :--- | :--- |
| **모델 크기** | 비교적 작음 (BERT Base 수준) | 매우 큼 (수십-수백억 파라미터 이상) |
| **평가 속도** | 매우 빠름 | 느림 (토큰 생성 과정 필요) |
| **평가 방식** | 점수 예측 (회귀) | 텍스트 생성 (e.g. ”0~10점 점수 매겨줘”) |
| **비용 효율성** | 매우 높음 | 매우 낮음 |
{: .styled-table }

<br>

---

#### Cross-Encoder를 이용한 평가 방법

Cross-Encoder는 두 개의 텍스트(Query, Document)를 쌍으로 입력받아, 그 사이의 연관성을 **연속적인 수치 값** <small style="color:gray">(score)</small>**로 예측**한다.  

이러한 Cross-Encoder 구현에는 대부분 HuggingFace의 `transformer` 라이브러리에서 제공하는 **AutoModelForSequenceClassification** 모델을 사용한다.  

이 모델은 원래 분류 전용이라 이름에 Classification <small style="color:gray">(분류)</small>가 들어가 있긴 하지만, 모델은 내부적으로 다음 질문을 던지면서 아래와 같은 계산을 수행한다.  

> “이 쿼리 <small style="color:gray">(Query)</small>와 이 문서 <small style="color:gray">(Document)</small>는 얼마나 밀접하게 관련되어 있는가?” 

이때 예측되는 결과값이 바로 **유사도** <small style="color:gray">(score)</small>이다.  

모델의 이름과 달리 분류가 아닌 일종의 **NLP 회귀** <small style="color:gray">(Regression)</small> 문제로 접근하는 것이라고 볼 수 있다.

<br>

---

#### 회귀인데 AutoModelForSequenceClassification 모델을 사용하는 이유

여기까지 보다보면 “이름이 Classification <small style="color:gray">(분류)</samll>인데 왜 Regression <small style="color:gray">(회귀)</small> 계산을 하지?”라는 의문이 드는데,  

그 이유는 `num_labels` 파라미터에 있다. 

<p style="margin-top: 2.3em;"></p>

`num_labels`란 **모델이 최종적으로 분류하고자 하는 레이블** <small style="color:gray">(Class)</small>**의 개수**를 의미하며, Transformers 라이브러리는 이 설정값에 따라 작동 방식을 다르게 처리한다.  

- `num_labels >= 2` → 분류  <small style="color:gray">(Classification)</small> 문제로 인식 (e.g. 긍정/부정)
- `num_labels = 1` → 회귀  <small style="color:gray">(Regression)</small> 문제로 인식 (e.g. 유사도 점수)

그러니까 `num_labels`를 `1`로 설정하면 모델은 특정 클래스를 고르는 게 아니라 **유사도 점수** <small style="color:gray">(score)</small> 자체를 예측하게  되고, 우린 이 점수를 기준으로 문서를 Re-Ranking하게 된다.

<p style="margin-top: 2.3em;"></p>

```python
# Cross-Encoder 모델 로드
model = AutoModelForSequenceClassification.from_pretrained(
    settings.MODEL_PATH
    # num_labels=1
    # (일반적인 Cross-Encoder 모델의 config.json 기본값이 1이라 생략 가능)
).to(device)
model.eval()
```

<br>

---

#### `num_labels`에 따른 모델 output 차이 비교

<p style="margin-top: 2.3em;"></p>

| 구분 | `num_labels = 1` (Cross-Encoder) | `num_labels >= 2` (일반 분류) |
| --- | --- | --- |
| 모델 출력 | logits, (*loss) | logits, (*loss) |
| Logits 형태 | `[batch_size, 1]` | `[batch_size, num_classes]` |
| Logits 범위 | **-∞ ~ +∞** (범위 제한 없음) | -∞ ~ +∞ (범위 제한 없음) |
| Logits의 의미 | 배치 내 각 텍스트 쌍의 **유사도 점수** | 각 클래스에 속할 **확률적 점수** |
| 내부 loss 함수 | MSELoss | CrossEntropyLoss |
| 해석 방법 | Sigmoid 함수를 통해 **0~1 사이의 확률값**으로 변환 | Argmax를 통해 **가장 높은 클래스 인덱스** 선택 |
| 최종 결과 | 유사도 (Similarity) | 분류 라벨 (Class Label) |
{: .styled-table }

<p style="margin-top: 2.3em;"></p>

<i>*loss 값은 모델 학습 시 `labels` 파라미터를 전달했을 때만 계산됨. 추론<small style="color:gray">(Inference)</small> 단계에서는 일반적으로 `loss`를 계산하지 않음</i>

<br>

---

#### Cross-Encoder (`num_labels=1`)의 점수 계산 방식

아래 코드는 PyTorch를 사용해서 Cross-Encoder가 최종 유사도 점수를 얻는 과정이다. 

```python
# 1) 추론 환경 설정
with torch.no_grad():
    logits = model(**features).logits
    # logits e.g. [[4.5], [-2.3], ...]

    # 2) sigmoid를 사용하여 0-1 사이의 확률값으로 변환
    sub_scores = torch.sigmoid(logits).squeeze(-1).cpu().tolist()
    if isinstance(sub_scores, float):
        sub_scores = [sub_scores]
    scores.extend(sub_scores)
```

1. 모델 추론 시에는 ‘학습’이 아닌 ‘문서간 유사도 비교’가 목적이다. 따라서 역전파에 필요한 미분값<small style="color:gray">(Gradient)</small>을 계산할 필요가 없다. 
    
    미분 계산 과정을 생략하면 메모리가 절약되고 속도가 빨라지므로 `with torch.no_grad():`를 사용한다. 
    
<p style="margin-top: 2.3em;"></p>

2. `AutoModelForSequenceClassification`의 출력인 `logits`는 **범위가 정해져 있지 않은 실수**<small style="color:gray">(-∞ ~ +∞)</small> 형태다.
    
    따라서 이 값을 우리가 이해할 수 있는 **0과 1 사이의 확률값(유사도 점수)**로 변환하기 위해 `torch.sigmoid()` 함수를 적용한다
    
<p style="margin-top: 2.3em;"></p>

이렇게 얻은 확률값이 곧 **유사도**<small style="color:gray">(Score)</small>이며, 이 점수를 기준으로 후보 문서들에 대해 re-ranking을 수행하게 된다.

<br>

---

#### 추가) 모델이 어떤 labels을 가지고 있는지 확인하는 방법

`num_labels >= 2`일 때 분류를 수행한다고 했는데, 이때 모델이 분류 기준으로 삼는 라벨의 이름을 확인하기 위한 속성은 `id2label`이다.

이 속성을 통해 모델을 학습시킨 사람이 0번 인덱스에 무엇으로 정의했는지<small style="color:gray">(e.g. 긍정/부정)</small> **사전 학습된 값**<small style="color:gray">(pre-trained value)</small>**이 아닌 학습 시 사용된 매핑 정보**를 확인할 수 있다. 

<aside>
💡

Cross-Encoder(`num_labels=1`)에서는 이 라벨 이름이 중요하지 않지만, 일반 분류 모델을 사용할 때는 반드시 확인해야 한다.

</aside>

```python
from transformers import AutoModelForSequenceClassification

# 모델 이름 정의 (암거나 확인하고 싶은 모델)
model_name = "roberta-large-mnli"

# config 정보만 로드
config = AutoConfig.from_pretrained(model_name)

# 라벨 확인
print(config.id2label)
```

출력 결과 예시

```python
# num_labels=2인 모델
{
  0: 'POSITIVE',
  1: 'NEGATIVE'
}

# num_labels=3인 모델
{
  0: 'CONTRADICTION',
  1: 'NEUTRAL',
  2: 'ENTAILMENT'
}
```