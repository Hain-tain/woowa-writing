# 이벤트 현명하게 다루기

### 이벤트, 왜 알아야 할까?

- 사용자가 특정 버튼을 누르면, 모달이 보여야 한다.
- 사용자가 입력할 수 없는 글자를 입력하려고 하면, 오류 메시지를 보여줘야 한다.
- 사용자가 검색창에 글자를 입력하고 엔터를 누르면, 검색 결과를 보여줘야 한다.

위와 같은 요구사항을 만족하기 위해서는 어떻게 해야 할까요? 바로 사용자와의 상호작용을 프로그래밍에서 다룰 수 있어야 합니다.

개발자는`이벤트`를 활용하여 사용자의 동작을 확인하고 제어할 수 있습니다. 사용자와 직접적으로 소통하는 UI를 만드는 프론트엔드 개발자라면, 이러한 이벤트에 대해 알고 적절하게 활용할 줄 알아야 합니다.

### 이벤트란?

이벤트란 웹 페이지나 애플리케이션에서 발생하는 특정 동작이나 사건을 말합니다. 예를 들어 사용자가 버튼을 누르면 해당 버튼에 `클릭 이벤트`가 발생합니다. 이처럼 이벤트는 마우스를 클릭하거나 키보드를 누르는 것과 같이 사용자의 액션에 의해 발생할 수도 있고, 비동기적 작업의 진행을 나타내기 위해서 API들이 생성할 수도 있습니다.

프로그래밍 세계에서 이벤트는 결국 객체입니다. 이 객체는 발생한 이벤트에 대한 다양한 정보를 포함하고 있습니다. 이벤트 객체의 구조와 속성은 이벤트의 종류에 따라 다르지만, 일반적으로 다음과 같은 공통적인 속성들을 가지고 있습니다.

- type: 발생한 이벤트의 종류를 나타내는 문자열 (예: 'click', 'keydown', 'submit' 등)
- target: 이벤트가 발생한 DOM 요소
- currentTarget: 현재 이벤트가 바인딩된 DOM 요소 (이벤트 버블링 중에 변경될 수 있음)
- timeStamp: 이벤트가 발생한 시간
- preventDefault(): 이벤트의 기본 동작을 취소하는 메서드
- stopPropagation(): 이벤트의 전파를 중지하는 메서드

특정 이벤트 유형에 따라 추가적인 속성들이 존재합니다. 예를 들어, 마우스 이벤트의 경우 clientX와 clientY로 마우스의 좌표를, 키보드 이벤트의 경우 key와 keyCode로 입력된 키에 대한 정보를 제공합니다.

이러한 이벤트 객체의 구조를 이해하고 활용함으로써, 개발자는 사용자의 상호작용이나 시스템의 상태 변화에 대해 더욱 세밀하고 정확하게 대응할 수 있습니다. 이벤트 객체가 제공하는 다양한 정보와 메서드들을 통해 복잡한 인터랙션을 구현하고, 사용자 경험을 향상시킬 수 있습니다.

### 이벤트의 종류

이벤트는 발생의 원인에 따라 크게 두 가지로 분류할 수 있습니다.

1. 사용자 이벤트: 사용자가 직접 발생시키는 이벤트로, 마우스 클릭, 키보드 입력, 터치 이벤트 등이 포함됩니다.

2. 브라우저 이벤트: 브라우저에 의해 자동으로 발생하는 이벤트로, 페이지 로드, 이미지 로드 완료, 스크롤 이벤트 등이 있습니다.

이 글에서는 구체적인 이벤트 타입에 대해서 세세하게 다루지 않을 예정입니다. 그러나 각 이벤트의 특성과 발생 시점을 잘 이해하고 활용하는 것이 중요합니다. 특히, 헷갈리기 쉬운 이벤트 간의 차이를 명확히 알고 있으면, 더 효과적으로 이벤트를 처리하고 사용자 경험을 향상시킬 수 있습니다.

예를 들어 아래 3가지 마우스 이벤트는 비슷해보이지만 발생 시점이 다릅니다.

- `click`: 사용자가 마우스를 클릭할 때 발생합니다. 이 이벤트는 마우스 버튼이 눌리고 떼어질 때 모두 포함됩니다.
- `mousedown`: 마우스 버튼이 눌릴 때 발생합니다. 이 이벤트는 버튼이 눌리는 순간에만 발생합니다.
- `mouseup`: 마우스 버튼이 떼어질 때 발생합니다. 이 이벤트는 버튼이 떼어지는 순간에만 발생합니다.

