# DACON_LOG_Outlier_dectection

https://www.dacon.io/competitions/official/235717/overview/description/

## 데이콘의 로그 OUTLIER DETECTION 대회 참여 코드들 입니다.

### 1. 데이터 전처리 코드

### 2. 모델링 코드로 이루어져 있습니다.



#### 날짜별 고민했던 내용

4/20
- 0~6등급의 보안 등급은 학습 기준 99.9 로 분류가 쉬움
- 7등급의 경우 테스트 데이터에만 존재해서 비지도 학습 필요


4/23 
- 비지도 학습으로 7등급을 구분하기 위해서 오토인코더 적용
- 하지만 뚜렷하게 구분이 되지 않음
- 이유는 토큰화 과정에서 이상한 의미없는 단어들이 같이 토큰화 되어 그런것으로 판단

4/25
- 문장의 전처리를 추가적으로 진행하여 단어만 남게 작업
- 실제 영어 단어인지 아닌지를 파악하여 아닌 경우도 제거
- 등급별로 구분이 아닌 문장의 타입별을 구분하여 오토인코더를 활용시 성능이 훨씬 개선

4/26
- 데이콘으로 부터 Train, test 의 단어를 비교하는 것은 Data leakage 라는 답변을 받음
- Train test 단어 비교가 아닌 train 단어를 train,test 공통으로 삭제하는 방향으로 

4/27
- 단어가 아닌 이상한 영어 단어를 삭제, train중에서 빈도수가 높은 단어는 큰의미 없다고 판단하여 삭제
- 단어가 영어단어를 뽑는 과정에서 시간이 너무 오래걸림(words.words()), 빠른 방법 필요

4/29
- 기존의 모든 문장을 각각 영단어 여부 체크 방식 -> 중복된 단어들이 많은데 일일히 체크 -> 중복제거 후에 단어여부 체크 (시간 향상)
- 베이스라인에서 의미 없는 단어들 제거 + 2글자 이하 제거한 전처리 (onlyword_train,test)로 0.885 정도로 성능 향상 -> 무의미한 단어 제거 효과
- 대소 문자 구별하는 것이 의미가 있는 것으로 확인 ( 7등급에서 명령어로 앞글자에 대문자로 들어오는 경우 발생 prevent,kill 등)

