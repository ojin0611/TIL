# Modules

[Mozilla 문서](https://developer.mozilla.org/ko/docs/web/javascript/guide/modules)

[TOC]

최신 브라우저가 기본적으로 모듈 기능을 지원하기 시작했다. 브라우저는 모듈의 로딩을 최적화할 수 있기 때문에 라이브러리를 사용하는 것보다 더 효율적이고, 클라이언트 측에서 추가 처리와 여분의 왕복을 모두 해야하는 것보다 효율적이다.



예제 파일 구조

```
index.html
main.js
modules/
    canvas.js
    square.js
```



# Export

## named exports

모듈 밖으로 내보내려는 항목 앞에 `export`를 배치한다. 최상위 항목만 내보낼 수 있다.

```javascript
// square.js
export const name = 'square';

export function draw(ctx, length, x, y, color) {
    ctx.fillStyle = color;
    ctx.fillRect(x, y, length, length);

    return {
        length: length,
        x: x,
        y: y,
        color: color
    };
}
```

또는 모듈 파일 끝에 하나의 export문을 사용하는 방법도 있다.

```javascript
export { name, draw };
```



## default export

`export` 대신 `export default`를 추가한다. 하나의 모듈은 하나의 default export만 허용하기때문에 import할 때는 default로 설정한 유일한 항목이 import된다.

```javascript
// export
export default function(ctx) {
  ...
}
// import
import randomSquare from './modules/square.js';
```





# Import

모듈에서 내보낸 기능을 사용할 수 있도록, 사용할 스크립트로 가져오는 가장 간단한 방법은 아래와 같다.

```javascript
// main.js
import { name, draw } from './modules/square.js';
```

import한 뒤에는 동일한 파일 내에 정의한 것처럼 기능을 사용할 수 있다.