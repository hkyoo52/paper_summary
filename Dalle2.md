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
* embedding을 GLIDE encoder에 넣음 -> diffusion 모델이 CLIP에서 capture 못한 자연어 관련 학습
* 고해상도 학습 (일부로 해상도를 낮추고 높은 해상도 나오게 학습시킴)
      * gaussian blur
      * diverse BSR degradation
      * inference는 목표 해상도에 바로 적용 (1024 * 1024)

#### Prior
* captions y -> zi, 아래 2개를 다 사용, text conditioning 중 일부 버림 등을 사용해서 성능 높임
      * Autoregressive prior : zi는 sequence, y는 자동 회귀
      * Diffusion prior : 가우시안 모델 사용
* CLIP image embedding zi -> image x
* PCA (319차원으로 만듬)
* SAM 
* quantize(양자화 319차원 -> 1024차원)
* Transformer : 결과의 순서 예측











## 단어
* leverage : 활용하다
* augressive : 자기 회귀
* diffusion : 확산
