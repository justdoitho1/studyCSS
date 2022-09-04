# 왜 쓰는지에 집중하고 공부하세용

# CSS 레이아웃

- 어떤 것을 어느 위치에 배치하는지.
- Flex & Grid만으로도 대부분의 레이아웃 구성 가능
- 전통적인 방식(IE 기준)은 float 레이아웃을 사용한다.
- 특수한 레이아웃 : table, ruby => 다루지 않는다. 보편적으로 사용하지 않기 때문에.

# CSS 박스 모델

- 모델 : 여러 가지 속성이 합쳐진 개념. box 모델일 경우, box라는 속성이 있다는 게 아니라 추상화된 개념을 이야기함
- contents-box : 요소가 본인의 콘텐츠를 담고 있는 영역. 개발자도구 코드에서 각 요소에 마우스를 올렸을 때 나타나는 파란색 부분. 크기 : width \* height
- border-box : border까지 포함한 box. 크기 : (width + padding + border) \* (height + padding + border)
- margin : 박스모델에 포함이 돼있긴 하지만, 박스의 크기까지 포함돼 있지는 않음
- 박스의 구성
  - width(너비), height(높이). 아무런 설정을 하지 않으면 콘텐츠 박스를 기준으로 한다.
  - border : 박스의 외곽선
  - padding : border와 박스 내부까지의 여백 크기
  - margin : 박스와 다른 박스 사이의 간격
- 개발자도구의 요소 탭에서 박스 모델을 실제로 볼 수 있음

- overflow : 콘텐츠박스보다 콘텐츠의 크기가 커서 box에서 넘친 상황
- normal flow : 콘텐츠가 넘치지 않은 상황
- 여기서 얻는 교훈
  - 1. 콘텐츠 길이 알 수 없으면 height 지정하지 말자. 지정하지 않으면 알아서 늘어남. width는 보통 지정하는데, heigth를 지정하면 피를 보는 경우가 많음.
  - 2. height를 지정해야 하면, overflow 상황을 조정하자. overflow: auto; 또는 overflow:scroll 등을 할 수 있음.
  - 3. width도 가끔 넘친다...

### 박스 모델 심화 과정(inside box)

- box-sizing : 박스 사이즈 계산 방식 바꿀 수 있음.
- box-sizing: content-box(기본값)
- box-sizing: border-box(width, height 기준이 border로 바뀜)
- 장점 : 박스 크기 계산이 용이해짐
- 단점 : 잘못 쓰면 어디서는 content-box, 어디서는 border-box라서 혼합되기 시작하면 헷갈림.
- box-sizing 쓸 거면 와일드카드 셀렉터(\*)를 사용해서 모든 요소에서 같은 box를 사용하도록 권장.
- 유지보수하기 좋은 코드 : 혼란을 주지 않는 코드 !

* {
  box-sizing : border-box;
  }

### margin 동작

- margin도 normal flow일 때는 합쳐짐. 이런 현상을 margin collapse라고 함.
- 언제 생기나요?
  - 1. normal flow인 형제끼리 : 보통 의도해서 이렇게 겹치게 함
  - 2. normal flow인 부모자식끼리 : 보통 버그. 부모 요소에 padding:0.1px 을 주면 margin-collapse를 없앨 수 있다. 이건 약간꼼수.

# CSS Normal Flow

- Block, Inline 을 지칭함.
- Block : 박스 하나가 영역 한 줄을 다 차지하는 형태. 위에서 아래로 쌓인다. <div>, <p> 같은 태그를 선언하면, <div> 옆에 <p>가 오는 게 아닌, 아래에 쌓인다. 이렇게 한 영역을 다 차지할 때 사용되는 속성은 margin이다. width로 크기를 제어하면, 나머지는 margin으로 메꿈. 왜? margin은 박스 간의 간격이기 때문에!
- 대표적인 요소 : <div>
- Inline 형태 : 텍스트로 취급이 되는 구조. 한 줄 내에서 다른 텍스트와 함께 보임
- inline 요소, block 요소를 외울 필요는 없음. 개발자도구에서 display 보면 inline, block 여부를 알 수 있음.
- 특정한 요소가 박스 전체를 차지하는 거 같으면 대부분 block
- 특정한 요소가 텍스트처럼 취급되면 inline
- 만약 레아이웃을 적용해 레이아웃을 바꾸는 경우 NorMal Flow에 해당하지 않음. Flex Flow, Grid Flow, Position Flow랑은 별도임.

# CSS Position

- 배치의 시작. 어떤 요소를 어떠한 위치(좌표)에 배치하는 데 쓰이는 기능.
- CSS Position을 쓴다 = 좌표 시스템을 쓴다.
- 개발자도구에서 position을 체크하면 position이 보임.

- 웹은 3차원이다. (x, y, z) 우리가 보는 게 2차원일 뿐이다.

### Position 속성

- ex) h1{ position : relative; right : 20px; }
- 기본값 : static => 좌표 시스템 사용 x
- relative : 나 자신을 기준으로 움직이겠다. 내 인생은 나의 것, 내 주인은 나야! 박스 자체를 기준으로 x, y축을 이동한다. 근데 relative를 이용해서 x, y축으로 이동시키는 기능은 잘 활용하지 않는다. 왜냐하면 좌표 시스템은 다른 박스를 무시하는 경향이 있음. 그렇다고 relative를 안 쓰는 건 아님. 잘 쓰임.
- absolute : 자신을 기준으로 했을 때, 조상 요소 중 가장 가까운 좌표 시스템(position 속성이 있는 곳)을 쓰는 요소를 기준으로 좌표를 찍는다. 부모의 부모의 부모의 부모를 탈 때도 있음.

  - 해당 요소의 부모/조상 요소에 position을 relative로 지정해서 쓴다. 그러면 무조건 그 위치로 감. 만약 position요소가 없다면 html을 부모라 생각하고 움직임.
  - absolute 언제 쓰나? 아이콘이 특정 위치에 예쁘게 있어야 하거나, 특정한 위치에 레이어를 띄워야 할 때.
  - absolute는 normal flow를 탈출한다.

- fixed : 다 무시하고 브라우저 기준으로 좌표를 찍음. 그래서 스크롤을 해도 고정돼 있다. 어어엄청 자주 쓰인다. 챗봇 아이콘 같은 것도 그렇고.
- sticky : 특정한 요소가, 특정한 조건에 맞물렸을 때만 fixed처럼 동작하는 기능.

- js랑 css 중 어떤 걸로 구현하는 게 더 성능이 좋나요? ==> 대부분 JS를 쓰는 게 무조건 느림. CSS로 할 수 있는 건 CSS로 하자.
- CSS로도 할 수 있는데 JS로 하는 이유 : 우리 입맛에 맞춰서 바꾸고 싶을 때

### 좌표

- x 축 : left, right
- y 축 : top, bottom
- z 축 : z-index
- 팁 : z축 관리를 잘해야. 잘 못하면, 뒤에 있어야 할 놈이 앞으로 나오고 막 그럼..
  - Styled Component : 따로 JS에서 상수로 관리
  - css : 문서로 관리. 내가 잘해도 다른 개발자가 잘못 넣으면 문제 발생.
  - 보통 5 단위로 쓰고 있음. 일반적인 경우 5씩 추가.
  - 큰 레이아웃 : 100씩
  - 무조건 높아야 함 : 10000

질문 .

- sticky 조건을 어케 거는 거지 ?
- flow라는 용어는.. 캐스케이딩이라서 그런걸까나?
- margin collapse...
