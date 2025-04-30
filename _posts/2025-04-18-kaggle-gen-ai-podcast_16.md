---
layout: post
title:  "[Kaggle Gen AI] Day 1 - 모델 평가와 출력 제어의 기술 실습 🚀"
author: me
categories: [ Kaggle Gen-AI, codelabs]
date: 2025-04-18 09:00:00
image: assets/images/20250402/day1_2.png
---
[지난 게시물](https://kbr1218.github.io/kaggle-gen-ai-podcast_15/)에서는  프롬프트 작성에 대한 코드를 봤고,  

이번에는 프롬프트를 통해 생성된 출력을 평가하는 몇 가지 기술을 알아본다. 평가 과정의 일부로, Gemini의 구조화된 데이터 처리 기능을 활용해 평가 결과를 Python 객체 형태로 정리하는 방법도 함께 살펴보겠다!

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 <a href="https://services.google.com/fh/files/blogs/neurips_evaluation.pdf" target="blank">대형 언어 모델 평가하기</a> 백서도 함께 참고하면 좋다. 
</div>

[👉 실습 원본 Kaggle notebook 링크 바로가기](https://www.kaggle.com/code/markishere/day-1-evaluation-and-structured-output)

<br>

---
### Evaluation (평가)
실제 환경에서 LLM을 사용할 때, 모델이 얼마나 잘 작동하는지 평가하는 것이 중요하다.  

하지만 LLM의 생성 결과는 매우 다양하고, open-ended 형태로 출력하기 때문에 평가가 쉽지 않다.  

여기서는 [Gemini 1.5 Pro technical report (기술보고서)](https://storage.googleapis.com/cloud-samples-data/generative-ai/pdf/2403.05530.pdf)를 사용하여 요약<small style="color:gray">(summarization)</small> 작업을 평가해보겠다.   

먼저 저 PDF 파일을 kaggle notebook 환경에 다운로드한 다음, Gemini API에서 사용할 수 있도록 업그레이드 해야 한다.  
```python
!wget -nv -O gemini.pdf https://storage.googleapis.com/cloud-samples-data/generative-ai/pdf/2403.05530.pdf

document_file = client.files.upload(file='gemini.pdf')
```

<br>

---
##### Summarise a document (문서 요약하기)
여기서 사용하는 요약 프롬프트는 비교적 기본적인 형태로, 특별한 지침 없이 훈련 콘텐츠만을 중심으로 요약하도록 요청하고 있다. 
```python
request = 'Tell me about the training process used here.'

def summarise_doc(request: str) -> str:
  """업로드된 문서에 대해 요청 실행하기"""
  # 출력 결과의 일관성을 높이기 위해 temp를 0.0으로 설정하기
  config = types.GenerateContentConfig(temperature=0.0)
  response = client.models.generate_content(
      model='gemini-2.0-flash',
      config=config,
      # 요청과 문서를 함께 전달
      contents=[request, document_file],
  )

  return response.text

summary = summarise_doc(request)
Markdown(summary)
```
<details>
  <summary><strong>Output</strong></summary>
  <div style="background-color:#f6f8fa; padding: 1em; border-radius: 5px;">
  Based on the document you provided, here's a breakdown of the training process used for Gemini 1.5 Pro:<br><br>
  <p><strong>1. Data</strong>
  <ul>
    <li>멀티모달 및 다국어 데이터:</li>
    <ul>
      <li>웹 문서</li>
      <li>코드</li>
      <li>이미지</li>
      <li>오디오</li>
      <li>비디오 콘텐츠</li>
    </ul>
  </ul></p>

  <p><strong>2. Infrastructure</strong>
  <ul>
    <li><strong>TPUv4 가속기:</strong> Google의 TPUv4 가속기 4096개 칩으로 구성된 여러 개의 팟(pod)에서 훈련 수행</li>
    <li><strong>분산 훈련:</strong> 이러한 가속기들은 여러 데이터 센터에 분산되어 있음</li>
  </ul></p>

  <p><strong>3. Model Architecture</strong>
  <ul>
    <li><strong>Mixture-of-Experts (MoE) Transformer:</strong> Gemini 1.5 Pro는 희소 MoE Transformer 아키텍처 기반</li>
    <ul>
      <li>모든 입력마다 일부 전문가만 활성화되어, 많은 파라미터를 가지면서도 효율적인 처리 가능</li>
    </ul>
  </ul></p>

  <p><strong>4. Training Phases</strong>
  <ul>
    <li><strong>Pre-training:</strong> 대규모 멀티모달 및 다국어 데이터셋으로 사전 학습 수행</li>
    <li><strong>Instruction Tuning:</strong> 지시문과 그에 대응하는 응답 쌍으로 구성된 멀티모달 데이터를 사용하여 미세 조정</li>
    <li><strong>Human Preference Tuning:</strong> 인간의 선호 데이터를 기반으로 추가 튜닝 수행</li>
  </ul></p>

  <p><strong>5. Long-Context Capabilities</strong>
  <ul>
    <li>최대 <strong>1,000만 토큰</strong>의 입력을 성능 저하 없이 처리할 수 있도록 아키텍처에 중요한 변화 적용</li>
  </ul></p>

  <p><strong>6. Optimization</strong>
  <ul>
    <li>아키텍처, 데이터, 최적화, 시스템 전반에 걸쳐 개선을 수행하여</li>
    <ul>
      <li>Gemini 1.0 Ultra에 견줄 만한 품질을</li>
      <li>훨씬 적은 훈련 계산량과 향상된 효율로 달성</li>
    </ul>
  </ul></p>

  <p><strong>Key Takeaways: </strong>
  <ul>
    <li>Gemini 1.5 Pro는 방대한 멀티모달/다국어 데이터, 고성능 하드웨어, 정교한 아키텍처를 활용</li>
    <li>훈련은 사전 학습 → 지시문 튜닝 → 인간 선호 튜닝의 여러 단계를 거침</li>
    <li>긴 문맥을 효율적으로 이해하는 능력 확보가 핵심 목표 중 하나</li>
  </ul></p>
</div>
</details><br>

---
##### Define an evaluator (평가자 정의하기)
이러한 요약 작업에서는, 출력을 여러가지 부분으로 평가하고 싶을 수 있다.  
예를 들어:  

- 모델이 프롬프트(요청)을 얼마나 잘 따랐는지 ⇒ **instruction following (지시 따르기)**
- 프롬프트에 있는 정보를 얼마나 잘 반영했는지 ⇒ **groundedness (근거성)**
- 생성된 텍스트가 얼마나 읽기 쉬운지 ⇒ **fluency (유창성)**
- 그 외에도 "**verbosity (장황성)**", "**quality (전반적인 품질)**" 등이 있다.   

이러한 평가 작업은 사람 평가자 <small style="color:gray">(human rater)</small>에게 지침을 주듯이 명확한 정의와 [평가 기준표(assessment rubric)](https://en.wikipedia.org/wiki/Rubric_%28academic%29)를 제공하여 LLM이 수행하도록 지시할 수 있다.  

이번에는 미리 작성된 "요약 평가"용 프롬프트를 사용하여 평가 에이전트<small style="color:gray">(evaluation agent)</small>를 정의하고, 생성된 요약문의 품질을 측정한다.  

<div style="background: #fff9e7; border-left: 4px solid #fdc82a; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💡 <strong>참고:</strong><br>
    groundedness<small style="color:gray">(근거성)</small>, safety<small style="color:gray">(안전성)</small>, coherence<small style="color:gray">(일관성)</small> 등 다양한 항목을 평가할 수 있는 더 많은 사전 작성된 평가용 프롬프트는 Google Cloud 공식 문서 <a href="https://cloud.google.com/vertex-ai/generative-ai/docs/models/metrics-templates">comprehensive list of model-based evaluation prompts</a>에서 확인할 수 있다.
</div><br>

아래 요약 평가 프롬프트 변수 `SUMMARY_PROMPT`의 내용을 요약하면 다음과 같다:  
모델에게 역할과 임무 / 평가 기준 / 점수 규칙 / 평가 방법을 알려주고 있다. 평가 기준은 **지시 사항을 잘 따랐는가** <small style="color:gray">(instruction following)</small>, **문서 내용에 충실한가** <small style="color:gray">(groundedness)</small>, **읽기 쉬운가** <small style="color:gray">(fluency)</small>로 구성되어 있고, 점수는 **5점 (very good)부터 1점 (very bad)으로 평가**한다. 

```python
import enum

# Define the evaluation prompt
SUMMARY_PROMPT = """\
# Instruction
You are an expert evaluator. Your task is to evaluate the quality of the responses generated by AI models.
We will provide you with the user input and an AI-generated responses.
You should first read the user input carefully for analyzing the task, and then evaluate the quality of the responses based on the Criteria provided in the Evaluation section below.
You will assign the response a rating following the Rating Rubric and Evaluation Steps. Give step-by-step explanations for your rating, and only choose ratings from the Rating Rubric.
Write your entire evaluation and explanation in Korean.

# Evaluation
## Metric Definition
You will be assessing summarization quality, which measures the overall ability to summarize text. Pay special attention to length constraints, such as in X words or in Y sentences. The instruction for performing a summarization task and the context to be summarized are provided in the user prompt. The response should be shorter than the text in the context. The response should not contain information that is not present in the context.

## Criteria
Instruction following: The response demonstrates a clear understanding of the summarization task instructions, satisfying all of the instruction's requirements.
Groundedness: The response contains information included only in the context. The response does not reference any outside information.
Conciseness: The response summarizes the relevant details in the original text without a significant loss in key information without being too verbose or terse.
Fluency: The response is well-organized and easy to read.

## Rating Rubric
5: (Very good). The summary follows instructions, is grounded, is concise, and fluent.
4: (Good). The summary follows instructions, is grounded, concise, and fluent.
3: (Ok). The summary mostly follows instructions, is grounded, but is not very concise and is not fluent.
2: (Bad). The summary is grounded, but does not follow the instructions.
1: (Very bad). The summary is not grounded.

## Evaluation Steps
STEP 1: Assess the response in aspects of instruction following, groundedness, conciseness, and verbosity according to the criteria.
STEP 2: Score based on the rubric.

# User Inputs and AI-generated Response
## User Inputs

### Prompt
{prompt}

## AI-generated Response
{response}
"""


# 평가 점수를 나타내는 enum 클래스
class SummaryRating(enum.Enum):
  VERY_GOOD = '5'
  GOOD = '4'
  OK = '3'
  BAD = '2'
  VERY_BAD = '1'

def eval_summary(prompt, ai_response):
  """주어진 프롬프트에 대해 생성된 요약문을 평가"""

  # Gemini 모델을 사용하여 채팅 세션 생성
  chat = client.chats.create(model='gemini-2.0-flash')

  # 평가 프롬프트를 문자열 formatiing으로 완성하고, 평가를 요청한다. 
  response = chat.send_message(
      message=SUMMARY_PROMPT.format(prompt=prompt, response=ai_response)
  )
  verbose_eval = response.text

  # 점수를 enum 형태로 변환하기 위한 설정 구성
  structured_output_config = types.GenerateContentConfig(
      response_mime_type="text/x.enum",
      response_schema=SummaryRating,
  )
  
  # 최종 점수만 추출해서 다시 요청
  response = chat.send_message(
      message="Convert the final score.",
      config=structured_output_config,
  )
  # 정형화된 점수 결과
  structured_eval = response.parsed

  return verbose_eval, structured_eval

# 평가 실행: 자세한 설명과 점수 받기
text_eval, struct_eval = eval_summary(prompt=[request, document_file], ai_response=summary)
# 마크다운 형식으로 출력
Markdown(text_eval)
```
<details>
  <summary><strong>Output</strong></summary>
  <br><strong>Evaluation Explanation</strong><br>
  전반적으로 Gemini 1.5 Pro의 학습 과정에 대한 개요를 잘 제시하고 있습니다. instruction following: 사용자의 질문에 맞춰 학습 과정에 대한 정보를 제공하고 있습니다. Groundedness: 제공된 문서에 근거하여 답변을 생성하고 있습니다. 외부 정보를 참조하지 않았습니다. Conciseness: 핵심 정보를 요약하고 있으며, 간결하게 작성되었습니다. Fluency: 문장 구성이 자연스럽고 읽기 쉽습니다.<br><br>
 하지만 학습 과정의 세부적인 내용 (예: 데이터 종류, TPUv4 가속기 사용, MoE Transformer 구조, pre-training, instruction tuning, human preference tuning, long-context capabilities, optimization)들을 나열하는 방식으로 요약되어 있어, 정보의 중요도나 관련성을 파악하기 어렵습니다. 따라서 4점으로 평가합니다.<br><br>
 <strong>Rating</strong><br>
 4: (Good). The summary follows instructions, is grounded, concise, and fluent.
</details><br>

이 예시에서는 모델이 채팅 형식의 문맥에서 텍스트 기반의 평가 근거 <small style="color:gray">(justification)</small>를 생성한다. 이렇게 생성된 전체 텍스트 응답은 사람에게는 해석에 도움이 되고, 모델에게는 텍스트를 평가하며 점수를 매기기 위한 **메모를 수집** <small style="color:gray">(collect notes)</small>하는 공간을 제공하는 역할을 한다.  

이러한 'note taking' 또는 ‘메모하기  <small style="color:gray">(thinking)</small>' 전략은 **오토리그레시브(auto-regressive)** 모델에서 특히 잘 작동하는데, 이는 매 단계에서 생성된 텍스트가 다음 단계 입력으로 다시 사용되기 때문이다.  

그러니까, 이 과정에서 작성된 메모는 최종 결과를 생성할 때 직접 활용된다.  

다음 단계에서는 모델이 이렇게 생성된 텍스트 출력을 **구조화된 응답으로 변환**한다.  

만약 점수를 집계하거나 프로그램적으로 활용하려면, 비정형 텍스트 출력을 일일이 파싱하는 방식은 피하는 게 좋다.  

여기서는 `SummaryRating` 스키마를 전달하여, 모델이 채팅 히스토리를 `SummaryRating` enum 인스턴스로 변환하게 했다. `struct_eval` 변수를 출력하면:  
```python
print(struct_eval)
```
```plaintext
<output>
<SummaryRating.GOOD: '4'>
```
구조화된 형태로 점수가 저장된 것을 확인할 수 있다.  

<br>

---
##### Make the summary prompt better or worse (프롬프트를 더 좋게 또는 더 나쁘게 만들어보기)
Gemini 모델은 간단한 프롬프트만으로도 요약 <small style="color:gray">(summarisation)</small> 작업을 꽤 잘 수행하는 편이다. 
따라서 아까 한 것 처럼 기본적인 프롬프트만으로도 `GOOD` 또는 `VERY_GOOD` 수준의 결과가 나오는 경우가 많다.  

프롬프트를 바꿔가면서 LLM의 출력과 evaluator를 사용한 평과 결과를 출력해본다면:
```python
# new prompt 설정: 훈련 과정을 5살 어린이에게 말하듯이 설명해줘
new_prompt = "Explain in Korean like I'm 5 the training process"
# Try:
#  ELI5 the training process
#  Summarise the needle/haystack evaluation technique in 1 line
#  Describe the model architecture to someone with a civil engineering degree
#  What is the best LLM?

# prompt가 비어있는 경우 에러 일으키기
if not new_prompt:
  raise ValueError("Try setting a new summarisation prompt.")

# 새로운 프롬프트로 요약문을 만들고, 그 결과를 평가하기
def run_and_eval_summary(prompt):
  # `summary_doc(new_prompt)`을 호출해 요약문 생성
  summary = summarise_doc(new_prompt)
  # 요약 결과 출력
  display(Markdown(summary + '\n-----'))
  # `eval_summary` 함수로 요약 결과 평가하기
  text, struct = eval_summary([new_prompt, document_file], summary)
  display(Markdown(text + '\n-----'))
  print(struct)

run_and_eval_summary(new_prompt)
```
<details>
  <summary><strong>output</strong></summary>
  <div style="background-color:#f6f8fa; padding: 1em; border-radius: 5px;">
    <p>네, 5살 아이에게 설명하는 것처럼 쉽게 설명해 드릴게요!</p>
    <p>자, 상상해봐. <strong>엄청 똑똑한 강아지</strong>가 있어. 이 강아지를 훈련시키는 거야.</p>
    <ol>
      <li><strong>데이터 주기:</strong> 먼저 강아지에게 많은 것을 보여주고 들려줘. 사진, 그림, 글, 소리 등 다양한 것을 보여주는 거야. 예를 들어, "사과"라는 단어를 가르치려면 사과 그림을 보여주면서 "사과"라고 말하는 거지.</li>
      <li><strong>강아지 생각하기:</strong> 강아지는 자기가 본 것과 들은 것을 가지고 "사과"가 무엇인지 생각하기 시작해.</li>
      <li><strong>잘못하면 혼내주기:</strong> 강아지가 "사과"를 잘못 생각하면 "땡!" 하고 알려줘. 그리고 다시 올바른 정보를 보여주는 거야.</li>
      <li><strong>잘하면 칭찬해주기:</strong> 강아지가 "사과"를 제대로 맞추면 "잘했어!" 하고 칭찬해줘.</li>
      <li><strong>계속 반복하기:</strong> 이 과정을 계속 반복하면 강아지는 점점 더 "사과"가 무엇인지 잘 알게 돼.</li>
    </ol>
    <p>이 강아지 훈련과정은 <strong>컴퓨터에게도 똑같이 적용</strong>돼요. 컴퓨터에게 많은 데이터를 주고, 컴퓨터가 생각하는 방식을 평가하고, 잘하면 칭찬하고, 잘못하면 혼내주는 과정을 반복하는 거죠. 이렇게 하면 컴퓨터는 점점 더 똑똑해져서 우리가 원하는 일을 더 잘 할 수 있게 된답니다!</p>
    <hr>
    <p><strong>Evaluation Explanation</strong></p>
    <ul>
      <li><strong>Instruction Following:</strong> 프롬프트에서 주어진 파일을 바탕으로 5살 아이에게 설명하듯이 훈련 과정을 설명하라는 지시를 잘 따랐습니다.</li>
      <li><strong>Groundedness:</strong> 제공된 파일의 내용을 바탕으로 답변을 생성했으며, 외부 정보를 참조하지 않았습니다.</li>
      <li><strong>Conciseness:</strong> 5살 아이의 눈높이에 맞춰 비유를 사용하여 간결하게 설명을 제공했습니다. 핵심 내용을 명확하게 전달하면서도 불필요한 정보를 줄였습니다.</li>
      <li><strong>Fluency:</strong> 문장 구성이 자연스럽고 이해하기 쉽습니다. 5살 아이에게 설명하는 듯한 어투를 사용하여 친근감을 더했습니다.</li>
    </ul>
    <p><strong>Summary:</strong><br>
    전반적으로 지시사항을 잘 따르고, 내용의 충실성, 간결성, 유창성 면에서 모두 우수한 답변을 생성했습니다.</p>
    <p><strong>Rating:</strong> 5 (Very good)</p><hr>
    <code>SummaryRating.VERY_GOOD</code>
  </div>
</details><br>

두 번째 프롬프트:  토목 공학 전공자에게 말하듯이 요약하기  
```python
# new prompt 설정: 훈련 과정을 토목 공학 전공자에게 말하듯이 설명해줘
new_prompt = "Describe the model architecture to someone with a civil engineering degree, in Korean"

# prompt가 비어있는 경우 에러 일으키기
if not new_prompt:
  raise ValueError("Try setting a new summarisation prompt.")

# 새로운 프롬프트로 요약문을 만들고, 그 결과를 평가하기
def run_and_eval_summary(prompt):
  # `summary_doc(new_prompt)`을 호출해 요약문 생성
  summary = summarise_doc(new_prompt)
  # 요약 결과 출력
  display(Markdown(summary + '\n-----'))
  # `eval_summary` 함수로 요약 결과 평가하기
  text, struct = eval_summary([new_prompt, document_file], summary)
  display(Markdown(text + '\n-----'))
  print(struct)

run_and_eval_summary(new_prompt)
```
<details>
  <summary><strong>output</strong></summary>
  <div style="background-color:#f6f8fa; padding: 1em; border-radius: 5px;">
    <p>알겠습니다. 토목 공학 전공자에게 Gemini 1.5 Pro 모델 아키텍처를 설명해 드리겠습니다.</p>
    <p><strong>Gemini 1.5 Pro 모델 아키텍처 설명 (토목 공학 전공자 대상)</strong></p>
    <p>안녕하세요. Gemini 1.5 Pro 모델은 쉽게 말해 매우 복잡한 구조물을 짓는 것과 같습니다. 이 구조물은 다양한 종류의 정보를 처리하고 이해할 수 있도록 설계되었습니다.</p>
    <p><strong>1. 기본 구조:</strong></p>
    <ul>
      <li><strong>Transformer 기반:</strong> 이 모델은 Transformer라는 특별한 신경망을 기반으로 합니다. Transformer는 마치 건물의 뼈대와 같은 역할을 하며, 정보 처리의 핵심 엔진입니다.</li>
      <li><strong>MoE (Mixture-of-Experts):</strong> 여러 전문가 그룹을 활용하는 방식입니다. 마치 다양한 분야의 전문가들이 모여 하나의 문제를 해결하는 것과 같습니다. 각 전문가는 특정 유형의 정보 처리에 특화되어 있어, 전체 모델의 효율성과 성능을 높입니다.</li>
    </ul>
    <p><strong>2. 정보 처리 과정:</strong></p>
    <ul>
      <li><strong>입력 (Input):</strong> 텍스트, 이미지, 오디오, 비디오 등 다양한 정보를 입력받습니다. 이는 마치 건물이 다양한 자재를 받아들이는 것과 같습니다.</li>
      <li><strong>토큰화 (Tokenization):</strong> 입력된 정보는 작은 조각(토큰)으로 나뉘며, 텍스트의 경우 단어나 하위 단위로 분할됩니다.</li>
      <li><strong>전문가 라우팅 (Expert Routing):</strong> 각 토큰은 MoE 구조 내에서 가장 적합한 전문가에게 전달됩니다. 이는 자재가 필요한 위치로 운반되는 과정과 비슷합니다.</li>
      <li><strong>정보 처리 및 추론:</strong> 선택된 전문가들은 토큰을 처리하고 문맥을 파악하며 필요한 추론을 수행합니다.</li>
      <li><strong>출력 (Output):</strong> 응답, 요약, 번역 등 다양한 결과를 생성합니다. 이는 마치 완성된 건물이 다양한 기능을 제공하는 것과 같습니다.</li>
    </ul>
    <p><strong>3. 주요 특징:</strong></p>
    <ul>
      <li><strong>긴 문맥 처리:</strong> 최대 1천만 토큰까지 처리할 수 있습니다. 이는 넓은 공간을 포괄하는 건물에 비유할 수 있습니다.</li>
      <li><strong>멀티모달 처리:</strong> 텍스트, 이미지, 오디오, 비디오 등 다양한 정보를 동시에 처리합니다. 이는 다양한 기능을 갖춘 복합 건물과 유사합니다.</li>
      <li><strong>높은 효율성:</strong> MoE 구조 덕분에 크고 복잡한 모델임에도 효율적으로 작동합니다. 이는 자재를 효율적으로 활용해 튼튼하게 지어진 건물과 비슷합니다.</li>
    </ul>
    <p><strong>4. 비유 요약:</strong></p>
    <ul>
      <li><strong>건물:</strong> Gemini 1.5 Pro 전체 모델</li>
      <li><strong>Transformer:</strong> 건물의 뼈대</li>
      <li><strong>MoE:</strong> 다양한 분야의 전문가 그룹</li>
      <li><strong>입력 정보:</strong> 자재 (텍스트, 이미지, 오디오, 비디오)</li>
      <li><strong>토큰:</strong> 자재의 작은 조각</li>
      <li><strong>전문가 라우팅:</strong> 자재를 필요한 위치로 운반</li>
      <li><strong>출력 결과:</strong> 건물의 기능 (응답, 요약, 번역 등)</li>
    </ul>
    <p>이 설명이 Gemini 1.5 Pro 모델 아키텍처를 이해하는 데 도움이 되었기를 바랍니다. 궁금한 점이 있다면 언제든지 질문해주세요.</p>
    <p><strong>Evaluation Explanation</strong><br>
    토목공학 전공자에게 모델 아키텍처를 설명하라는 지시를 잘 따랐습니다. Transformer 기반, MoE 등의 전문 용어들을 건축물에 비유하여 설명함으로써 토목공학 전공자가 이해하기 쉽도록 설명했습니다. 내용 또한 주어진 파일에 근거하여 작성되었으므로 hallucination은 없는 것으로 보입니다. 전체적으로 잘 요약되었고, 비유를 통해 이해를 도우려는 시도가 좋았습니다.</p>
    <p>Rating: 5</p>
    <hr>
   <code>SummaryRating: VERY_GOOD</code>
  </div>
</details><br>

---
### 💭 오늘 챙겨간 것들
LLM이 생성한 출력은 단순 정답 채점이 어려운 open-ended 형태가 많기 때문에, 평가 기준을 명확히 정의해야 한다. 
Gemini 모델을 사용하여 프롬프트 + 문서를 기준으로 요약 결과를 평가하고, **step-by-step 평가 근거와 최종 점수(정형화된 enum)**까지도 자동으로 받을 수 있다.  

특히 instruction following, groundedness, fluency 등 세부 항목별로 평가 기준을 나누고, 명시적인 점수 척도(rubric)를 정의하면 평가 품질이 좋아진다.  

다음 글에서는, **Pointwise / Pairwise 평가 기법**을 직접 실습하면서, LLM이 생성한 여러 응답들 중 어떤 것이 더 나은지를 비교 평가하는 방법을 정리해보겠다!

<br><br>