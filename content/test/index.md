---
emoji: ๐
title: React/ state ์ฌ๋ฐ๋ฅด๊ฒ ์ฌ์ฉํ๊ธฐ
date: '2021-10-22 10:23:00'
author: ์๋ค์
tags: useState
categories: React
---

# state ์ฌ๋ฐ๋ฅด๊ฒ ์ฌ์ฉํ๊ธฐ: 3์์น

## 01 state๋ฅผ ์ง์  ์์ ํ์ง ์์ต๋๋ค
this.state๋ฅผ ์ง์ ํ  ์ ์๋ ๊ณต๊ฐ์ constructor ๋ฟ์๋๋ค.(์ด๊ธฐ ์ธํ)

```
    class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

ํด๋์ค ์ปดํฌ๋ํธ๋ ํญ์ props๋ก ๊ธฐ๋ณธ constructor๋ฅผ ํธ์ถํด์ผ ํฉ๋๋ค.

๋ค์๊ณผ ๊ฐ์ด ์ฐ์ฌ์ง ์ฝ๋๋ ๋ฆฌ๋ ๋๋ง ๋์ง ์์ ์ ์์ต๋๋ค.
```
// Wrong
this.state.comment = 'Hello';
```
๋์ ์, setState๋ฅผ ์ฌ์ฉํด์ผ ํฉ๋๋ค.
```
// Correct
this.setState({comment: 'Hello'});
```

<br>

## 02 state ์๋ฐ์ดํธ๋ ๋น๋๊ธฐ์ ์ผ ์ ์์ต๋๋ค
๋ค์๊ณผ ๊ฐ์ด props์ ๊ธฐ์กด state๋ฅผ ํ๊บผ๋ฒ์ ์ฒ๋ฆฌํ๋ ๊ฒฝ์ฐ๊ฐ ์์ต๋๋ค.

```
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
```

this.props์ this.state๋ ๋น๋๊ธฐ์ ์ผ๋ก ์๋ฐ์ดํธ ๋  ์ ์๊ธฐ ๋๋ฌธ์ ๊ฐ์ฒด ํ์์ด ์๋ ํจ์๋ฅผ ์ธ์๋ก ์ฌ์ฉํ๋ ํํ๋ฅผ ์ฌ์ฉํฉ๋๋ค.

```
// Correct
// ํ์ดํ ํจ์
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));

// ์ผ๋ฐ ํจ์
this.setState(function(state, props) {
  return {
    counter: state.counter + props.increment
  };
});
```

<br>

## 03 state์ ์๋ฐ์ดํธ๋ ๋ณํฉ๋ฉ๋๋ค

state๋ ๊ฐ์ฒดํํ๋ก ์ง์ ์ด ๊ฐ๋ฅํฉ๋๋ค. 
```
 constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
    };
  }
```

๋ค์ํ ๋๋ฆฝ์ ์ธ ๋ณ์๋ฅผ ํฌํจํ  ์ ์๋๋ฐ, ์ด๋ฌํ ๋ณ์๋ค์ ๋ ๋๋ฆฝ์ ์ผ๋ก ์๋ฐ์ดํธ ํ  ์ ์์ต๋๋ค.

```
 componentDidMount() {
    // posts๋ง ์๋ฐ์ดํธ
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    // comments๋ง ์๋ฐ์ดํธ
    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }
```

this.setState({comments})๋ฅผ ์งํํ  ๋ this.state.posts์ ์ํฅ์ ์ฃผ์ง ์์ผ๋ฉฐ, 
๋ณํฉ์ ์๊ฒ ์ด๋ฃจ์ด์ง๋ฏ๋ก this.state.comments๋ ์์ ํ ๊ต์ฒด๋๋ค.

(๊น์ ๋ณํฉ deep merge, ์์ ๋ณํฉ shallow merge ์ฐธ๊ณ : https://velog.io/@jonmad/JS-Object-Deep-Merging-Shallow-Merging)