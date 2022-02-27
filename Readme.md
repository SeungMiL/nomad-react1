# 노마드코더 ReactJS로 영화 웹 서비스 만들기

## 리액트 탐구(2/26)

리액트가 왜 유용한가를 배우는 시간을 가짐.

```html
    <span>클릭 횟수 : 0</span>
    <button id="btn">Click me</button>

    <script>
        let counter = 0;
        let span = document.querySelector('span');
        const button = document.getElementById("btn");
        function handleClick(){
            counter = counter + 1;
            span.innerText = `클릭 횟수 : ${counter}`;
        }
        button.addEventListener('click',handleClick)
    </script>
```
총 클릭 횟수를 세주는 간단한 코드를 리액트로 바꾸면 어떻게 바뀌는지 변경해 가며 설명함.

리액트로 변경 후 코드
```html
    <div id="root"></div>

    <script src="https://unpkg.com/react@17/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@17/umd/react-dom.production.min.js"></script>
    <script>
        const root = document.getElementById('root');
        const h3 = React.createElement('h3',  {
            id : 'title',
            onMouseEnter : () => console.log('마우스엔터')
        }, '제일 멋있는 스팬');
        //두번째 프로퍼티에 스타일 추가도 가능
        const btn = React.createElement('button', {
            onClick : () => console.log('클릭클릭'),
            style : {backgroundColor : 'tomato'}
        }, '클릭 해줭');
        // 두번째 프로퍼티 위치 : class, id, event 같은게 들어감 아무것도 없으면 null
        const container = React.createElement('div', null, [h3, btn])
        ReactDOM.render(container, root); 
    </script>
```
**배운점**
1. React Js는 인터랙티브의 원동력, ReactDOM은 React element를 가져다가 HTML로 바꾸는 역할

1. 기존의 html에서 js로의 변경점을 주며 작업하는 방식에서
리액트는 js에서 html로 작업방향이 다른것이 흥미로웠음.

2. 기존 html은 데이터 바인딩할시 ejs 파일로 변환하는 등의 작업을 거치는 등 애로사항이 있는데 js에서 시작하다보니 데이터바인딩이 더 쉬워보임

3. 수많은 addEventListener 써야했던 것을 프로퍼티에서 event를 등록 할 수 있음


**h3, btn 을 jsx 형식으로 바꿨을 때**

```js
const title = <h3 id='title' onMouseEnter={()=> console.log('마우스엔터')}>제일 멋있는 스팬</h3>

const Button = <button  onClick={() => console.log('클릭클릭')} style={{ backgroundColor: 'tomato' }} >클릭해줭
</button>
```

**배운점**
1. jsx는 자바스크립트를 확장한 문법. html에서 사용한 문법과 매우 유사한 문법 사용. jsx 사용시 위 예시처럼 createElement 사용 X.

2. jsx를 사용하기 위해서는 babel을 이용해야 함. <br>
※ babel 자바스크립트 컴파일러(변환기)

**jsx 형식 컴포넌트 화**

```js
const root = document.getElementById('root');

function Title(){return <h3 id='title' onMouseEnter={ () => console.log('마우스엔터')}>제일 멋있는 스팬</h3>};

const Button = () => ( 
    <button  onClick={ () => console.log('클릭클릭')} style={{backgroundColor: 'tomato' }} >클릭해줭
    </button>);

const Container = () => (
        <div>
            <Title /> 
            <Button />
        </div>
        )
ReactDOM.render(<Container />, root); 
```

**배운점**
1. 컴포넌트화 된 변수들은 무조건 대문자를 사용해야함

2. function 함수명(){}을 쓴 경우에는 중괄호 안에`{return}` 문이 들어가야함

3. 화살표 함수의 경우 괄호()로 감싸진 부분이 return 된다(return문을 작성하지 않아도 return 됨).
<br>반면에 중괄호{}로 감싸진 다음과 같은 함수는 return문이 없다면 return 값을 반환하지 않는다.