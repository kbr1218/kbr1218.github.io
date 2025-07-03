---
layout: post
title:  "[Kaggle Gen AI] Day 4 - 도메인 특화 LLM: 의료에 뛰어들기 🚀"
author: me
categories: [ Kaggle Gen-AI]
date: 2025-07-01 09:00:00
image: assets/images/20250402/day4_1.png
---
[지난 글](https://kbr1218.github.io/kaggle-gen-ai-podcast_30/)에서는 **SecLM**이 사이버보안 분야에 어떤 혁신을 만들어내고 있는지를 살펴봤다.  
이번에는 조금 방향을 바꿔서, **의료 분야**에서 LLM이 어떤 가능성을 보여주고 있는지 정리해보겠다.

> what's really exciting is that these large language models are now able to understand and apply these complex medical concepts in a way that we haven't seen before.

MedLM, 그리고 특히 **Med-PaLM** 시리즈는 의료 AI의 새로운 장을 여는 모델들이라고 한다.  
이 모델들은 방대한 의학 지식을 다루고, 복잡한 질문에 대한 심층적인 답변을 생성하며, 궁극적으로 **의료 서비스의 품질과 효율성을 높이는 것**을 목표로 한다.

<div style="margin-top: 2.3em;"></div>

[👉 팟캐스트 원본 링크 바로가기](https://www.youtube.com/watch?v=MWqspvVvNzA&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=7)

<div style="margin-top: 3.3em;"></div>

---
### 🩺 MedLM: 의료 질문 답변의 혁신
AI로 의료 분야의 질문에 정확하게 답하는 것은 항상 큰 도전 과제였다.  
의학 지식의 양 자체가 방대하고, 항상 업데이트되며, 섬세한 추론을 요구하기 때문이다.

> accurately answering medical questions with AI has always been a huge challenge.

하지만 최근에는,  
- **의료 데이터의 가용성 증가** <small style="color:gray">increasing availability of medical data</small>  
- **의료 NLP(자연어 처리) 기술 발전** <small style="color:gray">advancements in in medical NLP</small>

이 두 가지가 맞물리면서 혁신을 위한 **완벽한 환경** <small style="color:gray">(perfect storm)</small>이 만들어지고 있다.

> it's creating this perfect storm for Innovation.

이런 배경 속에서 등장한 것이 **Med-PaLM** 이고, 이 모델은 Google의 PaLM 계열 모델을 기반으로 한 **의료 특화 LLM**이며, 이 모델의 목표는 **실제 건강을 개선**하는 것이다.  

<br>

---
### 🏥 활용 시나리오: 개인화된 의료 경험
MedLM이 의료 분야에서 활용될 수 있는 방식들은 다양하다.  

- **개인 맞춤형 건강 상담**
  : 환자가 본인의 병력에 대해 질문하고, 맞춤형 조언을 받는다.
- **환자 메시지 분류와 긴급도 판단**
  : 환자의 의료 기록을 바탕으로 메시지를 분류하고 적절한 의료진에게 전달한다.
- **동적 환자 정보 수집**
  : 표준화된 설문지를 넘어서는 포괄적이고 유연한 문진 프로세스를 지원한다.
- **진료 중 실시간 피드백**
  : 상담 과정에서 주요 내용을 요약하고 중요한 포인트를 환자와 의사 모두에게 제공한다.
- **의료진의 의사결정 지원**
  : 생소하거나 드문 사례에서, 최신 의학 지식에 기반한 조언을 제시한다.

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
💬 이 모든 것은 결국 환자 경험을 개인화하는 데 초점이 맞춰져 있다! <small style="color:gray"><i>(it's really about personalizing the patient experience.)</i></small>
</div>
<br>

---
### 👩🏻‍⚕️ 인간 중심 AI로의 전환
기존의 의료 AI 시스템은 **구조화된 출력**에 초점이 맞춰져 있었다.
- 예/아니오 답변
- 수치 데이터 출력

이런 방식도 유용하지만, 실제 의료 현장에서의 상호작용에 필요한 유연성이 부족하다.  
상황의 맥락을 고려해서 사람과 자연스럽게 상호작용할 수 있는 시스템을 개발하는 것이 목적이며,이것은 데이터와 알고리즘의 문제가 아니라 언어와 공감, 인간적인 이해에 관한 문제이기도 하다!

> it's about language, empathy, understanding the human element.

그리고 이 Med-PaLM은 **인간 중심적인 AI**를 만들기 위한 첫 단계로 소개된다.  
의료 질문 답변이라는 단순해 보이는 과제 안에,  
- 추론 능력
- 문맥 이해
- 공감적 언어 생성

이러한 능력들을 가지고 있다.

<br>

---
### 📈 성능과 평가: Med-PaLM의 발전
그렇다면 Med-PaLM의 진행 상황은 어땠을까  

- **Med-PaLM 1**
  : USMLE <small style="color:gray">(의사 국가시험)</small> 스타일 문제에서 최초로 합격 점수를 넘은 AI로,  
  : 첫 번째 버전은 정말정말 큰 사건 <small style="color:gray">(milestone)</small>이라고 할 수 있따.   

- **Med-PaLM 2**
  : 얘는 1보다 더 발전해서, 같은 시험에서 전문가 수준 성취를 달성했고,
  : 장문의 답변 품질과 깊이에서도 큰 진전을 보였다. 

<div style="margin-top: 3.3em;"></div>

AI의 의료지식을 측정하는 방법은?  
여기서는 포괄적인 평가 전략을 제시하고 있다: 바로 **정량적 지표 + 정성적 평가를 결합**하는 방식  

1.  먼저 **USMLE 스타일의 문제를 벤치마크로 사용**한다.  
  - 이 문제를 정확히 풀기 위해서는 의료 개졈에 대한 깊은 이해, 환자 정보 해석, 그리고 임상적 추론이 필요하기 때문이다.  (그냥 사실 암기만으로는 불가능!)  
  - MedPaLM에서 MedPaLM 2로의 성능이 합격 점수 67%에서 86.5%로 올라갔는데, 이는 기본적인 역량 수준에서 **실제 전문가 수준의 지식**으로 발전했다는 것을 의미한다
  - 이 평가는 객관식 문제에만 국한되지 않고 더 광범위하게 접근한다.  

2. **정성적 평가**
  - 정보의 사실성, 적절한 의료 지식 활용, 응답의 유용성, 잠재적 편향성과 위험 가능성을 살핀다.  
  - 단순히 점수만 보는 게 아니라 큰 그림을 본다는 것
  - AI가 안전하고 유익한 정보를 제공하는 지 철저히 확인하는 것이다.  

3. **전문가 비교 평가**
  - 답변이 과학적 합의와 일치하는지, 해로운지를 모두 평가하고
  - 질문을 잘 이해했는지, 정확한 정보를 가져왔는지, 임상적 추론이 타당한지 모두 살펴보는 방식으로 모델의 응답을 검토한다.  
  - MedPaLM과 의료진들이 같은 질문에 대해 독립적으로 작성해서 다른 전문가 평가단이 답변을 보고 심사하는 방식으로 인간 평가를 수행한다.  
  - 문체나 표현 방식을 평가하는 것이 아닌, **답변의 '내용'에 집중**해서 평가하는 것이 핵심이다.  
  
  <div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
  💬 즉, 모델이 질문을 얼마나 본질적으로 이해하고 있는지를 확인하려고 하는 것이다. <small style="color:gray"><i>(so they're really trying to get at the core of the model's understanding.)</i></small>
  </div>

이 과정에서도 중요한 점은, **데이터셋에서 높은 점수를 받았다고 실무 적합성이 자동 보장되는 건 아니라는 사실**이다.  
실제 환자 환경에서의 검증이 필수적이다.

> they're really scrutinizing the model's responses.

그래서 연구는 단계적 접근 방식을 권장하고 있다.  
1. **회귀적 분석** (과거 데이터로 검증) <small style="color:gray">(retrospective analysis)</small>
2. **전향적 관찰 연구** <small style="color:gray">(prospective observational studies)</small>
3. AI의 권고가 실제 환자 치료에 영향을 미치는 전향적 중재 연구  <small style="color:gray">(prospective interventional studies)</small>

이 세가지 단계를 거쳐 기술의 안전성과 유효성을 확보해야 한다.  

<br>

---
### 🔭 MedPaLM의 확장 가능성과 미래 로드맵
그리고 여기서는 **특정 작업에 최적화된 모델**과 MedPaLM처럼 **더 넓은 도메인에 최적화된 모델**의 차이도 말하고 있다.  

MedPaLM은 **도메인 특화 모델**이 얼마나 강력한 성과를 낼 수 있는지를 보여주며,  
특히, MedPaLM 2는 의료 분야에서의 **추론 능력**이 눈에 띄게 향상되었다.  

하지만 그렇다고 해서 모든 의료 업무에서 뛰어나다고 가정할 수는 없다.  
각각의 응용 사례는 **꼼꼼한 검증과 맞춤형 적응 과정**이 필요하다.  

<div style="margin-top: 3.3em;"></div>

또 중요한 점은, 의료 분야에서의 **멀티모달 특성**이다.  
- 영상 데이터
- 전자의무기록 EHR
- 센서 데이터
- 유전체 정보

이러한 다양한 정보를 통합하는 것이 필수적이고,  
현재는 이런 데이터를 모두 다룰 수 있는 **멀티모달 비전의 MedPaLM**을 연구하는 초기 단계에 있따고 했다.  

<div style="margin-top: 3.3em;"></div>

그리고 이런 모델이 단순히 **환자 진료에만 국한되지 않는다**는 점도 강조했다.  
- 과학적 발견에 활용
- 특정 형질과 연관된 유전자를 찾아내는 연구 지원

처럼 다양한 분야로의 응용 가능성이 열려 있다.  

<br>

---
### 🧬 훈련과 기법: 어떻게 학습되었나?
Med-PaLM 2는 기본 LLM인 PaLM 2를 기반으로 구축되었고, 수많은 **의학 질문-응답 데이터**로 파인튜닝되었다.  

여기서도 여러 가지 다른 프롬프트 기법이 사용되었는데,  
예를 들어서, 객관식 문제를 다룰 때는
- **Few-Shot Prompting**
- 모델이 추론 과정을 보여주도록 유도하는 **Chain-of-Thought Prompting**
- 정확성을 높이기 위한 **Self-Consistency**

같은 방법들이 적용된다.  

이렇게 하면 모델이 그냥 추측만 하는 게 아니라 문제를 실제로 **논리적으로 생각**하게 된다.  

또한,  
- **Ensemble Refinement**

이라는 기법도 사용되는데, 이건 모델이 **스스로 생성한 설명을 참고**해서, 최종 답변을 더 정교하게 개선하는 방식이다.  

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
💬 즉, 모델이 <strong>자기 자신에게 배우는 것</strong>이다!
</div>

> it's not just guessing—it's actually thinking through the problem.

<br>

---
### 💭 오늘 챙겨간 것들
여기까지 Day 4에서는 LLM이 **복잡한 도메인 특화 과제**(사이버 보안과 헬스케어 분야)에서 가진 잠재력을 확인했다!  

사이버 보안에서는 **SecLM**은  
- 시간이 많이 걸리던 업무를 자동화하고,  
- 인재 부족 문제를 해결하며,  
- 궁극적으로는 보안 실무 방식을 혁신함으로써  

AI와 인간 전문가의 역량을 결합한 진짜 게임체인저가 될 수 있다.  

<div style="margin-top: 3.3em;"></div>

헬스케어 분야에서는 **MedLM**이
- 점점 복잡해지는 의료 데이터를 다루고
- 지식을 발굴하며,
- 궁극적으로는 의료 서비스의 효과성과 효율성을 개선하는 것을 목표로 하고 있으며

어려운 의료 질문-응답 과제에서 **전문가 수준의 성능**을 보이고 있다.  

<div style="margin-top: 3.3em;"></div>

이렇게 **특정 분야에 특화된 기반 모델** <small style="color:gray">(vertical-specific foundation models)</small> 분야는 빠르게 진화하고 있으며, 엄청난 발전 가능성과 함께 많은 도전과제들도 안고 있다. 

> this is just the beginning—the journey of applying these powerful tools to solve real world problems is just getting started.

kaggle gen-ai intensive course의 마지막 날인 Day 5의 주제는 "MLOps for Generative AI  <small style="color:gray">(생성형 AI를 위한 MLOps)</small>"이다.   
마무리 잘 해보자구

<br><br>