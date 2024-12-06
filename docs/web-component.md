# ?
web api의 하나로 사용자 정의의 컴포넌트, 요소를 만들게 해주는 역할.

# 정의하기, define

## `CustomElements.define()`

```js
CustomElements.define(
	'custom-element-name', // 요소의 이름
	CustomElementFucntion, // 요소 정의 클래스
	{extends: 'standard-html-element-name'}
		// 어떤 표준 요소를 계승하는지 정의
		// 생략가능, 생략시 <custom-element-name>로 사용
		// <p>로 지정한 경우, <p is="custom-element-name">로 사용
			// 시멘틱 태그를 더 잘 쓸 수 있음	
)
```

## `CustomElementFunction`

```js
class CustomElementFunction extends HTMLElemnt {

	super() // define()에서 표준 요소를 계승한 경우
		// 아니면 생략 가능

	// 생명주기 콜백 사용해서 요소 정의
	connectedCallback(){
		 // 하위 요소 정의
		 let sub_element = document.creatElemetn('div');
		 
		 // 텍스트 삽입
		 sub_element.textContent = 'text content';
			 // or
		 sub_element.innerText = 'text content';

		// attribute 추가
		 sub_element.setAttribuet("attribute", "value");

		// 상위 요소의 attirubute의 값을 가져와서 하위요소 내용에 적용
		const upperAttributeValue = this.getAttribute("attribute")
		sub_element.textContent = upperAttributeValue;
	
		// 상위 요소에 추가
		this.appendChild(sub_element)
	}
```

### `render()`로 분리

```js
class CustomElementFunction extends HTMLElemnt {
	// or, 여러 생명주기 콜백을 사용하기 위해 render 함수 분리
	connectedCallback(){
		this.render()
	}
	
	render(){
		 let sub_element = document.creatElemetn('div');
		 sub_element.textContent = 'text content';
		 sub_element.innerText = 'text content';
		this.appendChild(sub_element)
	}
}
```

### 생명 주기 callback

```js
class CustomElementFunction extends HTMLElemnt {
	// 노드에 연결될 때, DOM 생성시 실행
	connectedCallback(){ }

	// DOM에서 연결 해제되었을때 호출
	disconnectedCallback(){ }

	// DOM 노드가 다른 문서로 이동할때 호출
		// 이미 생성되었는데 다른 요서의 하위로 들어가는 경우 등
	adoptedCallback(){ }


	// 특성들이 추가, 제거, 변경될 때 호출
	attributeChangedCallback(){}
		// 반드시 어떤 특성이 변경되는 것이 감지될지 명시되어야 함
	
	// 어떤 특성을 감시할지 정의
	static get observedAttributes(){
		return ['attribute-to-observe']
	}
	
}
```


### `constructor()`, 생명주기 콜백 없이 사용



# styling


## `<style>` 사용

```js
class CustomElementFunction extends HTMLElemnt {	
	connectedCallback(){
		const style = document.creatElement('style');
		style.textContent = `
			custom-element {
				color: red;
			}
		`;
		this.appendChild(style)
	}
}
```


## 전역 스타일 시트 적용

스타일 시트에 적어서 적용시키자