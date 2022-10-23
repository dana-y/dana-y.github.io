---
emoji: 😎
title: React/ state 올바르게 사용하기
date: '2021-10-22 10:23:00'
author: 양다은
tags: useState
categories: React
---

# state 올바르게 사용하기: 3원칙

## 01 state를 직접 수정하지 않습니다
this.state를 지정할 수 있는 공간은 constructor 뿐입니다.(초기 세팅)

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

클래스 컴포넌트는 항상 props로 기본 constructor를 호출해야 합니다.

다음과 같이 쓰여진 코드는 리렌더링 되지 않을 수 있습니다.
```
// Wrong
this.state.comment = 'Hello';
```
대신에, setState를 사용해야 합니다.
```
// Correct
this.setState({comment: 'Hello'});
```

<br>

## 02 state 업데이트는 비동기적일 수 있습니다
다음과 같이 props와 기존 state를 한꺼번에 처리하는 경우가 있습니다.

```
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
```

this.props와 this.state는 비동기적으로 업데이트 될 수 있기 때문에 객체 형식이 아닌 함수를 인자로 사용하는 형태를 사용합니다.

```
// Correct
// 화살표 함수
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));

// 일반 함수
this.setState(function(state, props) {
  return {
    counter: state.counter + props.increment
  };
});
```

<br>

## 03 state의 업데이트는 병합됩니다

state는 객체형태로 지정이 가능합니다. 
```
 constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
    };
  }
```

다양한 독립적인 변수를 포함할 수 있는데, 이러한 변수들을 또 독립적으로 업데이트 할 수 있습니다.

```
 componentDidMount() {
    // posts만 업데이트
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    // comments만 업데이트
    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }
```

this.setState({comments})를 진행할 때 this.state.posts에 영향을 주지 않으며, 
병합은 얕게 이루어지므로 this.state.comments는 완전히 교체된다.

(깊은 병합 deep merge, 얕은 병합 shallow merge 참고: https://velog.io/@jonmad/JS-Object-Deep-Merging-Shallow-Merging)