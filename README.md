# DACON_LOG_Outlier_dectection

https://www.dacon.io/competitions/official/235717/overview/description/

## 데이콘의 로그 OUTLIER DETECTION 대회 참여 코드들 입니다.

### 1. 데이터 전처리 코드

### 2. 모델링 코드로 이루어져 있습니다.
1. EDA.ipython [다양한 방법으로 데이터를 접근해 봤던 방법입니다.]
2. pattern.ipython [7등급 단어들의 pattern을 찾았던 파일입니다.(대회에서는 직접 사용 불가능)]
3. final.ipython [최종 결과물이 들어 있는 파일입니다(전처리~모델링)]


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

4/30
- 대소 문자 구별을 하게 되면, 7 이외에 다른 것들도 방해하는 경우가 많은 것으로 파악..
- 여러 모델을 섞어서 7을 분류하는 것도 하나의 방법일듯.
- Tokenizer 의 기능들 활용 계획 ( max_df, min_df, n-gram 등)

5/1
- n-gram으로 토큰화 하는  feature 추가시 오히려 성능저하로 이어짐
- 더이상의 전처리 및 threshold 설정은 무의미해 보임 -> 7을 따로 뽑아낼수 있는 방법 고민해보기

5/5
- 팀 병합 완료
- vaildation 수치를 활용해서 threshold 등급별로 조절 -> 조금 상승 
- 2,4,6 등급을 7을 잘 구분함, 0,1 등급에서 기존과 7의 유사도가 높아서 구분이 되지 않음


5/8
- threshold 를 등급별로 조절하는 것도 더 이상 상승 X -> 개수가 많은 0,1,3 의 등급에서 많이 섞여있음


5/10
- 기존의 threshold를 임의로 등급별로 잡는 방법말고 오토인코더를 통한 방식으로 91정도의 성능 확인
- 오토인코더로 reconstruction error를 test에 나와있는 것중 train에 있는 value 값 제거 -? 모여 있는 점들이 악성
- 모여 있는 것을 보는거 자체가 개별적으로 볼 수 없기 떄문에 Data leakage라고 결론.
- vaildation을 참고하여 threshold를 잡는 것이 규칙에는 어긋나지 않다는 것을 질문게시판에서 확인 (현재는 성능이 제일 좋음)

5/11
- 코드 제출을 위해서 코드 정리 중에 실수한 부분 발견(단어들을 많이 지워서 아예 없어지는 경우가 발생하는데 이는 등급분류가 안되는 치명적 오류 발생)


5/12
- 위의 분류 문제가 크게 영향을 주지는 않음 (개수 측면에서), 점수가 미세먼지보다 작은 만큼 상승 (7이 아니라서 조금 상승)
- 남은 제출 횟수 8번에 대한 전략을 세워야함 ( 지도 학습에서는 크게 개선 X, 결국 7을 조금 더 잘 분류해야함
- ** 7로 분류한것중 오분류한것을 잡아내기 위해서, train과 완전히 동일한 문장들은 기존의 분류기로 분류함 (7threshold 에서 제외)


5/14
- 

