---
layout: post
title:  "[Kaggle Gen AI] Day 1 - Pointwise & Pairwise 평가 실습: LLM 답변 비교해보기 🚀"
author: me
categories: [ Kaggle Gen-AI, codelabs]
date: 2025-04-19 09:00:00
image: assets/images/20250402/day1_2.png
---
[지난 게시물](https://kbr1218.github.io/kaggle-gen-ai-podcast_16/)에서는  LLM이 생성한 출력을 평가하는 방법과, Gemini API를 활용해 평가 기준을 정의하고 정형화된 점수까지 얻는 방법을 실습해봤다.  

이번에는 **여러 개의 응답 중 어떤 게 더 나은지 판단하는 평가 방법**인 **Pointwise & Pairwise 평가 기법**에 대한 코드를 작성해보기로!  

실제 서비스에서는 LLM이 생성한 여러 개의 응답 중 **"가장 적절한 답변"**을 골라야 하는 경우가 많기 때문에 이러한 비교 평가 방식은 아주 유용하다.  

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 <a href="https://services.google.com/fh/files/blogs/neurips_evaluation.pdf" target="blank">대형 언어 모델 평가하기</a> 백서도 함께 참고하면 좋다. 
</div><br>

[👉 실습 원본 Kaggle notebook 링크 바로가기](https://www.kaggle.com/code/markishere/day-1-evaluation-and-structured-output)

<br>

---
### 🧪 Pointwise 평가

하나의 입력-출력 쌍을 기준에 따라 평가하는 방식을  **Pointwise Evaluation**라고 한다.  

즉, LLM이 생성한 **단일 응답 하나만 보고**, "이게 좋은 답변인가?"<small style="color:gray">("was it good or bad?")</small>를 절대적인 기준으로 평가하는 방식이다.  

여기서는 여러 개의 질문에 대해 **다양한 guidance prompt**를 적용해보고, 각 출력이 얼마나 잘 수행되었는지를 Pointwise 방식으로 평가해본다.  
```python
import functools

# 최대한 간결한 ver.
terse_guidance = "Answer the following question in a single sentence in Korean, or as close to that as possible."
#  필요하다면 출처를 인용하지만, 답변에 꼭 필요한 만큼만 사용하는 간단한 ver.
moderate_guidance = "Provide a brief answer to the following question in Korean, use a citation if necessary, but only enough to answer the question."
# 문서를 인용하고 가능한 한 배경 정보도 함께 제공하는 구체적인 ver.
cited_guidance = "Provide a thorough, detailed answer to the following question in Korean, citing the document and supplying additional background information as much as possible."
guidance_options = {
    'Terse': terse_guidance,
    'Moderate': moderate_guidance,
    'Cited': cited_guidance,
}

questions = [
    # 더 많은 질문을 평가할수록 시간이 더 소요되지만, 신뢰도 높은 결과를 얻을 수 있다. 
    # 실제 production 시스템에서는 수백 개의 질문으로 복잡한 시스템을 평가하기도 한다. 
    "What metric(s) are used to evaluate long context performance?",
    "How does the model perform on code tasks?",
    "How many layers does it have?",
    "Why is it called Gemini?",
]

# 평가할 질문이 없을 경우
if not questions:
  raise NotImplementedError('Add some questions to evaluate!')

@functools.cache
def answer_question(question: str, guidance: str = '') -> str:
  """업로드된 문서와 instruction을 기반으로 질문에 대한 답변 생성하기"""
  config = types.GenerateContentConfig(
      temperature=0.0,
      system_instruction=guidance,
  )
  response = client.models.generate_content(
      model='gemini-2.0-flash',
      config=config,
      contents=[question, document_file],
  )
  
  return response.text
```

<br>

LLM에게 줄 instruction options과 질문들, 그리고 질문에 대한 답을 저장하는 함수를 정의했으니 각 instruction option별로 질문을 해본다:  

```python
# 첫 번째 질문-간결하게: What metric(s) are used to evaluate long context performance?
answer1 = answer_question(questions[0], terse_guidance)
Markdown(answer1)
```
<details>
  <summary>Output</summary><small>
    <div style="background-color:#f6f8fa; padding: 1em; border-radius: 5px;">
  긴 문맥 성능을 평가하는 데 사용되는 지표는 모달리티 전반에 걸친 니들 리콜, 긴 문서 QA, 긴 비디오 QA, 긴 문맥 ASR 등이 있습니다.
  </div></small></details><br>

```python
# 두 번째 질문-간단하게: "How does the model perform on code tasks?"
answer2 = answer_question(questions[1], moderate_guidance)
Markdown(answer2)
```
<details>
  <summary>Output</summary><small>
    <div style="background-color:#f6f8fa; padding: 1em; border-radius: 5px;">
Gemini 1.5 Pro는 코딩 능력이 뛰어나며, 내부적으로 웹 유출을 방지하기 위해 제작된 코드 생성 테스트 세트인 Natural2Code에서 Gemini 1.0 Ultra를 능가합니다 (Gemini Team et al., 2023).</div></small></details><br>

```python
# 세 번째 질문-자세하게: "How many layers does it have?"
answer3 = answer_question(questions[2], cited_guidance)
Markdown(answer3)
```
<details>
  <summary>Output</summary><small>
  <div style="background-color:#f6f8fa; padding: 1em; border-radius: 5px;">
죄송합니다. 해당 문서에서 Gemini 1.5 Pro의 레이어 수를 찾을 수 없습니다. 그러나 문서에는 Gemini 1.5 Pro가 Gemini 1.0의 연구 발전과 다중 모드 기능을 기반으로 구축된 희소 혼합 전문가(MoE) 변환기 기반 모델이라고 명시되어 있습니다.</div></small></details><br>


```python
# 네 번째 질문-자세하게: "Why is it called Gemini?"
answer4 = answer_question(questions[3], cited_guidance)
Markdown(answer4)
```
<details>
  <summary>Output</summary><small> <div style="background-color:#f6f8fa; padding: 1em; border-radius: 5px;">
해당 문서에서는 Gemini 모델의 이름에 대한 직접적인 언급은 없습니다. 하지만 Gemini 1.5 Pro 기술 보고서의 서론 부분에서 다음과 같은 정보를 얻을 수 있습니다.<br><br>
Gemini 제품군: Gemini 1.5 Pro는 Gemini 제품군의 최신 모델입니다.<br>
다양한 기능: Gemini 1.5는 효율성, 추론, 긴 문맥 성능의 경계를 넓히는 새로운 다기능 모델입니다.<br>
따라서 Gemini라는 이름은 특정 모델의 이름이라기보다는 Google에서 개발한 일련의 모델 제품군을 지칭하는 이름으로 이해할 수 있습니다.<br><br>

추가 배경 정보:<br>
일반적으로 모델의 이름은 개발 목표, 아키텍처, 또는 영감을 준 요소들을 반영하는 경우가 많습니다. Gemini라는 이름은 아마도 다음과 같은 의미를 내포하고 있을 가능성이 있습니다.<br>
Gemini (쌍둥이자리): Gemini는 쌍둥이자리라는 뜻으로, 두 가지 이상의 기능을 융합한 모델이라는 의미를 담고 있을 수 있습니다. Gemini 모델은 텍스트, 이미지, 오디오, 비디오 등 다양한 형태의 데이터를 처리할 수 있는 멀티모달(multimodal) 모델입니다.<br>
Geminate (쌍을 이루다): Gemini는 "쌍을 이루다"라는 뜻의 동사 "geminate"에서 유래했을 수도 있습니다. 이는 Gemini 모델이 다양한 데이터 유형을 연결하고, 복잡한 관계를 이해하는 능력을 강조하는 것일 수 있습니다.<br>
하지만 정확한 작명 이유는 Google만이 알 수 있습니다.</div></small></details><br><br>

이제 [pointwise QA 평가 프롬프트](https://cloud.google.com/vertex-ai/generative-ai/docs/models/metrics-templates#pointwise_question_answering_quality)를 사용하여 질문-응답 evaluator를 설정한다.  

```python
import enum

QA_PROMPT = """\
 (생략)

# User Inputs and AI-generated Response
## User Inputs
### Prompt
{prompt}

## AI-generated Response
{response}
"""

# 답변 평가 점수를 나타내는 enum 클래스
class AnswerRating(enum.Enum):
  VERY_GOOD = '5'
  GOOD = '4'
  OK = '3'
  BAD = '2'
  VERY_BAD = '1'


@functools.cache
def eval_answer(prompt, ai_response, n=1):
  """주어진 질문과 AI 응답 평가하기"""
  chat = client.chats.create(model='gemini-2.0-flash')

  # 평가용 전체 텍스트 생성 요청
  response = chat.send_message(
      message=QA_PROMPT.format(prompt=[prompt, document_file], response=ai_response)
  )
  verbose_eval = response.text


  # 결과를 구조화된 enum 형태로 변환하기 위한 설정
  structured_output_config = types.GenerateContentConfig(
      response_mime_type="text/x.enum",
      response_schema=AnswerRating,
  )
  # 최종 점수만 추출
  response = chat.send_message(
      message="Convert the final score.",
      config=structured_output_config,
  )
  structured_eval = response.parsed

  return verbose_eval, structured_eval

# 질문에 대해 답변을 평가하고 결과 출력
text_eval, struct_eval = eval_answer(prompt=questions[0], ai_response=answer1)
display(Markdown(text_eval))
print(struct_eval)
```

예시로 첫 번째 질문-응답에 대한 평가를 출력하면:

```plaintext
STEP 1: 평가 항목 분석

Instruction following: 프롬프트는 주어진 파일(PDF)을 참고하여 긴 문맥 성능 평가에 사용되는 지표를 묻고 있습니다. 답변은 이 지시사항을 따라야 합니다.
Groundedness: 답변은 제공된 PDF 파일 내의 정보만을 기반으로 해야 합니다. 외부 정보는 참조해서는 안 됩니다.
Completeness: 답변은 질문에 대한 충분한 정보를 제공해야 합니다. 즉, 긴 문맥 성능을 평가하는 데 사용되는 주요 지표들을 빠짐없이 언급해야 합니다.
Fluency: 답변은 자연스럽고 이해하기 쉬운 한국어 문장으로 구성되어야 합니다.
STEP 2: 답변 평가 및 점수 부여

제공된 답변은 PDF 파일의 내용을 바탕으로 긴 문맥 성능을 평가하는 데 사용되는 지표들을 나열하고 있습니다.

니들 리콜(Needle Recall),
긴 문서 QA (Long Document QA),
긴 비디오 QA (Long Video QA),
긴 문맥 ASR (Long Context ASR)
이러한 지표들은 PDF 파일에서 언급되었을 가능성이 높으며, 답변은 해당 파일의 내용을 잘 반영하고 있는 것으로 판단됩니다. (파일 내용을 직접 확인할 수 없으므로 이 부분은 추론에 기반합니다.) 답변은 질문에 대한 핵심적인 정보를 포함하고 있으며, 문장 또한 자연스럽습니다.

따라서, 위에서 제시된 평가 기준에 따라 이 답변은 5점 (Very good)을 받을 만하다고 판단됩니다.

Explanation:

Instruction following: 질문에 대한 명확한 이해를 바탕으로 답변을 제시했습니다.
Groundedness: 제공된 파일 내의 정보를 기반으로 답변을 작성한 것으로 판단됩니다. (파일 내용 확인 불가)
Completeness: 긴 문맥 성능 평가에 사용되는 주요 지표들을 충분히 언급하고 있습니다.
Fluency: 자연스럽고 이해하기 쉬운 한국어 문장으로 답변을 구성했습니다.

AnswerRating.VERY_GOOD
```
이런 식으로 **평가용 전체 텍스트**와 **구조화된 최종 점수**를 확인할 수 있다.   

<br>

여기서 주의해야 할 점은 평가 작업을 실행할 때 **instruction과 prompt는 모두 평가 에이전트에게 전달되지 않는다는 것**이다.  

만약 프롬프트를 평가 에이전트에게 넘긴다면, 모델은 그 instruction을 얼마나 잘 따랐는지를 기준으로 점수를 매긴다. 하지만 여기서는 개발자가 설정한 instruction이 아니라, **사용자의 질문에 대해 가장 좋은 결과를 찾는 것**이 목표이다.  

<div style="background: #fff9e7; border-left: 4px solid #fdc82a; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
💡 <strong>정리</strong><br>
사용자의 질문이랑 Gemini가 생성한 response만 가지고 평가하는 것!
</div><br>

이제 위에서 실행한 하나의 평가를 반복문을 사용해서 각 질문마다 2번씩 반복해서 실행하고 평균 점수를 낸다.  
> (4개의 질문) * (3개의 guidance_options) * 2

반복 횟수를 늘리면 시간이 오래 걸리지만, **오차를 줄이고 평균 결과**를 얻을 수 있다.   

```python
import collections
import itertools

# 각 작업을 반복할 횟수 설정
# 반복 횟수를 늘리면 시간이 오래 걸리지만, 오차를 줄이고 평균 결과를 얻을 수 있다 
NUM_ITERATIONS = 2

# 점수를 저장할 딕셔너리
scores = collections.defaultdict(int)

# 생성된 응답과 평가를 저장할 딕셔너리
responses = collections.defaultdict(list)

for question in questions:
  display(Markdown(f'## {question}'))
  for guidance, guide_prompt in guidance_options.items():

    for n in range(NUM_ITERATIONS):
      # 답변 생성
      answer = answer_question(question, guide_prompt)

      # 답변 평가 (prompt와 instruction은 전달하지 않음 주의!) 
      written_eval, struct_eval = eval_answer(question, answer, n)
      print(f'{guidance}: {struct_eval}')

      # 평가 점수 저장
      scores[guidance] += int(struct_eval.value)

      # 생성된 응답과 평가 결과를 저장
      responses[(guidance, question)].append((answer, written_eval))
```
```plaintext
<output>
What metric(s) are used to evaluate long context performance?
Terse: AnswerRating.VERY_GOOD
Terse: AnswerRating.VERY_GOOD
Moderate: AnswerRating.VERY_GOOD
Moderate: AnswerRating.VERY_GOOD
Cited: AnswerRating.GOOD
Cited: AnswerRating.VERY_GOOD

How does the model perform on code tasks?
Terse: AnswerRating.OK
Terse: AnswerRating.VERY_GOOD
Moderate: AnswerRating.VERY_GOOD
Moderate: AnswerRating.VERY_GOOD
Cited: AnswerRating.VERY_GOOD
Cited: AnswerRating.VERY_GOOD

How many layers does it have?
Terse: AnswerRating.VERY_GOOD
Terse: AnswerRating.VERY_GOOD
Moderate: AnswerRating.VERY_BAD
Moderate: AnswerRating.BAD
Cited: AnswerRating.OK
Cited: AnswerRating.GOOD

Why is it called Gemini?
Terse: AnswerRating.OK
Terse: AnswerRating.VERY_GOOD
Moderate: AnswerRating.OK
Moderate: AnswerRating.VERY_GOOD
Cited: AnswerRating.VERY_GOOD
Cited: AnswerRating.VERY_GOOD
```
각 질문에 대한 평가 점수를 `score` 변수에 저장했으니 각 프롬프트가 어떤 성능을 보이는지 확인하기 위해 **점수를 집계**해본다.  

```python
for guidance, score in scores.items():
  # 평균 점수 계산: 총 점수 / (반복 횟수 * 질문 수)
  avg_score = score / (NUM_ITERATIONS * len(questions))

  # 평균 점수를 가장 가까운 정수로 반올림한 뒤, AnswerRating enum으로 변환
  nearest = AnswerRating(str(round(avg_score)))
  print(f'{guidance}: {avg_score:.2f} - {nearest.name}')
```
```plaintext
<output>
Terse: 4.50 - GOOD
Moderate: 3.88 - GOOD
Cited: 4.50 - GOOD
```

<br>

여기까지 Pointwise 평가는 출력에 대해  **5단계 등급 체계(1~5점)**로 점수를 매기는 방식이었다.  

하지만 이 방식은 **결과의 차이를 정밀하게 구분하기 어렵거나** 이미 `VERY GOOD` 수준의 프롬프트를 **더 개선하고 싶을 때** 한계가 있을 수 있다.  

이럴 때 사용할 수 있는 또 다른 평가 방법이 **두 개의 출력 결과를 서로 비교하는 Pairwise Evaluation 방식**이다.  

<br>

---
### ⚖️ Pairwise 평가
이 방식은 **랭킹(rank) 및 정렬(sort) 알고리즘의 핵심 기법**으로,  프롬프트들을 **상대적으로 비교**할 수 있기 때문에,  Pointwise 평가를 **보완하거나 대체**하는 데에 효과적이다.  

이번 실습에서는 Google Cloud 문서에서 제공하는 [[Pairwise QA Quality Prompt]](https://cloud.google.com/vertex-ai/generative-ai/docs/evaluate/evaluation-prompts)를 사용하여  두 개의 응답 중 어떤 것이 더 나은지를 비교해보겠다.

- Response A가 더 나으면 → **"A"**  
- Response B가 더 나으면 → **"B"**  
- 두 응답이 비슷한 수준이면 → **"SAME"**  

<div style="background: #fff9e7; border-left: 4px solid #fdc82a; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
💡 간단하지만 실제 서비스에서 <strong>우열을 가리는 데 매우 유용한 방식</strong>이다!
</div><br>

```python
QA_PAIRWISE_PROMPT = """ (생략)
# User Inputs and AI-generated Responses
## User Inputs
### Prompt
{prompt}

# AI-generated Response

### Response A
{baseline_model_response}

### Response B
{response}
"""

# 평가 결과로 반환될 값 (A가 더 낫다 / B가 더 낫다 / 동일하다)
class AnswerComparison(enum.Enum):
  A = 'A'
  SAME = 'SAME'
  B = 'B'

@functools.cache
def eval_pairwise(prompt, response_a, response_b, n=1):
  """같은 질문에 대한 두 개의 답변 중 어떤 것이 더 나은지 평가"""

  # Gemini 모델 채팅 세션 생성
  chat = client.chats.create(model='gemini-2.0-flash')

  # 전체 평가 텍스트 생성 요청
  response = chat.send_message(
      message=QA_PAIRWISE_PROMPT.format(
          prompt=[prompt, document_file],
          baseline_model_response=response_a,
          response=response_b)
  )
  verbose_eval = response.text

  # 구조화된 출력 형식으로 점수 변환
  structured_output_config = types.GenerateContentConfig(
      response_mime_type="text/x.enum",
      response_schema=AnswerComparison,
  )
  # 최종 평가 결과를 enum 형태로 요청
  response = chat.send_message(
      message="Convert the final score.",
      config=structured_output_config,
  )
  structured_eval = response.parsed

  return verbose_eval, structured_eval
```
이렇게 프롬프트와 구조화된 출력을 위한 enum class, 평가 비교 함수를 정의하고 실제 실행을 하면:

```python
# 실제 실행: 하나의 질문에 대해 두 가지 가이드라인으로 생성된 답변 비교
question = questions[0]
answer_a = answer_question(question, terse_guidance)  # 간결한 답변
answer_b = answer_question(question, cited_guidance)  # 인용 포함 상세 답변

# pairwise 평가 실행
text_eval, struct_eval = eval_pairwise(
    prompt=question,
    response_a=answer_a,
    response_b=answer_b,
)

# 평가에 대한 설명과 최종 선택 결과 출력
display(Markdown(text_eval))
print(struct_eval)
```
```plaintext
<output>
STEP 1: Response A는 주어진 파일의 내용을 기반으로 질문에 대한 답변을 간략하게 제시하고 있습니다. 하지만 구체적인 설명이나 추가 정보는 부족합니다.
STEP 2: Response B는 Gemini 1.5 Pro 보고서를 기반으로 장문맥 성능 평가에 사용되는 주요 지표들을 상세하게 설명하고 있습니다. 각 지표에 대한 설명과 평가 방법, 그리고 추가 정보까지 제공하여 답변의 완성도가 높습니다.
STEP 3: Response B가 A보다 파일 내용을 기반으로 질문에 대한 답변을 더 자세하고 명확하게 제공하므로, 사용자의 요구사항을 더 잘 충족한다고 판단됩니다. 따라서 B가 A보다 더 나은 답변입니다.
STEP 4: B
STEP 5: Response B는 Gemini 1.5 Pro 보고서에 언급된 장문맥 성능 평가 지표들을 상세하게 설명하고 있으며, 각 지표의 의미와 평가 방법을 명확하게 제시합니다. 또한, 추가 정보와 멀티모달, 문맥 창 등의 용어 설명까지 제공하여 답변의 이해도를 높입니다. 반면, Response A는 단순히 지표들을 나열하는 수준에 그쳐 구체적인 정보가 부족합니다. 따라서 Response B가 Response A보다 더 유용하고 완전한 답변을 제공한다고 판단했습니다.

AnswerComparison.B
```
프롬프트에서 지시한 대로 두 가지 모델을 비교하고, 평가에 대한 설명과 최종 선택 결과를 이렇게 확인할 수 있다.  

---
Pairwise 평가 방식을 적용했으니,  이제 **여러 프롬프트 중 어떤 게 더 나은지 순위를 매기기 위해 필요한 것**은  **비교 함수(comparator)**이다.  

여기서는 전체 정렬<small style="color:gray">(total ordering)</small>을 위해 꼭 필요한 최소 연산자인  `==` 와 `<` 만을 구현하여

- 여러 프롬프트를 **쌍으로 비교하고**,  
- 이를 `n_iterations`만큼 반복하면서 **프롬프트 간 상대적 순위를 구해본다.**

이런 식의 반복 비교는 실제 시스템에서 프롬프트 최적화를 자동화하거나, AB 테스트를 대체할 수 있는 간단하지만 효과적인 방법이다!

```python
@functools.total_ordering
class QAGuidancePrompt:
  """질문-응답에서 사용하는 prompt 또는 instruction"""
    
  def __init__(self, prompt, questions, n_comparisons=NUM_ITERATIONS):
    """prompt 인스턴스를 생성
       평가에 사용할 질문 리스트와 각 질문당 평가를 몇 번 수행할지 결정"""
    self.prompt = prompt
    self.questions = questions
    self.n = n_comparisons

  def __str__(self):
    """문자열로 출력할 때 프롬프트 내용이 보이도록 설정"""
    return self.prompt

  def _compare_all(self, other):
    """모든 질문에 대해 n회씩 비교 수행하여 평균적인 비교 결과를 반환한다.
       이 값이 > 0이면 self가 더 낫고, < 0이면 other가 낫다"""
    results = [self._compare_n(other, q) for q in questions]
    mean = sum(results) / len(results)
    return round(mean)

  def _compare_n(self, other, question):
    """하나의 질문에 대해 n회 평가를 수행하고 평균 결과 반"""
    results = [self._compare(other, question, n) for n in range(self.n)]
    mean = sum(results) / len(results)
    return mean

  def _compare(self, other, question, n=1):
    """한 번의 질문 평가에서 두 프롬프트 중 무엇이 더 나은지 비교
       Gemini로부터 응답을 생성하고 pairwise 평가 수행"""
    answer_a = answer_question(question, self.prompt)
    answer_b = answer_question(question, other.prompt)

    _, result = eval_pairwise(
        prompt=question,
        response_a=answer_a,
        response_b=answer_b,
        n=n,  # Cache buster (캐시 무효화를 위한 인자)
    )
    # print(f'q[{question}], a[{self.prompt[:20]}...], b[{other.prompt[:20]}...]: {result}')

    # 평가 결과 enum을 정수로 변환 (A가 낫다 = 1, B가 낫다 = -1, 동일 = 0)
    if result is AnswerComparison.A:
      return 1
    elif result is AnswerComparison.B:
      return -1
    else:
      return 0

  def __eq__(self, other):
    """두 프롬프트가 동등한지 판단 (pairwise 평가 평균이 0인지로 판단)"""
    if not isinstance(other, QAGuidancePrompt):
      return NotImplemented

    return self._compare_all(other) == 0

  def __lt__(self, other):
    """self가 other보다 열등한지 판단 (평균 비교 결과가 음수이면 True)"""
    if not isinstance(other, QAGuidancePrompt):
      return NotImplemented

    return self._compare_all(other) < 0
```

이제 정렬 함수들을 `QAGuidancePrompt` 인스턴스에서 그대로 사용할 수 있다.  

`answer_question`과 `eval_pairwise` 함수는  `@functools.cache` 데코레이터가 적용된 [**memoized**](https://en.wikipedia.org/wiki/Memoization) 함수이기 때문에, **같은 질문-프롬프트 쌍에 대해서는 중복 생성 없이 캐시된 결과를 바로 재사용**할 수 있다.  

따라서 질문, 프롬프트, 반복 횟수(`n_iterations`) 이 세 가지 중 **하나라도 변경되지 않았다면**, 정렬 과정은 아주 빠르게 완료된다!
```python
# 각 프롬프트를 평가 가능한 객체로 설정
terse_prompt = QAGuidancePrompt(terse_guidance, questions)
moderate_prompt = QAGuidancePrompt(moderate_guidance, questions)
cited_prompt = QAGuidancePrompt(cited_guidance, questions)

# 프롬프트 정렬 (reverse = True로 설정해서 가장 우수한 프롬프트가 첫 번째로 오도록)
sorted_results = sorted([terse_prompt, moderate_prompt, cited_prompt], reverse=True)
for i, p in enumerate(sorted_results):
  if i:
    print('---')

  print(f'#{i+1}: {p}')
```
실행 결과는 아래와 같이 나왔는데:
```plaintext
#1: Answer the following question in a single sentence in Korean, or as close to that as possible.
---
#2: Provide a brief answer to the following question in Korean, use a citation if necessary, but only enough to answer the question.
---
#3: Provide a thorough, detailed answer to the following question in Korean, citing the document and supplying additional background information as much as possible.
```

같은 질문에 대해 서로 다른 프롬프트를 사용해 답변을 생성하고, 그 응답들을 **Pairwise 방식으로 비교 평가**해본 결과, 가장 높은 순위를 받은 건 **짧고 단순한 프롬프트**였다.  

한 문장으로 답하게 하는 프롬프트가 가장 높은 평가를 받고, 그다음으로는 **간결한 프롬프트**,  가장 낮은 평가는 **자세한 설명을 유도하는 프롬프트**였다. 

이건 모델이 **간결하고 명확한 답변을 생성할 때 더 긍정적으로 평가**되고,  정보량이 많더라도 **지나치게 장황하거나 비효율적인 표현**은 오히려 감점 요인이 될 수 있다는 걸 보여주는 거 같다.  

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
  💬 더 많은 정보 ≠ 더 나은 답변, <br> 명확하고 적절한 정보량이 중요!
</div>

아무튼 여기까지 실제 시스템에서 응답 품질을 높이고자 할 때, **프롬프트 스타일 조정만으로도 출력의 품질이 크게 달라질 수 있다**는 점을 확인할 수 있었다.

<br>

---
### ⚠️ 평가 시스템의 한계: LLM만으로는 부족한 이유
LLM을 evaluator로 사용하는 것이 매우 유용하긴 하지만, **모든 평가 작업에 항상 정확한 결과를 보장하지 않는다.**  

예를 들어,  단어 안의 문자 수를 세는 것처럼 **수리적인 작업**은 언어 모델의 특성상 정확하게 수행하기 어려운 경우가 있다.  

이는 **언어적 한계가 아니라 계산 능력의 한계**로, LLM evaluator 역시 이런 유형의 작업은 **정확하게 평가하지 못할 수 있다.**  

이런 경우에는
- 외부 도구 <small style="color:gray">(tool)</small>를 연결하거나
- 사람 평가자 <small style="color:gray">(human rater)</small>를 포함하여

**LLM 평가 시스템을 보완하고 기준선**<small style="color:gray">(baseline)</small>을 잡는 작업이 중요하다.  

<div style="margin-top: 3.3em;"></div>

LLM evaluator가 잘 작동하는 이유는 대부분 다음 조건이 충족될 때이다:

✔️ 평가에 필요한 정보가 **모두 입력 context에 포함**되어 있고,  
✔️ 모델이 그 정보에만 **집중(attend)**해서 평가를 수행할 수 있는 경우이다.  

모델은 그 정보에만 집중해서 결과를 만들어내기 때문에, 우리가 평가 프롬프트를 **커스터마이징**하거나 **자체 평가 시스템을 구축**할 때는
- 모델 내부의 **사전 지식이나**
- 외부 도구로 대체할 수 있는 기능에 의존하지 않도록  
주의해야 한다.  

<div style="background: #fff9e7; border-left: 4px solid #fdc82a; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
💡 가능한 한 <strong>정보가 명확히 주어진 상태에서 판단하도록 설계</strong>해야,  LLM 평가 결과의 신뢰성과 일관성을 확보할 수 있다.  
</div><br>

---
### 🔍 평가의 신뢰도를 높이는 법: 다양한 모델 활용하기
LLM 평가 시스템에서 **신뢰도**<small style="color:gray">(confidence)</small>를 높이고 싶다면,  
가장 효과적인 방법 중 하나는 **다양한 평가자**<small style="color:gray">(evaluators)</small>를 함께 사용하는 것이다.  

예를 들어:
- **Gemini Flash**, **Gemini Pro**  
- **Claude**, **ChatGPT**  
- **Gemma**, **Qwen** 등과 같은 모델이 있다. 

이처럼 서로 다른 계열의 모델들에 동일한 평가 작업을 수행시키면, 더 **다양한 의견**<small style="color:gray">(opinions)</small>을 얻을 수 있고,  **오차를 줄이고 신뢰도를 높일** 수 있다.  

이 접근은 앞서 봤던 **반복 평가**<small style="color:gray">(multiple trials)</small>와 유사하지만, 이번에는 **반복이 아니라 ‘모델의 다양성’**을 통해 의견을 확보한다는 점에서 차이가 있다.

<br>

---
### 💭 오늘 챙겨간 것들
이번 글에서는  `pointwise` vs. `pairwise` 평가 방식을 출력으로 직접 확인해봤다.  

또한,  
✔️ 평가의 한계(LM 자체의 수리적 약점 등)와  
✔️ 신뢰도를 높이기 위한 팁 (다양한 모델 활용, 반복 평가, 캐싱 등)까지 확인했고,  

여기가 Day1의 끝이다.  

<div style="margin-top: 3.3em;"></div>

##### 📚 Day 1 전체 요약
Day 1에서는 다음과 같은 내용을 다뤘다:  

1. **LLM 개념 잡기**  
트랜스포머 구조부터 파인튜닝, 추론 최적화까지 LLM 발전 흐름 정리  

2. **프롬프트 엔지니어링**  
효과적인 질문 구성 방법과 다양한 프롬프트 스타일 실습  

3. **코드랩 1: 프롬프트 엔지니어링**  
파라미터 조정과 프롬프트에 따라 출력이 어떻게 달라지는지 실험  

4. **코드랩 2: 평가와 구조화된 출력 만들기**  
Gemini를 활용한 자동 평가, enum 기반 structured output 생성  
Pointwise & Pairwise 평가 방식 비교 및 정렬

<div style="margin-top: 3.3em;"></div>

##### 🔜 Day 2
Day 2에서는 **임베딩(Embeddings)**과 **벡터 데이터베이스**를 중심으로 진행된다.  
바이바이

<br><br>