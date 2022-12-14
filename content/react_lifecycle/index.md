---
emoji: ๐
title: React/ React Lifecycle ์ ๋ณตํ๊ธฐ
date: '2021-10-22 10:23:00'
author: ์๋ค์
tags: lifecycle
categories: React
---

# Intro...
๋ฆฌ์กํธ์ lifecycle์ ์ธ ๋จ๊ณ๋ก ๊ตฌ๋ถํ  ์ ์๋ค.
<figure>
<img src="./lifecycle.png" />

<figcaption>์ถ์ฒ: https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/
</figcaption>
</caption>
</figure>

<br>

`Mounting -> Updating -> Unmounting`
๋ฆฌ์กํธ์ ๊ฐ ์ปดํฌ๋ํธ๋ค์ ๋ชจ๋ ์์ ๋ผ์ดํ์ฌ์ดํด์ ๋ฐ๋ฅธ๋ค. ์ฐ๋ฆฌ๋ `lifecycle method`๋ฅผ ์ด์ฉํ์ฌ ๋ผ์ดํ์ฌ์ดํด์ ํน์  ๋จ๊ณ์์ ์ํ๋ ์ฝ๋๋ฅผ ์คํํ๋๋ก ํ  ์ ์๋ค.

- `Mounting`๋ ๋์ ์์๋ค์ ๋ฃ๋ ๋จ๊ณ๋ค.
- `Updating`๋ ๋ง ๊ทธ๋๋ก ์ปดํฌ๋ํธ๊ฐ ์๋ฐ์ดํธ ๋๋ ๋จ๊ณ๋ค.
- `Unmounting`๋ ๋์์ ์ปดํฌ๋ํธ๊ฐ ์ ๊ฑฐ๋๋ ๋จ๊ณ๋ค.

์์ธํ ์ดํด๋ณด์.

# Mounting
์ปดํฌ๋ํธ์ ์ธ์คํด์ค๊ฐ ์์ฑ์ด ๋๊ณ  ๋์ ์ฝ์๋  ๋, ์๋์ `lifecycle method`๋ค์ ๋ค์๊ณผ ๊ฐ์ ์์๋ก ํธ์ถ์ด ๋๋ค.

(๊ตต์ ๊ธ์จ์ ํจ์๊ฐ ์์ฃผ ์ฐ์ด๋ ํจ์๋ค.)
<ol>
<li><b>constructor()</b></li>
<li>static getDerivedStateFromProps()</li>
<li><b>render()</b></li>
<li><b>componentDidMount()</b></li>
</ol>

> โ ๏ธ์ฃผ์<br>UNSAFE_componentWillMount() ํจ์๋ legacy code๋ก ์ฌ๊ฒจ์ง๊ณ  ์๊ธฐ ๋๋ฌธ์ ์ง์๋๋ค.

render()ํจ์๋ ํ์๊ณ  ํญ์ ํธ์ถ๋๋ฉฐ, ๋๋จธ์ง ํจ์๋ค์ ์ ํ์ฌํญ์ด๋ค.

## constructor() : mount ์ ์ ํธ์ถ
- constuctor()ํจ์๋ ์ปดํฌ๋ํธ๊ฐ ์์ฑ๋  ๋ ์ ์ผ ๋จผ์  ํธ์ถ๋๋ค.

- constructor()๋ฉ์๋๋ ์ธ์์ธ props์ ํจ๊ป ํธ์ถ๋๊ธฐ ๋๋ฌธ์ ๋ด๋ถ์ ์ ์ผ ๋จผ์  `super(props)`๋ฅผ ์ ์ธํด์ฃผ์ด์ผ ํ๋ค. `super(props)`๋ ๋ถ๋ชจ ์ปดํฌ๋ํธ์ constructor method๋ฅผ ์์ฑํด์ฃผ๊ณ , ์ปดํฌ๋ํธ๊ฐ ๋ถ๋ชจ ์ปดํฌ๋ํธ๋ก๋ถํฐ ์์๋ฐ๊ฒ ํด์ค๋ค. 

- ๋ํ์ ์ผ๋ก ์๋ ๋ ๊ฐ์ง์ ๋ชฉ์ ์ผ๋ก ์ฐ์ธ๋ค. 
    - ์ด๊ธฐ local state, ์ด๊ธฐ value์ ์ธํ
        - state๋ ๊ฐ์ฒด๋ก ํ ๋น์ด ๋๋ค. 
    - ์ธ์คํด์ค์ event handler๋ฅผ binding

