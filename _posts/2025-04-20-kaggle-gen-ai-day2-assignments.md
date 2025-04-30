---
layout: post
title:  "[Kaggle Gen AI] Day 2 과제 소개 - Embeddings & Vector Stores 🚀"
author: me
categories: [ Kaggle Gen-AI ]
date: 2025-04-20 09:00:00
image: assets/images/20250402/day2.png
---
약 3주가 걸린 Day1에 이은 Day2에서는 **임베딩(Embeddings)**과 **벡터 데이터베이스(Vector Stores)**의 개념을 익히고,  이를 활용해 LLM이 외부 지식과 연결되는 방식을 실습해보는 흐름으로 구성되어 있다.

진행 순서는 아래 정리한 목록대로 따라가면 되는데,  각 항목에는 관련된 팟캐스트나 백서 링크도 함께 적어놨으니 바로 확인 가능하다.  

개인적으로는 팟캐스트가 백서 내용을 아주 잘 요약해주고 있고, 설명 흐름도 매끄러워서 호다닥 공부해야 한다면 **팟캐스트를 굉장히 추천!**  

<br>

---

### 🎒 Today’s Assignments
1. Complete Unit 2 – “Embeddings and Vector Stores / Databases”  
   <small style="color:gray">유닛 2 – “임베딩과 벡터 저장소/데이터베이스”</small>
   - Listen to the [summary podcast episode](https://www.youtube.com/watch?v=xCAVsst6WJ8&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=4) for this unit  
     <small style="color:gray">유닛 2의 요약 팟캐스트 듣기</small>
   - To complement the podcast, read the “[Embeddings and Vector Stores/ Databases](https://www.kaggle.com/whitepaper-embeddings-and-vector-stores)" whitepaper  
     <small style="color:gray">팟캐스트를 보완하기 위해, 관련 백서 읽기</small>
   - Complete these codelabs on Kaggle:  
     <small style="color:gray">Kaggle 코드랩 실습 진행하기</small>  
     - [Build a RAG QA system over custom documents](https://www.kaggle.com/code/markishere/day-2-document-q-a-with-rag)  
       <small style="color:gray">사용자 문서 기반 RAG 질문응답 시스템 구축</small>  
     - [Explore text similarity with embeddings](https://www.kaggle.com/code/markishere/day-2-embeddings-and-similarity-scores)  
       <small style="color:gray">임베딩을 활용한 텍스트 유사도 탐색</small>  
     - [Neural classification with Keras and embeddings](https://www.kaggle.com/code/markishere/day-2-classifying-embeddings-with-keras)  
       <small style="color:gray">Keras를 활용한 임베딩 기반 분류기 만들기</small>  
   - Want to have an [interactive conversation](https://support.google.com/notebooklm/answer/15731776?hl=en&ref_topic=14272601&sjid=16012842710481496794-EU)? Try adding the whitepaper to [NotebookLM](https://notebooklm.google.com/?original_referer=https:%2F%2Fwww.google.com%23&pli=1).  
     <small style="color:gray">대화형 학습을 원한다면 NotebookLM에 백서를 추가해보기</small>


<br>

---
### **💡 What You’ll Learn**
> Today you will learn about the conceptual underpinning of embeddings and vector databases, and how they can be used to bring live or specialist data into your LLM application. You’ll also explore their geometrical powers for classifying and comparing textual data as well as how to evaluate embeddings.

📖 Day 2에서는 **임베딩과 벡터 데이터베이스의 핵심 개념**을 정리한다.  

🧾 이를 활용해 **외부 지식이나 도메인 특화 데이터**를 LLM에 연결하는 방법도 알아보고,  

🔎 임베딩의 **기하학적 성질(geometric properties)**을 이용해 텍스트 데이터를 분류하고 비교하는 방법, 그리고 임베딩을 어떻게 평가하는지까지 함께 다룰 예정이다.

<br>
