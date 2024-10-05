# Vue 2 기초

<details>
  <summary style="font-size:30px; font-weight:bold; font-style:italic;">
    data와 this
  </summary>
  
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

  <br>

<details>
  <summary style="font-size:20px; font-weight: bold;">
    <code>Context(문맥, 맥락) 이란?</code>
  </summary>

  Javscript에서 Context란 함수나 메소드가 호출될 때 그 안에서 this가 어떤 객체를 가리키는지를 결정하는 개념이다.  
  this는 현재 실행중인 함수가 "어떤 객체에 속해 있는지", 또는 그 함수가 호출된 방식에 따라 달라진다.

  1. 전역 컨텍스트에서 this  
    전역 범우에서 this는 전역 객체를 가리킨다.  
    브라우저 환경에서는 window 객체가 전역 객체이다.
      ```js
      console.log(this) // 전역에서 실행, 브라우저에서는 'window' 객체 출력
      ```

  2. 객체 메소드에서의 this  
      객체 메소드에서 this는 그 메소드가 속한 객체를 가리킨다.
      ```js
      const person = {
        name: 'YooHyeok',
        greet() {
          console.log(this.name); // 'this'는 person 객체를 가리킨다.
        }
      }
      person.greet(); // "YooHyeok" 출력
      ```
      위 코드에서 this는 person 객체의 맥락을 가리키며, person 객체의 속성인 name에 접근한다.

  3. **함수에서의 this  
    함수 내에서 this는 호출 방법에 따라 달라진다.  
    기본적으로 함수는 전역 컨텍스트에서 호출되며, this는 전역 객체를 가리킨다.
      ```js
      function func() {
        console.log(this); //전역 객체를 가리킨다. (브라우저에서는 `window`)
      }
      func(); // 전역에서 호출, 전역 객체 출력
      ```
  4. call이나 apply로 맥락을 명시적으로 설정  
    call()이나 apply() 메소드를 사용하면 함수를 호출할 때 this를 특정 객체로 지정할 수 있다.  
      ```js
      function greet() {
        console.log(this.name)
      }
      const person = {
        name: 'Bob'
      };

      greet.call(person) // `this`를 `person` 객체로 설정, "BOb" 출력
      ```
      초기 선언시점에서 greet 함수는 전역에 정의되어 있지만, call()을 사용해 this를 person 객체로 지정하였다.  
      따라서 greet 함수 내에서 this.name은 person을 참조하게 된다.  
  5. 생성자 함수에서의 this  
    생성자 함수에서는 this가 새로 생성된 객체를 기리킨다.  
      ```js
      function Person(name) {
        this.name = name;
      }

      const alice = new Person('Alice');
      console.log(alice.name); // "Alice" 출력
      ```
      this는 Person을 가리킨다.