- constructor()์์์ ์ด๊ธฐ ๊ฐ์ ์ธํํ๊ฑฐ๋ event handler๋ฅผ bindํ  ๋, setStateํจ์๋ฅผ ์ฌ์ฉํ์ง ์๊ณ  ์ง์  ํ ๋น์ ํ๋ค.

    state๋ฅผ ์ง์  ํ ๋นํ  ์ ์๋ ์ ์ผํ ๊ณณ์ด๋ค.
    ```
    constructor(props) {
    super(props);
    // Don't call this.setState() here!
    this.state = { counter: 0 };
    this.handleClick = this.handleClick.bind(this);
    }
    ```
- local state๊ฐ ํ์์๊ฑฐ๋ method๋ฅผ bindํ  ํ์ ์๋ค๋ฉด constructor()๋ฅผ ๊ตฌํ ํ  ํ์๊ฐ ์๋ค. (optional)
- constructor()์์์ side-effects๋ฅผ ๋ฐ์์ํค๊ฑฐ๋ subscriptions์ ํ์ง ์๋๋ค. (componentDidMount()์์ ์งํํ๋ค)

```
constructor(props) {
 super(props);
 // Don't do this!
 this.state = { color: props.color };
}
```
- ์ ์ฝ๋์ ์๋ชป๋ ์  
    - props.color๋ฅผ ์ง์  this.props.color๋ก ์ฌ์ฉํ๋ฉด ๋๋ค.
    - props๊ฐ state์ ๋ฐ์๋๊ธฐ ์ ์ color๋ฅผ updateํ๋ ค๊ณ  ํด ๋ฒ๊ทธ๊ฐ ๋๋ค.
    - update๋์ง ์๋ color์ ์ด๊ธฐ๊ฐ์ ํ ๋นํ๊ณ  ์ถ์ ๋๋ง, intialColor ๋ฑ์ ์ด๋ฆ์ผ๋ก ์ฌ์ฉํ๋ค. ์ด๊ธฐ๊ฐ์ผ๋ก resetํ  ๋ ์ฌ์ฉ.

## componentDidMount() : mount๋ ์งํ
- tree์ ์ปดํฌ๋ํธ๊ฐ ์ฝ์๋์๋ง์ ์ฆ, mount๋์๋ง์ ํธ์ถ๋๋ค.
- ๋ฐ์ดํฐ๋ฅผ ๋ก๋ํด์ผ ๋๋ค๋ฉด ๋คํธ์ํฌ ์์ฒญํ๊ธฐ ์ข์ ๊ณณ์ด๋ค.
- ๊ตฌ๋ํ๊ธฐ๋ ์ข์ ๊ณณ์ด๋ค. ๊ตฌ๋ํ์ผ๋ฉด componentWillUnmount()์์ ๊ตฌ๋ํด์ ํ๋ ๊ฒ๋ ์์ง ๋ง์!
- ์ฌ๊ธฐ์ setState()ํ๊ฒ ๋๋ฉด ๋ ๋๋ง์ ๋ ๋ฒ ์์ฒญํ๊ฒ ๋๋๋ผ๋ ์์ง ๋ธ๋ผ์ฐ์ ๋ ์คํฌ๋ฆฐ์ ์๋ฐ์ดํธ ํ๊ธฐ ์ ์ด๋ฏ๋ก ์ฌ์ฉ์๋ ์ฆ์ ์๋ฐ์ดํธ ๋ ํ์ด์ง๋ฅผ ๋ณด๊ฒ ๋๋ค.

# Updating
์ปดํฌ๋ํธ๋ ์ปดํฌ๋ํธ์ `state`๋ `props`์ ๋ณํ๊ฐ ์๊ธธ ๋ ์๋ฐ์ดํธ ๋๋ค.

๋ค์๊ณผ ๊ฐ์ ์์๋ก ์๋ฐ์ดํธ ๋๋ค

<ol>
<li>getDerivedStateFromProps()</li>
<li>shouldComponentUpdate()</li>
<li>render()</li>
<li>getSnapshotBeforeUpdate()</li>
<li>componentDidUpdate()</li>
</ol>

๋ง์ฐฌ๊ฐ์ง๋ก render()๋ ํ์์ ์ผ๋ก ์๊ตฌ๋๋ฉฐ ๋๋จธ์ง ํจ์๋ค์ optional์ด๋ค.

