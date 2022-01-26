# ch 02 이론
## 언어란 무엇인가?
- 정보 전달
- 자연어란?
	- 사람들이 일상적으로 쓰는 언어를 인공적으로 만들어진 언어인 인공어와 구분하여 부르는 개념
- 자연어처리란?
	- 사람이 이해하는 자연어를 컴퓨터가 이해할 수 있는 값으로 바꾸는 과정(NLU) : 사람 -> 컴퓨터
	- 더 나아가 컴퓨터가 이해할 수 있는 값을 사람이 이해하도록 바꾸는 과정(NLG) : 컴퓨터 -> 사람

### NLU
- Text classificaion
- Pos Tagging
- Sentiment Analysis
- Machine Reading Comprehension
- Named Entity Recognition
- Semantic Parsing

### NLG
- Language Modeling
- Article Generation

### NLG & NLU
- Chatbot
- Summarization
- Question Answering
- Machine Translation

### Traditional NLP
- 단어를 symbolic 데이터로 취급
	- 이산적, 심볼릭 공간
	- 사람이 인지하기 쉬움
	- 디버그 용이, 연산 속도 느림
- 여러 sub-module을 통해 전체 구성


### NLP with Deep Learning
- 단어를 continuous value로 변환
	- 연속적, 신경망 공간
	- 사람이 이해하기 어려움
	- 디버깅 어려움, 연산 속도 빠름
- 효율적인 Embedding을 통한 성능 개선
	- 단어, 문장, 컨텍스트 임베딩
- End-to-end 시스템 추구
	- 효율/성능 개선
	- 가볍고 빠르다

## AI(Artificial Intelligence) 3대장 + 1
### Computer Vision
- Image Recognition
- Object Detection
- Image Generation
- Super Resolution

### NLP(Natural Language Processing)
- Text Classification
- Machine Translation
- Summerization
- Question Answering

### Speech Processing
- Speech Recognition(STT)
- Speech Synthesis(TTS)
- Speaker Identification

### Reinforcement Learning

## NLP vs Others
### NLP
- Discrete Value를 다룸
	- 단어, 문장
- 분류 문제로 접근할 수 있음
- 샘플의 확률 값을 구할 수 있음
	- P(x=단어)
- 문자 생성(자연어 생성)
	- auto-regressive 속성을 지님
		- 과거의 속성이 현재에 영향을 미침
	- GAN 적용 불가

### Other Fields
- Continuous value를 다룸
	- 이미지, 음성
- 문제에 따라 접근 방식이 다름
- 샘플의 확률 값을 구할 수 없음
	- P(x=이미지)
- 이미지 생성
	- auto-regressive 속성 없음
	- GAN 적용 가능

## NLP는 왜 어려운가?
### Ambiguity 모호성
- 중의성 해소(word sense disambiguation)
	- 주변 단어를 통해 해소
		- 차를 마시러: tea
		- 가던 차: car
		- 그녀에게 차였다: dumped
- 문장 내 정보의 부족으로 인한 모호성이 발생
	- 최대한 짧은 문장 내에 많은 정보를 담고자 한다
		- 정보량이 낮은 내용(contexet)은 생략
		- 여기에서 모호함(ambiguity)이 발생
### Paraphrase
- 하나의 정보에 대해 서로 다른 표현 및 문장이 나올 수 있다

### Discrete, not Continuous
- 이산 값을 갖는 자연어는 사람의 입장에서 인지가 쉬울 수 있으나, 기계의 입장에서는 매우 어려운 값
- One-hot 인코딩으로 표현된 값은 유사도나 모호성을 표현할 수 없다
	- 서로 다른 One-hot 벡터끼리의 유사도나 거리는 모두 동일하다
	- 따라서 아래의 질문에 대답할 수 없다
		- 파랑과 핑크 중에 빨강에 가까운 단어는 무엇인가?
- 높은 차원으로 표현되어 매우 sparse하게 된다
- Word Embedding으로 해결

## 한국어 NLP는 왜 더 어려운가?
### 언어 종류
- 교착어
	- 어간에 접사가 붙어 단어를 이루고 의미와 문법적 기능이 정해짐
	- 한국어, 일본어, 몽골어
- 굴절어
	- 단어의 형태가 변함으로써 문법적 기능이 정해짐
	- 라틴어, 독일어, 러시아어
- 고립어
	- 어순에 따라 단어의 문법적 기능이 정해짐
	- 영어, 중국어

### 어떤 것들 때문에 어려운가?
- 접사 추가에 따른 의미 파생
- 유연한 단어 순서 규칙(어순이 중요하지 않음)
- 모호한 띄어쓰기
- 평서문과 의문문의 차이 부재
- 주어 부재
- 한자 기반의 언어
	- 단어 중의성 발생

### 왜 한국어만 어려운가?
- 한글은 굉장히 늦게 만들어진 문자
	- 기존 다른 문자들의 장점을 흡수
	- 굉장히 과학적으로 만들어짐
- 효율이 극대화 되었기 때문에 더욱 어려운 것
