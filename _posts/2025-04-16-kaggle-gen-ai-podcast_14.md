---
layout: post
title:  "[Kaggle Gen AI] Day 1 - Codelab 시작, 환경 세팅부터 프롬프트 엔지니어링 실습까지 🚀"
author: me
categories: [ Kaggle Gen-AI, codelabs]
date: 2025-04-15 09:00:00
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

요기까지 환경설정 프롬프트 실습은 여기부터~ (작성중)

<br><br>
