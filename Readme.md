# 노마드코더 ReactJS로 영화 웹 서비스 만들기

## 리액트 탐구(2/26~)

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

**jsx에 변수 추가 및 옛날 재렌더링 방법**

```js
const root = document.getElementById('root');

let count = 0;
function countUp(){
    count = count + 1;
    render();
}
function render(){
    ReactDOM.render(<Container />, root); 
}

const Container = () => (
    <div>
        <h3>총 클릭 횟수 : {count}</h3>
        <button onClick = {countUp}>클릭 해줭</button>
    </div>
)
render()
```
**배운점**
1. 리액트를 배우기 전에는 html을 먼저 작성하고 데이터 넣는 부분에 자바스크립트로 이벤트를 더해주고 UI를 업데이트 하는 방식이었으나, 리액트는 jsx로 html 구조와 비슷하게 만들고 데이터를 넣어서 변경해 줘야 할 구간에 변수를 넣을 수 있음

2. 리액트의 장점은 모든 부분이 아닌 데이터가 바뀐 부분만 재렌더링 하게 해줌

**useState를 이용한 재렌더링 방식**

```js
const root = document.getElementById('root');
const App = () => {
    const [counter, setCounter] = React.useState(0);
    const onClick = () => {
        setCounter(counter + 1);
    }
    return (
        <div>
        <h3>총 클릭 횟수 : {counter}</h3>
        <button onClick={onClick}>클릭 해줭</button>
    </div>
    );
}
ReactDOM.render(<App />, root); 
```

**배운점**
1. useState를 안쓰고 똑같은 효과를 배우기 위해서는 렌더 자체를 함수로 만들어서 데이터가 바뀌는 이벤트 함수에 렌더 함수를 넣어주는 것으로 변화를 일으킨 반면에, useState를 이용하여 state를 바꾸면 리액트가 컴포넌트를 refresh 해줌

2.state를 세팅하는 데는 2가지 방법
* 직접 할당 :setState(state +1)
* 함수를 할당:setState(state => state +1) (함수의 첫번째 인자는 현재 state)

현재 state랑 관련이 없는 값을 새로운 state로 하고싶은 경우에는 직접 할당,
현재 state에 조금의 변화를 주어서 새로운 state를 주고 싶은 경우에는 함수를 할당


**input에 useState 사용**


```js
const root = document.getElementById('root');
const App = () => {
    const [minutes, setMinutes] = React.useState();
    const onChange = (e) => {
        //input의 value 값을 가져옴
        setMinutes(e.target.value)
    }
    const reset = () => setMinutes(0);
    return (
        <div>
            <div>
                <h1>Super Converter</h1>
                <label htmlFor="minutes">분</label>
                <input value={minutes} id="minutes" placeholder="분" type="number" onChange={onChange} />
            </div>
            <div>
                <label htmlFor="hours">시간</label>
                <input value={Math.round(minutes/60)} id="hours" placeholder="시간" type="number" disabled/>
            </div>
            <button onClick={reset}>reset</button>
        </div>
    );
}
ReactDOM.render(<App />, root); 
```

**배운점**
1. useState를 이용하여 input의 value값을 onchange 속성으로 쉽게 받아옴
2. Math.round() 속성을 이용하면 소수점 반올림 하여 정수로 변환
3. disabled 속성을 이용하면 input에 값 못 넣게 할 수 있음

**input에 useState 사용2**

```js
const root = document.getElementById('root');
const App = () => {
    const [amount, setAmount] = React.useState(0);
    const [flipped, setFlipped] = React.useState(false);

    const onChange = (e) => {
        setAmount(e.target.value)
    }
    const reset = () => setAmount(0);
    const onFlip = () => {
        reset();
        //current는 이벤트가 달린 요소를 가리킴
        setFlipped((current) => !current);
    }
    return (
        <div>
            <div>
                <h1>Super Converter</h1>
                <label htmlFor="minutes">분</label>
                <input value={flipped ? amount *60 : amount} id="minutes" placeholder="분" type="number" onChange={onChange} disabled={flipped} />
            </div>
            <div>
                <label htmlFor="hours">시간</label>
                <input value={flipped ? amount : Math.round(amount/60)} id="hours" placeholder="시간" type="number"  disabled={!flipped}/>
            </div>
            <button onClick={reset}>reset</button>
            <button onClick={onFlip}>Flip</button>
        </div>
    ) ;
}
ReactDOM.render(<App />, root); 
```
**배운점**
1. 삼항연산자를 이용하여 if 문을 간단하게 구현함 <br>
형태는 `(조건) ? true에 나오는 형태 : false에 나오는 형태`로 되 있음
2. const onFlip = () => setFlipped(!flipped);
-> flipped가 true라면 부정명제인 !flipped는 false
-> false라면 true <br>
state값으로 input을 enabled할지 disabled 할지를 결정할 수 있음