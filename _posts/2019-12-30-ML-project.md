---
layout: post
title: 'Make StreetFighter II Champion Using Reinforcement Learning'
categories:
  - Project
tags:
  - deep learning
  - reinforcement learning
  - project detail
---

고려대학교 김현우 교수님의 기계학습 수업 때 진행한 프로젝트를 정리한 포스트입니다.

* [한국정보과학회 format report](/assets/pdf/SF_report.pdf)
* [presentation slide](/assets/pdf/SF_presentation.pdf)

### Abstract
마리오나 소닉처럼 언제 장애물, 버섯, 동전이 나오는지 다 정해져 있는 규칙 기반의 환경은 agent가 환경을 암기해서 학습을 할 수 있고 그렇다면 이것은 비교적 간단한 task이다. 그러나 스트리트 파이터는 격투게임의 특성상 두 명의 캐릭터가 상호작용 하면서 진행되기 때문에 규칙 기반이 아닌 stochastic한 환경이고 이는 현실세계와 닮아있다. 그래서 이번 프로젝트에서 스트리트 파이터를 주제로 선정하게 되었다.  
이번 프로젝트의 목적은 스트리트 파이터에 존재하는 12가지 캐릭터를 normal level에서 모두 이기는 agent를 학습하는 것이다.

### Method
기본적인 Baseline은 **DQN** 과 **A2C** 를 사용하였다. 두가지 모두 논문을 보고 재구현하였고 atari pong이 아닌 스트리트 파이터에 적용하기 위해서 Preprocessing, action space 등 여러가지 부분을 수정하였다. 첫번째 목표는 한가지 캐릭터를 완벽히 이기는 것이었다. 바이슨이라는 캐릭터를 골랐고 학습 결과 easy는 완벽하게 이길 수 있었지만 normal에게는 그렇지 못한 결과를 확인 할 수 있었다.  
그래서 normal level에서도 완벽하게 이기기 위해서 **curriculum learning** 을 사용하였다. A2C는 DQN과 다르게 여러개의 actor가 병렬적으로 학습을 하기 때문에 이 장점을 사용하였다. A2C의 actor를 12개로 설정하고 학습 초기에는 12개의 actor 모두 easy level, 중반에는 6개 easy, 6개 normal, 후반에는 12개 모두 normal level로 학습을 하였다. 그 결과 normal level도 완벽하게 이길 수 있었다.  
영상추가  
한가지 캐릭터를 완벽히 이긴 다음 목표는 12가지 캐릭터를 모두 이기는 것이었다. General 한 agent를 만들기 위해 A2C의 장점을 다시 사용하여 12개의 actor에 12가지 캐릭터를 배정하였다. 학습결과 agent가 상대방 캐릭터를 구별하지 못하고 모두에게 같은 policy를 학습하는 것을 확인할 수 있었다. 그래서 단거리 공격을 하는 다른 캐릭터와는 달리 장거리 공격을 하는 달심이라는 캐릭터에게 무참히 패배하였다.  
이 문제를 해결하기 위해서 구상한 모델은 **A2C3(Advantage Actor Critic + Character Classify)** 이다. 모델 구성은 다음과 같다.
![MLproject-02](/assets/img/project/ML/A2C3.png)   
기존의 A2C 모델에 상대방 캐릭터를 구별할 수 있는 능력을 주기 위해서 새로운 layer를 추가하였다. 그 결과 agent가 상대방 캐릭터를 구별하고 policy를 학습해서 12가지 캐릭터를 이길 수 있었다.  
이번 프로젝트에서는 직접 reward를 디자인해서 학습하였다. 강화학습은 reward를 기반으로 학습을 하기 때문에 agent의 특성을 조절할 수 있었다. 첫번째는 체력이 깎이면 패널티를 주어서 방어형 캐릭터를 만든 결과이고 두번째는 체력이 깎여도 패널티를 주지 않아서 공격형 캐릭터를 만든 결과이다.  
{% include sf.html id="GGeVO6JX-DM" %}  

### 소감 및 마무리  
이 스트리트 파이터 프로젝트는 타임라인 전부를 직접 전부 구현하였다. 그래서 정말 힘들었지만 그래도 배운 것이 정말 많았다. 학습된 agent와 직접 게임을 해볼 수 있는 코드도 구현하여 발표시간에 demo를 진행하였다. 좋은 성적과 결과를 얻어서 뿌듯한 프로젝트였다. 이번 프로젝트에서 reward를 디자인해서 구현하였는데 다음번에는 이겼을 때 1, 졌을 때 -1 만 줘서 agent가 맞는 것이 패널티이고 때리는 것이 reward임을 스스로 깨닫게 할 수 있는 부분을 구현하고 싶다. 그리고 최근 Deepmind에서 사용한 방법처럼 self play를 통해서 정해진 timestep마다 모델을 저장하고 그 저장된 모델끼리 league전을 돌려서 학습을 해서 더 강한 agent를 만들어보고 싶다.  
