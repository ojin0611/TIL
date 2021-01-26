# Module

모듈(module)은 특정 기능을 하는 코드를 담고 있는 파일(또는 스크립트)입니다. 

## 모듈 사용법

### 모듈
```py
import module
from module import var, function, Class
from module import *
```

### 패키지
```py
from package import module
from package.module import var, function, Class
from package.module import function as f

```





## `__name__` 

`__name__` 변수는 실행중인 프로그램이 시작점인지 아닌지에 따라 저장하고있는 값이 다르다.

```python
if __name__ == '__main__':    # 프로그램의 시작점일 때만 아래 코드 실행
```

