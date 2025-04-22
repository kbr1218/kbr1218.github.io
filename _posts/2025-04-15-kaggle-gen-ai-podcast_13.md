---
layout: post
title:  "[Kaggle Gen AI] Day 1 - 프롬프트 엔지니어링 핵심 기법 총정리 part. 2 🚀"
author: me
categories: [ Kaggle Gen-AI ]
date: 2025-04-15 09:00:00
image: assets/images/20250402/day1_2.png
---
[지난 게시물](https://kbr1218.github.io/kaggle-gen-ai-podcast_12/)에서는 Zero-shot, Few-shot부터 System/Role Prompting, CoT까지 
LLM의 출력을 더 정밀하게 유도할 수 있는 **프롬프트 설계의 핵심 기법들**을 정리했다.

이번에는 그 연장선으로 **고차원적 추론**, **외부 도구와의 연동**, **프롬프트의 자동 생성**, 그리고 **코드 중심의 프롬프트 설계**까지 
LLM을 **더 똑똑하고 실용적으로 활용할 수 있는 전략들**을 다뤄볼 예정.

팟캐스트에서는 Tree of Thoughts, ReAct, APE 등 **프롬프트 엔지니어링의 진화된 형태**들을 소개하면서, 
이런 기법들이 Kaggle처럼 문제 정의가 유동적이고 정답이 없는 환경에서 **특히 강력한 도구**가 될 수 있다고 했다.  

✔️ 문제 해결 경로를 확장하고,  
✔️ 외부 데이터와의 상호작용을 가능하게 하고,  
✔️ 효과적인 프롬프트를 모델이 스스로 생성하게 만드는 것.

또한, **Kaggle에서 자주 쓰이는 코드 관련 프롬프트 설계 팁**과 앞으로 더 주목받을 **멀티모달 프롬프트**까지 함께 정리해보겠다.

[👉 팟캐스트 원본 링크 바로가기](https://www.youtube.com/watch?v=CFtX0ZyLSAY&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=2)

<br>

---
### 🌳 1️⃣ Tree of Thoughts — 브랜칭 방식의 추론
이름에서부터 **복잡하고 분기된 문제 해결 방식**을 암시하는 **Tree of Thoughts (2T)**는 **정답이 명확하지 않은 문제들**에 잘 어울리는 전략이다.  

Tree of Thoughts는 이전에 다룬 **Chain of Thought (CoT)**의 확장 버전이다. 
CoT가 하나의 생각 흐름(추론 경로)를 따라갔다면, **2T는 여러 경로를 동시에 탐색**하면서 그 중 더 나은 쪽으로 가지를 쳐나가는 방식이다.  

모델이 한 가지 방식으로만 답을 구하는 게 아니라, **여러 방향으로 생각을 확장했다가, 필요하면 되돌아가는**<small style="color:gray" >(backtracking)</small> 식으로 접근하는 셈인 거다. 
> it's like the model is branching out, exploring different avenues and backtracking if needed - just like we do when we're tackling a tough problem.

이 방식은 특히  
✔️ 복잡한 추론이 필요한 문제  
✔️ 해답이 명확하지 않고 탐색이 필요한 문제 <small style="color:gray" >(isn't one clear solution path)</small>  
✔️ 창의적인 접근이 필요한  태스크 <small style="color:gray" >(challenging problems)</small>  
에서 유용하다.  

백서에서는 2T와 CoT와의 시각적으로 명확히 보여주고 있는데, 그림을 보면 2T가 얼마나 **더 폭넓은 탐색과 창의적 문제 해결**을 가능하게 하는지 한 눈에 알 수 있따. 

![Tree of Thoughts](/assets/images/20250415/cot_and_2t.png)  
<small style="color:gray" >A visualization of chain of thought prompting on the left versus. Tree of Thoughts prompting on the right</small>

<br>

---
### ⚙️ 2️⃣ ReAct (Reason + Act) — 생각하고, 실행하고, 또 생각하고
이름부터 직관적인 **ReAct**는 **Reason + Act**를 뜻한다.  **추론하고, 행동하고, 그 결과를 바탕으로 다시 추론**하는 방식이다.  

이 기법은 모델의 **추론 능력**<small style="color:gray" >(reasoning capabilities)</small>과  **외부 도구 사용 능력**<small style="color:gray" >(interact with external tools  and data sources)</small>을 결합하여  **능동적인 에이전트처럼 동작할 수 있게 만든다**는 점에서 특히 흥미롭다.  

모델이 실제로 **검색 엔진을 활용하고**<small style="color:gray" >(search engines)</small>, **코드를 실행**<small style="color:gray" >(code interpreters)</small>하거나, **API를 호출**하는 등 실제 작업을 수행할 수 있도록 설계된 구조이다.  

예를 들어, ReAct를 사용하면 모델이 단순히 코드를 출력하는 것을 넘어서  
→ **직접 실행해보고**<small style="color:gray" >(actually execute code)</small>,  
→ **그 결과를 분석하고**<small style="color:gray" >(analyze the results)</small>,  
→ **다음 행동을 결정**<small style="color:gray" >(use that information to guide its next steps)</small>  
하는 방식으로 이어질 수 있다.  

> it's like the LLM is becoming an active participant in your workflow, not just a passive code generator.

백서에는 **LangChain과 Vertex AI**를 활용한 예시가 나오는데, 모델이 먼저 검색를 통해 정보를 찾고, 그걸 바탕으로 다시 추론하고 결정하는 흐름을 보여준다. 

<br>

---
### 🛠️ 3️⃣ APE (Automatic Prompt Engineering) — AI가 스스로 쓰는 프롬프트
APE는 **Automatic Prompt Engineering**의 약자로, 말 그대로 **모델이 스스로 프롬프트를 만들어내고, 그 중 가장 효과적인 것을 고르는 방식**이다.  
> the idea of an AI writing prompts for itself is pretty mind-blowing.

기본 구조는 이렇다:  
1. 모델에게 특정 태스크에 대해 **다양한 프롬프트 버전을 생성하도록 요청**하고<small style="color:gray" >(ask the model to generate various prompt variations for a task)</small>  
2. 생성된 프롬프트들을 실제 **성능 기준으로 평가**<small style="color:gray" >(evaluate those variations based on their performance)</small>  
3. 가장 성능이 좋은 프롬프트를 선택!  

이 방식은 특히  
✔️ 코드 생성 <small style="color:gray" >(code generation)</small>  
✔️ 데이터 분석 <small style="color:gray" >(data analysis)</small>  
같은 작업에서 **프롬프트 실험을 자동화**하는 데 유용하다.  

직접 손으로 실험하면서 프롬프트를 다듬는 대신, 모델이 수많은 조합을 자동으로 생성하고 평가해주기 때문에 **시간도 아끼고**, **사람이 떠올리기 힘든 효과적인 문구를 발견**할 수도 있다.  

<br>

---
### 💻 Code Prompting — 코드 작업에 최적화된 프롬프트 전략들
팟캐스트에선 **코드와 관련한 프롬프트 엔지니어링**도 추가로 설명하고 있다.  
> prompt engineering isn't just for natural language tasks - it's a incredibly useful for anything code releated.

LLM을 코드 생성에 활용하려면 단순히 *'코드 짜줘'* 수준이 아니라, **문제 상황에 따라 적절한 프롬프트 전략을 적용**해야 한다.  

주요 적용 분야는 다음과 같다:  

<div style="margin-top: 3.3em;"></div>

##### 💻 4-1. 코드 생성 (Code Generation)
가장 기본적이면서도 자주 쓰이는 형태이다. 
LLM을 활용하면 코드 작성 속도가 확연히 빨라질 수 있다. 

하지만, **환경을 바꿔버리는 명령어**가 포함될 경우, 예기치 않은 문제가 생길 수 있으므로 **반드시 실행 전에 검토**해야 한다. 

<div style="margin-top: 5.3em;"></div>

##### 📖 4-2. 코드 설명 (Code Explanation)
이번에는 모델에게 **생성한 코드의 기능을 설명**하도록 요청하는 방식을 사용할 수 있다.  

이 방식은 특히  
✔️ 다른 사람의 코드를 이해할 때 <small style="color:gray" >(understanding unfamiliar code)</small>  
✔️ 팀원 간 협업이 필요한 상황 <small style="color:gray" >(collaborating with teammates)</small>  
에서 유용하다.

> it's like having an instant code explainer on hand.

<div style="margin-top: 5.3em;"></div>

##### 🔄 4-3. 코드 변환 (Code Translation)
다른 언어로 작성된 알고리즘을 변환하고 싶을 때 유용하다.  
변환된 코드가 실제로 작동하는지는 **반드시 검증이 필요**하다!

>it's important to double check the translated code and make sure it works as expected

<div style="margin-top: 5.3em;"></div>

##### 🐛 4-4. 코드 디버깅 & 리뷰 (Debugging & Review)
코드를 작성하면서도 모든 사람이 한 번쯤은 겪는 문제인 **에러 잡기**이다.  

단순한 오류 수정에 그치지 않고, **코드를 더 견고하고 효율적으로 개선하는 제안**을 요구할 수 있다.

> even cooler, it can sometimes suggest ways to make your code more robust and efficient.

<div style="margin-top: 3.3em;"></div>

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 "항상 옆에 붙어 있는 코딩 파트너처럼 느껴진다" <small style="color:gray" ><em>("It’s like having a coding buddy who’s always there to help you debug and improve your code.")</em></small>
</div>
<br>

---
### 🖼️ 멀티모달 프롬프트 — 텍스트 너머의 입력까지
다음으로 간단히 언급된 주제는 **Multimodal Prompting**이다.  
텍스트 외에도 **이미지나 오디오 같은 다양한 입력 모달리티**를 포함하는 프롬프트 기법이다.  

이건 기존의 프롬프트 엔지니어링과는 **완전히 다른 차원의 영역**이며 아직은 활발히 발전 중인 분야이다.  
> that's a whole other level of prompt engineering - and it's still an evolving area.

<br>

---
### 📌 실전에 바로 쓰는 Best Practices — 프롬프트 설계 팁 모음
지금까지 다양한 프롬프트 기법을 배웠다면, 이제는 **어떻게 하면 이 기술들을 더 잘 활용할 수 있을지**에 대한 실전 팁이다.  

<div style="margin-top: 3.3em;"></div>

##### ✅ 6-1. 예시를 제공하자 (Provide Examples)
**너무 단순해서 놓치기 쉬운 원칙**이다.  
One-shot / Few-shot Prompting처럼, 모델에게 ***"이런 식으로 해줘"***라는 예시를 보여주는 것만으로도 결과의 질이 크게 달라진다. 

<div style="margin-top: 5.3em;"></div>

##### ✅ 6-2. 심플하게 설계하자 (Design with Simplicity)
프롬프트는 **간결하고 명확하게**, 모델이 이해하기 쉬우면서 사람도 이해하기 쉽게 작성해야 한다.  
→ 특히 코드 작성 요청 시에는 **쓸데없는 수식어, 추상적인 표현은 피하고** <small style="color:gray" >(avoid jargon)</small>  
→ **명확한 지시 문장과 강한 동사**<small style="color:gray" >(action verbs)</small>로 전달할 것!  

그리고 **원하는 출력 포맷**이 있다면 꼭 명시해줘야 한다.  

<div style="margin-top: 5.3em;"></div>

##### ✅ 6-3. 제약보다 ‘지시문’을 사용하자 (Use Instructions over Constraints)
모델에게 **금지 조건을 나열**하는 대신, **원하는 동작을 명확하게 지시**하는 것이 더 효과적이다.  

**부정적 제약 보다는 긍정적 지시문**을 사용하자. 
> focusing on positive instructions can often be more effective than relying on constraints

<div style="margin-top: 5.3em;"></div>

##### ✅ 6-4. 현실적인 제약도 고려하자 (Practicalities Matter)
특히 자원이 한정적인 환경에서는  
- **토큰 길이 제한** <small style="color:gray" >(control the max token length)</small>  
- **처리 시간 제어** <small style="color:gray" >(manage processing time)</small>  
이런 실용적인 요소들도 중요하다.  

또한, 프롬프트를 계속 재사용하고 확장할 수 있도록 **변수를 활용한 동적 프롬프트 구성**도 추천한다.  

<div style="margin-top: 5.3em;"></div>

##### ✅ 6-5. 필수적인 실험 (Experiment Relentlessly)
마지막으로, **프롬프트는 써봐야 안다.**  
> experiment with input formats and writing styles - try phrasing your prompt in different ways: as questions, statement, instructions, ...

✔️ 표현 방식 바꾸기  
✔️ 입력 포맷 다양화  
✔️ 분류 문제에선 **예시 순서(클래스 순서) 바꾸기**로 편향을 줄이는 것도 중요한 팁이다.

<div style="margin-top: 3.3em;"></div>
<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 프롬프트 엔지니어링은 <strong>정형화된 정답이 없는 영역</strong>인 만큼, Best Practice를 바탕으로 <strong>계속 실험하고, 기록하고, 개선해나가는 것</strong>이 중요하다!

</div>
<br>

---
### 🧭 프롬프트 엔지니어링, 실전에서 더 잘 쓰는 방법들
지금까지 프롬프트 엔지니어링의 다양한 기법과 베스트 프랙티스를 살펴봤다면, 이번엔 **프롬프트를 실제 프로젝트에 통합하고 운영할 때** 챙겨야 할 팁들에 대한 내용이다.  

<div style="margin-top: 3.3em;"></div>

##### 🔄 7-1. 모델 변화에 민감해지기
AI는 정말 빠르게 진화하고 있고, **새로운 모델과 기능들이 계속해서 릴리즈**되고 있다.  
> new models and features are constantly being released and staying ahead of the  curve can give you a real edge.  

새로운 모델이 나오면 **실제로 실험해보고,**, 내 프로젝트에 **어떤 식으로 활용될 수 있을지 테스트**하는 게 중요하다.  

<div style="margin-top: 5.3em;"></div>

##### 🗂️ 7-2. 출력 포맷에도 실험이 필요하다
CSV나 JSON 같은 **구조화된 출력 포맷**이 필요할 때는 이런 포맷을 제대로 출력하도록 프롬프트를 설계해야 후속 처리가 훨씬 수월해진다. 
> using a structured format can make your life so much easier.

만약 LLM이 **JSON 포맷을 정확히 못 맞추는 경우**에는 JSON 도구 <small style="color:gray">(e.g. JSON repair 라이브러리)</small>도 활용이 가능하다.  
그리고 더 정교하게 설계하고 싶다면, **JSON 스키마**로 데이터 구조를 명시할 수 있다. 

<div style="margin-top: 3.3em;"></div>

##### 🤝 7-3. 협업 == 가속도
Kaggle은 팀으로 진행하는 대회도 많고, **다른 프롬프트 엔지니어들과 아이디어를 나누는 것**만으로도 큰 시너지가 생긴다.  
> bouncing ideas off each other and sharing successful prompt strategies can really accelerate your learning and lead to breakthroughs.

<div style="margin-top: 5.3em;"></div>

##### 📝 7-4. 모든 프롬프트 시도를 ‘문서화’하기
**프롬프트 실험을 꼼꼼히 기록**해두는 건 성공 전략을 찾는 데도, 실패 원인을 분석하는 데도 핵심이다. 
> keep detailed records of everything you try, the results you get, and your feedback on each prompt.  

Vertex AI Studio 같은 플랫폼을 사용할 경우 **프롬프트를 저장하고 링크해두면** 나중에 다시 돌아보기도 훨씬 쉬워진다.

<div style="margin-top: 5.3em;"></div>

##### 🧪 7-5. 코드에 통합할 때는 관리 전략이 중요
프롬프트를 코드 안에 넣어서 사용하는 경우에는 테스트, 유지보수, 재사용성을 고려한 관리 전략이 꼭 필요하다.  

결국 코드처럼 프롬프트도 **끊임없이 실험하고 개선해나가는 과정**이다!
> prompt engineering is an iterative process - it's a journey, not a destination.

<br>

---
### 💭오늘 챙겨간 것들
여기까지 Unit 1. Prompt Engineering에서는 프롬프트 엔지니어링의 거의 모든 **핵심 기법**들과, 실전에서 **어떻게 활용할 수 있을지**를 중심으로 정리했다.  

✔️ **Zero-shot / One-shot / Few-shot** 같은 기본부터,  
✔️ **System / Role / Contextual Prompting**을 통한 문맥 설계,  
✔️ **Step-back Prompting, Chain of Thought, Self-consistency, Tree of Thoughts, ReAct, APE** 등 고급 기법,  
✔️ 그리고 **코드 생성/설명/번역/디버깅**까지 아우르는 Code Prompting  
✔️ 마지막으로, **실전에 유용한 Best Practices와 운영 전략들**까지!  

프롬프트가 엉성하면 아무리 좋은 모델도 성능이 안 나올 수 있지만, **기술과 기법을 제대로 이해하고 적용**하면 LLM을 훨씬 더 강력하게 사용할 수 있다. 
> poorly crafted prompts can hold you back — but mastering these techniques and best practices can give you a real advantage in the competitive world <small>(of Kaggle)</small>  

여기까지가 Unit 1이었고, Unit 2에서는 LLM을 **실제 태스크에 적용하고 파인튜닝하는 방법**들을 이어서 다뤄볼 예정입니다요.

<br><br>
