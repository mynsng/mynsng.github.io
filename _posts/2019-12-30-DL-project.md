---
layout: post
title: 'Question Answering Network for Physical Reasoning'
categories:
  - Project
tags:
  - deep learning
  - reinforcement learning
  - project detail
---

고려대학교 김현우 교수님의 딥러닝 수업 때 진행한 프로젝트를 정리한 포스트입니다.

* [ICML format report](/assets/pdf/PHYRE_report.pdf)
* [presentation slide](/assets/pdf/PHYRE_presentation)

### Abstract
사람은 처음 본 상황에서도 자신이 어떤 행동을 해야 하는지 알 수 있다. 이전의 경험들로부터 **physical concept** 를 알아냈기 때문이다. 우리는 이러한 능력을 agent에게도 부여하고 싶었다. 그래서 Facebook에서 공개한 [PHYRE](https://phyre.ai/)를 사용하였다. 이 곳에는 물리학적 특성을 기반으로 한 퍼즐들이 존재한다. 우리의 목적은 agent가 **physical concept** 를 이해해서 학습시에 보지 못했던 task에서도 좋은 성능을 낼 수 있도록 하는 것이다.

### Method
[PHYRE paper](https://arxiv.org/abs/1908.05656)에 나와있는 기본 baseline들 중에서 DQN을 우리의 baseline으로 선택하였고, agent가 physical concept를 더 잘 이해할 수 있도록 하기 위해서 Q&A module을 추가하였다. Q&A module은 다음과 같다.  
![DLproject-01](/assets/img/project/DL/Q&Amodule.png)  
Question은 action을 하고 t만큼의 시간이 흘렀을 때 움직이는 3가지 공(빨강, 파랑, 초록)의 위치를 맞추게 하는 것이다. t는 random하게 input으로 들어가게 설정하였다. 기존 baseline에서 State와 Action을 [FiLM Network](https://arxiv.org/abs/1709.07871)을 통해서 joint embedding하였다. 그래서 Question의 embedding 또한 FiLM Network를 사용하였다.  

이번 project에서 내가 구현한 부분은 **THQAN(Two-Headed Question Answering Network)** 이다. 모델의 구성은 다음과 같다.  
![DLproject-02](/assets/img/project/DL/THQAN.png)  
THQAN는 Question의 대한 Answer로써 공의 위치를 맞추는 부분, 그리고 게임이 성공했는지 실패했는지 맞추는 부분 2가지로 나뉜다. Q&A module을 추가함으로써 State-Action joint embedding에 physical concept까지 표현되도록 하였다.  
첫번째 output으로 나오는 Answer부분을 Visualize 해본 결과 위치 예측을 잘 해내는 것을 알 수 있었다. 그러나 성능은 baseline과 큰 차이가 있지 않았다. 이것은 Q&A network에서 충분히 답을 낼 수 있을만큼 학습이 되어서 이전 layer로 정보가 전파가 되지 않는다고 생각하였다.

그래서 새롭게 구상한 모델은 같은 팀원 중 안수영군의 **Attention Question Answering Network(ATQAN)** 이다. 모델의 구성은 다음과 같다.
![DLproject-03](/assets/img/project/DL/ATQAN.png)  

이전 모델과 다른점은 Q&A Network로부터 나온 vector로 기존 state-action joint embedding에 attention을 해준다는 것이다. 이전 layer까지 전파되지 않던 문제점을 attention을 통해서 해결하고자 하였다. 이를 통해서 기존 baseline보다 높은 성능을 얻을 수 있었다.

<u>자세한 실험 결과와 디테일은 레포트를 참고 해주세요</u>

### 소감 및 마무리
같은 팀원 중 이호준군이 정리해준 baseline 코드 위에서 작업을 시작하였다. 내가 직접 구현한 부분은 visualizing, Answer Labels, THQAN이다. 아직 코드를 깔끔하고 효과적으로 짜지 못한다는 것을 많이 느낀 프로젝트였다. 하지만 이것보다 더 부족하다고 느낀 것은 글쓰기 실력이었다. 보고서를 ICML format으로 영어로 논문을 써서 제출하는 형식이었는데 팀 리더한테 4~5번은 피드백을 받고 고쳤던 것 같다. 부족한 점이 전공 지식 뿐만이 아니라 아주 많다는 것을 느낀 프로젝트였다. 그래도 좋은 성적과 결과를 얻어서 뿌듯하다!