## [rarely used] static getDerivedStateFromProps
- ์๋ฐ์ดํธ ์ ์ต์ด๋ก ํธ์ถ๋๋ ํจ์๋ค.
- ์๋ฐ์ดํธ ๋ ๊ฐ์ฒด๋ฅผ ๋ฐํํ๊ฑฐ๋ ์๋ฐ์ดํธ ๋ ๋ถ๋ถ์ด ์์ผ๋ฉด null์ ๋ฐํํ๋ค. 
- initial props๋ฅผ state์ ์ธํํ๊ธฐ ์ข์ ์ฅ์๋ค.
```
class Header extends React.Component {
  constructor(props) {
    super(props);
    this.state = {favoritecolor: "red"};
  }
  static getDerivedStateFromProps(props, state) {
    return {favoritecolor: props.favcol };
  }
  changeColor = () => {
    this.setState({favoritecolor: "blue"});
  }
  render() {
    return (
      <div>
      <h1>My Favorite Color is {this.state.favoritecolor}</h1>
      <button type="button" onClick={this.changeColor}>Change color</button>
      </div>
    );
  }
}

ReactDOM.render(<Header favcol="yellow"/>, document.getElementById('root'));
```
๋ฒํผ์ ๋๋ฅด๋ฉด changeColorํจ์๊ฐ ํธ์ถ๋๋ฉด์ favoritecolor๊ฐ blue๋ก ๋ณ๊ฒฝ๋๋ ์๋ฐ์ดํธ๊ฐ ์ผ์ด๋๊ณ , ๊ทธ ์งํ getDerivedStateFromPropsํจ์๊ฐ ํธ์ถ๋๊ธฐ ๋๋ฌธ์ favoritecolor๋ props๋ก ๋ฐ์ yellow๋ก ๋ ๋๋ง ๋๋ค.
- props๋ฅผ ๋ฐ์์ ๋ฐ์ดํฐ ํ์นญํ๊ฑฐ๋ ์ ๋๋ฉ์ด์ ํจ๊ณผ๋ฅผ ์ฃผ๋ ๋ฑ์ side effect๋ฅผ ์ํ๋ค๋ฉด componentDidUpdate()์์ ํ๋ ๊ฒ ์ข๋ค.
## [rarely used] shouldComponentUpdate()
- shouldComponentUpdate(nextProps, nextState)

- ๋ฆฌ์กํธ๊ฐ ๋ ๋๋ง์ ๊ณ์ ํด์ผ ํ๋์ง์ ์ฌ๋ถ๋ฅผ boolean๊ฐ์ผ๋ก ๋ฐํํ๋ค.
- ๋ ๊ตฌ์ฒด์ ์ผ๋ก ๋งํ๋ฉด ์ปดํฌ๋ํธ์ ์ถ๋ ฅ(ํ์ด์ง)์ด ํ์ฌ state๋ props์ ๋ณํ์ ์ํฅ์ ๋ฐ์๋์ง ์ฌ๋ถ๋ค.
- ๊ธฐ๋ณธ์ ์ผ๋ก๋ ๋ชจ๋  state๊ฐ ๋ณํ  ๋๋ง๋ค ๋ฆฌ๋ ๋๊ฐ ์ผ์ด๋๋ค.
- ์๋ก์ด props๋ state๋ฅผ ๋ฐ์์ ๋ ๋ ๋๋ง ์ ์ ํจ์๊ฐ ํธ์ถ์ด ๋๋ฉฐ, ์ด๊ธฐ๋ ๋๋ง์ด๋ forceUpdate()์์๋ ํธ์ถ๋์ง ์๋๋ค. 
- ๋ ๋๋ง์ ๋ฐฉ์งํ๊ธฐ ์ํด ์ฐ๋ฉด ๋ฒ๊ทธ๋ฅผ ์ผ์ผํฌ ์ ์๊ณ , ์ค์ง ์ฑ๋ฅ ์ต์ ํ๋ฅผ ์ํด ์ด๋ค.
- ์ง์  ์ด ๋ฉ์๋๋ฅผ ์์ฑํ๊ธฐ๋ณด๋ค ๋ด์ฅ๋ pureComponent(์์ ๋น๊ต๋ก shouldComponentUpdate()๊ฐ ๊ตฌํ๋๊ณ  ํ์ํ ์๋ฐ์ดํธ๋ฅผ ๋ฐ์ด๋์ ํ๋ฅ ์ ์ค์ฌ์ค)๋ฅผ ์ฐ๋ ๊ฑธ ์ถ์ฒํ๋ค.
- ์ด ๋ฉ์๋๊ฐ false๋ฅผ ๋ฐํํ๋ฉด ๋ค์ UNSAFE_componentWillUpdate(), render(), and componentDidUpdate()๋ ํธ์ถ๋์ง ์๋๋ค.



## render()
- ์๋ฐ์ดํธ ์ ์๋ก์ด ๋ณ๊ฒฝ์ฌํญ๊ณผ ํจ๊ป HTML์ DOM์ ๋ค์ ๋ ๋๋ง ํฉ๋๋ค.

