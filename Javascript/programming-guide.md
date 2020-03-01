### 1. 빈 컴포넌트 UI 만들기

- UI를 먼저 생각하고 빈 컴포넌트를 만든다.

- 빈 데이터를 만들어놓은 후, 그 데이터를 불러오는 컴포넌트를 작성한다.

- 상세 컴포넌트는 어디에 들어가도 상관없이 해놓고, 레이아웃을 비롯한 스타일은 감싸는 div에서 해준다.

- 한 파일에서 보는 데에 복잡해지기 시작하면 스타일시트 분리한다.

- 쓰는 데이터를 만든 후에 값을 넣기 시작한다.

  ```javascript
    const accidentList = [{}, {}, {}]
  ```

- 상대방이 이해할 수 없는 태그에 대한 처리

  - 주석을 단다.

  - 태그의 역할을 의미하는 컴포넌트를 만든다.

    ```javascript
      <span className="red" />
      -> <RedPoint />
    ```

- key에 id를 넣을 때는 글로벌한 이름으로 짓는다.

  ```javascript
    key={`list-item-${i}`}
  ```

- export default 시에 이름을 넣는 게 sl의 습관인데(바벨에서 버그가 난 적이 있으므로...) 개발 성향마다 다를 수 있다.

- 라이브러리는 공통적으로 사용하는 라이브러리부터 적는다.

  ```javascript
    // AccidentMap.js
    import React from 'react';
    import ReactMapGL from 'react-map-gl';
    ...
    import './AccidentMap.css';
  ```

### 2. 데이터 분리

- 공통적으로 사용하는 시점부터 가지고 오는 데이터로 분리한다.

  e.g. 모든 데이터는 api에서 생성(index는 뷰 역할만)

  ```javascript
    // api/index.js
    import { MODE } from 'settings';
    import SAMPLE_DATA from 'api/sampledata';
    
    const getAccidentList = () => {
    	if(MODE === 'development') {
    		return SAMPLE_DATA;
    	} else {
    			alert('');
    	}
    };
    
    export { 
    	getAccidentList
    };
  
    // page/index.js
    import { getAccidentList } from 'api';
    ...
    
    export default () => {
    	const accidentList = getAccidentList();
    	return (
    		<TempAccList accidentList={accidentList} />
    	  <Map accidentList={accidentList} />
    	)
    }
  ```

- `import { useState } from 'react'` vs `React.useState('')`

  → 한 공간 안에서 외워야하는 변수 이름을 줄이고 싶은 마음에(namespace 강조) `React.useState('')` 형태로 작성하는 습관이 있다.

### 3. 함수 분리와 이름 짓기

- 분리 시에 사용하는 쪽을 기준으로 이름을 고민한다.

  ```javascript
    // page/index.js
    import useAccidentList from 'store';
    
    export default () => {
    	const [accidentList] = useAccidentList();
    }
  
    // store/index.js
    import { MODE } from 'settings';
    import SAMPLE_DATA from 'api/sampledata';
    
    const getAccidentList = () => {
    	if(MODE === 'development') {
    		return SAMPLE_DATA;
    	} else {
    			alert('');
    	}
    };
    
    export { 
    	getAccidentList
    };
  
    // api/sampledata.js
    // 이마저도 api는 데이터를 처리하는 곳은 아님.
    // 좀 더 명확한 이름을 찾다가 accident list를 다루는 무언가 -> store로 명명
    const SAMPLE_DATA = [];
  ```

- 함수를 분리하는 기준?

  - e.g. getAccidentList()에서 getColor()를 호출. getColor()를 굳이 별도로 뺀 이유가 무엇인지?
  - '어떻게'는 가리고 '무엇인지' 값만 드러내고 싶을 때.
  - '설명이 필요해질 때' 분리한다.
    - e.g. 100에 0.1을 더하는 걸 만들 때, "100+0.1"이 무엇인지 함수 이름에 설명을 넣는다

- 스타일시트는 해당 컴포넌트만 바라보므로 컴포넌트와 같은 위치에 놓는다.

- 코드 스택을 얕게 만드는 것을 선호한다.

  ```javascript
    if(aaa) {
    	return bbb;
    }
    // else를 사용하지 않음으로써 코드 스택을 줄임
    return ccc;
  ```