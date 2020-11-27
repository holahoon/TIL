# React Context API

### What is Context API?

2020년 3월 리액트 16.3이 정식적으로 릴리즈되면서 기존에 존재하던 Context API가 새로워 졌다. 이 Context API는 전역적(global)으로 사용할 값을 관리 할 때 사용된다. 리액트의 특징상 parent component -> child component 방식으로 아래로 props를 전달하지만 Context API를 사용하게 되면 그럴 필요 없이 바로 가장 밑 하단에 있는 자녀 컴포넌트에게 직방으로 전달할수도, 또 다시 올릴수도 있는 편리함이 있다.

> 개인적으로 놀라운 점은 내가 자주 사용하던 react-router, -redux, 또 styled-component도 Context API 기반으로 구현 되있다고 한다. 

### Then, how the heck do you use it?

