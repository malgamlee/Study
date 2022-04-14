# Exports & Imports

## Export

```jsx
const person = {
	name : 'Max'
}

export default person
```

```jsx
export const clean = () => { ... }

export const baseData = 10;
```

## Import

```jsx
import person from './person.js'
import prs from './person.js'

import {baseData} from './utility.js'
import {clean} from './utility.js'
```

- person.js
    - default
        - 파일에 있는 콘텐츠를 가져오면 기본 내보내기가 되는 것
        - 기본 Import 및 수신 파일 이름 내보내기는 사용자가 원하는 대로 정할 수 있다.
        - 한 번 default로 지정해놓으면 언제나 참조됨
- utility.js
    - 두 가지 상수를 가져왔으므로 구문을 가져올 때 중괄호를 사용한다.
        - 파일에 있는 특정한 콘텐츠를 대상으로 하기 위해
    - 이름으로 내보내기 라고 부른다.
        - default로 지정한 게 없기 때문

### 정리

- default export
    
    ```jsx
    import person from './person.js'
    import prs from './person.js'
    ```
    
    - person 이름을 마음대로 지을 수 있음
- named export
    
    ```jsx
    import {clean} from './utility.js'
    import {clean as Clean} from './utility.js'
    import * as bundled from './utility.js'
    ```
    
    - 정확히 똑같은 파일 이름을 입력해야 함
    - 별칭을 정해 할당한 후 as를 사용해서 파일을 가져오면 됨
    - 여러 파일을 내보내고 싶다면 *을 입력해서 가져올 수 있음
        - bundled.clean 으로 내보내면 됨