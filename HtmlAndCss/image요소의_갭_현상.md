# 이미지요소 하단에는 왜 작은 갭이 생길까

블록요소 안에 이미지요소를 넣을 때 이미지의 밑에 여백처럼 작은 갭이 생기는 것을 볼 수 있다.  
왜 생기는 것이며 갭을 없애는 방법을 알아보자.  

![틈이생긴image](https://images.velog.io/images/ursr0706/post/5a3fd814-e687-48d2-b611-c587be534c40/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-22%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2012.15.41.png)

위의 그림은 부모요소의 height값이 auto인 상태로 안에 image가 자식요소로 들어가있다.  
그런데 화면을 출력해보면 이미지의 밑에 작은 틈이 생겨있는것이 보인다.  
이미지요소에 margin을 준 것도 아니고, 부모요소에 padding값을 준 것도 아니다.  
또한 이미지요소에 border-bottom값이 들어가있는 것도 아니다.  
밑에 이 작은 틈은 왜 생긴 것일까?  
결론부터 이야기 하자면 **image요소는 inline레벨요소이기 때문**이다.

## inline레벨요소의 특성

***inline레벨 요소는 text와 동급이다.***  

![image옆 텍스트 추가](https://images.velog.io/images/ursr0706/post/5b5d6c9d-68d0-4b2c-9d99-2a1683c43f09/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-22%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2012.32.45.png)

```html
<div class="container">
    <img src="./bg-swallow1.jpeg" alt="swallow" />
    dog cat pig
</div>
```

image요소 다음에 "dog cat pig"라는 텍스트를 적어봤다.  
그러면 이미지와 텍스트가 한줄로 나오는데,  
이미지와 텍스트는 모두 inline요소이기 때문에 **baseline**을 기준으로 배치된다.  
이미지나 텍스트뿐만 아니라 모든 **inline요소는 baseline을 기준으로 배치**가 된다.  
인라인요소의 배치에 기준이 되는 `vertical-align`속성은 기본값으로 `baseline`이 지정되어있는데 때문에 작은 갭이 생기는 것이다.  

### baseline _ inline레벨 요소

![gap at the between baseline and descent](https://images.velog.io/images/ursr0706/post/9ac02971-c425-4914-8425-97463cafad3d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-22%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2012.32.45.png)

영어 소문자에는 한글과는 다르게 g, j, p, q, y와 같이 글자의 끝이 기준선 밑으로 내려가는 글자(decender)가 있는데 그 부분을 표현하기위해 baseline은 밑에 약간의 공간을 두고있다.  
그 공간이 gap을 유발한다.  

## Gap을 없애는 방법

![baseline](https://images.velog.io/images/ursr0706/post/e92ce792-0935-4687-9e1a-ab39b4659b08/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-22%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2011.38.07.png)

img와 text가 나란히 있다.  
앞서 살펴봤듯이 inline레벨 요소는 baseline을 기준으로 배치되며 img의 경우 baseline의 밑 공간 때문에 Gap이 생긴 것 처럼 보인다.  
이 때, 밑 부분의 Gap을 없애기 위한 방법들을 살펴보자.
(text의 영역이 좀 더 잘 보이게 하기 위해 tomato배경색을 넣었다.)

### 1. display값을 block으로 설정한다

요소의 `display`값이 더 이상 inline이나 inline-block이 아니게 하는 단순한 방법이다.  

![block_img](https://images.velog.io/images/ursr0706/post/0a1ef4f6-8639-4a48-b51a-3393f34fb000/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-22%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.06.45.png)

먼저 글자 없이 img요소만 놓고 display값을 block으로 설정한 결과이다.  
원래 이미지 하단에 있던 작은 갭이 없어졌다.  
img가 더 이상 text와 같은 inline요소가 아니게 되면서 baseline위에 놓이지 않게 되어 Gap이 사라진다.  
위의 그림을 보면 이미지요소에 `display:block`을 적용하여 밑의 갭을 없앤 것을 볼 수 있다.  

![display_block_img](https://images.velog.io/images/ursr0706/post/a19780bd-c375-4439-ba75-ba59a263b116/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-22%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2011.45.39.png)

text와 함께 놓으면 이렇게 text가 img요소 바로 밑에 놓여지게 된다.
img요소가 block레벨 요소가 되면서 라인을 모두 차지하게 되어 밑으로 내려간 것이다.  
Gap을 없앨 손쉬운 방법이긴 하지만 text가 밑으로 밀려났듯이, 만약 img요소에 inline레벨로서의 다른 css작업을 한 상태라면 `display:block`을 적용하는것은 문제가 될 수 있다.

### 2. vertical-align속성의 값을 변경한다

#### vertical-align: top

```css
img {
vertical-align: top;
}
```

![vertical_align_top_img](https://images.velog.io/images/ursr0706/post/0d5c40f3-825f-4006-8665-7074e3358bdb/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-22%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2011.26.03.png)

기본값이 `baseline`으로 되어있는 `vertical-align`속성의 값을 `top`으로 입력한 상태다.  
하단에 있던 갭이 사라진 것을 볼 수 있다.  
이는 기본적으로 적용되어있는 line-height안의 top-line과 이미지의 윗부분 끝이 맞춰진것이라 보면 된다.  
(위의 예시에 경우 img요소의 크기는 200px이며 기본 font-size는 16px로 img가 font-size보다 훨씬 커서 baseline 밑의 공간을 덮어쓴 샘이다.)

![vertical_align_top_img_with_text](https://images.velog.io/images/ursr0706/post/78a59f5f-3d6f-437b-8d71-eb70e288eb50/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-23%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%209.10.01.png)

text와 함께 놓일 땐 이렇게 아래에 있던 틈이 없어지면서 이미지의 오른쪽 위로 텍스트가 올라온 모습으로 보이는데,  
이것은 사실 이미지에 `vertical-align: top;`이 설정되면서 기존에 baseline 위에 위치하던 이미지가 요소의 상단부분이 text의 top-line이 맞게 위치가 내려간 것이다.  
때문에 이미지의 아래 여백이 사라지고 텍스트는 위로 올라온것 같은 모양이 되는것이다.  

#### vertical-align: bottom

마찬가지로 기준을 `bottom`으로 주면 text의 bottom에 위치하게 된다.  

```css
img {
vertical-align: bottom;
}
```

![vertical_align_bottom_img_with_text](https://images.velog.io/images/ursr0706/post/0d44d3f5-5077-4c44-9bd9-8ab3407cef63/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-23%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%209.22.26.png)

이것 또한 이미지의 `vertical-align: bottom;`이 설정되면서 이미지 하단부분이 text의 bottom-line에 위치하면서 밑의 여백이 사라진 것이다.  

#### vertical-align: middle

마지막으로 이미지에 `vertical-align: middle;`이 적용된 예시이다.  

```css
img {
vertical-align: middle;
}
```

![vertical_align_middle_img_with_text](https://images.velog.io/images/ursr0706/post/fae4d437-1bed-42e6-a198-1aa8a77f646e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-23%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%209.24.13.png)

텍스트의 middle line을 기준으로 설정되면서 이미지의 중앙부분이 text의 middle라인에 위치하면서 밑의 여백은 사라지고 text가 중간으로 올라온 것 같은 모습이 된다.