</details>

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

  # *v-bind*
  Dom element 속성에 값을 바인딩 시켜준다.  
  `v-bind:속성=값` 혹은 축약형인 `:속성=값` 형태로 사용한다.  

  a 태그로 예시 코드를 작성한다.  
  ```js
  <body>
    <div id="app">
      <a v-bind:href="link"> {{ title }} </a>
      <a :href="link"> {{ title }} </a> <!-- 축약형 -->
    </div>
    <script>
      new Vue({
        el: '#app',
        data:{
          title: '유혁스쿨 티스토리 블로그',
          link: 'https://u-it.tistory.com',        
        },
      })
    </script>
  </body>
  ```
  <a href="https://u-it.tistory.com" >유혁스쿨 티스토리 블로그</a>

  ## Method 바인딩
  return 형태의 메소드 바인딩도 가능하다.
  ```js
  <body>
    <div id="app">
      <a :href="getYooHyeokSchoolLink('u-it')"> {{ title }} </a> <!-- 축약형 -->
    </div>
    <script>
      new Vue({
        el: '#app',
        data:{
          value: '유혁스쿨 티스토리 블로그',
          linkPrefix: 'https://',        
          linkSuffix: '.tistory.com',        
        },
        methods: {
            getYooHyeokSchoolLink(key) {
              return this.linkPrefix + key + this.linkSuffix;
            }
          }
      })
    </script>
  </body>
  ```

  ## Object 바인딩 (Attributes, Props)

  v-bind를 통해 Attribute 혹은 Props에 Object 형태로 바인딩이 가능하다.  
  *단, 축약형은 적용되지 않는다. (콘솔 Error 발생)*

  - ### Attributes
    ```js
    <!-- 생략 -->
    <body>
      <div id="app">
        <input v-bind="inputAttr">
      </div>
      <script>
        new Vue({
          el: '#app',
          data: {
            inputAttr: {
              type: 'number',
              value: '33',
            }
          },
        })
      </script>
    </body>
    ```

    브라우저 출력 결과 : `<input type="number" value="33">`

    이는 리액트에서도 spread attributes 문법을 통해 동일하게 적용된다.
    ```jsx
    import { useState } from 'react';

    function app() {
      const [inputAttr, setInputAttr] = useState({ type: 'number', value: '33' }) 
      return (
        <input {...inputAttr}>
      )
    }
    ```

    - ### Props
    Object 바인딩의 경우 커스텀 컴포넌트에 Props로 넘길수도 있다.  
    특징은, 부모 컴포넌트에서 v-bind 적용시 속성명을 입력하지 않을 경우 해당 Object의 각 property가  
    개별적으로 props로 넘어간다.

    ```js
    <body>
      <div id="app">
        <ExComponent v-bind="propsInputAttrProps">
      </div>
      <script>
        new Vue({
          el: '#app',
          data:{
            propsInputAttrProps: {
              type: 'number',
              value: '33',
            }
          },
        })
      </script>
    </body>
    ```

    ```js
    const html = String.raw; // 템플릿 구문강조 - Vue VSCode Snippets 플러그인 설치 후 사용

    Vue.component('ex-component', {
      template: html`
        <input v-bind:type="type" bind:value="value">{{ message }}</input>
      `,
      name: "ExComponent"
      props: {
        type: String,
        value: Number
      },
    });
    ```
    
    
    *주의할 점은 속성명과 props변수명이 일치한다고 하더라도 v-bind로 적용할때 속성명을 꼭 입력해줘야 한다.*  

    이는 리액트에서도 spread attributes 문법을 통해 동일하게 적용된다.
    ```jsx
    import { useState } from 'react';

    function app() {
      const [propsInputAttrProps, setPropsInputAttrProps] = useState({ type: 'number', value: '33' }) 
      return (
        <ExComponent {...propsInputAttrProps}>
      )
    }
    ```
    ```jsx
    function ExComponent({type, value}) {
      return (
        <input type={type} value={value}>
      )
    }
    ```
</details>


<details>
  <summary style="font-size:30px; font-weight:bold; font-style:italic;">
    Event

  </summary>

  `v-on:이벤트="메소드"` 혹은 `@이벤트="메소드` 와 같은 형태로 이벤트에 메소드를 바인딩 시켜 호출한다.


  주의할 점으로는 템플릿에서는 전역객체에 접근할 수 없다.
  예를들어 `window.alert()`, `window.confirm`, `console.log` 등이 있다.
  Vue의 템플릿 컴파일 과정에서 템플릿 내의 모든 표현식이 컴포넌트 인스턴스의 컨텍스트 내에서 평가된다.  
  (react와는 다르게 익명함수를 먼저 선언하고도, 전역객체에 접근할 수 없다.)  
  따라서, methods에 함수를 정의한 뒤 해당 함수를 통해 호출하도록 코드를 작성해야 한다.  

  ```js
  <body>
    <div id="app">
      <button type="alert">Alert!</button>
    </div>
    <script>
      new Vue({
        el: '#app',
        methods: {
          alert(msg) {
            alert(msg);
          },
        },
      })
    </script>
  </body>
  ```
</details>

