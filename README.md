![image](https://user-images.githubusercontent.com/487999/79708354-29074a80-82fa-11ea-80df-0db3962fb453.png)

# 서비스 시나리오

배달의 민족 커버하기 - https://1sung.tistory.com/106

기능적 요구사항
1. 고객이 메뉴를 선택하여 주문한다
1. 고객이 결제한다
1. 주문이 되면 주문 내역이 입점상점주인에게 전달된다
1. 상점주인이 확인하여 요리해서 배달 출발한다
1. 고객이 주문을 취소할 수 있다
1. 주문이 취소되면 배달이 취소된다
1. 고객이 주문상태를 중간중간 조회한다
1. 주문상태가 바뀔 때 마다 카톡으로 알림을 보낸다

비기능적 요구사항
1. 트랜잭션
    1. 결제가 되지 않은 주문건은 아예 거래가 성립되지 않아야 한다  Sync 호출 
1. 장애격리
    1. 상점관리 기능이 수행되지 않더라도 주문은 365일 24시간 받을 수 있어야 한다  Async (event-driven), Eventual Consistency
    1. 결제시스템이 과중되면 사용자를 잠시동안 받지 않고 결제를 잠시후에 하도록 유도한다  Circuit breaker, fallback
1. 성능
    1. 고객이 자주 상점관리에서 확인할 수 있는 배달상태를 주문시스템(프론트엔드)에서 확인할 수 있어야 한다  CQRS
    1. 배달상태가 바뀔때마다 카톡 등으로 알림을 줄 수 있어야 한다  Event driven

# 분석/설계

## AS-IS 조직 (Horizontally-Aligned)
  ![image](https://user-images.githubusercontent.com/487999/79684144-2a893200-826a-11ea-9a01-79927d3a0107.png)

## TO-BE 조직 (Vertically-Aligned)
  ![image](https://user-images.githubusercontent.com/487999/79684159-3543c700-826a-11ea-8d5f-a3fc0c4cad87.png)


## Event Storming 결과
![MSA_food_delivery](https://user-images.githubusercontent.com/114388258/212527482-d6c83e7a-9bc5-4e73-b3b6-59a23bb65d4e.png)



# 체크포인트

## 1. Saga(Pub/Sub)

![new](https://user-images.githubusercontent.com/114388258/212528685-ec73a22e-03f9-4a87-a3eb-897eb1730399.png)

## 2. CQRS
![주문코드](https://user-images.githubusercontent.com/114388258/212527512-e05e760b-9a69-4187-8482-cf0c6b9d4fe2.png)

![CQRS_1](https://user-images.githubusercontent.com/114388258/212527548-a2727c20-f33c-4e59-b3eb-656cd895a95c.png)

![CQRS_2](https://user-images.githubusercontent.com/114388258/212527551-f17d7bfe-056e-43fa-95c7-fa8dea4d25bc.png)

![CQRS_3](https://user-images.githubusercontent.com/114388258/212527554-8c1997db-a3ec-47fc-a4cd-de204ff750bf.png)

## 3. Compensation /Correlation

![3_조리시작](https://user-images.githubusercontent.com/114388258/212527565-97ba1a34-be7f-43de-9adb-369de8c606a0.png)

![3_취소불가능](https://user-images.githubusercontent.com/114388258/212527951-7601b844-8b3f-400d-9938-29bb7bce0f3e.png)