`click` 이벤트는 `mousedown`과 `mouseup` 이벤트가 모두 발생한 후에야 발생합니다. 따라서, `mousedown` 이벤트가 발생한 후에 사용자가 버튼을 떼지 않으면 `click` 이벤트는 발생하지 않습니다. 반면, `mousedown`은 버튼이 눌리는 순간에 즉시 발생합니다.

따라서 상황에 맞는 이벤트를 적절하게 골라 사용하는 것이 중요합니다.

### 이벤트 전파

이벤트 전파는 브라우저에서 발생한 이벤트가 DOM(Document Object Model) 구조 내에서 어떻게 전달되는지에 관한 중요한 개념입니다.

이벤트는 특정 DOM 요소에서 발생하지만, 해당 이벤트가 동일한 구조 내에서 상위 또는 하위 요소로 전파될 수 있습니다. 이러한 이벤트의 흐름을 `이벤트 전파`라고 합니다. 이벤트는 `캡처링 단계`, `타깃 단계`, `버블링 단계` 순으로 전파됩니다.

![이벤트 전파](<이벤트 전파 그림.jpg>)

1. `캡처링 단계`: 이 단계에서는 최상위 요소에서부터 이벤트가 발생한 대상 요소까지 이벤트가 **상위에서 하위**로 전달됩니다.

2. `타깃 단계`: 이 단계에서는 이벤트가 실제로 발생한 대상 요소에 도달합니다.

3. `버블링 단계`: 이 단계에서는 이벤트가 발생한 요소에서부터 다시 상위 요소까지 **하위에서 상위로** 전달됩니다.

이벤트 전파는 필요에 따라 특정 단계에서 중지할 수 있습니다. `event.stopPropagation()` 과 같은 매서드를 사용하여 이벤트가 더 이상 상위 요소로 전파되지 않도록 제어할 수 있습니다. 주로 특정 요소에서만 이벤트가 발생하고, 상위 요소에서는 처리되지 않도록 할 때 이벤트 전파를 중지합니다.

### 이벤트 핸들러

우리가 이벤트를 알아야 하는 이유는, 특정 이벤트가 발생하면 그 이벤트를 처리해주기 위함이었습니다. 이벤트가 발생하면, 해당 요소에 등록된 이벤트 핸들러가 호출되어 해당 이벤트에 대한 처리를 수행합니다. 따라서 우리는 이벤트 핸들러를 등록하여 해당 이벤트가 발생하였을 때 어떤 동작을 수행할지 결정할 수 있습니다.

#### 이벤트 핸들러 등록

순수 자바스크립트에서 이벤트 핸들러를 등록하는 방법에는 3가지 방법이 있습니다.

1. 인라인으로 이벤트 핸들러 등록하기

   ```js
   <button onclick="alert('Clicked!')">Click me</button>
   ```

   이 방법은 HTML 내에서 직접 이벤트를 처리할 수 있어 간단한 경우에 빠르게 사용할 수 있습니다. 그러나 인라인 이벤트 핸들러는 HTML과 JavaScript가 뒤섞여 가독성을 떨어뜨리고, 코드 재사용성이 낮아 유지보수가 어렵습니다. 또한, 여러 개의 요소에 같은 이벤트를 적용할 때 비효율적이며, 보안 문제(예: XSS 공격)에 취약할 수 있습니다. 따라서 이 방법은 사용하지 않는 것이 좋습니다.

2. 프로퍼티로 이벤트 핸들러 등록하기

   ```js
   element.onclick = function () {
     alert('Button clicked!');
   };
   ```

   이 방법은 간단하고 직관적이며, 작은 프로젝트나 간단한 이벤트 처리에 적합합니다. 하지만 이 방법은 한 요소에 대해 하나의 이벤트 핸들러만 등록할 수 있다는 단점이 있습니다. 기존 핸들러를 덮어쓰게 되어, 여러 핸들러를 동시에 사용할 수 없습니다. 또한, HTML과 JavaScript가 혼합되어 코드의 가독성이 떨어지고, 유지보수가 어려워질 수 있습니다.

