---
layout: post
title:  "[Kaggle Gen AI] Day 1 - 프롬프트 엔지니어링 실습 두 번째 🚀"
author: me
categories: [ Kaggle Gen-AI, codelabs]
date: 2025-04-17 09:00:00
image: assets/images/20250402/day1_2.png
---
[지난 게시물](https://kbr1218.github.io/kaggle-gen-ai-podcast_14/)에서는 Kaggle notebook 환경 세팅이랑 기본적인 프롬프트 작성 방법을 정리했고,  

이번에는 보다 실전적인 기법들을 적용해 모델의 활용도를 높이는 방법을 중심으로 실습을 따라해 볼 예정. 
zero-shot, one-shot, few-shot 같은 다양한 입력 방식부터, Chain of Thought(CoT), JSON mode, ReAct 접근법까지   

[👉 실습 원본 Kaggle notebook 링크 바로가기](https://www.kaggle.com/code/markishere/day-1-prompting)

<br>

---
### ✏ 1. Zero-shot Prompting
Zero-shot 프롬프트는 별도의 예시 없이 요청만 전달하는 방식이다.  
```python
model_config = types.GenerateContentConfig(
    temperature=0.1,
    top_p=1,
    max_output_tokens=5,
)
# 영화 리뷰를 긍정 / 중립 / 부정으로 분류하기
zero_shot_prompt = """Classify movie reviews as POSITIVE, NEUTRAL or NEGATIVE.
Review: "Her" is a disturbing study revealing the direction
humanity is headed if AI is allowed to keep evolving,
unchecked. I wish there were more movies like this masterpiece.
Sentiment: """

response = client.models.generate_content(
    model='gemini-2.0-flash',
    config=model_config,
    contents=zero_shot_prompt)

print(response.text)
```
```plaintext
<output>
POSITIVE
```
이번에는 프롬프트의 review만 조금 변경해서 다시 질문
```python
# 영화 리뷰를 긍정 / 중립 / 부정으로 분류하기
zero_shot_prompt = """Classify movie reviews as POSITIVE, NEUTRAL or NEGATIVE.
Review: movie "Her" was quite interesting. but kinda boring some part.
Sentiment: """

response = client.models.generate_content(
    model='gemini-2.0-flash',
    config=model_config,
    contents=zero_shot_prompt)

print(response.text)
```
```plaintext
<output>
Sentiment: NEUTRAL
```

모델은 텍스트를 생성하도록 학습되어 있지만, 가끔은 모델이 원하지 않는 텍스트를 생성할 수 있다.  

위에서 실행한 두 번의 예시를 보면 모델은 label을 출력하지만 가끔 그 앞에 `"Sentiment:"`라는 단어를 붙이기도 하고, 출력 토큰 수에 제한이 없다면 설명 텍스트를 덧붙이기도 한다.  

그래서 Gemini API에는 출력값을 고정된 값 집합 중 하나로 제한할 수 있는 **Enum mode**라는 게 있다.  


<br>

---
### 🔌2. Enum mode
```python
import enum

# 출력값을 세 가지 중 하나로 고정
class Sentiment(enum.Enum):
    POSITIVE = "positive"
    NEUTRAL = "neutral"
    NEGATIVE = "negative"


response = client.models.generate_content(
    model='gemini-2.0-flash',
    config=types.GenerateContentConfig(
        response_mime_type="text/x.enum",
        response_schema=Sentiment
    ),
    # 위에서 작성한 프롬프트를 다시 넣기
    contents=zero_shot_prompt)

print(response.text)
# 모델이 생성한 텍스트 그대로를 보여줌
```
```plaintext
<output>
neutral
```
enum과 같은 제한된 출력을 사용하면, Python SDK는 모델의 텍스트 응답을 자동으로 Python 객체로 변환하려고 시도하고, 변환된 값은 `response.parsed`에 저장된다.  

그래서 `enum_response` 변수에 `response.parsed` 값을 저장하고, `enum_response`와 그 type을 출력하면
```python
enum_response = response.parsed
print(enum_response)
print(type(enum_response))
```
```plaintext
<output>
Sentiment.NEUTRAL
<enum 'Sentiment'>
```
모델이 출력한 값을 Sentiment Enum 클래스의 인스턴트로 자동 변환한 `Sentiment.NEUTRAL` 객체가 출력되고,  

`enum_response`라는 객체는 `Sentiment`라는 Enum 클래스의 멤버라는 의미의 `<enum 'Sentiment>`가 출력된다.  

<br>

---
### 💬 3. One-shot and few-shot
모델에게 기대하는 응답의 예시를 하나 제공하는 것을 One-shot 프롬프트라고 하고, 여러 개의 예시를 제공하는 경우를 Few-shot 프롬프트라고 한다.  

사용자의 입력과 함께 원하는 JSON Response 여러 개를 예시로 제공하는 프롬프트는 아래와 같이 작성할 수 있다.
```python
few_shot_prompt = """Parse a customer's pizza order into valid JSON:

EXAMPLE:
I want a small pizza with cheese, tomato sauce, and pepperoni.
JSON Response:
`` `
{
"size": "small",
"type": "normal",
"ingredients": ["cheese", "tomato sauce", "pepperoni"]
}
`` `

EXAMPLE:
Can I get a large pizza with tomato sauce, basil and mozzarella
JSON Response:
`` `
{
"size": "large",
"type": "normal",
"ingredients": ["tomato sauce", "basil", "mozzarella"]
}
`` `
ORDER:
"""

customer_order = "버섯과 치즈가 들어간 라지 피자를 주십쇼"

response = client.models.generate_content(
    model='gemini-2.0-flash',
    config=types.GenerateContentConfig(
        temperature=0.1,
        top_p=1,
        max_output_tokens=250,
    ),
    contents=[few_shot_prompt, customer_order])

print(response.text)
```
```plaintext
<output>
```json
{
"size": "large",
"type": "normal",
"ingredients": ["mushroom", "cheese"]
}
`` `
```

<br>

---
### 📑 4. JSON mode
응답 결과의 스키마를 제어하고, 텍스트나 마크다운 없이 **JSON 형식**만 받도록 하려면 Gemini API의 [**JSON 모드**](https://github.com/google-gemini/cookbook/blob/main/quickstarts/JSON_mode.ipynb)를 사용할 수 있다.  

이걸 사용하면 모델이 디코딩할 때 제공된 스키마에 맞춰 토큰을 선택하도록 강제할 수 있다. 피자를 주문받는 이전 예시를 JSON 모드를 사용하는 걸로 변경해보자면:
```python
import typing_extensions as typing

class PizzaOrder(typing.TypedDict):
    size: str
    ingredients: list[str]
    type: str


response = client.models.generate_content(
    model='gemini-2.0-flash',
    config=types.GenerateContentConfig(
        temperature=0.1,
        response_mime_type="application/json",
        response_schema=PizzaOrder,
    ),
    contents="Can I have a large dessert pizza with apple and chocolate")

print(response.text)

```
```plaintext
{
  "size": "large",
  "ingredients": ["apple", "chocolate"],
  "type": "dessert"
}
```

<br>

---
### 🔗 5. Chain of Thought (CoT)
LLM에 직접 프롬프트를 주는 방식은 빠르게 응답을 받을 수 있고 토큰 사용량 측면에서도효율적이지만,  **환각(hallucination)**현상이 발생할 가능성이 있다.

답변이 언어와 문법 측면에서는 "그럴듯해 보일" 수 있지만, 실제 사실이나 논리적으로는 틀린 경우가 많다. 

Chain-of-Thought(CoT) 프롬프트 기법은 모델에게 **중간 추론 과정을 출력**하라고 지시하는 방식이고, 특히 **few-shot 예제**와 함께 사용할 때 더 나은 결과를 얻을 수 있다.   

하지만 이 방법도 환각을 완전히 제거할 수는 없고, 추론 과정이 길어지면서 **토큰 사용량이 많아져 비용이 더 들 수 있다.**

Gemini 모델과 같은 계열은 기본적으로 chatty하고 thoughtful한 스타일로 답변하도록 학습되어 있어서, 변도로 요청하지 않아도 **reasoning 단계**를 스스로 제공하는 경우가 많다.  

그래서 간단한 예제에서는 **"추론 없이 바로 답해라"**는 식으로 명시해주면 직접적인 답변을 얻을 수 있다.  우선 추론 없이 답하라고 했을 때:
```python
# 추론 없이 답하라고 했을 때
prompt = """When I was 4 years old, my partner was 3 times my age. Now, I
am 20 years old. How old is my partner? Return the answer directly."""

response = client.models.generate_content(
    model='gemini-2.0-flash',
    contents=prompt)

print(response.text)
```
```plaintext
52
```
바로 답을 하라고 했더니 잘못된 답을 출력하는데, 이번에는 추론 과정을 출력하는 CoT 기법을 사용해보면:
```python
# 추론 과정을 출력하라고 지시했을 때
prompt = """When I was 4 years old, my partner was 3 times my age. Now,
I am 20 years old. How old is my partner? Let's think step by step."""

response = client.models.generate_content(
    model='gemini-2.0-flash',
    contents=prompt)

Markdown(response.text)
```
```plaintext
Here's how to solve this problem step-by-step:

Find the age difference: When you were 4, your partner was 3 times your age, meaning they were 4 * 3 = 12 years old.
Calculate the age difference: The age difference between you and your partner is 12 - 4 = 8 years.
Determine the partner's current age: Since your partner is 8 years older than you, they are currently 20 + 8 = 28 years old.
Therefore, your partner is currently 28 years old.
```
이렇게 추론 과정과 함께 제대로 된 답을 출력한다. 짱이군!

<br>

---
### ⚙️ 6. ReAct: Reason and act
여기서는 **ReAct 프롬프트**를 직접 Gemini API 입력해 실행하고, 필요한 검색 과정은 스스로 수행한다.  

이 프롬프트는 **정해진 구조**<small style="color:gray">(well-defined structure)</small>를 따르기 때문에 프롬프트를 감싸서 도구 호출<small style="color:gray">(tool call)</small>을 자동화하는 형태의 프레임워크들도 함께 활용할 수 있다.   

```python
# model instructions
# 질문에 답을 할 때는 생각(thought), 행동(action), 관찰(observation) 단계를 번갈아가면서 수행하세요. 
# '생각'은 현재 상황에 대한 추론입니다
# '관찰'은 '행동'의 결과로 얻은 정보를 해석하는 것입니다.
# '행동'은 세 가지 타입 중 하나입니다
# (1) <search>엔티티</search>: 위키백과에서 해당 엔티티를 검색하고 첫 문단 반환하기. 문단이 없으면 유사한 항목들 제시
# (2) <lookup>키워드</lookup>: 현재 문맥에서 키워드가 포함된 다음 문장을 반환하기. (정확히 일치하는 단어만 찾고 검색어는 짧게 유지하세요)
# (3) <finish>정답</finish>: 정답을 반환하고 작업 종료하기

model_instructions = """
Solve a question answering task with interleaving Thought, Action, Observation steps. Thought can reason about the current situation,
Observation is understanding relevant information from an Action's output and Action can be one of three types:
 (1) <search>entity</search>, which searches the exact entity on Wikipedia and returns the first paragraph if it exists. If not, it
     will return some similar entities to search and you can try to search the information from those topics.
 (2) <lookup>keyword</lookup>, which returns the next sentence containing keyword in the current context. This only does exact matches,
     so keep your searches short.
 (3) <finish>answer</finish>, which returns the answer and finishes the task.
"""

# 질문 >> 생각 >> 행동 >> 관찰 >> 생각 >> 행동 >> 관찰 >> 생각 >> 행동 (반복)
example1 = """Question
Musician and satirist Allie Goertz wrote a song about the "The Simpsons" character Milhouse, who Matt Groening named after who?

Thought 1
The question simplifies to "The Simpsons" character Milhouse is named after who. I only need to search Milhouse and find who it is named after.

Action 1
<search>Milhouse</search>

Observation 1
Milhouse Mussolini Van Houten is a recurring character in the Fox animated television series The Simpsons voiced by Pamela Hayden and created by Matt Groening.

Thought 2
The paragraph does not tell who Milhouse is named after, maybe I can look up "named after".

Action 2
<lookup>named after</lookup>

Observation 2
Milhouse was named after U.S. president Richard Nixon, whose middle name was Milhous.

Thought 3
Milhouse was named after U.S. president Richard Nixon, so the answer is Richard Nixon.

Action 3
<finish>Richard Nixon</finish>
"""

example2 = """Question
What is the elevation range for the area that the eastern sector of the Colorado orogeny extends into?

Thought 1
I need to search Colorado orogeny, find the area that the eastern sector of the Colorado orogeny extends into, then find the elevation range of the area.

Action 1
<search>Colorado orogeny</search>

Observation 1
The Colorado orogeny was an episode of mountain building (an orogeny) in Colorado and surrounding areas.

Thought 2
It does not mention the eastern sector. So I need to look up eastern sector.

Action 2
<lookup>eastern sector</lookup>

Observation 2
The eastern sector extends into the High Plains and is called the Central Plains orogeny.

Thought 3
The eastern sector of Colorado orogeny extends into the High Plains. So I need to search High Plains and find its elevation range.

Action 3
<search>High Plains</search>

Observation 3
High Plains refers to one of two distinct land regions

Thought 4
I need to instead search High Plains (United States).

Action 4
<search>High Plains (United States)</search>

Observation 4
The High Plains are a subregion of the Great Plains. From east to west, the High Plains rise in elevation from around 1,800 to 7,000 ft (550 to 2,130m).

Thought 5
High Plains rise in elevation from around 1,800 to 7,000 ft, so the answer is 1,800 to 7,000 ft.

Action 5
<finish>1,800 to 7,000 ft</finish>
"""
```

**한 번에 한 단계씩 결과를 생성**하고, 환각 현상이 일어나는 Observation 단계를 무시하기 위해서 **stop_sequences**를 사용하여 생성 과정을 원하는 시점에 멈출 수 있다.  

그리고 Observation 단계에 해당하는 답을 사람이 직접 찾아서 수동으로 모델에게 제공하는 건데, 간단한 예제가 아래에 있다.  

Transformer NLP 논문을 작성한 저자들 중 가장 나이가 어린 사람이 누군지 묻는 질문인데, `Observation`이라는 단어를 `stop_sequence`로 설정해서 Observation 단계마다 직접 해당하는 입력을 모델에게 제공한다.  
```python
question = """Question
Who was the youngest author listed on the transformers NLP paper?
"""

# observation 단계 직전까지만 출력하도록 제한
react_config = types.GenerateContentConfig(
    stop_sequences=["\nObservation"],
    # 위에서 작성한 instruction과 예제 두 개 넣기 (CoT + few-shot)
    system_instruction=model_instructions + example1 + example2,
)

# 모델에게 지시와 예제를 미리 입력한 채로 대화 시작하고
react_chat = client.chats.create(
    model='gemini-2.0-flash',
    config=react_config,
)

# 질문을 던지면 모델은 thought와 action까지 출력하고 observation은 생략한다.
# 모델이 어떻게 행동해야 할 지 생각(thought)하고, 사람이 직접 행동(action)하여 환각현상 방지하는 것
resp = react_chat.send_message(question)
print(resp.text)
```
```plaintext
<output>
Thought 1
I need to find the transformers NLP paper, find the list of authors, and then find the youngest author.

Action 1
<search>transformers NLP paper</search>
```
이제 모델이 사람이 어떤 Action을 해야할 지 알려줬으니, transformer NLP paper를 **\<search\>**하면 된다. 

```python
# 위 질문에 대해 수동으로 observation 1을 모델에게 제공하기
# (공동저자가 너무 많아서 대충 4명으로 줄임)
observation = """Observation 1
[1706.03762] Attention Is All You Need
Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit,
We propose a new simple network architecture, the Transformer, based solely on attention mechanisms, dispensing with recurrence and convolutions entirely.
"""
resp = react_chat.send_message(observation)
print(resp.text)
# 모델에게 observation을 제공하면 이걸 보고 이어서 thought 2를 한다. 그리고 action2 를 제안한다
```
```plaintext
<output>
Thought 2
Okay, I have the paper and the list of authors: Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit. I need to find the youngest author. I will start with searching for Ashish Vaswani.

Action 2
<search>Ashish Vaswani</search>
```
그럼 \<output\>에서 보이는 것 처럼, 모델은 Ashish Vaswani를 먼저 **Search**하는 action을 선택한다.  
Ashish Vaswani의 나이를 구글링한 다음에 이어서 observation 2 입력한다.  
```python
observation2 = """Observation 2
Ashish Vaswani was born in 1986.
"""
resp = react_chat.send_message(observation2)
print(resp.text)
```
```plaintext
<output>
Thought 3
Ashish Vaswani was born in 1986. Now I need to check Noam Shazeer.

Action 3
<search>Noam Shazeer</search>
```
그럼 모델은 다음으로 **Noam Shazeer의 나이를 검색**해야 한다고 생각한다. 다시 구글링해서 나이 넣어주기 반복..~

```python
observation3 = """Observation 3
Noam Shazeer was born in 1975.
"""
resp = react_chat.send_message(observation3)
print(resp.text)
```
```plaintext
<output>
Thought 4
Noam Shazeer was born in 1975. Now I need to check Niki Parmar.

Action 4
<search>Niki Parmar</search>
```
이제 모델은 사람이 찾아서 제공해준 정보인 Ashish Vaswani와 Noam Shazeer의 정확한 나이를 알고 있고, 다음으로 **Niki Parmar의 나이**를 검색하려고 한다. 
```python
observation4 = """Observation 4
I couldn't find Niki Parmar's exact birthdate, but I found that she graduated with her bachelor's degree in 2015.
"""
resp = react_chat.send_message(observation4)
print(resp.text)
```
```plaintext
<output>
Thought 5
Niki Parmar graduated with her bachelor's degree in 2015. This would make her likely younger than Ashish Vaswani and Noam Shazeer. Now I need to check Jakob Uszkoreit.

Action 5
<search>Jakob Uszkoreit</search>
```
그런데 내가 Niki Parmar의 나이를 못 찾아서, 어쩔 수 없이 모델에게 학사 졸업 년도만 알려줬다.  
Gemini가 졸업년도 정보를 보고 앞서 찾은 두 명보다 젊을 것이라고 추측을 하고, 마지막으로 **Jakob Uszkoreit의 나이**를 검색하려고 하는 걸 확인할 수 있다.  

이어서 `observation5` 넣어주기.  
```python
observation5 = """Observation 5
Jakob Uszkoreit was born in 1982.
"""
resp = react_chat.send_message(observation5)
print(resp.text)
```
```plaintext
<output>
Thought 6
Jakob Uszkoreit was born in 1982. Comparing the dates, Noam Shazeer (1975), Jakob Uszkoreit (1982), and Ashish Vaswani (1986), Niki Parmar who graduated in 2015 should be the youngest.

Action 6
<finish>Niki Parmar</finish>
```
이제 모델은 내가 첫 번째 observation에서 입력으로 넣어준 4명의 공동 저자들의 나이를 알게 되었고, 그 중에 가장 나이가 어린 사람을 알아냈으니 프롬프트에 지시한 것처럼 **`<finish>`**로 **정답을 반환하고 작업을 종료**한다. 

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 <a href="https://github.com/google-gemini/cookbook/blob/main/examples/Search_Wikipedia_using_ReAct.ipynb" target="_blank">Wikipedia 예제</a>를 실행해보면 <strong>완전히 자동화된 ReAct</strong> 시스템이 어떻게 동작하는 지 확인할 수 있다. 
</div>
<br>

---
### 💭 7. Thinking mode
Gemini Flash 2.0의 **Thinking** 모델은 모델이 응답을 생성하는 과정에서 **사고 과정**<small style="color:gray">(thinking process)</small> 자체를 함께 생성하도록 학습되었다.  

**Thinking mode**를 사용하면 앞에서 살펴본 특수한 프롬프트 기법을 따로 쓰지 않아도 **고품질의 응답**을 받을 수 있다.  

모델이 최종 답변을 생성하기 전에 관련 정보를 스스로 **브레인스토밍 또는 생각하는 듯한** 형태로 생성해 그 결과를 문맥에 반영하기 때문이다.  

하지만 API를 통해 요청할 경우에는 **최종 응답만 반환**되고, 그 과정에서 생성된 **생각**<small style="color:gray">(thoughts)</small>들은 따로 확인할 수 없다.  

중간 사고 과정<small style="color:gray">(intermediate thoughts)</small>을 보고 싶다면, [AI Studio](https://aistudio.google.com/prompts/new_chat?model=gemini-2.0-flash-thinking-exp-01-21)에서 thinking mode model을 실행해야 한다.  

transformer NLP 논문의 저자 중 가장 나이가 어린 사람이 누군지 묻는 질문을 thinking mode를 사용해서 다시 해보면: 
```python
import io
from IPython.display import Markdown, clear_output


response = client.models.generate_content_stream(
    model='gemini-2.0-flash-thinking-exp',
    contents='Who was the youngest author listed on the transformers NLP paper?',
)

buf = io.StringIO()
for chunk in response:
    buf.write(chunk.text)
    # Display the response as it is streamed
    print(chunk.text, end='')

# And then render the finished response as formatted markdown.
clear_output()
Markdown(buf.getvalue())
```
```plaintext
The paper you're referring to is "Attention Is All You Need," which introduced the Transformer model.

Based on publicly available information and common accounts, the youngest author on the paper was Aidan N. Gomez.

At the time the paper was published in 2017, Aidan Gomez was reportedly an undergraduate student (or just finishing his degree) at the University of Toronto. The other authors were more senior researchers at Google Brain. He was in his early 20s when the paper was written/published.
```
내가 말하는 논문은 "Attention Is All You Need"이며, 구체적인 설명과 함께 가장 어린 저자는 Aidan N. Gomez라고 답한다.  

<br>

---
### 💻 8. Code prompting

##### 코드 생성 (Generating Code)
Gemini model family는 코드, 설정 파일, 스크립트 등을 생성하는 데 사용할 수 있다.  

<div style="background: #fff9e7; border-left: 4px solid #fdc82a; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
💡 LLM은 당근 실수할 수 있고, 학습 데이터 일부를 반복적으로 출력할 가능성도 있기 때문에 생성된 코드는 직접 읽고, 테스트하고, 필요한 경우 라이선스 조건도 확인하는 것이 중요하다. 
</div>

```python
# 팩토리얼을 계산하는 파이썬 함수 생성 (설명 없이 코드만)
code_prompt = """
Write a Python function to calculate the factorial of a number. No explanation, provide only the code.
"""

response = client.models.generate_content(
    model='gemini-2.0-flash',
    config=types.GenerateContentConfig(
        temperature=1,
        top_p=1,
        max_output_tokens=1024,
    ),
    contents=code_prompt)

Markdown(response.text)
```
```plaintext
<output>
def factorial(n):
    if n == 0:
        return 1
    else:
        return n * factorial(n-1)
```

<br>

##### 코드 실행 (Code Execution)
Gemini API는 생성한 코드를 자동으로 실행하고, 그 결과를 함께 반환할 수 있다.  
```python
from pprint import pprint

config = types.GenerateContentConfig(
    tools=[types.Tool(code_execution=types.ToolCodeExecution())],
)

code_exec_prompt = """
Generate the first 14 odd prime numbers, then calculate their sum.
"""

response = client.models.generate_content(
    model='gemini-2.0-flash',
    config=config,
    contents=code_exec_prompt)

for part in response.candidates[0].content.parts:
  pprint(part.to_json_dict())
  print("-----")
```
API 응답은 **일반 텍스트 응답**<small style="color:gray">(opening/closing text)</small>과 **생성된 코드**<small style="color:gray">(executable_code)</small>, 그리고 **코드 실행 결과**<small style="color:gray">(code_execution_result)</small> 부분으로 나누어져 있다.  

모델의 응답 결과를 파트별로 나누어 출력할 수 있다.  
```python
for part in response.candidates[0].content.parts:
    if part.text:
        display(Markdown(part.text))
    elif part.executable_code:
        display(Markdown(f'```python\n{part.executable_code.code}\n```'))
    elif part.code_execution_result:
        if part.code_execution_result.outcome != 'OUTCOME_OK':
            display(Markdown(f'## Status {part.code_execution_result.outcome}'))

        display(Markdown(f'```\n{part.code_execution_result.output}\n```'))
```

<br>

##### 코드 설명 (Explaining Code)
Gemini model family는 **코드 설명 기능**도 지원한다.  

예시로 bash 스크립트를 제공하고 해당 코드에 질문하기:
```python
file_contents = !curl https://raw.githubusercontent.com/magicmonty/bash-git-prompt/refs/heads/master/gitprompt.sh

explain_prompt = f"""
Please explain what this file does at a very high level. What is it, and why would I use it?
`` `
{file_contents}
`` `
"""
response = client.models.generate_content(
    model='gemini-2.0-flash',
    contents=explain_prompt)

Markdown(response.text)
```




<br>

---
### 💭 오늘 챙겨간 것들
여기까지 프롬프트 엔지니어링 기법들 실습이다.  

✔️ Zero-shot, One-shot, Few-shot 방식으로 다양한 입력 유형 다루기  
✔️ Enum Mode와 JSON Mode를 활용해 출력 결과를 구조화하고 제어하기  
✔️ Chain of Thought(CoT) 기법으로 추론 과정을 유도해 답변 품질 높이기  
✔️ ReAct 프롬프트로 외부 검색을 통한 복합 문제 해결하기  
✔️ Thinking Mode를 통해 별도 지시 없이 모델의 사고 과정을 활용하기  
✔️ Code Prompting을 통해 코드 생성, 실행, 디버깅 흐름 확인하기  

다음 글에서는 Day 1 - Evaluation and Structured Output 실습을 해보기로!  

<br><br>