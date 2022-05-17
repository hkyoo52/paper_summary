## Abstract
* prior : 단어를 통해 CLIP image embedding 생성
* decoder : embedding -> 이미지
    * 다양하게 생성하게 해줌
    * joint shot 방식으로 언어안에서 이미지 조작 가능하게 해줌



## Method
* pair (x,y) : x : 이미지, y : caption
* x -> zi & zt   (zi : CLIP image embedding, zt : text embedding)
    * prior : P(zi|y) : y -> zi
    * decoder : P(x|zi,y) : zi,y -> x
    * P(x|y) = P(x,zi|y) = P(x|zi,y)P(zi|y)


#### Decoder
* diffusion 모델로 CLIP image embedding -> image (Nichol et al. (2021) 사용)
* embedding을 GLIDE encoder에 넣음 -> diffusion 모델이 CLIP에서 capture 못한 자연어 관련 학습
* 고해상도 학습 (일부로 해상도를 낮추고 높은 해상도 나오게 학습시킴)
      * gaussian blur
      * diverse BSR degradation
      * inference는 목표 해상도에 바로 적용 (1024 * 1024)

#### Prior

* captions y -> zi, 아래 2개를 다 사용, text conditioning 중 일부 버림 등을 사용해서 성능 높임
    - Autoregressive prior : zi는 sequence, y는 자동 회귀
    - Diffusion prior : 가우시안 모델 사용

* AR
   - CLIP image embedding zi -> image x
   - PCA (319차원으로 만듬)
   - SAM 
   - quantize(양자화 319차원 -> 1024차원)
   
* DR
   - Transformer : 결과의 순서 예측 -> noise 안된 CLIP image embedding(unnoised zi) 예측
   - AR에서의 zi, zt와 다르게 여기서는 zi 2개 생성 & zt와 내적해서 더 높은거 하나 선택

* unnoised zi 예측

![image](https://user-images.githubusercontent.com/63588046/168708164-3a7ee31d-aede-4049-8691-c01b85a2c4a4.png)



## Image Manipulation
* encode로 x -> latent representation(zi,xT),      DDIM 사용
* zi는 CLIP을 설명, xT : x를 재생성하기 위한 필요한 정보

#### Variation
* DDIM에서 n이라는 변수 사용, n=0일 경우 reconstruct img는 원본하고 동일
* n은 이미지를 x를 중심으로 연속적으로 변화게 해줌
* n이 커지면 CLIP image embedding 정보를 많이 말함

#### Interpolation
* 이미지 x1, x2를 섞고 싶음
* spherical interpolation 를 사용 -> CLIP representations : ziΘ = slerp(zi1, zi2, Θ)
* DDIM latent 생성 2가지 방법
   * xT1, xT2 interpolation (xTΘ = slerp(xT1, xT2, Θ))
   * x1과 x2 사이의 random (origin image랑 동일하지 않게 됨)




## 단어
* leverage : 활용하다
* augressive : 자기 회귀
* diffusion : 확산
* bipartite : 이분적인
* variation : 변화
* stochasticity : 확률
* perceptually : 지각
* interpolation : 보간
* traverse : 탐색하다


