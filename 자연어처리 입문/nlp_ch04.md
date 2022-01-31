#ch 04 이론
## Word
- Discrete, not continuous
- 단어는 discrete symbol & categorical value 형태이지만, 우리의 머릿속에서는 다르게 동작
	- 어휘는 계층적 의미 구조를 지니고 있으며,
	- 이에 따라 단어 사이의 유사성을 지님
	- e.g. <파랑>과 <핑크> 중에서 <빨강>에 가까운 단어는 무엇인가?
- One-hot 인코딩으로 표현된 값은 유사도나 모호성을 표현할 수 없다
	- Dense vector로 표현하는 것이 유리

## Feature Vectors
- Feature(특징)
	- 샘플을 잘 설명하는 특징
	- 특징을 통해 우리는 특정 샘플을 수치화할 수 있다
- Feature Vector
	- 각 특징들을 모아서 하나의 vector로 만드는 것
	- 단어의 feature vector는 무엇이 될까?

## Representation Learning via Dimension Reduction
- 신경망은 x와 y 사이의 관계를 학습하는 과정에서 자연스럽게 x의 feature를 추출하는 방법을 학습함
- 레이어 중간의 hidden representation은 y의 값을 구하기 위해 x에서 필요한 정보를 더 작은 차원에 압축 표현(오토인코더) 한 것이라 할 수 있음

## Word Embedding
- 딥러닝의 시대에 들어와 신경망의 이러한 특성을 활용하여 단어를 연속적인 값으로 표현하고자 하는 시도가 이어짐
- 이전에 비해 훌륭한 dense vector를 얻을 수 있게 되어, 단어의 필요한 특징을 잘 표현할 수 있게 되었음
	- 유사도 등의 연산에 유리함
- 단어를 벡터로 표현하는 방법으로, 단어를 밀집 표현으로 변환
	- 희소 표현(Sparse Representation)
		- 벡터 또는 행렬의 값이 대부분이 0으로 표현되는 방법(One-hot vector는 희소 벡터: 고차원)
	- 밀집 표현(Dense Representation)
		- 희소 표현과 반대되는 표현 -> 벡터의 차원이 조밀해짐
		- 사용자가 설정한 값으로 모든 단어의 벡터 표현의 차원을 맞춤
		- 0과 1만 가진 값이 아니라 실수값을 가지게 된다(임베딩 벡터: 저차원)

### Word Sense
- Word Sense Disambiguation(WSD)가 필요
- 다음 언어들은 One-hot 인코딩 불가 -> 다른 방법 찾아야 함
#### Homonym(동형어)
- 차
#### Polysemy(다의어)
- 지하철역사, 역 주변 상권
#### Synonyms(동의어) & Antonyms(반의어)
#### Hypernyms(상위어) & Hyponyms(하위어)
- 단어는 개념적 의미를 지님
- 개념을 포괄하는 상위 개념이 존재하며, 계층적 구조를 지님

### WordNet
- Thesaurus(어휘분류사전, 시소러스)
- 동의어 집합(Synset) 또는 상위어(Hypernym)나 하위어(Hyponym)에 대한 정보가 특히 잘 구축되어 있는 것이 장점
	- Directed Acyclic Graph(유방향 비순환 그래프)
- 한국어: KorLex, Korean WordNet(KWN)
- Distance: 클 수록 거리가 멀다 -> 유사도를 만들 수 있다
	- similarity(w,w') = -log distance(w,w')
	- 거리가 클 수록 유사도가 작아진다
- 단어의 계층적 구조를 파악할 수 있음
- 동의어 집합을 구할 수 있음
- 단어 사이의 유사도를 계산할 수 있음
-> 더 깊이 나아가고자 한다면 Data-driven 방식이 필요

### Data-driven Methods
- Thesaurus 기반 방식은 사전에 대한 의존도가 높으므로, 활용도가 떨어질 수 있음 -> 상대적으로 코퍼스(or 도메인) 특화된 표현 가능
- 데이터에 기반한 방식은(데이터가 충분하다면) task에 특화된 활용이 가능
- 여전히 sparse한 vector로 표현됨
	- PCA를 통해 차원 축소를 하는 것도 방법

