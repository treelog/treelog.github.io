TransUNet
=========

Motivation
----------

기존 Segmentation 모델은 CNN-based Approach를 사용해왔다 하지만 CNN이 가지고 있는 locality라는 특성은 long-range dependency를 모델링하는데 한계가 있다. 이는 환자간의 texture, shape, size가 차이가 있을 경우 낮은 성능으로 이어졌다.

이를 극복하기 위해 NLP에서 좋은 성능을 보여준 Transformer를 적용하는 시도가 있었지만 바로 적용하는 것은 문제가 있었다. Transformer는 1D Sequence을 input으로 받아 global context를 학습하는 데에만 특화되어있어 detailed localization information를 잃어버리는 문제가 있다. 이를 해결하기 위해 CNN-Transformer가 하이브리드하게 적용된 TransUNet을 제안한다.

Architecture
------------

![그림6](/assets/그림6.png) 
1) Image Sequentialization 
PxP 크기의 patch들을 sequence vector x_p로 만든다.

2) Patch Embedding 각 patch들을 linear projection한 후 positional embedding을 더한다. ![그림7](/assets/그림7.png) 그 후 Transformer를 통과시켜 embedding을 구한다 ![그림8](/assets/그림8.png)

3) CUP(Cascaded Upsampler): 기존 UNET에서 사용하는 Decoder Architecture와 거의 유사.

실험결과
--------

Synapse multi-organ segmentation dataset(CT), The ACDC challenge(MRI) 사용함. V-Net, U-Net, AttnUNet과 비교하였다.

![그림9](/assets/그림9.png) ![그림10](/assets/그림10.png)

여러 Encoder/Decoder 조합과 비교

ViT – None: Encoder는 ViT, Decoder는 없음 ViT – CUP: Decoder로 CUP를 사용 R50-ViT – CUP: Resnet50, ViT 하이브리드 Encoder 사용, Decoder로 CUP를 사용

실험 결과를 보면 하이브리드 Encoder, CUP를 추가함에 따라 성능이 올라가는 것을 확인할 수 있고 모두 사용했을 때 결과가 좋았다.
