최근 transformer 기반의 vision model이 많이 나오고 있지만 자연어에 비해 성능이 좋지 않다. 논문에서는 크게 두가지 문제가 있다고 지적한다.
1. visual element들은 크기가 다 다르다
2. NLP에서의 단어의 개수에 비해 pixel의 개수가 훨씬 많다.

1, 2번 문제를 해결하기 위해 제시한 것이 hierarchical representation이다. 처음에는 small patch로 시작해서 점진적으로 patch의 크기를 키운다. 


![1](https://user-images.githubusercontent.com/46548053/113129186-92574380-9255-11eb-8b93-ae316db58655.PNG)


hierarchical representation를 보면 Feature Pyramid와 형태가 유사하다는 것을 알 수 있는데 저자도 hierarchical representation를 이용하여 segmentation 등의 dense prediction을 수행할 수 있다고 주장한다. 


또한 중요한 technique이 sliding window댜. Window를 sliding시키지 않은 기존 방식의 경우 window간의 connection이 없어서 modeling power가 떨어진다. Sliding window를 통해 window간의 connection을 만들면 성능이 개선된다고 주장한다.
 

![2](https://user-images.githubusercontent.com/46548053/113129189-93887080-9255-11eb-8104-164d59e73eef.PNG)


![3](https://user-images.githubusercontent.com/46548053/113129193-94210700-9255-11eb-9b0b-adbf80b79928.PNG)


하나의 stage는 두개의 swin transformer block으로 이루어지고 한 block에서는 그냥 window, 다른 한 block에서는 swin block를 사용한다.

실험
---

Classification

ImageNet 1K에 대해서 실험함. ImageNet 22K에 대해서 pretrain한 모델과 안한 모델 두가지를 실험하였다. 


![4](https://user-images.githubusercontent.com/46548053/113129197-94210700-9255-11eb-8de0-be1a3c7ad573.PNG)


Swin-T, Swin-S, Swin-B, Swin-L을 학습시켰는데 채널 수, layer수에 따라 모델의 이름을 붙인 것이다.


![5](https://user-images.githubusercontent.com/46548053/113129200-94b99d80-9255-11eb-88ef-245330eda4da.PNG)


RegNetY, EfficientNet, ViT, DeiT과 비교했을 때 높은 성능을 보여주었다. 비슷한 FLOPs, parameter수에 대해서 높은 성능을 보여주었다.

Object Detection and Instance Segmentation


Object Detection and Instance Segmentation Task에 대해서 BackBone을 변경해가며 실험했다. Cascade Mask R-CNN, ATSS, RepPoints v2, and Sparse RCNN framework에 대해서 실험하였고 ResNext, DeiT와 비교하였다. 
 

![6](https://user-images.githubusercontent.com/46548053/113129201-94b99d80-9255-11eb-961c-4f5dd7916abb.PNG)


흥미로운 점은 기존 DeiT-S, R50에 비해서 꽤 많은 성능향상을 보여주었고 R50에 비해 FLOPs, FPS가 크게 작지는 않다. DeiT-S에 비해서는 속도, 성능이 모두 좋다. 
