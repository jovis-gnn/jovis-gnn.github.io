---
title: "[Python] Functools의 reduce를 활용하여 연산하기"
exerpt: "from functools improt reduce"
categories:
  - Python
tags:
  - [Python, reduce]
last_modified_at: 2020-04-02
---

반복적인 연산을 코드한 줄로 간단하게 해주는 functools의 reduce를 소개한다.

```python
functools.reduce(function, iterable[, initializer])
```

[파이썬 공식문서](https://docs.python.org/3/library/functools.html)에서 가져온 함수의 
사용법이다. 위의 notation이 이해가 가지 않을 때는 [BNF 표기법]()을 참고한다.
argments로 function, iterable한 객체(list, tuple, ...), 초깃값(optional)을 전달할 수 있다.

기본적으로 어떻게 사용되는지 확인하고, 세부적으로 어떻게 사용되는지 살펴보겠다.

```python
from functools import reduce

result = reduce(lambda x, y: x+y, [1, 2, 3, 4, 5], 100)
print(result)

# 115
```

첫 번째 인자로 x와 y를 받아서 더하기 연산을 한 결과를 return하는 함수를 넣어주었는데, 
**함수의 argments는 항상 두개여야 한다.** 두 args를 활용해서 누적연산을 하기 때문이다.
두 번째로 [1, 2, 3, 4, 5]의 list, 마지막으로 초깃값의 100을 전달했다.
결과는 초깃값 100에다가 1, 2, 3, 4, 5를 모두 더한 값인 115를 return한 것을 볼 수 있다.

내부적으로 어떻게 동작하는지 살펴보기 위해 print문을 추가해봤다.

```python
from functools import reduce

def plus(x, y):
  print('x={0}, y={1}'.format(x, y))
  return x + y

result = reduce(lambda x, y: x+y, [1, 2, 3, 4, 5], 100)
print(result)

# x=100, y=1
# x=101, y=2
# x=103, y=3
# x=106, y=4
# x=110, y=5
# 115
```

함수가 총 5번 호출된 것을 확인할 수 있다. 처음엔 초깃값 100을 x로 받고 y는 리스트의 
원소 하나씩 차례로 받는다. 다음 x는 이전 호출의 결과 값인 것을 보아, reduce함수는 
재귀형식임을 알 수 있다.

**초깃값을 주지 않는다면 어떻게 될까?**

```python
result = reduce(lambda x, y: x+y, [1, 2, 3, 4, 5], 100)
print(result)

# x=1, y=2
# x=3, y=3
# x=6, y=4
# x=10, y=5
# 15
```

초깃값을 주지 않으면 리스트의 앞쪽 두 개의 요소를 받아서 연산을 시작하게 된다. 따라서 
함수는 총 4번 호출된다.

reduce는 python2에서는 기본적으로 제공하는 함수였으나 python3으로 오면서 functools
로 옮겨졌다. 알고리즘을 작성할 때 자주쓰이니 익혀두면 좋을 것이다.