3. `addEventListener()` 메서드로 이벤트 핸들러 등록하기

   ```js
   element.addEventListener('click', function () {
     alert('Button clicked!');
   });
   ```

   이 방법은 하나의 요소에 여러 개의 이벤트 핸들러를 등록할 수 있으며, 기존 핸들러를 덮어쓰지 않습니다. 또한 이벤트 캡처링과 버블링을 제어할 수 있는 옵션을 제공하며,코드의 가독성을 높이고 HTML과 JavaScript를 분리할 수 있어 유지보수가 용이합니다.

#### 이벤트 핸들러의 메모리 관리

이벤트 핸들러는 DOM 요소와 연결되기 때문에, 잘못된 메모리 관리로 인해 메모리 누수(memory leak)가 발생할 수 있습니다. 특히 이벤트 리스너가 DOM 요소에 남아 있을 경우, 그 요소가 제거된 후에도 메모리에서 해제되지 않아 성능 저하를 일으킬 수 있습니다. 이를 방지하기 위해 적절한 메모리 관리가 필요합니다. 따라서 더 이상 필요하지 않은 이벤트 리스너는 `removeEventListener()`로 제거해야 합니다.

### 리액트에서 이벤트 다루기

#### 브라우저 이벤트와 리액트 이벤트의 차이

브라우저의 이벤트와 리액트의 이벤트는 둘 다 웹 애플리케이션에서 사용자와 상호작용할 때 중요한 역할을 하지만, 처리 방식에서 몇 가지 차이점이 있습니다.

1. 브라우저 이벤트 (Native Browser Event)
   브라우저 이벤트는 HTML 요소에서 발생하는 기본적인 DOM 이벤트입니다. 브라우저의 이벤트 모델을 통해 발생하며, 아래와 같은 특징이 있습니다.

- DOM Level 이벤트: 브라우저는 이벤트가 발생할 때 HTML 요소에 직접 이벤트 리스너를 연결하여 이벤트를 처리합니다.
- 이벤트 버블링과 캡처링: 브라우저 이벤트는 버블링(bubbling) 또는 캡처링(capturing) 방식을 통해 부모나 자식 요소로 이벤트가 전파됩니다.
- 기본 동작이 존재: 브라우저 이벤트는 종종 기본 동작을 가지고 있습니다. 예를 들어, 링크를 클릭하면 기본적으로 새로운 페이지로 이동하는 동작이 있습니다. 이를 막기 위해서는 event.preventDefault()를 호출해야 합니다.
- 이벤트 객체 (Event Object): 브라우저에서 발생한 이벤트에 대한 정보를 포함하는 이벤트 객체가 자동으로 전달됩니다. 이 객체에는 이벤트의 타입, 타겟 요소, 마우스 위치 등 다양한 정보가 포함되어 있습니다.

2. 리액트 이벤트 (React Synthetic Event)
   브라우저 이벤트는 실제 DOM에서 직접 처리되지만, 리액트 이벤트는 가상 DOM을 기반으로 하여 이벤트를 추상화하고 관리합니다. 리액트는 효율적인 이벤트 처리를 위해 브라우저의 네이티브 이벤트 시스템 위에 자체적인 이벤트 시스템을 구현했습니다. 이를 '합성 이벤트(Synthetic Events)'라고 부릅니다. 즉, 리액트 이벤트는 리액트가 자체적으로 관리하는 SyntheticEvent라는 추상화된 이벤트 시스템을 사용합니다. 이는 브라우저 이벤트를 추상화한 것이며, 크로스 브라우저 호환성을 높이기 위해 설계되었습니다.

- Synthetic Event: 리액트는 실제 DOM 이벤트를 감싸서 SyntheticEvent라는 객체로 처리합니다. 이 객체는 브라우저 간의 차이점을 추상화하여 일관된 이벤트 인터페이스를 제공합니다.
- 가상 DOM과 결합: 리액트의 이벤트는 리액트의 가상 DOM과 함께 작동합니다. 따라서 실제 DOM이 아닌 가상 DOM에서 이벤트가 관리되며, 리렌더링 시 성능 최적화가 가능합니다.
- 이벤트 위임: 리액트는 모든 이벤트를 최상위 DOM 노드에서 한 번만 바인딩하고, 이후 하위 요소로 이벤트를 위임하는 방식으로 처리합니다. 이를 통해 성능이 최적화되고, 이벤트 리스너를 더 적게 사용합니다.

#### 리액트에서의 이벤트 핸들러

