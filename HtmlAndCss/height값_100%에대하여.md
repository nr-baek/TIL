[YouTube 빔캠프 VEAMCAMP채널](https://www.youtube.com/c/veamcamp/featured)_ CSS 갈증해소 프로젝트 OACSS 시리즈의 강좌를 보며 정리한 글입니다.

# 생각처럼 동작하지 않는 height: 100%

화면을 가득 채우고 싶은 요소에 `height:100%`를 주면 화면에 가득 채워질까?  
문제를 살펴보기 앞서, 먼저 height와 width값의 기본값을 알아야 한다.  

## height와 width속성의 default값

height와 width속성에 특정 단위로 값을 따로 설정하지 않는다면 두 속성의 값은 default값인 **auto**로 지정된다.  

```css
.pig {
  width: auto;
  height: auto;
}
```

auto값은 브라우저가 그 크기를 계산해준다.  

### display가 block일 때

block요소일 경우 `width: auto;`는 `width: 100%;`와 같이 해석 되어 사용가능한 최대 가로너비를 사용한다.  
(=요소의 부모요소 너비 기준으로 가득찬다)  
`height: auto;`는 높이값이 **0부터 시작**해서 필요한 만큼의 값을 갖도록 해석된다.  
(=요소의 자식높이 기준으로 자동 조절된다)  

### display가 inline일 때

inline요소일 경우 자식요소가 없을 때 `width: auto;`와 `height:auto;`는 둘 다 0과 같은데,  
inline요소는 크기를지정할 수 없고 필요한 만큼의 높이와 너비만 갖게 되어있기 때문이다.  

## div의 표현

![swallow](https://images.velog.io/images/ursr0706/post/ccb3c99a-d4ec-40d8-a3f2-3f8002bc2223/swallow.jpg)

위의 그림을 background-image로 넣은 div요소를 화면에 가득차게 하려한다.  

```html
<div class="image"></div>
```

```css
.image {
  background-image: url(./swallow.jpg);
 }
```

위의 상태에서는 당연히 width와 height값이 모두 auto이다.  
block요소의 특성 때문에 높이에 따로 값을 지정해주지 않은 default값인 경우 자식요소가 없다면 높이가 0이 된다.  

![text in element](https://images.velog.io/images/ursr0706/post/1e485454-9ce0-4b16-b6d8-8f9cb96a4aa4/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-08-15%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.48.00.png)

div요소 안에 text를 넣었더니 div가 text만큼의 높이값을 갖게 되어 그림의 일부가 보인다.  

### 높이를 화면에 꽉차게 만들기

div를 화면에 꽉 채워보기 위해  

```css
.image {
  background-image: url(./swallow.jpg);
  height: 100%;
 }
```

이렇게 `height:100%`값을 주었다.  

![text in element](https://images.velog.io/images/ursr0706/post/1e485454-9ce0-4b16-b6d8-8f9cb96a4aa4/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-08-15%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.48.00.png)

그러나 결과는 달라지지 않고 그대로 나타난다.  
height값을 100%로 준다는 것은 부모요소를 기준으로 100%만큼의 높이를 갖겠다는 것인데, 현재 .image요소의 부모요소는 body요소이므로 body요소의 높이가 화면을 꽉 채우지 않았다는게 된다.  
body또한 height값을 따로 지정하지 않았기 때문에 default값인 auto로 되어있으므로 자식요소인 .image요소만큼의 높이만 갖게 된다.  
그렇다면 body에 `height:100%`를 지정하면 어떨까?  

```css
body {
  height: 100%;
}

.image {
  background-image: url(./swallow.jpg);
  height: 100%;
 }
```

위와같이 .image요소에 `height: 100%;`, body요소에도 `height: 100%;`를 주었다.  

![text in element](https://images.velog.io/images/ursr0706/post/1e485454-9ce0-4b16-b6d8-8f9cb96a4aa4/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-08-15%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.48.00.png)

이번에도 기대와는 달리 화면을 꽉 채우지 않고 그대로이다.  
body는 body의 부모인 html을 기준으로 100%를 계산하기 때문이다.  
그렇다면 최상위 요소인 html에까지 `height: 100%;`를 설정해야 한다.  

```css
html, body {
  height: 100%;
}

.image {
  background-image: url(./swallow.jpg);
  height: 100%;
 }
```

![full](https://images.velog.io/images/ursr0706/post/2cd9f1c5-0018-4d98-b01d-a3b541cba44c/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-08-15%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.05.21.png)

이렇게 html요소의 height값까지 100%로 설정하고 나서야 비로소 .image의 크기가 화면에 꽉 찰 수 있게 되었다.  

### CSS3의 방법으로 화면에 꽉 채우기_vh단위 사용

```css
.image {
  background-image: url(./swallow.jpg);
  height: 100vh;
 }
 ```

vh단위는 viewport기준의 height비율이다.  
100vh를 하게되면 화면의 높이 100%를 의미한다.  

![full](https://images.velog.io/images/ursr0706/post/2cd9f1c5-0018-4d98-b01d-a3b541cba44c/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-08-15%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.05.21.png)

이렇게 하면 상위요소에 일일이 100%의 height를 줄 필요가 없다.  

> 참고영상 - [생각처럼 동작하지 않는 height: 100% | CSS 갈증해소 프로젝트 OACSS | 빔캠프](https://youtu.be/zBHxp3xKsF0)
