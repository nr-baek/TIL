
# word-break속성

[YouTube 빔캠프 VEAMCAMP채널](https://www.youtube.com/c/veamcamp/featured)_CSS 갈증해소 프로젝트 OACSS 시리즈의 문단 줄바꿈 강좌를 보며 정리한 글입니다.

> css의 word-break 속성은 문자가 내용 밖으로 벗어날 경우, 이를 줄바꿈할 것인지에 대한 여부를 설정하는 속성이다. 속성값에 따라 줄바꿈은 단어기준이 되거나 글자기준이 된다.

![textbox](https://images.velog.io/images/ursr0706/post/44fd4cdb-7e7e-4b5c-9f0d-565bcd939c30/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-30%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.24.17.png)

영어 텍스트와 한글 텍스트를 넣은 상자가 있다.  
텍스트가 담긴 상자의 영역에 맞게 텍스트가 자연스럽게 줄바꿈이 된 것을 볼 수 있다.
그런데 자세히 보면 영어는 어절이 온전히 유지되며 줄바꿈이 되었지만 한국어는 '숲을'의 '숲'다음 줄바꿈이 일어나 어절이 끊겨있다.  
왜 이런 현상이 나타났을까?  

## word-break속성 기본값 normal

```css
word-break: normal;
```

word-break속성의 기본값은 normal이다.
그런데 위의 그림에서 보면 영어 text와 한글 text의 줄바꿈이 일어나는 기준이 다른듯 하다.  

![word-break_normal](https://images.velog.io/images/ursr0706/post/c0024a2a-979a-428f-bf47-61159ea6bbac/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-30%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.26.26.png)

container의 너비를 줄여봤더니 영어와 한국어의 줄바꿈 기준에 대한 차이가 더 잘 보인다.  
`word-break`속성의 값이 둘 다 `normal`이지만 둘은 다르게 표현이 된다.  

## word-break: break-all

break-all속성값은 어절이 유지되지 않고 끊어져서 줄바꿈된다.  
한글text의 경우 word-break속성이 normal상태에서는 break-all이 적용되는데,
한글뿐만 아니라 한자나 일본어에서도 따로 word-break값을 주지 않았을 때 break-all이 기본상태이다.  

![break-all](https://images.velog.io/images/ursr0706/post/52fd7c33-b922-410c-b59e-4b80fe277153/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-30%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.16.16.png)

container클래스에 word-break값을 break-all을 주었더니, 한글text는 그대로이지만 영어text는 유지되어 있던 어절이 끊기며 줄바꿈 된 것을 볼 수 있다.

## word-break: keep-all

keep-all속성값은 어절을 유지하며 줄바꿈을 하며 영어text의 normal상태와 같다.

![keep-all](https://images.velog.io/images/ursr0706/post/f09e9fd7-0d51-46a0-a763-5c5584528a14/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-30%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.36.17.png)

container클래스에 word-break값을 keep-all을 주면 어절이 끊기지않고 유지되며 줄바꿈 되는 것을 볼 수 있다.

### word-break 속성값 선택

![break and keep](https://images.velog.io/images/ursr0706/post/decf6573-d912-42c5-9713-cf88f56cb8c1/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-30%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.27.44.png)

**break-all**값은 어절이 끊기면서 줄바꿈이 일어나기 때문에 읽을 때 불편할 수 있다.  
하지만 영역을 최대한 채우며 줄바꿈이 생겨서 네모꼴이 유지되기 때문에 시각적으로 안정감을 준다.  

**keep-all**값은 어절을 유지하며 줄바꿈이 일어나기 때문에 상대적으로 텍스트를 읽기 쉽다.  
하지만 어절을 유지하기 위해 영역을 채우기 전 줄바꿈되는 단어들이 있어서 들락날락한 모습이 나타난다.

반응형웹을 만들 때 화면이 늘어나고 줄어듬에 따라 줄바꿈 형태를 어떻게 할지 선택하면 된다.

>참고영상 - [반응형웹에서 문단 줄바꿈이 마음에 들지 않다면? | CSS 갈증해소 프로젝트 OACSS | 빔캠프](https://youtu.be/t_J5s3Fs13A)