#### TF-IDF
- 텍스트 마이닝(Text-Mining)에서 중요하게 사용
- 어떤 단어 w가 문서 d 내에서 얼마나 중요한지 나타내는 수치(입력: 단어, 문서 / 출력: 중요도)
- TF(Term Frequency)
	- 단어의 문서 내에 출현한 횟수
	- 숫자가 클수록 문서 내에서 중요한 단어
	- 하지만 'the'와 같은 단어도 TF값이 매우 클 것
- IDF(Inverse Document Frequency)
	- 그 단어가 출현한 문서의 숫자의 역수(inverse)
	- 값이 클수록 'the'와 같이 일반적으로 많이 쓰이는 단어
- TF-IDF(w,d) = TF / DF
- TF-IDF를 feature로 사용할 수 없을까? -> TF-IDF Matrix
	- TF-IDF는 문서에서 해당 단어가 얼마나 중요한지 수치화
	- 중요한 문서가 비슷한 단어들은 비슷한 의미를 지닐까?
	- 각 문서에서의 중요도를 feature로 삼아서 vector를 만든다면?

#### TF-IDF Matrix
- 단어의 각 문서(문장, 주제)별 TF-IDF 수치를 vector화
#### Based on Context Window(Co-occurence)
- Co-occurrence Probability: 동시 등장 확률은 동시 등장 행렬로부터 특정 단어 i의 전체 등장 횟수를 카운트하고, 특정 단어 i가 등장했을 때 어떤 단어 k가 등장한 횟수를 카운트하여 계산한 조건부 확률
- 함께 나타나는 단어들을 활용
- 가정:
	- 의미가 비슷한 단어라면 쓰임새가 비슷할 것
	- 쓰임새가 비슷하기 때문에, 비슷한 문장 안에서 비슷한 역할로 사용될 것
	- 따라서 함께 나타나는 단어들이 유사할 것
- Context Window를 사용하여 windowing을 실행
	- window의 크기라는 hyper-parameter 추가
	- 적절한 window 크기를 정하는 것이 중요

### Similarity distance(유사도 거리)
- L1, L2 Norm과 Infinity Norm은 강조하고자 하는 것에 따라 사용
- 맨하튼 거리(L1 distance)
- 유클리디안 거리(L2 distance)
- 코사인 유사도: 벡터의 방향을 중요시 함
	- Feature vector의 각 차원의 상대적인 크기가 중요할 때 사용

### Word2Vec
- Classification
- 중심 단어로 주변 단어를, 주변 단어로 중심 단어를 예측하는 과정에서 단어를 벡터로 임베딩하는 방법론
- 임베딩된 단어의 내적이 코사인 유사도가 되도록 한다
- Objective:
	- 주변(context window)에 같은 단어가 나타나는 단어일 수록 비슷한 벡터 값을 가져야 한다
	- 주어진 단어로 주변 단어를 예측하자
	- y를 예측하기 위해 정보가 z에 있어야 한다
		- 주변 단어를 잘 예측하기 위해 x를 잘 압축하자
- 문장의 문맥에 따라 정해지는 것이 아님
	- context window의 사이즈에 따라 embedding의 성격이 바뀔 수 있다
- 저차원 벡터공간에 임베딩된 단어벡터 사이의 유사도를 측정하는 데는 LSA에 비해 좋은 성능을 가지지만, 사용자가 지정한 윈도우(주변 단어 몇개만 볼지) 내에서만 학습/분석이 이뤄지기 때문에 말뭉치 전체의 공기정보(co-occurrence)는 반영되기 어려움

#### Skip-gram
- 기본 전략
	- 주변 단어를 예측하도록 하는 과정에서 적절한 단어의 임베딩(정보의 압축)을 할 수 있다
	- Non-linear activation func.이 없음
- 기본적인 개념은 오토인코더와 굉장히 비슷함
	- y를 성공적으로 예측하기 위해 필요한 정보를 선택 / 압축
- 장점(at that time):
	- 쉽다
	- 빠르다
	- 비교적 정확한 벡터를 구할 수 있다
- 단점:
	- 현재는 느리다
	- 출현 빈도가 적은 단어일 경우 벡터가 정확하지 않다