<details>
  <summary style="font-size:30px; font-weight:bold; font-style:italic;">
    데이터 바인딩과 v-model
  </summary>
  
  # 단방향
  JavaScript → HTML 한 방향으로만 데이터를 동기화 하는 것을 의미한다.  
  value와 event를 함께 바인딩한다.  
  keyup 혹은 change 등의 이벤트 함수를 통해 target value에 접근하여 value에 바인딩한 변수를 초기화한다.  
  ```js
  <body>
    <div id="app">
      <input type="text" :value="onewWayBinding" v-on:keyup="updateText"> <br>
      {{ onewWayBinding }} <br>
    </div>
    <script>
      new Vue({
        el: '#app',
        data: {
          onewWayBinding: 'text',
        },
        methods: {
          updateText(e) {
            this.onewWayBinding = e.target.value
          },
        },
      })
    </script>
  </body>
  ```
  
  # 양방향
  JavaScript ↔ HTML 양쪽 방향으로 데이터를 동기화 하는 것을 의미한다.  
  JavaScript ↔ HTML 사이 ViewModel을 통해 하나로 묶여 바인딩 됨으로써, 둘 중 하나만 변경되어도 함께 변경된다.  
  단방향 에서 event와 같은 js 코드가 필요없이 ViewModel로 사용될 state 변수 하나만 사용한다.
  v-model 속성을 사용한다.  
  `v-model=state변수명`
  ```js
  <body>
    <div id="app">
      <input type="text" v-model="twowWayBinding"> <br>
      {{ twowWayBinding }} <br>
    </div>
    <script>
      new Vue({
        el: '#app',
        data: {
          twowWayBinding: 'text',
        },
      })
    </script>
  </body>
  ```
</details>
<details>
  <summary style="font-size:30px; font-weight:bold; font-style:italic;">
    computed 속성
  </summary>

  ```js
  <div>
    {{ number+1 }}
  </div>
  ```
  템플릿 내에 표현식을 넣으면 편리하다.  
  ```js
  <div>
    {{ message.split("").reverse().join('') }}
  </div>
  ```
  그러나 위와 같이 너무 많은 연산을 템플릿 내에서 하게 된다면 코드가 비대해지고 유지보수 하기 어려움이 있다.
  이때 computed 속성을 사용한다.  

  - computed 예제  
    computed 속성에 함수를 선언하고, state에 접근하여 데이터를 가공한 뒤 가공한 데이터를 반환한다.  
    이때, 함수명은 template에서 변수명으로 사용할 수 있게 된다.
    **주의할 점은 computed속성에 선언한 함수는 함수로서 호출할 수 없고 변수로써 사용한다.**
    ```js
    <body>
      <div id="app">
        {{ convertMsg }}
      </div>
      <script>
        new Vue({
          el: '#app',
          data: {
            computedMsg: 'Hello',
          },
          computed: {
            convertMsg() {
              return this.computedMsg.split("").reverse().join('')
            },
          },
        })
      </script>
    </body>
    ```
    Vue 인스턴스가 처음 생성될 때, mount 전 data속성이 정의된 computed속성이 정의된다. 또한, state의 변경을 감지한다. (state값이 변경되면 작동됨.)

    커스텀으로 getter와 setter를 제공하지만, 예제에서는 이를 하나의 메소드로 적용하였다.
    ```js
    computed: {
      convertMsg: {
        get() {
          console.log("get")
          return this.computedMsg
        },
        set(value) {
          console.log("set : ", value)
          this.computedMsg = value.split("").reverse().join('')
        },
      }
    },
    methods: {
      convertMsgF(newValue) {
        return this.convertMsg = newValue
      },
    }
    ```
    computed의 convertMsg의 변경이 감지되면 convertMsg의 convertMsg를 value로 받아온 뒤 state에 초기화 한다.
    즉, 특정 블록 내에서 computed속성에 정의한 변수(property)를 초기화 하는 로직이 작동 해야만 커스텀 set get 방식을 적용할 수 있게 된다.

    또한 computed를 통해 한번 계산된 데이터는 캐싱이라는 기능으로 가져다가 사용할 수 있으며,
    이로 인해 반복적인 함수 호출과 계산을 줄여준다

    ```html
    <body>
      <div id="app">
        {{ convertMsg() }}
        {{ convertMsg() }}
        {{ convertMsg() }}
        {{ convertMsg() }}
      </div>
      <script>
        new Vue({
          el: '#app',
          data: {
            computedMsg: 'Hello',
          },
          methods: {
            convertMsg() {
              return this.computedMsg.split("").reverse().join('')
            },
          },
        })
      </script>
    </body>
    ```
    위와 같이 메소드를 여러번 호출한다면, 호출할 때 마다 반환한다.

    ```html
    <body>
      <div id="app">
        {{ convertMsg }}
        {{ convertMsg }}
        {{ convertMsg }}
        {{ convertMsg }}
      </div>
      <script>
        new Vue({
          el: '#app',
          data: {
            computedMsg: 'Hello',
          },
          computed: {
            convertMsg() {
              return this.computedMsg.split("").reverse().join('')
            },
          },
        })
      </script>
    </body>
    ```
    그러나 computed는 접근한 data 변수가 변경되지 않는 이상 한번 연산된 결과값이 캐싱되어 출력된다.

