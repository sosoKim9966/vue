# test-vue

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).



Vue.js : MVVM 패턴의 뷰모델에 해당하는 화면단 라이브러리
MVVM 패턴 : Model, View, View Model
- 화면의 요소들을 제어하는 코드와 데이터 제어 로직을 분리하여 코드를 더 직관적으로 이해할 수 있음
- 유지보수 용이

View : 사용자에게 보이는 화면
DOM  : HTML 문서에 들어가는 요소(태그, 클래스 속성 등)의 정보를 담고 있는 데이터 트리
DOM Listener : 돔의 변경 내역에 대해 즉각적으로 반응하여 특정 로직을 수행하는 장치
Model : 데이터를 담는 용기. 보통은 서버에서 가져온 데이터를 자바스크립트 객체 형태로 저장
Data Binding : View에 표시되는 내용과 모델의 데이터를 동기화
View Model : 뷰와 모델의 중간 영역. 돔 리스너와 데이터 바인딩을 제공하는 영역

##Vue 인스턴스
new Vue({
el: ,
data: {

}
});

Vue 생성자 - 뷰 라이브러리를 로딩하고 나면 접근 할 수 있음.
- 뷰로 개발할 때 필요한 기능들을 생성자에 미리 정의해 놓고 사용자가 그 기능을 재정의
* 생성자 : 객체를 새로 생성할 때 자주 사용하는 옵션과 기능들을 미리 특정 객체에 저장해 놓고, 새로 객체를 생성할 때 기존에 포함된 기능과 더불어 기존 기능을 쉽게확정하여 사용하는 기법

##Vue 인스턴스 옵션 속성
1)data
- 미리 정의되어있는 속성
  2)el
- 미리 정의되어 있으며 뷰로 만든 화면이 그려지는 시작점, 뷰 인스턴스로 화면을 랜더링할 때 화면이 그 려질 위치의 돔 요소를 지정
  ex: el: '#id'
  3)template
- 화면에 표시할 HTMl, CSS등의 마크업 요소를 정의하는 속성
  4)methods
- 화면 로직 제어와 관련된 메서드를 정의하는 속성
  5)created
- 뷰 인스턴스가 생성되자마자 실행할 로직을 정의할 수 잇는 속성

##Vue 인스턴스의 유효범위 - 뷰 인스턴스를 생성하면 HTML의 특정 범위 안에서만 옵션 속성들이 적용되어 나타남.

##뷰 인스턴스 라이프 사이클
1)beforeCreate - 인스턴스가 생성되고 나서 가장 처음으로 실행되는 라이프 사이클 단계 data, methods속성 정의x, 돔 화면 요소에 접근x
2)created - data, methods속성 정의, 정의된 값 접근o/돔요소 접근x
3)beforeMount - template 속성ㅇ레 지정한 마크업 속성을 render() 함수로 변환 후 el 속성에 지정한 화면 요소에 인스턴스를 부착하기 전에 호출되는 단계
render() : 자바스크립트로 화면의 돔을 그리는 함수
4)mounted - el 속성에서 지정한 화면 요소에 인스턴스가 부착되고 나면 호출 화면 요소 제어 로직 사용 용이
5)beforeUpdate - 인스턴스에 정의한 속성들이 화면에 치환, 치환된 값은 뷰의 반응성을 제공하기 위해 $watch 속성으로 감시(데이터관찰)
6)update - 데이터 변형 후 가상 돔으로 다시 화면을 그리고나면 실행되는 단계  
7)beforeDestory - 뷰 인스턴스가 파괴되기 직전에 호출, 뷰 인스턴스의 데이터를 삭제하기 좋은 단계  
8)destroyed - 뷰 인스턴스가 파괴되고 나서 호출되는 단계

## 컴포넌트
조합하여 화면을 구성할 수 있는 블록(화면의 특정 영역)

### 전역 컴포넌트 등록

```
Vue.conponent('컴포넌트 이름', {
    // 컴포넌트 내용
});

```
### 지역 컴포넌트 등록

```
new Vue({
    components: {
        '컴포넌트 이름': 컴포넌트 내용
    }
});
```


## props 속성
- '상위 컴포넌트'에서 '하위 컴포넌트'로 데이터를 전달할 때 사용하는 속성

```
Vue.component('child-component', {
    props: ['props 속성 이름'],
});
```
<child-component v-binding:props 속성이름 = "상위 컴포넌트의 data 속성"></child-component>

1.new Vue 인스턴스 생성
2.Vue.component로 하위 컴포넌트 child-component 등록
3.child-component의 내용에 props 속성으로 propsdata 정의
4.HTML에 컴포넌트 태그 추가
5.<child-component>태그 v-bind propsdata="message"는 상위 컴포넌트의 message 속성 값인 텍스트를 하위 컴포넌트의 propsdata로 전달

#이벤트 발생과 수신
'하위 컴포넌트'에서 '상위 컴포넌트'로 데이터를 전달할 때 사용하는 속성

$emit() / v-on 속성 사용

// 이벤트 발생
1)this.$emit('이벤트명');
호출하는 위치는 하위 컴포넌트의 특정 메서드 내부
this = 하위 컴포넌트


// 이벤트 수신
2)<child-component v-on:이벤트명="상위 컴포넌트의 메서드명"></child-component>


*상위->하위 = props / 하위->상위 = 이벤트

#같은 레벨의 컴포넌트 간 통신
옆 컴포넌트에 값을 전달하려면 하위에서 공통 상위 컴포넌트로 이벤트를 전달한 후 공통 상위 컴포너트에서 2개의 하위 컴포넌트에 props를 내려보내야 함.
1. 이벤트로 상위컴포넌트 전달
2. 상위 컴포넌트에서 하위 컴포넌트로 props
   -> 상위 컴포넌트가 필요 없음 강제로 두어야 함.
   -> 해결책 : 이벤트 버스

#관계 없는 컴포넌트 간 통신 - 이벤트 버스
상위 하위 관계를 유지하고 있지 않아도 컴포넌트간 데이터 전달 가능

로직을 담는 인스턴스 + 새 인스턴스 이벤트를 보내고 받음
보내는 컴포넌트 - .$emit()
받는 컴포넌트 - .$on()


















