리액트에서 이벤트 핸들러 등록은 일반적인 JavaScript와 유사하지만, JSX 문법을 사용하여 컴포넌트 내에서 이벤트를 처리하는 방식이 특징적입니다. 리액트에서는 이벤트 이름이 camelCase로 작성되며, 이벤트 핸들러는 함수로 전달됩니다.

예를 들어 클릭 이벤트에 대한 핸들러를 정의하거나 props 로 받은 뒤, 해당 핸들러 함수를 JSX 에 위와같이 전달해줍니다.

```js
const MyComponent = () => {
  const handleClick = () => {
    alert('Button clicked!');
  };

  return <button onClick={handleClick}>Click Me</button>;
};
```

이벤트 핸들러에 전달되는 첫 번째 인자는 이벤트 객체입니다. 이 객체는 발생한 이벤트에 대한 정보를 담고 있으며, 다양한 속성과 메서드를 통해 이벤트의 세부 사항을 확인하고 조작할 수 있습니다. 예를 들어, 클릭한 요소에 대한 정보, 키보드 입력, 마우스 위치 등을 알 수 있습니다.

```js
import React from 'react';

const MyComponent = () => {
  const handleClick = (event) => {
    // 이벤트 객체를 사용하여 클릭한 요소의 정보를 출력
    console.log('클릭한 요소:', event.target);
    alert('Button clicked!');
  };

  return <button onClick={handleClick}>Click Me</button>;
};

export default MyComponent;
```

위의 코드에서 handleClick 함수는 클릭 이벤트가 발생할 때 호출되며, 이벤트 객체를 인자로 받아 클릭한 요소에 대한 정보를 콘솔에 출력합니다. event.target을 통해 클릭한 버튼 요소를 확인할 수 있습니다. 이처럼 이벤트 객체를 활용하면 사용자 상호작용에 대한 보다 세밀한 처리가 가능합니다.

### 이벤트 현명하게 다루기

웹 애플리케이션에서 사용자 상호작용을 효율적으로 처리하는 것은 매우 중요합니다. 불필요한 연산을 줄이고, 성능을 향상시켜 더 좋은 사용자 경험을 제공할 수 있기 때문입니다.

이 글에서는 세 가지 주요 이벤트 처리 기술인 이벤트 위임, 디바운스, 쓰로틀링에 대해 소개하고자 합니다.

#### 이벤트 위임

이벤트 위임은 여러 요소에 대해 각각 이벤트 리스너를 추가하는 대신, 상위 요소에 하나의 이벤트 리스너를 추가하여 하위 요소의 이벤트를 처리하는 기술입니다. 이벤트 위임을 사용하면 많은 수의 하위 요소에 대해 개별적인 이벤트 리스너를 생성하지 않아도 되기 때문에 메모리 사용량을 줄일 수 있습니다. 또한 페이지에 동적으로 추가되는 하위 요소에 대해서도 별도의 처리 없이 이벤트를 처리할 수 있습니다.

예를 들어, 리스트 항목을 동적으로 추가할 때, 각 항목에 이벤트 리스너를 추가하는 대신 부모 요소에 리스너를 추가하여 처리할 수 있습니다.

```ts
import React from 'react';

const EventDelegationExample: React.FC = () => {
  const handleClick = (event: React.MouseEvent<HTMLUListElement>) => {
    const target = event.target as HTMLElement;
    if (target.tagName === 'LI') {
      const page = target.getAttribute('data-page');
      if (page) {
        console.log(`Navigating to ${page}`);
        // 실제 페이지 이동 로직을 여기에 구현
      }
    }
  };

  return (
    <ul onClick={handleClick}>
      <li data-page='/home'>홈</li>
      <li data-page='/about'>소개</li>
      <li data-page='/contact'>연락처</li>
    </ul>
  );
};

export default EventDelegationExample;
```

위ㄴ 예시에서는 <ul> 요소에 단일 클릭 이벤트 리스너를 추가하고, 클릭된 요소가 <li>인 경우에만 처리합니다. 이를 통해 많은 수의 리스트 항목을 효율적으로 관리할 수 있습니다.

#### 디바운스