</details>
<details>
  <summary style="font-size:30px; font-weight:bold; font-style:italic;">
    watch 속성
  </summary>

  관찰할 state를 등록한 뒤, 등록 된 state 상태가 변경되면 동작한다.  
  ```html
  <body>
    <div id="app">
      {{ convertMsg }} <br> <!-- 우로헬 -->
      oldVal : {{ oldVal}} <br> <!-- Hello -->
      newVal : {{ newVal}} <!-- 헬로우 -->
    </div>
    <script>
    new Vue({
      el: '#app',
      data: {
        newVal: '',
        oldVal: '',
        computedMsg: 'Hello'
      },
      watch: {
        computedMsg(newVal, oldVal) { // computed의 converMsg을 통해 수정함.
          this.newVal = newVal
          this.oldVal = oldVal
        }
      },
      computed: {
        convertMsg(e) {
          this.computedMsg = "헬로우" // 여기서 watch 대상을 수정함.
          return this.computedMsg.split("").reverse().join('')
        },
      },
    })
    </script>
  </body>
  ```

</details>
<details>
  <summary style="font-size:30px; font-weight:bold; font-style:italic;">
    class & style 속성 바인딩
  </summary>

  Object, Array 형태로 바인딩이 가능하며, 여러 형태의 조건부 바인딩을 지원한다. 
  
  # class 속성

  - #### Array 
    기본 형태는 여러개의 클래스를 배열 요소로 나열할 수 있다.  
    컴포넌트 내에서 따로 state로 관리할 수 있다는 장점이 있다.
    ```html
    <style>
      .red {color: red;}
      .font-bold {font-weight: bold;}
    </style>
    <body>
      <div id="app">
        <div 
          :class="['red', 'font-bold']"
        >
          Hello
        </div>
      </div>
      <script>
        new Vue({
          el: '#app',          
        })
      </script>
    </body>
    ```
    ```html
    <style>
      .red {color: red;}
      .font-bold {font-weight: bold;}
    </style>
    <body>
      <div id="app">
        <div 
          :class="clazz"
        >
          Hello
        </div>
      </div>
      <script>
        new Vue({
          el: '#app',
          data : {
            clazz: ['red', 'font-bold']
          }          
        })
      </script>
    </body>
    ```


  ## 조건부 바인딩

    - #### Object 
      ```html
      <style>
        .red {color: red;}
        .font-bold {font-weight: bold;}
      </style>
      <body>
        <div id="app">
          <div :class="{ red: isRed,
            'font-bold': isBold
          }">
            Hello
          </div>
        </div>
        <script>
          new Vue({
            el: '#app',
            data: {
              isRed: false,
              isBold: false,
            },
          })
        </script>
      </body>
      ```

  - #### Array 
    배열의 경우 3항 연산자 형태로 조건부 바인딩을 한다.
    ```html
    <style>
      .red {color: red;}
      .font-bold {font-weight: bold;}
    </style>
    <body>
      <div id="app">
        <div 
          :class="[ 
            isRed ? 'red' : '',
            isBold ? 'font-bold': ''
          ]"
          :class="[ 
            isRed && 'red',
            isBold && 'font-bold'
          ]"
        >
          Hello
        </div>
      </div>
      <script>
        new Vue({
          el: '#app',
          data: {
            isRed: false,
            isBold: false,
          },
        })
      </script>
    </body>
    ```

    && 혹은 || 연산을 통해 조건부 바인딩도 가능하다.
    ```html
    <style>
      .red {color: red;}
      .font-bold {font-weight: bold;}
    </style>
    <body>
      <div id="app">
        <div 
          :class="[ 
            isRed && 'red',
            !isBold || 'font-bold' /* !isBold가 거짓이면 적용 (isBold가 true)*/
          ]"
        >
          Hello
        </div>
      </div>
      <script>
        new Vue({
          el: '#app',
          data: {
            isRed: false,
            isBold: false,
          },
        })
      </script>
    </body>
    ```
    배열의 특징을 잘 활용한다면 Object 형태를 담을 수도 있다.
    ```html
    <style>
      .red {color: red;}
      .font-bold {font-weight: bold;}
      .back {background-color: blue;}
    </style>
    <body>
      <div id="app">
        <div 
          :class="[ 
            {back: isBack},
            isRed && 'red',
            !isBold || 'font-bold'
          ]"
        >
          Hello
        </div>
      </div>
      <script>
        new Vue({
          el: '#app',
          data: {
            isBack: true,
            isRed: false,
            isBold: false,
          },
        })
      </script>
    </body>
    ```
  # style 속성
  state로 관리가 가능하다.
  이때 주의할점은 `-` 하이픈이 들어간 style 속성의 경우 js 객체의 키 문법상 하이픈을 포함할 수 없으므로 카멜 표기법을 쓰거나, 'xxx-yyy' 형태와 같이 따옴표 등으로 묶어야 적용이 가능하다.  
  카멜 표기법은 react에서도 동일하게 적용하는것이 바로 위 이유이다.
  ```html
  <body>
      <div id="app">
        <div 
          :style="{
            color: color
            fontSize: `${fontSize} px` 
          }"
        >
          Hello
        </div>
      </div>
      <script>
        new Vue({
          el: '#app',
          data: {
            color: 'red',
            fontSize: 30
          },
        })
      </script>
    </body>
  ```
  위와같이 적용하면 인터렉티브한 UI 효과를 부여할 수 있다.  

  또한 객체 형태로도 관리할 수 있다.
  ```html
  <body>
      <div id="app">
        <div 
          :style="style"
        >
          Hello
        </div>
      </div>
      <script>
        new Vue({
          el: '#app',
          data: {
            style: {
              color: 'red',
              fontSize: '30px'
            },
          },
        })
      </script>
    </body>
  ```
