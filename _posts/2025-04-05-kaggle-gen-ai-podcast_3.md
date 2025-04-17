---
layout: post
title:  "[Kaggle Gen AI] Day 1 - Transformer 이후, LLM 진화 타임라인 🚀"
author: me
categories: [ Kaggle Gen-AI ]
date: 2025-04-05 09:00:00
image: assets/images/20250402/day1.png
---
[지난 게시물](https://kbr1218.github.io/kaggle-gen-ai-podcast_2/)에서는 요즘 모델들이 왜 Decoder-Only 구조를 주로 채택하는지에 대해 살펴봤다.

이번 글에서는, 첫 번째 Transformer 논문 발표 이후 LLM들이 **어떻게 발전해 왔는지,** 
GPT-1부터 최신 모델인 Gemini, LLaMA까지의 **LLM 진화 타임라인**을 따라가며 그 흐름을 정리해보겠다!  

**어떤 모델이 전환점을 만들었는지**, 그리고 지금 우리가 쓰는 초거대 AI들이 어떤 배경에서 나왔는지 알 수 있을 것이다.

[👉 팟캐스트 원본 링크 바로가기](https://www.youtube.com/watch?v=Na3O4Pkbp-U&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=1)

<br>

---
### 1️⃣ GPT-1 (2018) — Decoder-Only & Unsupervised Pretraining의 시작점
2018년, OpenAI는 **GPT-1 (Generative Pre-trained Transformer)**이라는 모델을 공개했다. 
> GPT1 from OpenAI in 2018 was a real turning point.

<div style="margin-top: 1.3em;"></div>

##### 📌 주요 특징
- **Decoder-Only 구조**
  - Transformer 논문에서는 Encoder-Decoder 구조가 기본이었지만, GPT-1은 **Decoder 블록만 쌓은 구조**를 선택했다.  

<div style="margin-top: 1.3em;"></div>

- **Unsupervised Pretraining + Supervised Fine-tuning**
  - 대규모 책 데이터셋 BooksCorpus로 **지도 학습 없이**<small style="color:gray">(unsupervised)</small> 먼저 언어 패턴을 학습했다.
  - 이후, 특정 태스크에 대해 **소량의 레이블된 데이터로 파인튜닝**<small style="color:gray">(fine-tuning)</small>을 수행하는 방식.
    ➡ 지도 학습 없이, 방대한 텍스트만으로 **언어적 일반 패턴**<small style="color:gray">(language pattern)</small>을 효과적으로 학습할 수 있다는 걸 보여줬다.

<div style="background: #fff9e7; border-left: 4px solid #fdc82a; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
  ⚠️ GPT-1의 한계: 긴 문맥 유지에 어려움이 있고, 같은 문장을 반복하거나 점점 비자연스러운 텍스트를 생성하는 경우가 있다.  
</div>
<br>

---
### 2️⃣ BERT (2018) — Encoder-Only, 텍스트 이해에 최적화된 구조
같은 해, Google은 GPT-1과는 완전히 다른 접근법을 택한 모델은 **BERT (Bidirectional Encoder Representations from Transformers)**를 발표했다.  
GPT가 문장을 **생성**<small style="color:gray">(generation)</small>하는 데 초점을 맞췄다면, BERT는 문장을 **이해**<small style="color:gray">(understanding)</small>하는 데 집중한 것!

![Google-BERT](https://www.vizion.com/wp-content/smush-webp/2019/10/google-bert-update.png.webp)

##### 📌 주요 특징
- **Encoder-Only 구조**
  - Transformer의 Encoder 블록만을 활용하여, 입력 문장을 **양방향**<small style="color:gray">(Bidirectional)</small>으로 처리할 수 있다.
  - 즉, 단어의 왼쪽과 오른쪽 문맥을 모두 고려해, 의미를 더 정확히 파악한다.

<div style="margin-top: 1.3em;"></div>

- **Pretraining Task: MLM + NSP**
  - Masked Language Modeling (MLM): 문장 중 일부 단어를 [MASK]로 가리고, 그 단어를 예측하게 함
  - Next Sentence Prediction (NSP): 두 문장을 입력받고, **“두 문장이 실제로 이어지는 문장인가?”**를 판단하게 함

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
   💬 BERT의 NSP는 <strong>"다음 문장을 생성"하는 게 아니라, "두 문장이 연결된 의미인지 이해"하는 분류 문제</strong>다.
</div>

- **GPT-1과 비교하면**
  - GPT는 한 방향(좌→우)으로만 문장을 생성하지만,
  - BERT는 양방향 문맥을 동시에 고려하여 이해에 훨씬 강하다.  
  - GPT-1은 말을 하긴 했지만 맥락이 자주 어긋났고, BERT는 문맥 이해는 잘 했지만, 대화를 이어갈 수는 없었다.

<br>

---
### 3️⃣ GPT-2 (2019) — 스케일의 힘 & Zero-shot Learning의 등장
2019년, OpenAI는 GPT-1을 대규모로 확장한 GPT-2를 발표했다.  
이 모델은 단순한 크기 증가를 넘어서, **모델의 일반화 능력**까지 한 단계 끌어올린 전환점이 되었따.

<div style="margin-top: 1.3em;"></div>

##### 📌 주요 특징
- **더 큰 모델, 더 많은 데이터**
  - Reddit 기반의 **WebText**라는 대규모 데이터셋 사용
  - GPT-1보다 훨씬 더 많은 파라미터 수로 모델 규모 대폭 증가

<div style="margin-top: 1.3em;"></div>

- **더 나아진 언어 생성 능력**
  - 문장 간 연결이 더 자연스러워졌고, 긴 문맥에서도 일관성 있는 응답을 할 수 있게 됨!

<div style="margin-top: 1.3em;"></div>

- **Zero-shot Learning**
  - 별도의 학습 없이도, **프롬프트에 태스크 예시만 주면 알아서 수행**한다.
  - "*"예를 들어 이런 식으로 해줘"*만 알려주면, 그 태스크의 패턴을 파악하고 답변 가능!
  - 이건 나중에 GPT-3로 이어지는 핵심 기술의 기반이 된다.


<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;"> 💬 GPT-2는 단순한 크기 확장을 넘어서,
  <strong>"모델이 태스크를 스스로 유추하는" Zero-shot 시대</strong>의 문을 열었다. LLM의 활용 가능성을 대폭 넓힌, <strong>기술적 도약</strong>이었다!
  <small style="color:gray"><em>(it was quite a leap!)</em></small>

</div>

<br>

---
### 4️⃣ GPT-3 & GPT-4 - 대규모 파라미터와 멀티모달
OpenAI는 2020년부터 GPT 시리즈를 본격적인 **'초거대 모델'** 시대로 끌어올리기 시작했다.  
GPT-3부터 GPT-4에 이르기까지, LLM은 단순히 커지는 것을 넘어 **새로운 방식의 학습과 입력 이해 능력**을 갖추게 된다.  

![from GPT-1 to GPT-4](https://www.dataquest.io/wp-content/uploads/2024/02/What-are-GPT-Models-and-How-Will-They-Be-Used-for-AI-Chatbots-in-2024_-1024x548.png.webp)

##### 🧠 GPT-3 (2020) — Few-shot 학습과 Instruction Tuning의 시작
- GPT-3는 무려 1750억 개의 파라미터를 가진 초대형 모델로 등장했다.
  GPT-2의 구조를 기반으로 하지만, 규모를 압도적으로 키움으로써 모델의 언어 능력에 큰 도약을 가져왔다.

- GPT-3는 특히 소량의 예시만으로도 학습할 수 있는 **Few-shot learning** 능력이 크게 향상되었다.

- 자연어로 작성된 명령을 더 잘 따르도록 별도 학습<small style="color:gray">(instruction tuning)</small>된 버전인 **InstrucGPT** 모델도 나오고, **코드 이해 및 생성**에 매우 뛰어난 GPT-3.5도 나왔다

<div style="margin-top: 3.3em;"></div>

##### 🧩 GPT-4 (2023) — 멀티모달 & 초장기 문맥 처리의 등장
GPT-4는 이전 모델들과 비교해 완전한 **Game Changer**였다.
- 진정한 **멀티모달**<small style="color:gray">(Multimodal)</small> 모델로, 이미지와 텍스트를 함께 입력받아 이해하는 능력이 있었다.
- **컨텍스트 윈도우**<small style="color:gray">(Context Window)</small> 크기도 폭발적으로 증가해서, 한 번에 수만~수백만 토큰의 텍스트를 넣어도 문맥을 놓치지 않고 이해할 수 있는 구조가 되었다.

<br>

---
### 5️⃣ LaMDA (2021) — 대화에 특화된 자연스러운 AI
2021년, Google은 **LaMDA (Language Model for Dialogue Applications)**라는 대화 특화 언어 모델을 공개하며 **대화형 AI 영역에 집중된 접근법**을 보여주었다.
LaMDA는 대화형 인공지능<small style="color:gray">(Conversational AI)</small>의 가능성을 전면에 내세운 모델이었다.

![Google LaMDA](https://opendatascience.com/wp-content/uploads/2022/07/google_lamda_logo_1.png)

##### 📌 주요 특징
- **대화에 최적화된 설계**
  - 처음부터 자연스럽고 유창한 대화를 위해 설계된 **대화 특화 모델.**
  - GPT 시리즈가 점점 범용<small style="color:gray">(general-purpose)</small> 모델로 발전해갔지만, LaMDA는 오직 ‘대화’에 집중하였고, 그 성능이 잘 드러났다!

<br>

---
### 6️⃣ GLaM (2021) — Mixture of Experts로 더 가볍고 빠르게
2021년, Google은 **GLaM (Generalist Language Model)**을 공개했다.  
이 모델은 **Mixture of Experts (MoE)** 방식을 적용하여, 초거대 모델임에도 불구하고 더 효율적인 연산을 가능하게 했다.

<div style="margin-top: 1.3em;"></div>

##### 📌 주요 특징
- **MoE(Mixture of Experts) 구조 활용**
  - 전문가 네트워크를 선택적으로 활성화해, 큰 모델임에도 속도와 효율을 확보했다.

<div style="margin-top: 1.3em;"></div>

- **Dense 모델 대비 효율적인 성능**
  - GPT-3 같은 Dense 모델과 비슷하거나 더 나은 성능을 보이면서도 **훨씬 적은 연산 자원**<small style="color:gray">(compute power)</small>으로 동작했다.

<br>

---
### 7️⃣ Chinchilla (2022) — 모델 크기보단 데이터 양!
2022년, DeepMind는 기존의 "모델은 클수록 좋다"는 통념에 강한 도전장을 던지는 연구를 발표했다.
> Chinchilla was a really important paper.

![DeepMind Chinchilla](https://www.pcguide.com/wp-content/uploads/2023/01/Chat-GPT-Alternatives.jpg)

##### 📌 주요 특징
- **"Scaling Law"에 대한 반론**
  - 이전까지는 모델의 파라미터 수만 늘리면 성능이 오른다는 믿음이 있었지만,
  - Chinchilla는 **같은 파라미터 수라도 훨씬 많은 데이터로 학습했을 때** 더 좋은 성능을 낼 수 있다는 걸 보여줬다. <small style="color:gray">(bigger ≠ always better)</small>

<div style="margin-top: 1.3em;"></div>

- **70B 파라미터 + 초대규모 데이터셋**
  - Chinchilla는 70억 개 파라미터<small style="color:gray">(GPT-3보다도 작음)</small>를 가지고도
  - GPT-3보다 **4배 더 많은 텍스트 데이터로 학습**함으로써 훨씬 뛰어난 성능을 달성했다.

<div style="margin-top: 1.3em;"></div>

- **"데이터 효율성"의 시대 개막**
  - 이 연구 이후, LLM을 설계할 때 **파라미터 수와 데이터량의 균형을 맞추는 것**이 중요한 기준이 되었따.
  - 더이상 "모델만 키우면 된다"는 단순한 스케일 전략은 통하지 않게 된 것! 

<br>

---
### 8️⃣ PaLM & PaLM 2 — 효율적인 스케일링 + 더 똑똑해진 후속 모델
2022년, Google은 **PaLM (Pathways Language Model)**을 공개하며 초거대 모델 경쟁에 본격적으로 뛰어들었다.
이 모델은 다양한 벤치마크에서 뛰어난 성능을 보였고, 그 배경에는 Google의 Pathways 시스템이 있었다.

![Google PaLM](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRB1PnUmq_jvA7oJ9eRUcKCGNvv-DhACeK4bw&s)

##### 📌 주요 특징
- **Pathways 시스템 기반**
  - 여러 작업을 하나의 모델로 처리할 수 있도록 설계된 시스템으로, **효율적인 확장(스케일링)**이 가능하다.
  - 덕분에 다양한 언어 태스크에서 높은 성능을 낼 수 있었다.

<div style="margin-top: 1.3em;"></div>

- **PaLM 2 (2023): 파라미터 수는 줄고, 성능은 향상**
  - 후속 모델인 PaLM 2는 PaLM에 비해 파라미터 수는 줄었지만, 추론<small style="color:gray">(reasoning)</small>, 수학, 코딩 등에서 더 강력한 성능을 보여줬다.
  - 현재 Google Cloud의 **생성형 AI 서비스들에 핵심으로 사용되고 있는 모델**이기도 하다

<br>

---
### 9️⃣ Gemini — 멀티모달, 더 빠르고 더 길게
Google은 Gemini라는 이름으로 새로운 세대의 LLM 시리즈를 선보였다.
Gemini는 처음부터 **멀티모달(multimodal)**을 염두에 두고 설계된 모델로, 단순한 언어 이해를 넘어서 다양한 입력을 처리할 수 있다.
> Gemini is really pushing the boundaries.

![Google Gemini](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8a/Google_Gemini_logo.svg/1200px-Google_Gemini_logo.svg.png)

##### 📌 주요 특징
- **멀티모달 지원**
  - 텍스트뿐 아니라 **이미지, 오디오, 비디오**까지 함께 처리할 수 있는 구조이다.
  - GPT-4와 비슷한 방향이지만, Gemini는 **처음부터 멀티모달을 목적으로 설계**되었다.

<div style="margin-top: 1.3em;"></div>

- **확장성과 효율성에 초첨**
  - Google의 **TPUs**<small style="color:gray">(Tensor Processing Unit)<small>에 최적화되어 **매우 빠르게 작동**한다.
  - **MoE**<small style="color:gray">(Mixture of Experts)</small> 구조도 일부 버전에 적용하여 효율적 연산이 가능하다.

<div style="margin-top: 1.3em;"></div>

- **여러 크기의 모델 버전**
  - **Ultra, Pro, Nano, Flash** 등 다양한 크기로 제공되어, 용도에 따라 선택이 가능하여 경량 모델부터 대규모 모델까지 유연하게 대응이 가능하다.

<div style="margin-top: 1.3em;"></div>

- **Gemini 1.5 Pro: 긴 문맥 처리 능력**
  - 특히 **Gemini 1.5 pro**는 **수백만 토큰까지 처리 가능한 초장기 컨텍스트 윈도우**를 지원한다.
  - 대용량 문서 분석, 코드 해석에서 강력한 능력을 발휘한다.

<br>

---
### 💭오늘 챙겨간 것들
이번 글에서는 Transformer 이후 등장한 주요 LLM들의 흐름을 따라가며, GPT-1부터 Gemini까지 어떤 변화와 진화가 있었는지 살펴봤다.
- Decoder-Only 구조의 시작 (GPT 시리즈)
- 텍스트 이해 특화 모델 BERT
- 스케일이 성능을 바꾸는 GPT-2 ~ GPT-4
- 대화형 모델 LaMDA, 효율형 GLaM
- 데이터 효율성을 강조한 Chinchilla
- Pathways 기반 PaLM 시리즈
- 멀티모달 Gemini 시리즈

이 흐름으로, 이제 LLM은 단순 크기 경쟁이 아니라, 모달리티/속도/효율성/문맥 처리 능력까지 고려하는 **종합적 기술 경쟁** 시대로 들어왔다는 걸 알 수 있다!  

다음 글에서는, 최근 뜨고 있는 **오픈소스 모델들**에 대해서도 살펴보겠다. 🚀

<br>

---
<ul>
 Day-1
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-day1-assignments/">[Kaggle Gen AI] Day 1 과제 소개 - LLM & Prompt Engineering 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_1/">[Kaggle Gen AI] Day 1 - Transformer 구조 한 눈에 보기🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_2/">[Kaggle Gen AI] Day 1 - Decoder-Only 구조, 왜 LLM은 디코더만 쓸까? 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_4/">[Kaggle Gen AI] Day 1 - 오픈소스 LLM 생태계, 한눈에 보기 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_5/">[Kaggle Gen AI] Day 1 - LLM 파인튜닝 A to Z 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_6/">[Kaggle Gen AI] Day 1 - 프롬프트 엔지니어링, LLM을 잘 쓰는 기술 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_7/">[Kaggle Gen AI] Day 1 - 샘플링으로 바꾸는 LLM의 말하기 스타일 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_8/">[Kaggle Gen AI] Day 1 - 정답이 없어서 더 중요한 LLM 평가법 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_9/">[Kaggle Gen AI] Day 1 - 품질, 속도, 비용 사이에서 균형 잡기: LLM 추론 가속 기법들 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_10/">[Kaggle Gen AI] Day 1 - LLM은 지금 어디에 쓰이고 있을까? 실전 적용 사례 🚀</a></small></li>
</ul>
<br><br>