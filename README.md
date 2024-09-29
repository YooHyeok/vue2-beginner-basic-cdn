# vue-begginer-basic-cdn

# data와 this
Vue 인스턴스에서 data 속성에 정의된 State(상태)는 Vue 인스턴스의 프로퍼티로 등록된다.  
this는 해당 Vue 인스턴스를 가리키며, 인스턴스의 모든 속성 및 메서드에 접근할 수 있다.  
- Vue는 내부적으로 data 속성에 정의된 모든 상태를 Vue 인스턴스의 프로퍼티로 프로토타입 체인을 통해 연결한다.  
Vue 인스턴스가 생성될 떄, data에 정의된 속성들을 Object.defineProperty 메소드로 getter와 setter을 설정하고,  
이를 Vue 인스턴스의 프로퍼티로 바인딩한다.  
- 즉 data 속성은 Vue 인스턴스의 직접적인 속성이 되며, this를 통해 접근할 수 있다.

```js
new Vue ({
  data: {
    msg: 'Hello Vue!',
  },
  methods: {
    callMsg() {
      console.log(this.msg);
    }
  }
})
```
this.msg 는 Vue 인스턴스의 msg 속성에 접근하는 방식이다.  
Vue는 msg를 this를 통해 접근할 수 있도록 Vue 인스턴스의 프로퍼티로 바인딩 해 둔다.  

- 명확한 인스턴스 참조  
javascript 객체 지향 모델에서 this는 객체의 맥락(context)를 나타낸다.  
Vue 컴포넌트는 Vue 인스턴스의 상태, 메소드, 속성 등을 하나의 객체로 관리하므로, this를 통해 그 객체의 속성에 접근하는 것이 자연스럽다.  
- Vue의 일관성  
data, methods, computed, watch 등 모든 옵션들이 Vue 인스턴스의 프로퍼티로 바인딩되므로, this를 통해 접근하게 하여 일관된 API 설계를 제공한다.

## 템플릿에서 this를 생략하는 이유
Vue 템플릿에서 this를 생략할 수 있는 이유는 Vue의 템플릿 컴파일러가 이를 자동으로 처리하기 떄문이다.  
- Vue 템플릿은 렌더링 단계에서 Javascript 함수로 컴파일 된다.  
이 함수가 생성될 때, Vue는 data, methods, computed 등 인스턴스의 모든 속성을 템플릿의 렌더링 컨텍스트로 포함시킨다.  
- 컴파일 된 템플릿 내부에서 모든 속성은 자동으로 Vue 인스턴스에서 바인딩된 상태를 참조하도록 변환되기 때문에, 템플릿에서는 this를 명시적으로 사용할 필요가 없다.  
Vue는 자동으로 각 속성이 this(Vue 인스턴스)를 가리키도록 설정해 준다.

```vue
<template>
  <div>{{ msg }}</div>
</template>
```

msg는 사실상 this.msg를 가리키지만, 템플릿에서는 이를 생략할 수 있다.

- 가독성  
템플릿에서 this를 생략함으로써 코드가 더 간결하고 직관적이게 된다.  
템플릿은 주로 UI 요소를 표현하기 위한 것이므로, this를 반복하는 것은 불필요한 중복으로 간주될 수 있습니다.
- 자동 컴파일  
Vue는 템플릿을 Javascript로 컴파일 할 때, 내부적으로 this를 자동으로 ㅊ마조하게 설정하므로, 개발자가 직접 this를 추가할 필요가 없다.  
이로 인해 템플릿에서의 코드가 더 깔끔해 진다.

### 내부적으로 변환된 렌더 함수 예시
Vue 템플릿은 내부적으로 아래와 같은 렌더 함수로 변환된다.
```js
render(h) {
  return h('div', this.msg);
}
```
템플릿에서 {{ msg }} 라고 작성하면, Vue는 컴파일 단계에서 자동으로 this.msg로 변환하여 렌더링 하는 함수로 변환한다.

#### 정리
- this로 접근하는 원리  
Vue 인스턴스가 생성될 때 data 속성은 인스턴스의 프로퍼티로 등록되며, this를 통해 인스턴스의 상태에 접근할 수 있게 된다.
- 템플릿에서 this 생략  
Vue 템플릿 컴파일러가 자동으로 this를 처리하기 때문에 템플릿에서 this를 명시하지 않아도 Vue 인스턴스의 속성에 접근할 수 있다.  
이는 가독성을 높이고 코드를 간결하게 만들기 위한 설계이다.