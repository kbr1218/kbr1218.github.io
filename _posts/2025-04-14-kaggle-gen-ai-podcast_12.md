---
layout: post
title:  "[Kaggle Gen AI] Day 1 - 프롬프트 엔지니어링 핵심 기법 총정리 part. 1 🚀"
author: me
categories: [ Kaggle Gen-AI ]
date: 2025-04-14 09:00:00
image: assets/images/20250402/day1_2.png
---
[지난 게시물](https://kbr1218.github.io/kaggle-gen-ai-podcast_11/)에서는 LLM이 어떤 출력을 하게 만들지를 제어하는 방법인 
 **출력 길이, 샘플링 방식 (temperature, top-k, top-p)** 같은 **출력 제어 기법들**에 대해 살펴봤다.

이번 글부터는 프롬프트 엔지니어링의 **진짜 핵심 기법**들을 정리해 볼 예정!  

팟캐스트에서는 이 부분을  
> “Let's dive into the heart of prompt engineering — the actual techniques themselves.” 

라고 표현하면서, **좋은 프롬프트가 좋은 예측을 만든다**는 것을 강조했다.  
단순한 지시가 아닌, **구체적인 전략과 예시**가 LLM의 출력을 더 정밀하게 이끌어낼 수 있다는 것!  

그래서 이번 글에서는  
✔️ Zero-shot / One-shot / Few-shot  
✔️ System, Role, Contextual Prompting  
✔️ Step-back Prompting  
✔️ Chain of Thought & Self-consistency  
같은 **핵심 프롬프트 설계 기법들**을 중심으로 정리해보겠다.  

[👉 팟캐스트 원본 링크 바로가기](https://www.youtube.com/watch?v=CFtX0ZyLSAY&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=2)

<br>

---
### 🧩 1️⃣ General Prompting (Zero-shot Prompting)
프롬프트 엔지니어링의 가장 기본적인 형태는 **General Prompting (Zero-shot Prompting)**이다.  
이 방식은 **예시 없이 단순히 작업 설명과 입력만 제공**해서 모델이 바로 작업을 수행하게 만드는 구조이다.  

예를 들어, Kaggle 대회에서 데이터를 전처리하는 코드를 만들고 싶을 때, 단순히 *"이 데이터를 이렇게 변환해줘"*라고 설명하면, 모델은 **학습 과정에서 익힌 지식을 바탕**으로 적절한 코드를 생성해줄 수 있다.  

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 모델이 방대한 양의 코드를 학습했기 때문에, 사용자의 요청을 이해할 가능성이 꽤 높다. <small style="color:gray" >(the model has seen a massive amount of code so it should have a decent shot at understanding your request)</small>
</div>

팟캐스트에서는 **프롬프트를 문서화하는 습관의 중요성** <small style="color:gray" >(documenting your prompts)</small>도 강조했다.  

Kaggle처럼 **계속 실험하고 개선해야 하는 환경**에서는,  
✔️ 어떤 프롬프트가 잘 작동했는지  
✔️ 어떤 실패가 있었고, 그 이유는 무엇인지  
이런 내용을 구조화된 형태로 기록해두는 것이 나중에 큰 도움이 될 것임!

> so a structured approach to documenting prompts is key.

<div style="margin-top: 3.3em;"></div>

하지만 Zero-shot Prompting만으로 부족한 상황도 생긴다. **출력 형식을 더 정밀하게 통제**하거나, 모델이 더 **복잡한 작업을 정확히 이해**하길 바란다면?

<br>

---
### 🧩 2️⃣ One-shot & Few-shot Prompting
Zero-shot만으로 원하는 결과가 나오지 않을 때, 더 구체적으로 **모델의 출력을 유도**하고 싶을 때는 **One-shot** 또는 **Few-shot Prompting**을 사용하면 된다.  

이 방식은 프롬프트에 **입력-출력 예시를 함께 넣어주는 것**이다. 
모델이 예시를 보고 작업의 의도와 출력 형식을 학습하게 되는 원리이다. 

<div style="margin-top: 2.3em;"></div>

예를 들어, Kaggle에서 예측 결과를 특정 **JSON 포맷**으로 출력해야 할 때, 프롬프트 안에 **입력 데이터와 기대하는 JSON 출력 예시**를 몇 개 보여주면
모델이 그 형식에 맞춰 결과를 생성한다. 

<div style="margin-top: 2.3em;"></div>

이 방식은 특히  
✔️ 정보 추출 <small style="color:gray" >(extracting specific information from data)</small>  
✔️ 예측 결과 포맷 맞추기 <small style="color:gray" >(formatting your model predictions)</small>    
같은 태스크에서 강력한 효과를 보인다.  

<div style="margin-top: 2.3em;"></div>
<div style="background: #fff9e7; border-left: 4px solid #fdc82a; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
  ⚠️ <strong>주의사항</strong><br>
  예시의 품질과 정확성이 모델의 응답 품질을 결정짓는다. 작은 오류 하나만 있어도 모델이 엉뚱한 출력을 하게 될 수 있다. <em>so<strong> Garbage In, Garbage Out</strong> - as they say.</em>
</div>
<div style="margin-top: 2.3em;"></div>

또한, **경계값**<small style="color:gray" >(edge cases)</small>  예시를 넣는 것도 중요하다.  
**다양한 입력 상황이 발생하는 환경**에서는 모델이 예상치 못한 상황에서도 잘 작동하려면 예시 안에 그런 상황들을 **미리 포함해두는 게 중요**하다. 

<br>

---
### 🧩 3️⃣ System / Role / Contextual Prompting
앞에서 다룬 Zero-shot, One-shot, Few-shot 방식이 **"어떤 작업을 할 건지"**를 모델에 알려주는 방식이었다면,   
이번에는 모델에게 **어떤 태도와 환경에서 그 작업을 수행해야 하는지**를 전달하는 방식이다.  

> these sound like different way to prvide more highly-guided prompting

<div style="margin-top: 3.3em;"></div>

##### 🛠️ System Prompting
System Prompt는 모델에게 **전체적인 역할과 목적**<small style="color:gray" >(overall context and purpose)</small>을 알려주는 프롬프트다.  

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 일종의 <strong>작업 지침서</strong>나 <strong>상황 설명서</strong>라고 할 수 있다. <small style="color:gray" >(it's like defining the big picture for the model.)</small>
</div>
<div style="margin-top: 1.3em;"></div>

예를 들어, *"너는 데이터 사이언티스트를 돕는 유능한 코딩 어시스턴트야"* 라는 식의 역할을 부여하거나, 예측 결과를 반드시 `{ "label": "positive", "score": 0.98 }`와 같은 **특정 JSON 포맷**으로 출력하라고 명시할 수 있다.  

<div style="margin-top: 1.3em;"></div>

제출 포맷이 정해져 있는 경우에는 **System Prompt**를 통해 **출력 형식을 명확히 지정하는 것**이 실수를 줄이는 데 효과적이다.  

<div style="margin-top: 3.3em;"></div>

##### 👤 Role Prompting
Role Prompting은 모델에게 **특정한 역할이나 페르소나**<small style="color:gray" >(a specific Persona or identity)</small>를 부여하는 방식이다. 
이를 통해 모델의 **톤, 문제, 응답 방식**을 조정할 수 있다. 

<div style="margin-top: 1.3em;"></div>

예를 들어, 문서화 작업을 하면서 모델에게 *"시니어 소프트웨어 엔지니어처럼 설명해줘"* 혹은 *"기술 블로그 작가처럼 설명해줘"* 라고 하면 그에 맞는 스타일로 결과가 생성된다.  

> it's about giving the model a clear understanding of the perspective you need it to take. 

<div style="margin-top: 3.3em;"></div>

##### 📚 Contextual Prompting
Contextual Prompt는 모델에게 **현재 작업과 관련된 구체적인 배경 정보를 제공**하는 방식이다.  
질문을 할 때도, 단순히 *내 코드가 동작하지 않아* 라고 묻는 게 아니라 **실제 코드, 에러 메시지, 사용 중인 라이브러리 등 구체적인 맥락**을 함께 제공해야 한다.  

> the more context you provide, the more relevant and helpful the model's response is likely to be.

특히 **복잡한 디버깅이나 분석 상황**에서 이런 맥락 정보가 응답의 질을 크게 좌우한다. 

<div style="margin-top: 2.3em;"></div>
<div style="background: #fff9e7; border-left: 4px solid #fdc82a; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
💡 <strong>프롬프트에 담는 건 단순한 질문이 아니라 '상황 전체'다.</strong><br> 작업 목적, 페르소나, 배경 정보까지 풍부하게 줄수록 모델의 답변도 더 똑똑해진다! </div>
<br>

---
### 🧩 4️⃣ Step-back Prompting
Step-back Prompting은 이름 그대로, 모델에게 **직접적인 요청을 하기 전에 한 걸음 물러나서** 먼저 관련된 **더 넓은 질문**을 던지는 방식이다. 

> it's like priming the pump.

모델의 지식 기반을 먼저 활성화시켜서 **더 깊고 창의적인 응답을 이끌어내는 테크닉**이다. 

<div style="margin-top: 2.3em;"></div>

예를 들어, Kaggle 대회에서 **새로운 feature를 만들 고 싶을 때**, 바로 *"이 데이터에서 어떤 feature를 만들까?"* 라고 묻는 대신, 먼저 이렇게 이렇게 물어보는 것이다. 
> "일반적으로 효과적인 feature engineering 원칙에는 어떤 것들이 있지? "

이런 식으로 보다 **일반적인 개념을 먼저 끌어낸 뒤**, 그 다음에 **구체적인 데이터셋을 활용한 아이디어**를 물어보면, 훨씬 더 통찰력 있는 결과를 얻을 수 있다.  
    - it can lead to more insightful and creative outputs and the paper even suggests it can help mitigate biases in the lm's responses

이 기법은 특히  
✔️ 창의적인 발상 <small style="color:gray" >(creative outputs)</small>,  
✔️ 문제 해결의 새로운 접근 <small style="color:gray" >(insightful outputs)</small>,  
✔️ 모델의 편향 완화 <small style="color:gray" >(help mitigate biases)</small>  
에 효과적이라고 소개된다.  

<br>

---
### 🧩 5️⃣ Chain of Thought Prompting & Self-consistency
이 게시물의 마지막으로, 프롬프트 엔지니어링 분야에서 가장 주목받는 기술 중 하나인 **Chain of Thought Prompting (CoT)**이다.  
CoT는 특히 **여러 단계를 거치는 추론 문제** <small style="color:gray" >(multi-step reasoning problems)</small>에서 강력한 효과를 발휘한다. 

<div style="margin-top: 3.3em;"></div>

##### 🧠 Chain of Thought Prompting (CoT)
CoT의 핵심은 **최종 답을 바로 요청하지 않고, 먼저 중간 추론 과정을 글로 풀어내도록 유도하는 것**이다. 
> CoT is a powerful technique for boosting the model's reasoning capabilities.

예를 들어, 어떤 문제의 해결 방법이 궁금할 때, 단순히 결과만 받는 대신 **그 결과에 도달하기까지의 생각 흐름**을 함께 받아볼 수 있다. 
이건 LLM의 판단을 신뢰하고 활용하는 데 큰 도움이 된다. 

<div style="margin-top: 1.3em;"></div>

CoT가 가진 장점도 명확하다:  
✔️ 구현이 간단하다 <small style="color:gray" >(easy to implement)</small>  
✔️ 대부분의 LLM에서 잘 작동한다 <small style="color:gray" >(works well with readily available LLMs)</small>  
✔️ 추론 과정을 투명하게 볼 수 있다 <small style="color:gray" >(makes the reasoning process transparent)</small>  
✔️ 버전이 다른 모델에서도 안정적인 성능을 보인다 <small style="color:gray" >(seems to improve the robustness of the reasoning across different LLM versions)</small>  

<div style="margin-top: 1.3em;"></div>

하지만 당근 trade-off도 있다.  
중간 단계까지 출력하니 **출력 토큰 수가 늘어나고**, 따라서 **비용이나 처리 시간**이 더 들 수 있다. 
특히 **자원이 제한된 환경**에서는 주의가 필요하다.  

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 <strong>예시: </strong><br>
    수학 문제를 단순하게 묻자 잘못된 답이 나왔지만, "단계별로 생각해보라"고 유도하자 정답이 나왔다. 
</div>
<div style="margin-top: 2.3em;"></div>

또한, CoT에 **하나의 예시**만 추가해도 성능이 더 올라간다고 한다. ⇒ 이걸 **Single-shot CoT**라고 한다.  
> combining CoT with a single example can futher improve results - they call that **Single-shot CoT**.

<div style="margin-top: 2.3em;"></div>

이처럼 CoT는  
✔️ 복잡한 코드 디버깅 <small style="color:gray" >(debugging intricate code)</small>  
✔️ 모델 성능 병목의 원인 파악 <small style="color:gray" >(figuring out why my model's performance is stuck)</small>  
✔️ 코드 생성 <small style="color:gray" >(figuring out why my model's performance is stuck)</small>  
같은 작업에 특히 유용하고,  

✔️ 코드 생성 <small style="color:gray" >(code generation)</small>   
✔️ synthetic data 제작 <small style="color:gray" >(creating synthetic data)</small>  
에도 적용 가능하다고 한다.

<div style="margin-top: 5.3em;"></div>

##### 🔁 Self-consistency: 더 신뢰할 수 있는 답을 위한 한 수
Self-consistency는 CoT의 확장 기법이다.  
같은 질문에 대해 **여러 번 다른 reasoning path**를 생성하게 한 뒤, 그 중 **가장 많이 등장하는 답변을 선택**하는 방식이다.  

> it's like getting multiple expert opinions from the model and then going with a consensus.

이렇게 하면 **우연히 잘못된 논리 흐름을 따르는 결과를 줄이고**, 전체적인 **정확성과 신뢰도**를 높일 수 있다.  

<div style="background: #fff9e7; border-left: 4px solid #fdc82a; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
💡 당근 <strong>계산량이 늘어나지만</strong>, 중요한 작업이라면 고려해볼 만한 전략이다. 
    <small style="color:gray" >(it can be a bit more computationally intensive, but for high-stakes tasks, it can be a worthwhile trade-off.)</small> </div>
<br>



---
### 💭오늘 챙겨간 것들
이번 글에서는 프롬프트를 **어떻게 구성하느냐에 따라 모델의 출력이 얼마나 달라질 수 있는지**를 다양한 기법들을 통해 살펴봤다.  
✔️ **Zero-shot / One-shot / Few-shot Prompting**: 예시 없이 또는 예시 몇 개만으로 모델을 유도하는 방식. 예시의 품질과 다양성이 결과에 큰 영향을 준다.  
✔️ **System / Role / Contextual Prompting**: 모델에게 역할, 톤, 작업 맥락 등을 명확히 전달해 더 정확하고 일관된 응답을 유도할 수 있다.  
✔️ **Step-back Prompting**: 직접적인 질문 전에 넓은 질문을 던져 더 창의적이고 편향을 줄인 응답을 이끌어내는 방식.  
✔️ **Chain of Thought + Self-consistency**: 모델의 추론 과정을 명시적으로 설계하고, 여러 경로로 생각하게 해 신뢰도 높은 결과를 얻는 기법.

<div style="margin-top: 2.3em;"></div>

다음 글에서는 **Tree of Thoughts, ReAct, APE** 등 LLM을 더 똑똑하고 실용적으로 쓰는 고급 전략들을 이어서 정리할 예정!

<br><br>

