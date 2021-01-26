# Day 7 Gradient Descent

## 미분(differentiation)

- 변수의 움직임에 따른 함수 값의 변화를 측정하기 위한 도구
- 컴퓨터로 미분 계산 가능

```python
import sympy as sym
from sympy.abc import x
sym.diff(sym.poly(x**2 + 2*x + 3), x)
```

- 증가시키고 싶다면 미분값을 더하고 감소시키고 싶으면 미분값을 뺍니다
- 미분값을 더하면 경사상승법(gradient ascent), 함수의 극대값을 구할 때 사용
- 미분값을 빼면 경사하강법(gradient descent), 함수의 극소값을 구할 때 사용
- 경사하강법 알고리즘

```python
var = init
grad = gradient(var)
while(abs(grad) > eps): # eps보다 작을 때 종료
		var = var - lr * grad # lr은 합습률. 업데이트 속도 조절
		grad = gradient(var) # 종료조건이 성립하기 전까지 미분값 업데이트
```

![Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled.png](Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled.png)

- 벡터가 입력인 다변수 함수의 경우 편미분(partial differentiation)을 사용

![Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%201.png](Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%201.png)

- 각 변수 별로 편미분을 계산한 그레디언트 벡터를 이용하여 사용 할 수 있다

![Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%202.png](Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%202.png)

- 경사하강법 : 알고리즘

```python
var = init
grad = gradient(var)
while(norm(grad) > eps):
		var = var - lr * grad
		grad = gradient(var)
```

![Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%203.png](Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%203.png)

- 경사하강법으로 선형회기 계수 구하기
- 선형회귀의 목적식은 $\left\|\mathbf{y-X}\beta \right\|_2$ 이고 이를 최소화 하는 $\beta$를 찾아야 하므로 다음과 같은 그레디언트 벡터를 구해야 한다

![Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%204.png](Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%204.png)

![Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%205.png](Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%205.png)

![Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%206.png](Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%206.png)

- 이제 목적식을 최소화하는 $\beta$를 구하는 경사하강법 알고리즘은 다음과 같다

![Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%207.png](Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%207.png)

![Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%208.png](Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%208.png)

![Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%209.png](Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%209.png)

![Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%2010.png](Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%2010.png)

- 경사하강법 기반 선형회귀 알고리즘

```python
for t in range(T):
		error = y - X @ beta
		grad = - transpose(X) @ error
		beta = beta - lr * grad
```

- 이제 경사하강법 알고리즘으로 역행렬을 이용하지 않고 회귀계수를 계산 가능

![Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%2011.png](Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%2011.png)

- 경사하강법 알고리즘에선 학습률과 학습횟수가 중요한 hyperparameter
- 이론적으로 경사하강법은 미분가능하고 볼록(convex)한 함수에 대해선 적절한 학습률과 학습횟수를 선택했을 때 수렴이 보장되어 있다
- 특히 선형회귀의 경우 목적식 $\left\|\mathbf{y-X}\beta \right\|_2$은 회기계수 $\beta$에 대해 볼록함수 이기 떄문에 알고리즘을 충분히 돌리면 수렴이 보장
- 하지만 비선형회귀 문제의 경우 목적식이 볼록하지 않을 수 있으므로 수렴이 항상 보장되지는 않는다

### 확률적 경사하강법(stochastic gradient descent)

- 확률적 경사하강법은 모든 데이터가 아닌 데이터 일부를 활용하여 업데이트
- 볼록이 아닌(non-convex) 목적식은 SGD를 통해 최적화 할 수 있다

![Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%2012.png](Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%2012.png)

- SGD는 데이터의 일부를 가지고 업데이트 하기 떄문에 좀 더 효율적

![Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%2013.png](Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%2013.png)

- SGD는 미니배치를 가지고 그레디언트 벡터를 계산. 미니배치는 확률적으로 선택하므로 목적식 모양이 바뀌게 된다

![Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%2014.png](Day%207%20Gradient%20Descent%2058099359900c4d969ed9c0f41c08b86c/Untitled%2014.png)

- SGD는 볼록이 아닌 목적식에서도 사용 가능하므로 경사하강법보다 머신러닝 학습에 더 효율적