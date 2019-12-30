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
* [presentation slide](/assets/pdf/SF_presentation)

### Abstract
마리오나 소닉처럼 언제 장애물, 버섯, 동전이 나오는지 다 정해져 있는 규칙 기반의 환경은 agent가 환경을 암기해서 학습을 할 수 있고 그렇다면 이것은 비교적 간단한 task이다. 그러나 스트리트 파이터는 격투게임의 특성상 두 명의 캐릭터가 상호작용 하면서 진행되기 때문에 규칙 기반이 아닌 stochastic한 환경이고 이는 현실세계와 닮아있다. 그래서 이번 프로젝트에서 스트리트 파이터를 주제로 선정하게 되었다.  
이번 프로젝트의 목적은 스트리트 파이터에 존재하는 12가지 캐릭터를 normal level에서 모두 이기는 agent를 학습하는 것이다.

### Method
기본적인 Baseline은 DQN과 A2C를 사용하였다. 두가지 모두 논문을 보고 재구현하였고 atari pong이 아닌 스트리트 파이터에 적용하기 위해서 Preprocessing, action space 등 여러가지 부분을 수정하였다. 첫번째 목표는 한가지 캐릭터를 완벽히 이기는 것이었다. 바이슨이라는 캐릭터를 골랐고 학습 결과 easy는 완벽하게 이길 수 있었지만 normal에게는 그렇지 못한 결과를 확인 할 수 있었다.  