## [rarely used] getSnapshotBeforeUpdate
- ์๋ฐ์ดํธ ์ ์ props์ state ๊ฐ์ ์ ๊ทผํ  ์ ์๋ ํจ์์๋๋ค.
- ๋ง์ธ ์ฆ์จ, ์๋ฐ์ดํธ๊ฐ ์ผ์ด๋ ํ์๋ ์๋ฐ์ดํธ ์ ์ ๊ฐ์ ํ์ธํ  ์ ์์ต๋๋ค.
- componentDidUpdate()์ ๊ฐ์ด ์จ์ผ ํฉ๋๋ค. ์ ๊ทธ๋ผ ์๋ฌ ๋ฐ์!
- ์๋์ ๊ฐ์ด ์ฌ์ฉํฉ๋๋ค.
```
class Header extends React.Component {
  constructor(props) {
    super(props);
    this.state = {favoritecolor: "red"};
  }
  componentDidMount() {
    setTimeout(() => {
      this.setState({favoritecolor: "yellow"})
    }, 1000)
  }
  getSnapshotBeforeUpdate(prevProps, prevState) {
    document.getElementById("div1").innerHTML =
    "Before the update, the favorite was " + prevState.favoritecolor;
  }
  componentDidUpdate() {
    document.getElementById("div2").innerHTML =
    "The updated favorite is " + this.state.favoritecolor;
  }
  render() {
    return (
      <div>
        <h1>My Favorite Color is {this.state.favoritecolor}</h1>
        <div id="div1"></div>
        <div id="div2"></div>
      </div>
    );
  }
}

ReactDOM.render(<Header />, document.getElementById('root'));
```

## componentDidUpdate()
- componentDidUpdate(prevProps, prevState, snapshot)
- DOM์ update๊ฐ ๋์ ๋ง์ ํธ์ถ๋๋ค. ์ต์ด ๋ ๋๋ง ์์๋ ํธ์ถX
- ์ด๋ค ๊ฐ์ ๋ณํ์ ๋ฐ๋ผ ๋คํธ์ํฌ ์์ฒญ์ ํด์ผํ  ๋ ์ด๋ค.
```
componentDidUpdate(prevProps) {
  // Typical usage (don't forget to compare props):
  if (this.props.userID !== prevProps.userID) {
    this.fetchData(this.props.userID);
  }
}
```
- ์ฌ๊ธฐ์ setState๋ฅผ ํ  ๋, ๊ผญ ์กฐ๊ฑด๋ฌธ์ผ๋ก ๊ฐ์ธ์ผ ํ๋ค. ์๊ทธ๋ผ ๋ฌดํ ๋ฃจํ์ ๋น ์ง๋ค.
- ๋ ์ถ๊ฐ์ ์ผ๋ก ๋ฆฌ๋ ๋๋ง์ ๋ฐ์์ํค๊ธฐ ๋๋ฌธ์ ์ ์ ์๊ฒ ์ํฅ์ ์๋๋ผ๋ ํผํฌ๋จผ์ค์ ์ํฅ์ ์ค ์ ์๋ค.
- shouldComponentUpdate() ๊ฐ false๋ฅผ ๋ฐํํ๋ฉด ํธ์ถ๋์ง ์๋๋ค.

# Unmounting
์์์ ๋งํ๋ฏ์ด, DOM์ ์ปดํฌ๋ํธ๊ฐ ์ ๊ฑฐ๋๋ ๋จ๊ณ์๋๋ค.

๋ง์ดํธ๊ฐ ํด์ ๋  ๋ ํธ์ถ๋  ์ ์๋ ๋ฉ์๋๊ฐ componentWillUnmount ํ๋์๋๋ค.

## componentWillUnmount
- ์ปดํฌ๋ํธ๊ฐ DOM์์ ์ ๊ฑฐ๋๋ ค๊ณ  ํ๊ธฐ ์ง์ ์ ํธ์ถ๋ฉ๋๋ค.
- ํ์ด๋จธ๋ฅผ ๋ฌดํจํ ํ๊ฑฐ๋ ๋คํธ์ํฌ ์์ฒญ์ ์ทจ์ํ๊ฑฐ๋ ๋๋, componentDidMount()์์ ๋ง๋  ๊ตฌ๋์ ํด์ ํ๋ ๋ฑ์ ํ์์ ์ธ cleanup์ ์ฌ๊ธฐ์ ๊ตฌํํ๋ฉด ๋ฉ๋๋ค.
- ์ฌ๊ธฐ์ setState๋ ํ์ง ๋ง์ธ์. ๋ฆฌ๋ ๋๋ง ์ ๋ ์๋ฉ๋๋ค. ์ปดํฌ๋ํธ ์ธ์คํด์ค๊ฐ ํ ๋ฒ ์ธ๋ง์ดํธ ๋๋ฉด ๋ค์๋ ๋ง์ดํธ๋์ง ์์ต๋๋ค.
