---
title:  "Revisiting ResNets: Improved Training and Scaling Strategies"
categories: update
---

Vision model의 성능은 the architecture, training methods and scaling strategy의 조합이다. 하지만 새로운 architecture + 새로운 training methods and scaling strategy vs 기존 architecture + 기존 training methods and scaling strategy으로 비교되는 경우가 종종 있다. 이 논문에서는 training methods and scaling strategy의 영향을 비교해보고자 한다.

Architecture
---
ResNet-D와 Squeeze-and-Excitation Network를 사용. 
ResNet-D는 기존 ResNet에서 
1. Stem을 7x7 convolution에서 3개의 3x3 convolution으로
2. 첫 2개의 convolution의 stride를 바꿈
3. stride 2의 1x1 convolution을 stride 2의 2x2 average pooling + 1x1 convolution
4. stride 2 3x3 max pooling을 제거하고 다음 BottleNeck의 3x3 convolution에서 downsampling
으로 바꾸었다.

Training Methods
---
EfficientNet이랑 비슷하지만 조금 바뀌었다.
1. cosine learning schedule
2. RandAugment
3. Momentum Optimizer
를 사용했다는 차이가 있다.

실험결과
---
![캡처1](https://user-images.githubusercontent.com/46548053/111450300-99f5f300-8753-11eb-833a-900ced9c8415.PNG)


여러 training methods를 추가하니 Top-1 Accuracy가 79.0%에서 82.2%로 상승했다. 추가적으로 Squeeze-and-Excitation, ResNet-D를 추가하니 1.2% 상승했다.


![캡처2](https://user-images.githubusercontent.com/46548053/111450316-9c584d00-8753-11eb-9342-9ab6fe40b5af.PNG)


흥미로운 점은 weight decay를 낮추니 성능이 향상된 점이다. 초기 weight decay 값은 1e-4였는데 초기값을 유지한 상태에서 여러 regularization을 추가하니 over-regularization 되었다. Weight decay 값을 낮추니 over-regularization이 해결된 것.


Scaling Strategies
---
FLOPs do not accurately predict performance in the bounded data regime
작은 모델에서는 FLOPs와 error간의 power law가 성립했는데 모델 사이즈가 커짐에 따라 그 관계가 깨졌다. 같은 FLOPs라도 depth, width, resolution에 따라 error가 많은 차이가 났다.


![캡처3](https://user-images.githubusercontent.com/46548053/111450320-9d897a00-8753-11eb-99bb-3602b4b886a2.PNG)

 
Depth Scaling vs Width Scaling
---
적은 epoch에서는 width를 늘리는 것이 효과적이었고 많은 epoch에서는 depth를 늘리는 것이 합리적이었다. 저자는 overfitting이 일어날 것 같으면 depth를, 아니면 width를 늘리라고 한다.
Slow Image Resolution Scaling
기존 EfficientNet에 비해 Image Resolution Scaling 천천히 해야 성능이 좋다는 것을 발견했다. 


새로운 Scaling Strategy를 적용한 ResNets-RS를 EfficientNet과 비교해보았다.

FLOPs vs Latency
---
기존 연구들은 computation에 대한 metric으로 FLOPs를 사용했지만 실제 환경에서는 많은 문제가 있다. Memory access costs나 modern matrix multiplication의 최적화 정도에 따라 많은 속도 차이가 난다. 예를 들어 Inverted BottleNeck의 경우 depthwise convolutions을 사용하는데 memory당 computation이 작아 실제 환경에서는 efficient하지 않았다. 


![캡처4](https://user-images.githubusercontent.com/46548053/111450335-a0846a80-8753-11eb-97d9-80ee9798517a.PNG)


Parameters vs Memory
---
마찬가지로 parameters수는 실제 Memory 사용량과 다른데 Memory 사용량은 activation의 크기에도 영향을 받기 때문이다. Activation이 큰 EfficientNet(large image resolution)은 parameter 수에 비해 많은 Memory를 사용하게 된다.


![캡처5](https://user-images.githubusercontent.com/46548053/111450346-a37f5b00-8753-11eb-8757-e44b2b81b7cf.PNG)

 