디바운스는 연속적으로 발생하는 이벤트를 그룹화하여, 마지막 이벤트 발생 후 일정 시간이 지난 뒤에 한 번만 처리하는 기술입니다. 빈번한 이벤트 발생 시 처리 횟수를 줄일 수 있습니다. 이를 통해 불필요한 함수 호출 감소시킬 수 있습니다. 만약 이벤트 핸들러에서 API 호출과 같은 비용이 큰 작업이 실행된다면 불필요한 API 호출을 줄이고, 성능을 향상시킬 수 있습니다.

```ts
import React, { useState, useCallback, useEffect } from 'react';
import axios from 'axios';

function debounce<F extends (...args: any[]) => any>(
  func: F,
  delay: number
): (...args: Parameters<F>) => void {
  let timeoutId: NodeJS.Timeout;
  return (...args: Parameters<F>) => {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => func(...args), delay);
  };
}

const DebounceExample: React.FC = () => {
  const [searchTerm, setSearchTerm] = useState('');

  const debouncedSearch = useCallback(
    debounce((term: string) => {
      console.log(`Searching for: ${term}`);
      axios.get(`/api/search?q=${term}`);
    }, 300),
    []
  );

  useEffect(() => {
    debouncedSearch(searchTerm);
  }, [searchTerm, debouncedSearch]);

  const handleInputChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    setSearchTerm(event.target.value);
  };

  return <input type='text' value={searchTerm} onChange={handleInputChange} />;
};

export default DebounceExample;
```

위 예시에서는 사용자가 입력을 멈춘 후 300ms가 지나면 검색 API를 호출합니다. 이를 통해 사용자가 타이핑하는 동안 불필요한 API 호출을 방지할 수 있습니다.

#### 쓰로틀링

쓰로틀링은 일정 시간 간격으로 함수 실행을 제한하는 기술입니다. 연속적으로 발생하는 이벤트 중에서 일정 주기마다 하나의 이벤트만 처리합니다. 이 기법을 사용하면 매우 빈번하게 발생하는 이벤트의 처리 횟수를 제한할 수 있습니다. 특히 스크롤, 리사이즈와 같은 고빈도 이벤트 처리에 유용합니다.

```ts
import React, { useState, useCallback, useEffect } from 'react';

function throttle<F extends (...args: any[]) => any>(
  func: F,
  limit: number
): (...args: Parameters<F>) => void {
  let inThrottle: boolean;
  return (...args: Parameters<F>) => {
    if (!inThrottle) {
      func(...args);
      inThrottle = true;
      setTimeout(() => (inThrottle = false), limit);
    }
  };
}

const ThrottleExample: React.FC = () => {
  const [scrollPosition, setScrollPosition] = useState(0);

  const throttledHandleScroll = useCallback(
    throttle(() => {
      const position = window.pageYOffset;
      setScrollPosition(position);
      console.log(`Scroll position: ${position}`);
    }, 200),
    []
  );

  useEffect(() => {
    window.addEventListener('scroll', throttledHandleScroll);
    return () => window.removeEventListener('scroll', throttledHandleScroll);
  }, [throttledHandleScroll]);

  return <div>현재 스크롤 위치: {scrollPosition}px</div>;
};

export default ThrottleExample;
```

위 예시에서는 스크롤 이벤트를 200ms마다 한 번씩만 처리합니다. 이를 통해 스크롤 위치 업데이트의 빈도를 제어하고, 불필요한 렌더링을 방지할 수 있습니다.

지금까지 이벤트 위임, 디바운스, 쓰로틀링에 대해 알아보았습니다. 이 세가지 기술은 각각 다른 상황에서 유용한 이벤트 처리 기술입니다. 이벤트 위임은 많은 수의 유사한 요소를 처리할 때, 디바운스는 연속적인 이벤트의 최종 결과만 필요할 때, 쓰로틀링은 일정 주기로 이벤트를 처리해야 할 때 적합합니다. 이러한 기술들을 적절히 활용하면 웹 애플리케이션의 성능을 크게 향상시킬 수 있습니다.

### 마치며

이벤트 처리는 웹 애플리케이션의 핵심적인 부분으로, 사용자가 페이지와 상호작용할 때 필수적인 역할을 합니다. 올바르고 현명하게 이벤트를 처리하면 사용자의 동작에 빠르고 정확하게 반응할 수 있으며, 더 나은 사용자 경험을 제공할 수 있습니다. 이 글이 이벤트를 효과적으로 다루고 싶은 여러분들에게 도움이 되었기를 바라며 글을 마칩니다.
