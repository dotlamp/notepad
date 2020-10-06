출처 : https://www.youtube.com/watch?v=FJNV_qoRRks (백명석 유튜브)

# CodeReview

## CodeReview

### 시장과 비즈니스의 요구사항
```
 Maketplace is volatile, uncertain, complex, ambigous (마켓 예측 불가)
 +Businesses must innovate faster (사업 이노베이션 빠르게)
=> Deliver software rapidly, frequently, reliably (빠르게, 자주, 안전하게 배포)
```

### 아키텍처의 중요성

#### 엉망진창의 징후(signature of mess)

* 기능이 동작만 하면 아키텍처 개선 없이 다음 기능 구현 Burndown Chart
* 기능 변경 : 복붙 + 일부수정 -> 향후 수정시 문제
* 공유 부족으로 소수의 개발 인력에 대한 의존도 높아짐

#### 클린코드, 좋은설계, 아키텍처에 주의를 기울여야됨

### SW개발의 단순한 진리
* The only way to go fast, is to go well. (바쁘더라도 고퀄리티 코딩해야 함)
  - 생산성 저하와 비용 증가를 뒤집을 수 있는 유일한 방법
  - SW 아키텍처의 품질에 대해서 신중해져야 함
  
배포하고 나면 수정할 시간이 없다...

### Architecture란?

* The Two values of SW (소프트웨어의 2가지 가치)
  - Secondary value of SW is it's behavior (현재 SW가 현재 사용자의 현재 요구사항을 만족)
  - Primary Value of SW(80%) : 지속적으로 변화하는 요구사항 수용(telerate, faciliate) : 코드를 추가하고 변경을 얼마나 잘 할 수 있느냐
 
* 극단적 가정 
  - 동작하지만 고칠수 없는 SW
  - 고칠수 있지만 동작하지 않는 SW


 