### GloVe(Global Vectors for Word Representation)
- Regression
- 임베딩된 두 단어 벡터의 내적이 말뭉치 전체에서의 동시 등장 확률 로그값이 되도록 목적함수를 정의 -> 임베딩된 단어벡터 간 유사도 측정을 수월하게 하면서도 말뭉치 전체의 통계 정보를 좀 더 잘 반영하고자 하는 목표
- 단어 x와 윈도우 내에 함께 출현한 단어들의 빈도를 맞추도록 훈련
- 출현 빈도가 적은 단어에 대해서는 loss의 기여도를 낮춤
	- 출현 빈도가 적은 단어에 대해 부정확해지는 단점을 보완
- 장점: 더 빠르다
	- 전체 코퍼스에 대해 각 단어 별 co-occurence를 구한 후, regression을 수행
	- 출현 빈도가 적은 단어도 벡터를 비교적 정확하게 잘 구할 수 있다

### FastText
- Sum of subword embedding
- upgrade version of skip-gram
	- 단어 안의 내부 단어들 고려(subword)
	- e.g. "where" = {"<wh", "whe", "her", "ere", "re>"}
- 기존의 word2vec은 저빈도 단어에 대한 학습과 OoV에 대한 대처가 어려웠음
- 학습시
	1. 단어를 subword로 나누고
	2. Skip-gram을 활용하여, 각 subword에 대한 embedding vector에 주변 단어의 context vector를 곱하여 더한다
	3. 이 값이 최대가 되도록 학습을 수행
- 최종적으로 각 subword에 대한 embedding vector의 합이 word embedding vector가 된다

### Embedding Layer
- 임베딩 레이어에 원핫벡터를 넣는다
	- 임베딩 레이어를 통해 원핫벡터를 각 task에 맞는 dense representation으로 바꿀 수 있음
- linear layer와 유사
- x(one-hot vector) -> embedding layer -> z(embedding vector) -> layer -> activation func -> layer -> activation func -> y hat
- linear + linear = linear인데 왜 굳이 embedding layer를 넣는가?
	- 계산의 효율성을 위해
	- x의 값이 index가 주어졌을 때 원하는 index만 추출하기 위해

## Word Embedding 적용 사례
### 자연어처리 분야
- 딥러닝의 모토는 end-to-end solution을 만드는 것
- 단어 임베딩이 최종 목표인 경우는 거의 없음
	- 따라서 word embedding vector를 활용하여 제품을 만드는 일은 흔치 않다
- Word2Vec의 비지도학습 특성을 활용하려는 시도는 있으나, 상용화하기엔 부족

### 추천시스템: 상품 임베딩(Product2vec)
- 상품의 특징
	- Discrete & Sparse
	- 클릭/구매 순서에 따른 상품들의 sequence 생성 가능
- 가정:
	- 사용자가 함께 클릭/구매한 상품들이 비슷한 상품들은 비슷한 임베딩 값을 가진다
- 예시: 상품의 ID 자체가 단어가 되는 것
	- 258437, 572837, 567288

### 주식 종목 임베딩(Stock2Vec)
- 종목의 특징
	- Discrete & Sparse
	- Sequence 뭐라도 만들어보자 (e.g. 사용자가 매수/매도한 sequence)
- 가정:
	- 개장일에 함께 오르고 내린 종목들은 비슷한 임베딩 값을 갖는다
- 예시: 상한가부터 하한가까지 쭉 줄 세워보자
	- 016710(대성홀딩스), 009450(경동나비엔), 001440(대한전선), 005380(현대자동차)

## Sentence Embedding
- 흔히 시도되는 방법은 문장 내 단어 임베딩 벡터들을 모두 더하는 것
- 하지만 어순을 무시한 채 평균을 구하는 것이므로 detail이 뭉개질 수 있다
	- I did not say that I am idiot
	- I did say that I am not idiot

### Context Embedding
- 단어는 문맥(또는 문장 내 위치)에 따라서 의미가 변화
	- 따라서 문맥에 따른 임베딩이 필요함
- 문맥을 고려하기 위해서는 주변 단어들의 쓰임새를 살펴야 함
	- 이후 다룰 <텍스트 분류, RNN>, <자연어 생성, Seq2seq> 주제들의 기본 원리
