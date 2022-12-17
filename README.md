# project_4 (PPT 있습니다!)


올리브영에서 제품 구매해본 적 있으시죠?

저같은 경우, 세럼을 사려고 검색을 했을 때 무려 세럼만 626개의 상품이 검색되었습니다.

제품이 좋은지 리뷰를 보려고 하니 제품 하나가 15,423개의 리뷰가 있어서 그걸 하나하나 읽을 수 없었습니다.



## ⭐ 프로젝트 주제 : 키워드 분석 모델을 통해 화장품 리뷰를 요약해서 개인에게 맞는 화장품을 찾아주자 ! (To. 올리브영 리뷰 읽기 귀찮은 분들에게)

------------------------------------------------------------------------------------------------------------------------------

## 결론


### <데이터>

- 올리브영 온라인몰에서 세럼 27종 크롤링

- 컬럼 : 세럼 이름, 리뷰, 별점, 코멘트로 구성

- 리뷰, 코멘트 컬럼 ->  모델에 사용될 것이기 때문에 한국어와 공백을 제외하고 모두 제거 (영어, 중국어, 이모티콘 등)

- 피부타입 컬럼 추가 -> 0 : 건성,  1 : 지성,  2 : 복합성
- 피부고민 컬럼 추가 -> 0 : 진정에 좋아요,  1 : 보습에 좋아요,  2 : 주름/미백에 좋아요
- 피부자극 컬럼 추가 -> 0 : 자극없이 순해요,  1 : 보통이에요,  2 : 자극이 느껴져요


------------------------------------------------------------------------------------------------------------------------------

### <사용 모델 - KoBERT>

- SKT에서 기존 BERT의 한국어 성능 한계를 극복하기 위해 개발한 koBERT 사용하여 키워드 추출


------------------------------------------------------------------------------------------------------------------------------

### <모델 1 - 피부타입>

- 모델 1 : 2epoch

- 코멘트와 피부타입을 넣은 모델 1의 test 정확도는 약 0.6


### <모델 2 - 피부고민>

- 모델 2 : 3 epoch

- 코멘트와 피부고민을 넣은 모델 2의 test 정확도는 약 0.6


### <모델 3 - 피부자극>

- 모델 3 : 2 epoch

- 코멘트와 피부자극을 넣은 모델 3의 test 정확도는 약 0.6


------------------------------------------------------------------------------------------------------------------------------

### <모델 1 적용>

- 사용자가 피부고민을 입력했을 때 피부 타입을 예측함 

- 입력 : 사계절 내내 건조해요   /   출력 : 검색하신 고민은 '건성'과 관련한 고민이라고 생각됩니다. 건성과 관련한 화장품을 찾아보세요 !

- 입력 : T존 유분이 없어졌으면 좋겠어요   /   출력 : 검색하신 고민은 '복합성'과 관련한 고민이라고 생각됩니다. 건성과 관련한 화장품을 찾아보세요 !


### <모델 2 적용>

- 사용자가 제품명만 입력하면 피부고민 키워드를 요약하여 알려줌 

- 입력 : 에스트라 에이시카365 흔적 진정 세럼   /   출력 : 검색하신 세럼은 '진정'과 관련한 세럼이라고 요약됩니다.

- 입력 : 차앤박(CNP) 프로폴리스 에너지 액티브 앰플   /   출력 : 검색하신 세럼은 '보습'과 관련한 세럼이라고 유추됩니다.


### <모델 3 적용>

- 사용자가 제품명만 입력하면 피부자극 키워드를 요약하여 알려줌 

- 입력 : 에스트라 에이시카365 흔적 진정 세럼   /   출력 : 검색하신 세럼은 '자극 없이 순함'의 확률이 높습니다.

- 입력 : 차앤박(CNP) 프로폴리스 에너지 액티브 앰플   /   출력 : 검색하신 세럼은 '자극이 보통'의 확률이 높습니다.


------------------------------------------------------------------------------------------------------------------------------


![image](https://user-images.githubusercontent.com/110079085/208228662-b30616bc-0237-4066-b1a7-0bb5f74a48a5.png)


------------------------------------------------------------------------------------------------------------------------------


### 개선 방향

- 데이터 전처리 할 때 한국어와 공백을 제외하고 모두 제거  ->  keyBERT 모델도 함께 사용했다면 영어 리뷰 키워드도 추출할 수 있지 않을까 하는 아쉬움

- 각 모델 당 2~3 epoch 적용  ->  추후 시간적 여유 있다면 더 많은 epoch를 사용하여 모델을 학습시킬 수 있을 것이라 생각

- 3개 모델 모두 정확도가 0.6으로 낮은 편,  정확도가 낮은 이유를 추측해보자면,
  1. “‘복합성’인 저와 ‘건성’인 친구에게 잘 맞아요“, “ ‘보습’보다 ‘진정＇효과를 더 중요하게 생각하는데…＂ 등 한 코멘트에 여러 키워드 들어 있는 경우가 많음
  2. 3개 모델 다 키워드 비율이 고르게 분포되어 있지 않음
  -> 추후 이를 해결한다면 더 높은 정확도를 가질 수 있을 것이라 예상

- 이번 프로젝트를 바탕으로 리뷰에 별점만 표시되는 쿠팡, 카카오톡 선물하기 등 여러 분야의 쇼핑몰에 키워드 추출 모델을 적용할 수 있을 것이라 예상



