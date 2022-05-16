## Abstract
* prior : 단어를 통해 CLIP image embedding 생성
* decoder : embedding -> 이미지
    * 다양하게 생성하게 해줌
    * joint shot 방식으로 언어안에서 이미지 조작 가능하게 해줌



## Method
* pair (x,y) : x : 이미지, y : caption
* x -> zi & zt   (zi : CLIP image, zt : text embedding)
    * prior : P(zi|y) : y -> zi
    * decoder : P(x|zi,y) : zi,y -> x
    * P(x|y) = P(x,zi|y) = P(x|zi,y)P(zi|y)


#### Decoder
* diffusion 모델로 CLIP image embedding 생성 (Nichol et al. (2021) 사용)
 













## 단어
* leverage : 활용하다
* augressive : 자기 회귀
* diffusion : 확산