</details>
<details>
  <summary style="font-size:30px; font-weight:bold; font-style:italic;">
    조건부 렌더링 v-if, v-show
  </summary>

  Vue2는 조건부 렌더링을 지원한다.  
  `v-if`와 `v-show` 속성을 사용한다.  
  v-if의 경우 컴포넌트 자체에 대한 렌더링을 결정하기 때문에, 렌더링 당시 혹은 값이 자주 변경되지 않는 경우에 사용을 권장하며,  
  v-show의 경우 기본 style="display:none" 속성을 제어하므로, 초기 렌더링시점에 무조건 렌더링되며, 값이 자주 변경되는 경우 사용을 권장한다.

  - #### v-if ~ v-else 구문
    ```html
    <body>
      <div id="app">
        <div v-if="show">YES</div>
        <div v-else>NO</div>
        <button @click="toggle">Toggle</button>
      </div>
      <script>
        new Vue({
          el: '#app',
          data: {
            show: true,
          },
          methods: {
            toggle() {
              this.show = !this.show;
            },
          }
        })
      </script>
    </body>
    ```
  - #### v-if ~ v-else-if ~ v-else 구문
    ```html
    <body>
      <div id="app">
        <template v-if="number === 1">
          <div>1</div>
          <div>2</div>
          <div>3</div>
        </template>
        <div v-else-if="number === 2">ELSEIF</div>
        <div v-else>ELSE</div>
        <button @click="increase">Increase</button>
      </div>
      <script>
        new Vue({
          el: '#app',
          data: {
            number: 1,
          },
          methods: {
            increase() {
              this.number ++;
            },
          }
        })
      </script>
    </body>
    ```

    <details>
      <summary style="font-size:30px; font-weight:bold; font-style:italic;">
        template 태그
      </summary>

      렌더링 되지 않는 컨테이너 요소로, HTML 구조를 그룹화하는데 사용되며, 직접적으로 DOM에 나타나지 않는다.  
      스타일이나 클래스 속성은 적용되지 않으며, 디렉티브(v-if, v-show, v-for등) 속성만 지원된다.
    </details>


  - #### v-show 구문
    ```html
    <body>
      <div id="app">
        <div v-show="show">YES</div>
        <div v-show="!show">NO</div>
        <button @click="toggle">Toggle</button>
        <template v-show="number === 1">
          <div>1</div>
          <div>2</div>
          <div>3</div>
        </template>
        <div v-show="number === 2">number = 2</div>
        <div v-show="number === 3">number = 3</div>
        <button @click="increase">Increase</button>
      </div>
      <script>
        new Vue({
          el: '#app',
          data: {
            show: true,
          },
          methods: {
            toggle() {
              this.show = !this.show;
            },
            increase() {
              this.number ++;
            },
          }
        })
      </script>
    </body>
    ```

