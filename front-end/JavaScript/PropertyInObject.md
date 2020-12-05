# JavaScript - Check if the property exists in an object

reference: [Medium article](https://medium.com/javascript-in-plain-english/4-ways-to-check-whether-the-property-exists-in-a-javascript-object-20c2d96d8f6e)

아래와 같은 객체가 있다고 가정하자. 나이는 신경쓰지말자 =)
```javascript
const introduction = {
  firstName: 'David',
  lastName: 'Kim',
  age: 30,
  occupation: 'FE developer'

if (introduction.firstName !== 'undefined'){
  console.log('property exists =)') // "property exists =)"
} else {
  console.log('property does not exist =(')
}
```

### 1st). `hasOwnProperty()` 메서드를 사용
> boolean 을 반환한다.

```javascript
if (introduction.hasOwnProperty('lastName')){ // true
  console.log('property exists =)') // "property exists =)"
} else {
  console.log('property does not exist =(')
}
```
이 메서드를 사용해서 조금 더 확실한 체크를 하고 싶으면 이렇게 쓰면 될거같다.
```javascript
if (introduction.hasOwnProperty('lastName' && introduction.lastName === 'Park')){
  console.log('property exists =)')
} else {
  console.log('property does not exist =(') // "property does not exist =("
}
```

### 2nd). `in` 오퍼레이터 사용
> boolean 을 반환한다.

```javascript
if ('age' in introduction){ // true
  console.log('property exists =)') // "property exists =)"
} else {
  console.log('property does not exist =(')
}
```

### 3rd). `typeof` 사용하여 `undefined` 와 비교
> 만약 property기 존재하지 않으면 undefined를 반환한다.

```javascript
if (typeof introduction.occupation !== 'undefined'){ // true
  console.log('property exists =)') // "property exists =)"
} else {
  console.log('property does not exist =(')
}
```
이렇게 `typeof` 를 사용해서 `undefined` 인지 아닌지를 확인한다.

### 4th). `!!`(double bang) 오퍼레이터 사용
> 모든 값에는 true/false가 연결되있다. null 은 false, 'abc' 는 true 같이 되어있다. 여기에 !를 사용하면 반대가 되지만 !!를 사용하면 truthy value를 확인할 수 있다. (ex. `!!null` -> false)

```javascript
if (!!introduction.age){ // true
  console.log('property exists =)') // "property exists =)"
} else {
  console.log('property does not exist =(')
}
```