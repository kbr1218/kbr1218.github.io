---
layout: post
title:  "[Kaggle Gen AI] Day 1 - Codelab 시작, 환경 세팅부터 프롬프트 엔지니어링 실습까지 🚀"
author: me
categories: [ Kaggle Gen-AI, codelabs]
date: 2025-04-16 09:00:00
image: assets/images/20250402/day1_2.png
---
[지난 게시물](https://kbr1218.github.io/kaggle-gen-ai-podcast_13/)까지 LLM 프롬프트들의 핵심 기법들을 살펴봤고, 이제 Kaggle의 CodeLab에서 실습을 해 볼 차례!  

우선 CodeLab을 처음 시작하는 거니까 **첫 번째 노트북: Prompting Fundamentals**를 따라가면서  
✅ 환경 세팅부터  
✅ Gemini API 연결,  
✅ 그리고 **기초적인 프롬프트 엔지니어링 실습**  
까지 함께 정리해보겠다!

[👉 실습 원본 Kaggle notebook 링크 바로가기](https://www.kaggle.com/code/markishere/day-1-prompting)

<br>

---
### ⚙️ 1. 실습 전 준비 사항
이번 CodeLab은 **Python SDK를 통해 Gemini API를 호출**하는 방식으로 진행된다.  
하지만 모든 프롬프트는 **Google AI Studio 링크**도 함께 제공하기 때문에 **파이썬 코드 없이도 클릭으로만 실습**할 수 있다고 한다.

<br>

---
### 🔑 2. Gemini API 키 발급받기
우선 실습에서 사용하는 생성형 AI는 Gemini이므로 Gemini API 키를 먼저 발급받아야 한다.  

1. 먼저 [Google Cloud Console](https://console.cloud.google.com/)에 접속해서 새로운 프로젝트를 만든다.
     <div style="flex: 4; min-width: 120px;">
         <img src="/assets/images/20250416/1.png" 
             alt="프로젝트 만들기" 
            style="max-width: 100%; display: block; margin-left: auto; border: 1px solid #ddd; border-radius: 6px;" />
            <small style="color:gray">프로젝트 이름 입력하고 [만들기]</small>
    </div>
    <div style="margin-top: 3.3em;"></div>

2. 그 다음 [Gemini API](https://ai.google.dev/gemini-api/docs)로 이동 → `Get a Gemini API Key` 선택 → `API 키 만들기` 선택  
     <div style="flex: 4; min-width: 120px;">
         <img src="/assets/images/20250416/2.png" 
             alt="API 키 만들기" 
            style="max-width: 100%; display: block; margin-left: auto; border: 1px solid #ddd; border-radius: 6px;" />
            <small style="color:gray">[Get a Gemini API Key] 선택</small>
    </div>
     <div style="flex: 4; min-width: 120px;">
         <img src="/assets/images/20250416/3.png" 
             alt="API 키 만들기" 
            style="max-width: 100%; display: block; margin-left: auto; border: 1px solid #ddd; border-radius: 6px;" />
            <small style="color:gray">[API 키 만들기] 선택</small>
    </div>
    <div style="margin-top: 3.3em;"></div>

3. 방금 만든 프로젝트를 선택하고 API 키를 생성한다.
     <div style="flex: 4; min-width: 120px;">
         <img src="/assets/images/20250416/4.png" 
             alt="API 키 생성하기" 
            style="max-width: 100%; display: block; margin-left: auto; border: 1px solid #ddd; border-radius: 6px;" />
    </div>
    <div style="background: #fff9e7; border-left: 4px solid #fdc82a; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💡 생성된 API 키는 복사해서 <strong>따로 안전하게 보관</strong>하기! (공개되어 있는 곳에 올리면 안됨) 
    </div>
    <br>

---
### 🔐 3. Kaggle notebook에 API Key 등록하기
이제 이 키를 Kaggle 환경에서 사용할 수 있도록 설정해야 한다. 

- Kaggle 노트북 상단 메뉴에서 `Add-ons > Secrets > Add Secret` 선택
- Label은 `GOOGLE_API_KEY`로,  Value에는 복사한 API Key를 입력  

     <div style="flex: 4; min-width: 120px;">
         <img src="/assets/images/20250416/5.png" 
             alt="프로젝트 만들기" 
            style="max-width: 100%; display: block; margin-left: auto; border: 1px solid #ddd; border-radius: 6px;" />
    </div>

<br>

---
### 🛠️ 4. 코드를 실행할 때 에러가 난다면?
여기까지 설정하고 코드 셀을 실행했을 때 아래와 같은 경고가 뜨면서 코드 실행이 안된다면?
```python
    WARNING: Retrying (Retry(total=4, connect=None,read=None, redirect=None, status=None)) after connection broken by 'NewConnectionError(' ... ): /simple/google-genai/
    ...
    NewConnectionError: Failed to establish a new connection
```
이건 대부분 Kaggle 계정에 **전화번호 인증이 안 된 상태**라서 발생한 거 일 수도 있따.  
Google API에 접근하려면 인증된 계정이 필요하므로, Kaggle **계정 설정에서 `phone verification`**을 먼저 해주기.

<br>

---

### 📝 5. 실습

#### 5-0. Run your first prompt
API가 제대로 설정되었는지 확인하기 위해 간단한 프롬프트로 API에 요청을 보내보기  

Python SDK에서는 클라이언트 객체를 사용하여 API 요청을 보내는데, 클라이언트 객체는 어떤 백엔드<small style="color:gray">(Gemini API oder Vertex AI)</small>를 사용할지 선택할 수 있게 해주고, 인증<small style="color:gray">(여기서는 API Key)</small>도 처리해준다.  

여기서는 `gemini-2.0-flash` 모델을 사용  
```python
client = genai.Client(api_key=GOOGLE_API_KEY)

response = client.models.generate_content(
    model="gemini-2.0-flash",
    contents="Explain AI to me like I'm a kid.")

print(response.text)
```

응답은 `markdown` 형식으로 자주 반환되는데, kaggle notebook 안에서 렌더링하려면 `Markdown(response.text)`를 사용하면 된다.  

그냥 `print()`를 사용하는 대신 제목/굵은 글씨/리스트를 표현할 수 있어서 더 깔끔하다.  

```python
Markdown(response.text)
```

<br>

---
#### 5-1. Start a chat
이전 예시는 한 번의 질문과 한 번의 응답으로 이루어진 **싱글턴 방식**<small style="color:gray">(single-turn)</small> 구조를 사용했다.  
여러 차례 대화를 주고 받는 형태인 **멀티턴 방식**<small style="color:gray">(multi-turn)</small>으로도 설정할 수 있다.  
```python
chat = client.chats.create(model='gemini-2.0-flash', history=[])
response = chat.send_message('Hello! My name is Mal.')
print(response.text)
```
```plaintext
<output>
Hello Mal! It's nice to meet you. How can I help you today?
```

이렇게 내 이름 정보를 주고, 같은 `chat` 객체에 대해서 다른 메시지 보내기
```python
response = chat.send_message('Can you tell me something interesting about Taylor Swift?')
print(response.text)
```
```plaintext
<output>
Okay, here's an interesting fact about Taylor Swift:

Beyond her obvious musical talent, Taylor Swift is...
```

`chat` 객체가 활성화된 상태로 유지되는 동안 대화의 문맥은 계속 유지되는데, 이를 확인하기 위해 첫 번째 대화에서 알려준 내 이름을 다시 물어본다.  
```python
response = chat.send_message('Do you remember what my name is?')
print(response.text)
```
그럼 아래와 같이 내 이름을 기억하는 걸 알 수 있다. 

```plaintext
<output>
Yes, your name is Mal.
```

<br>

---
#### 5-2. Choose a model
Gemini API를 통해서 Gemini model family에 속한 다양한 모델을 사용할 수 있다.  
각 모델의 특징과 기능은 [model overview page](https://ai.google.dev/gemini-api/docs/models/gemini)에서 확인할 수 있고, 여기서는 API를 호출하여 사용 가능한 모델 목록을 조회하는 거 해보기  
```python
for model in client.models.list():
  print(model.name)
```

`model.list` 응답은 모델의 기능에 대한 추가 정보도 제공한다. <small style="color:gray">(e.g. 토큰 제한, 지원되는 매개변수 etc.)</small>
```python
# gemini-2.0-flash 모델의 자세한 설정 정보를 확인

from pprint import pprint

for model in client.models.list():
  if model.name == 'models/gemini-2.0-flash':
    pprint(model.to_json_dict())
    break
```

<br>

---
#### 5-3. Explore generation parameters
##### 1️⃣ Output length (출력 길이)
`max_output_tokens`라는 매개변수 값을 설정하면, 지정된 토큰 수에 도달할 경우 텍스트 생성을 멈춘다.  

<div style="background: #fff9e7; border-left: 4px solid #fdc82a; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
     ⚠️ <strong>주의사항</strong><br>
    출력을 간결하게 만들어주는 게 아니라, 그냥 <strong>중간에 멈추게 하는 것</strong>. 설정된 길이 안에서 완성도 있는 응답을 받기 위해서는 <strong>프롬프트를 잘 설계</strong>해야 한다. 
</div>

```python
from google.genai import types

# max_output_tokens는 여기서 설정
short_config = types.GenerateContentConfig(max_output_tokens=200)

response = client.models.generate_content(
    model='gemini-2.0-flash',
    config=short_config,
    contents='Write a 1000 word essay on the importance of olives in modern society (in korean).')

print(response.text)
```
이렇게 `max_output_tokens`를 `200`으로 제한해놓고 현대 사회에서 올리브의 중요성에 대해 1000 단어 에세이를 써달라고 부탁하면, 
```plaintext
<output>
## 현대 사회에서 올리브의 중요성: 맛과 건강, 그리고 지속가능성의 조화 (현대 사회에서 올리브의 중요성: 맛과 건강, 그리고 지속가능성의 조화)

현대 사회는 급변하는 환경 속에서 건강, 환경, 그리고 윤리적 소비에 대한 관심이 높아지고 있습니다. 이러한 시대적 요구에 부응하며, 올리브는 단순히 식재료를 넘어 건강한 라이프스타일, 지속가능한 농업, 그리고 미식의 즐거움을 연결하는 중요한 매개체로 자리매김하고 있습니다. 이 글에서는 올리브가 현대 사회에서 가지는 다각적인 중요성을 맛, 건강, 그리고 지속가능성의 측면에서 심도 있게 논하며, 그 가치를 재조명하고자 합니다.

**1. 미식의 즐거움: 다채로운 맛과 활용의 무한함**

올리브는 특유의 짭짤
```
여기서 끊긴 output을 확인할 수 있따.   
이번에는 같은 토큰 사이즈 설정으로, **short poem**을 써달라고 부탁하면?
```python
response = client.models.generate_content(
    model='gemini-2.0-flash',
    config=short_config,
    contents='Write a short poem on the importance of olives in modern society (in korean again)')

print(response.text)
```
```plaintext
<output>
Okay, here's a short poem in Korean about the importance of olives in modern society:

**올리브, 작은 열매**

푸른 열매, 기름 되어
삶의 빛을 더하네.
샐러드 위에, 파스타 속에,
지중해 향기 퍼지네.

미용에도, 건강에도,
현대의 보물 되네.
작은 올리브, 큰 힘 되어
세상을 풍요롭게 하네.
```
이렇게 웃기지만 내용은 완성된 짧은 시가 출력되는 걸 확인할 수 있다.  

<br>

##### 2️⃣ Temperature (온도)
temperature는 언어 모델이 다음에 어떤 단어(토큰)을 선택할 때 얼마나 **"랜덤하게"** 고를지를 조절하는 매개변수이다.  

**온도가 높을 수록**(temp 값이 클 수록)
- 선택 가능한 후보 토큰 수가 많아지고
- 결과가 더 창의적이고 다양해질 수 있다
- e.g. `temperature = 1.0` : 더 풍부하고 예측 불가능한 답변  

**온도가 낮을 수록**(temp 값이 작을 수록)
- 가장 가능성 높은(확률이 높은) 단어를 더 자주 선택한다
- e.g. `temperature = 0`: 가장 확실한 답만 선택한다
- **greedy decoding**  

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 temperature는 무조건 무작위<small style='color:gray'>(random)</small>로 만드는 설정은 아니고, 단지 결과가 얼마나 다양해질 수 있는지를 <strong>"살짝 밀어주는 (nudge)"</strong> 역할을 할 뿐이다. 
</div>

예제에서는 temp 값을 다르게 해서 랜덤 색상을 하나만 고르라는 질문을 5번 반복한다.
```python
# temp를 2.0으로 설정
high_temp_config = types.GenerateContentConfig(temperature=2.0)

for _ in range(5):
  response = client.models.generate_content(
      model='gemini-2.0-flash',
      config=high_temp_config,
      contents='Pick a random colour... (respond in a single word)')

  if response.text:
    print(response.text, '-' * 25)
```
```plaintext
<output>
Purple
 -------------------------
Orange
 -------------------------
Turquoise
 -------------------------
Teal
 -------------------------
Orange
 -------------------------
```
나름 다양한 답변 생성한다.  이번에는 temp를 0으로 설정하고 똑같은 질문을 5번 반복  
```python
# temp를 0.0으로 설정
low_temp_config = types.GenerateContentConfig(temperature=0.0)

for _ in range(5):
  response = client.models.generate_content(
      model='gemini-2.0-flash',
      config=low_temp_config,
      contents='Pick a random colour... (respond in a single word)')

  if response.text:
    print(response.text, '-' * 25)
```
```plaintext
<output>
Azure
 -------------------------
Azure
 -------------------------
Azure
 -------------------------
Azure
 -------------------------
Azure
 -------------------------
```
이번에는 '파란색'이라는 답변을 5번 연속으로 생성한다. 

<br>

##### 3️⃣ Top-P
**"누적 확률"**을 기준으로 다음 토큰을 선택할 후보를 제한하는 방식이다.  
- e.g. `top_p = 0.9`로 설정하면
    - 모델은 가장 확률이 높은 토큰들 중에서, 누적 확률이 90%에 도달할 때까지만 후보로 포함시킨다. 
    - 그 후 나머지 확률 낮은 토큰은 무시한다. 

매개변수에 대한 자세한 개념들은 [여기](https://kbr1218.github.io/kaggle-gen-ai-podcast_11/)에 이미 정리했으니 코드 중심으로 확인하는 걸로!  

```python
model_config = types.GenerateContentConfig(
    # 이게 gemini-2.0-flash 모델의 기본값
    temperature=1.0,
    top_p=0.95,
)

# gemini-2.0-flash의 기본 설정으로 모험을 떠나는 고양이에 관한 짧은 글을 써달라고 했을 때
story_prompt = "You are a creative writer. Write a short story in korean about a cat who goes on an adventure."
response = client.models.generate_content(
    model='gemini-2.0-flash',
    config=model_config,
    contents=story_prompt)

print(response.text)
```

똑같은 질문을 `top_p` 값만 `0.0`으로 바꿔서 해봤는데, 사실 출력의 차이를 크게 못 느끼겠다. `0.0` 설정으로 출력한 글에 반복되는 단어들이 많은 것 같기도 하고

<details> <summary><strong>top_p 값이 0.95일 때</strong></summary>
## 밤의 수수께끼 (Bam-eui Susukkekki - Riddle of the Night)<br>
깜깜한 밤, 솜뭉치처럼 하얀 고양이, 미루(Miru)는 조심스럽게 담벼락을 뛰어넘었다. 낮에는 햇살 아래 졸고, 생선 냄새에 꼬리를 흔드는 평범한 고양이였지만, 밤이 되면 미루는 모험가가 되었다. 그의 눈은 어둠 속에서도 빛나는 두 개의 초롱불 같았다.<br>
오늘은 특별한 밤이었다. 늙은 고양이 할머니로부터 들은 '달빛 정원'의 전설을 따라, 미루는 금지된 숲으로 향했다. 달빛 정원은 밤에만 피어나는 신비로운 꽃들로 가득한 곳이라 했다. 꽃잎은 별빛을 머금고 있어, 만지는 자에게 행운을 가져다준다고.<br>
숲은 예상보다 훨씬 어두웠다. 나무들은 거대한 손처럼 뻗어 미루를 위협했고, 풀숲에서는 이름 모를 벌레들이 울어댔다. 미루는 불안했지만, 할머니의 이야기에 매료되어 용기를 냈다. 작은 코를 킁킁거리며, 그는 달콤하고 은은한 향기를 따라 숲 속 깊숙이 들어갔다.<br>
얼마나 걸었을까, 드디어 미루는 작은 냇가에 도착했다. 냇물은 달빛을 받아 은빛으로 빛났고, 그 주변에는 꿈결같은 풍경이 펼쳐져 있었다. 키가 큰 꽃들이 부드럽게 빛을 내며 흔들리고 있었다. 바로 달빛 정원이었다!<br>
미루는 숨을 죽이고 정원으로 들어갔다. 꽃잎은 정말 별빛처럼 반짝거렸다. 그는 조심스럽게 가장 아름다운 꽃 한 송이를 골라 앞발로 살짝 만졌다. 순간, 온 몸에 따뜻한 기운이 퍼지는 듯했다.<br>
그때, 어디선가 날카로운 울음소리가 들려왔다. 미루는 깜짝 놀라 소리가 나는 쪽으로 고개를 돌렸다. 작은 새 한 마리가 덫에 걸려 발버둥치고 있었다.<br>
미루는 고민했다. 행운을 가져다주는 꽃을 만졌으니, 그냥 돌아가는 게 좋을까? 하지만, 새의 애처로운 눈빛을 외면할 수 없었다.<br>
미루는 용기를 내어 덫으로 다가갔다. 작은 발톱으로 조심스럽게 덫을 풀자, 새는 자유로워졌다. 새는 고맙다는 듯 미루의 볼에 부드럽게 부리를 대고 하늘로 날아갔다.<br>
미루는 아쉬운 마음은 없었다. 작은 생명을 구했다는 뿌듯함이 그 어떤 행운보다 값지다는 것을 깨달았기 때문이다.<br>
집으로 돌아가는 길은 더 이상 무섭지 않았다. 미루는 밤하늘을 올려다보며 생각했다. 진정한 행운은, 용기를 내어 누군가를 도울 때 찾아온다는 것을. 그리고 그는, 이미 누구보다 행복한 고양이였다.<br><br>

**Translation:**<br>
## Riddle of the Night<br>
In the dark night, Miru, a cat as white as a cotton ball, carefully jumped over the fence. By day, he was an ordinary cat, dozing in the sunlight and wagging his tail at the smell of fish, but by night, Miru became an adventurer. His eyes were like two bright lights shining in the darkness.<br>
Tonight was a special night. Following the legend of the 'Moonlight Garden' told by the old cat grandmother, Miru headed for the forbidden forest. The Moonlight Garden was said to be a place filled with mysterious flowers that only bloomed at night. Their petals held the starlight and brought good luck to anyone who touched them.<br>
The forest was much darker than expected. The trees stretched out like huge hands, threatening Miru, and unknown insects chirped in the bushes. Miru was anxious, but he was fascinated by the grandmother's story and mustered his courage. Sniffing with his small nose, he went deeper into the forest, following a sweet and subtle fragrance.<br>
How long had he walked? Finally, Miru arrived at a small stream. The stream shimmered silver in the moonlight, and a dreamlike landscape unfolded around it. Tall flowers swayed gently, emitting a soft light. It was the Moonlight Garden!<br>
Miru held his breath and entered the garden. The petals really sparkled like starlight. He carefully chose the most beautiful flower and gently touched it with his front paw. In that instant, a warm energy seemed to spread throughout his body.<br>
Then, a sharp cry was heard from somewhere. Startled, Miru turned his head towards the sound. A small bird was struggling, caught in a trap.<br>
Miru hesitated. He had touched a flower that brought good luck, so should he just go back? But, he couldn't ignore the bird's pitiful eyes.<br>
Miru mustered his courage and approached the trap. Carefully using his small claws, he released the trap, and the bird was freed. As if to say thank you, the bird gently touched Miru's cheek with its beak and flew into the sky.<br>
Miru didn't feel any regret. He realized that the satisfaction of saving a small life was more valuable than any luck.<br>
The way home was no longer scary. Miru looked up at the night sky and thought. True luck comes when you have the courage to help someone. And he was already, more than ever, a happy cat.
</details> 
<details> <summary><strong>top_p 값이 0.0일 때</strong></summary>
## 별똥별을 쫓는 고양이 (Byeolddongbyeoreul Jjochneun Goyangi - The Cat Who Chased a Shooting Star)<br>
달빛이 쏟아지는 밤, 담벼락 위에서 졸고 있던 삼색 고양이 나비는 갑자기 눈을 번쩍 떴다. 꼬리를 살랑거리며 하품을 하려던 찰나, 하늘을 가로지르는 빛줄기를 본 것이다. 별똥별이었다!<br>
나비는 평범한 고양이가 아니었다. 낡은 골목길을 제 집처럼 누비며, 쥐를 잡는 것보다 새로운 냄새를 쫓는 것을 더 좋아했다. 그날 밤, 나비는 별똥별이 떨어진 곳을 찾아 떠나기로 결심했다.<br>
"야옹!" 나비는 작게 울며 담벼락에서 뛰어내렸다. 차가운 밤공기가 콧등을 간지럽혔다. 낯선 냄새들이 코를 찔렀지만, 나비는 꿋꿋이 앞으로 나아갔다.<br>
골목길을 빠져나오자 넓은 공원이 나타났다. 밤의 공원은 낮과는 전혀 다른 모습이었다. 풀벌레 소리가 귓가를 맴돌고, 나무 그림자가 괴물처럼 흔들렸다. 나비는 긴장했지만, 별똥별을 향한 호기심이 두려움을 이겼다.<br>
공원을 가로질러 숲길에 들어섰다. 숲은 더욱 어두웠다. 나뭇가지에 걸린 달빛만이 희미하게 길을 비춰주었다. 갑자기 부엉이 울음소리가 들려왔다. 나비는 깜짝 놀라 털을 곤두세웠지만, 멈추지 않았다.<br>
한참을 걸었을까, 숲 속 깊은 곳에서 희미한 빛이 보였다. 나비는 조심스럽게 빛을 향해 다가갔다. 빛은 작은 연못가에서 뿜어져 나오고 있었다. 연못 가운데에는 반짝이는 조약돌이 놓여 있었다.<br>
나비는 조약돌을 자세히 살펴보았다. 따뜻한 온기가 느껴지고, 은은한 빛을 내뿜는 것이 정말 별똥별 조각 같았다. 나비는 조약돌 옆에 앉아 밤하늘을 올려다보았다. 수많은 별들이 반짝이고 있었다.<br> 
나비는 깨달았다. 별똥별은 특별한 곳에 떨어진 것이 아니라, 어디든 빛을 선물할 수 있다는 것을. 그리고 자신 역시, 작은 조약돌처럼 누군가에게 작은 빛이 될 수 있다는 것을.<br>
나비는 조약돌을 뒤로하고 다시 골목길로 돌아왔다. 낡은 담벼락 위에 올라앉아 밤하늘을 바라보았다. 더 이상 별똥별을 쫓을 필요는 없었다. 나비는 이미 자신의 마음속에 작은 별을 품고 있었으니까.<br>
그리고 다음 날 아침, 나비는 골목길에서 굶주린 새끼 고양이들에게 따뜻한 우유를 나눠주었다. 나비의 작은 별빛이, 또 다른 작은 별들을 밝히기 시작한 것이다.
</details> 

<br>

---
### 💭 오늘 챙겨간 것들
여기까지가 Kaggle CodeLab 첫 번째 실습 **Prompting Fundamentals** 의 일부이다.  

✔️ Gemini API 키 발급과 환경 세팅,  
✔️ 모델 호출과 간단한 프롬프트 작성,  
✔️ **멀티턴 대화 세션(chat)** 시작하기,  
✔️ 사용 가능한 모델 목록 조회 및 세부 설정 살펴보기,  
✔️ 그리고 출력 길이, temperature, Top-P 같은 **생성 제어 파라미터 조정**까지!

이번 Unit에서는 기본적인 프롬프트 엔지니어링 흐름과, Gemini 모델을 다루는 데 필요한 핵심 설정들을  봤고, 이제 더 복잡하고 섬세한 프롬프트를 설계할 준비 완료   

다음 글에서는 **Zero-shot**, **One-shot**, **Few-shot** 방식으로 모델을 다루는 방법부터,  
✔️ **JSON Mode**를 활용해 구조화된 데이터 뽑아내기,  
✔️ **Chain of Thought (CoT)** 와 **ReAct** 전략으로 복잡한 문제 해결하기,  
✔️ **Thinking Mode**를 설정해서 모델 reasoning 능력 끌어올리기,  
✔️ **Code Prompting**을 통해 코드를 생성하고 디버깅하는 방법까지,  

**실전에서 바로 쓸 수 있는 고급 프롬프트 기법**들을 다뤄볼 예정입니다야호.

<br><br>