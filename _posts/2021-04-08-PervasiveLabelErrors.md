---
title:  "Pervasive Label Errors in Test Sets Destabilize Machine Learning Benchmarks"
categories: update
---

ImageNet과 같은 Dataset의 labeling은 Crowdsourcing 등을 통해 이루어지는데, 이는 label 에러를 유발할 위험이 있다. 기존 연구는 training set의 label 에러에 대한 연구였다면, 이 연구는 test set의 label 에러에 대한 연구이다. 


이 저자가 이전에 연구했던 confident learning을 이용해서 test set의 label error를 automatically 파악하고 human evaluation을 통해 검증하니 test set에 3.4%, validation set에 6%의 error가 문제가 있었다.


![1](https://user-images.githubusercontent.com/46548053/114011417-2b164080-98a0-11eb-9c29-d4013750ccae.PNG)


Correctable set은 여러명의 haman evaluator들이 모두 consensus에 도달한 set인데 ImageNet Validation set과 Correctable set의 경향성이 다르게 나타난다. 기존 val set에서 높은 성능을 보여준 NASnet의 성능은 저조한 반면 가장 높은 성능을 보여준 모델은 ResNet-18(?!)이다


ResNet-18의 성능이 높은 이유에 대해서 저자들은 low-capacity가 Regularization 효과를 주기 때문이라고 분석했다. 저자들은 전체 test set에서 Correctable set의 비중을 높여가면서 Accuracy의 경향성을 실험했는데 Correctable set의 비중이 올라갈수록 low-capacity 모델의 성능이 올라간다.


![2](https://user-images.githubusercontent.com/46548053/114011421-2c476d80-98a0-11eb-8bb2-c368658a9de3.PNG)


Noise Prevalence = % of test set with correctable labels
Pytorch 1.0의 Resnet50과 Keras 2.2.4의 Resnet50의 성능이 다른 것은 의문이다.

내 생각
---
최근 Andrew NG 교수님의 MLOps 관련 강의도 그렇고 OpenAI의 연구(CLIP, DALL-E 등)들도 그렇고 딥러닝에서 데이터를 잘 다루는 것의 중요성이 부각되고 있다. 나도 연구를 하면서 느끼는 부분이 데이터를 잘 처리하는게 굉장히 중요하다는 점이다. 이 연구도 앞선 논의의 연장선상에서 해석될 수 있다. 적절한 test set을 설정하지 않는다면 모델 선택 등에서 잘못된 결정을 내릴 수 있어서 공을 들여야 한다.