</details>
<details>
  <summary style="font-size:30px; font-weight:bold; font-style:italic;">
    v-for
  </summary>

  # 배열
  `v-for="(요소,인덱스) in 배열"` 형태로 사용한다.  
  이때 필수적으로 key 속성과 같이 사용해야 한다.  
  key에는 primary한 고유값이 들어가야 한다.  
  만약 배열의 값이 삭제 혹은 추가되지 않고 단순히 출력만 한다면 index를 사용해도 되지만,
  배열 요소 중 하나가 삭제가 된다면 index를 재생성 해야 하기 때문에 성능/버그 이슈가 있기 때문이다. (리액트도 마찬가지)  

  - #### 배열 요소 기본 바인딩  
    ```html
    <body>
      <div id="app">
        <div>
          {{people[0].name}} {{people[0].age}}
        </div>
        <div>
          {{people[1].name}} {{people[1].age}}
        </div>
        <div>
          {{people[2].name}} {{people[2].age}}
        </div>
      </div>
      <script>
        new Vue({
          el: '#app',
          data: {
            people: [
              { id: 1, name: 'a', age: 20 },
              { id: 2, name: 'b', age: 21 },
              { id: 3, name: 'c', age: 22 },
            ],
          },
        })
      </script>
    </body>
    ```
  - #### 배열 요소 v-for 바인딩 (index)  
    ```html
    <body>
      <div id="app">
        <div v-for="(_, index) in people" :key="_.id">
          {{people[index].name}} {{people[index].age}}
        </div>
      </div>
      <script>
        new Vue({
          el: '#app',
          data: {
            people: [
              { id: 1, name: 'a', age: 20 },
              { id: 2, name: 'b', age: 21 },
              { id: 3, name: 'c', age: 22 },
            ],
          },
        })
      </script>
    </body>
    ```

  - #### 배열 요소 v-for 바인딩 (index)  
    ```html
    <body>
      <div id="app">
        <div v-for="(_, index) in people" :key="_.id">
          {{people[index].name}} {{people[index].age}}
        </div>
      </div>
      <script>
        new Vue({
          el: '#app',
          data: {
            people: [
              { id: 1, name: 'a', age: 20 },
              { id: 2, name: 'b', age: 21 },
              { id: 3, name: 'c', age: 22 },
            ],
          },
        })
      </script>
    </body>
    ```

  # 객체
  객체 또한 각 property의 키, 값에 순차적으로 접근이 가능하다.  
  `v-for="(키,값,인덱스) in 객체"` 형태로 사용한다.  
  - #### 객체 요소 v-for 바인딩 (index)  
    ```html
    <body>
      <div id="app">
        <table border>
          <thead>
            <tr>
              <td v-for="(value, key, index) in people[0]" :key="index">{{key}}</td>
            </tr>
          </thead>
          <tbody v-for="(object, index) in people" :key="object.id">
            <tr>
              <td v-for="(value, key, index) in object" :key="index">{{value}}</td>
            </tr>
          </tbody>
        </table>
      </div>
      <script>
        new Vue({
          el: '#app',
          data: {
            people: [
              { id: 1, name: 'a', age: 20 },
              { id: 2, name: 'b', age: 21 },
              { id: 3, name: 'c', age: 22 },
            ],
          },
        })
      </script>
    </body>
    ```

</details>
<details>
  <summary style="font-size:30px; font-weight:bold; font-style:italic;">
    목차 접은글 기본 템플릿
  </summary>

</details>