# ch 03 이론
## NLP Project Workflow
1. 문제 정의
	- 단계를 나누고 simplify
	- x와 y를 정의
2. 데이터 수집
	- 문제 정의에 따른 수집
	- 필요에 따라 레이블링
3. 데이터 전처리 및 분석
	- 형태를 가공
	- 필요에 따라 EDA 수행
4. 알고리즘 적용
	- 가설을 세우고 구현/적용
5. 평가
	- 실험 설계
	- 테스트셋 구성
6. 배포
	- RESTful API를 통한 배포
	- 상황에 따라 유지/보수

## Preprocessing Workflow
1. 데이터(코퍼스) 수집
	- 구입, 외주
	- 크롤링을 통한 수집
2. 정제
	- Task에 따른 노이즈 제거
	- 인코딩 변환
3. 레이블링(Optional)
	- 문장마다 또는 단어마다 labeling 수행
4. Tokenization
	- 형태소 분석기를 활용하여 분절 수행
5. Subword Segmentaion(Optional)
	- 단어보다 더 작은 의미 단위 추가 분절 수행
6. Batchify
	- 사전 생성 및 word2index 맵핑 수행
	- 효율화를 위한 전/후처리

## Service Pipeline
1. 정제
	- 학습 데이터와 같은 방식의 정제 수행
2. Tokenization
	- 학습 데이터와 같은 방식의 분절 수행
3. Subword Segmentation
	- 학습 데이터로부터 얻은 모델을 활용하여 똑같은 분절 수행
4. Batchify
	- 학습 데이터로부터 얻은 사전에 따른 word2index 맵핑 수행
5. Prediction
	- 모델에 넣고 추론 수행
	- 필요에 따라 search 수행(자연어 생성)
6. Detokenization(Optional)
	- 사람이 읽을 수 있는 형태로 변환(index2word)
	- 분절 복원

## 말뭉치(Corpus)
- 자연어처리를 위한 문장들로 구성된 데이터셋
- 복수표현: Corpora
- 포함된 언어 숫자에 따라
	- Monolingual Corpus
	- Bi-lingual Corpus
	- Multilingual Corpus
- Parallel Corpus: 대응되는 문장 쌍이 labeling 되어 있는 형태

### 코퍼스 수집
#### 데이터 구입 및 외주의 한계
- 구입
	- 정제 및 레이블링이 완료된 양질의 데이터를 얻을 수 있음
	- 양이 매우 제한적
	- 구입처: 대학교, 한국전자통신연구원(ETRI), 플리토 등
- 외주
	- 수집, 정제 및 레이블링을 외주 줄 수 있음
	- 가장 높은 비용 -> 양이 매우 제한적
	- 품질 관리를 위한 인력이 추가로 필요

#### 무료 공개 데이터
- 공개 사이트
	- AI-HUB
	- WMT competention
	- Kaggle
	- OPUS
- 양이 매우 제한적
- 한국어 코퍼스는 흔치 않음

#### Crawling
- 무한한 양의 코퍼스 수집 가능
	- 원하는 도메인 별로 수집 가능
- 품질이 천차만별이며, 정제 과정에 많은 노력 필요
	- 특수문자, 이모티콘, 노이즈, 띄어쓰기
- 아직은 회색지대, 적접한 절차에 따른 크롤링이 필수
	- robots.txt 참고(!wget https://www.ted.com/robots.txt)

### 코퍼스 정제
1. 기계적인 노이즈 제거
	- 전각문자 변환
		- 유니코드 이전의 한글, 한자, 일본어
		- 한자 등과 함께 표기된 반각 문자로 표기 가능한 경우 반각 문자로 치환
	- Task에 따른 (전형적인) 노이즈 제거
2. Interactive 노이즈 제거
	- 코퍼스의 특성에 따른 노이즈 제거
	- 작업자가 상황을 확인하며  작업 수행
	- 정규식을 활용한 정제
		- 정규식(regular expression)을 활용하면 복잡한 규칙의 노이즈도 제거/치환 가능
		- 코딩 없이 단순히 텍스트 에디터(Sublime Text, VSCode 등)도 가능
	- 규칙에 의해 노이즈를 제거하기 때문에, 노이즈 전부를 제거하는 것은 어려움 -> 반복적인 규칙 생성 및 적용 과정이 필요
	- 끝이 없는 과정
		- 노력과 품질 사이의 trade-off
		- Sweet spot을 찾아야함
3. 대소문자 통일
	- 코퍼스에 따라 대소문자 표기법이 다름
	- 하나의 단어를 다양하게 표현하면 희소성이 높아짐
	- 딥러닝의 시대에 오면서 필요성 하락 및 생략 가능
4. 주의할 점
	- Task에 따른 특성
		- 풀고자 하는 문제의 특성에 따라 전처리 전략이 다름
		- 신중한 접근이 필요
			- 이모티콘(감성분석에서 중요)
	- 언어, 도메인, 코퍼스에 따른 특성
	- 각 언어, 도메인, 코퍼스 별 특성이 다르므로 다른 형태의 전처리 전략이 필요

### RegEx 적용 방법
1. Text Editor 활용
	- 파일을 열어 적용 과정을 보면서 정제
	- 바로 결과를 확인할 수 있음
	- 적용 과정이 log로 남지 않음
		- 재활용 불가
2. 전용 모듈 작성 및 활용
	- Python 등을 활용하여 모듈을 만들고 regex 리스트를 파일로 받아서 처리
	- 한번에 모든 regex를 적용
		- 중간 결과 확인 불가
	- regex 재활용 가능

#### 코드
- []: [2345egh] 다음 중 하나라도 있으면 알람
- [-]: [2-5c-e] 2~5, c~e
- [^]: [^2-5c-] not
- (): (x)(yz) x를 변수 1에 지정, yz를 변수 2에 지정
- ([a-z])bc([a-z]) -> \1\2
- |: (x|y)
- ?: x? 0번 나타나거나 1번 나타남
- +: x+ x가 한 번 이상 나타남
- *: x* x가 나타나지 않을 수도, 반복될 수도 있음
- {n}: x{n} n번 반복
- {n,}: x{n, } n번 이상 반복
- {n,m}: x{n,m} n번부터 m번까지 반복
- .: any character, 매우 강력한 표현. 유의해서 사용해야 함
- ^$: ^x$ 문장의 시작과 끝을 표시
- ^(positive|negative)\t[.,0-9\-=|;]+$

### 코퍼스 레이블링
#### Label
- Text Classification
	- INPUT: sentence
	- OUTPUT: class
	- TSV 형태의 하나의 파일
		- 각 row가 문장과 대응되는 레이블
		- 문장 column과 레이블 column 구성
- Token Classification
	- INPUT: sentence
	- OUTPUT: tag for each token -> sequence
- Sequence-to-Sequence
	- INPUT: sentence
	- OUTPUT: sentence

### Tokenization
- 두 개 이상의 다른 token들의 결합으로 이루어진 단어를 쪼개어, vocabulary 숫자를 줄이고, 희소성(sparness)을 낮추기 위함
#### Sentence Segmentation
- 보통 훈련 시 우리가 원하는 입력 데이터는
	- 1 sentence/line
- 우리가 수집한 corpus는
	- 한 라인에 여러 문장이 들어있거나
	- 한 문장이 여러 라인에 들어있음
- Sentence Segmentation을 통해 원하는 형태로 변환
	- 마침표 등을 단순히 문장의 끝으로 처리하면 안됨
- NLTK를 활용하여 변환 가능
	- from nltk.tokeniz import sent_tokenize

#### Korean Tokenization
- 교착어: 어근에 접사가 붙어 다양한 단어가 파생됨
- 띄어쓰기 통일의 필요성
- 접사를 분리하고 희소성을 낮추기
- 영어: 띄어쓰기가 이미 잘 되어 있음. NLTK를 사용하여 comma 등 후처리
- 중국어: 기본적인 띄어쓰기가 없음. character 단위로 사용해도 무방
- 일본어: 기본적인 띄어쓰기가 없음

#### 형태소 분석 및 품사 태깅(Part of Speech Tagging)
- 형태소 분석: 형태소를 비롯하여, 어근, 접두사/접미사, 품사(POS, part-of-speech) 등 다양한 언어적 속성의 구조를 파악하는 것
- 품사 태깅: 형태소의 뜻과 문맥을 고려하여 그것에 마크업을 하는 일

## 분절 길이에 따른 장단점
### 평균 길이
#### 짧을 수록
- Vocabulary 크기 감소
	- 희소성 문제 감소
- OoV(Out of Vocabulary)가 줄어듬
- Sequence의 길이가 길어짐
	- 모델의 부담 증가
- 극단적 형태: Character
#### 길 수록
- Vocabulary 크기 증가
	- 희소성 문제 증대
- OoV가 늘어남
- Sequence의 길이가 짧아짐
	- 모델의 부담 감소
### 정보량에 따른 이상적인 형태
- 빈도가 높을 경우 하나의 token으로 나타내고
- 빈도가 낮을 경우 더 잘게 쪼개어, 각각 빈도가 높은 token으로 구성한다

## 서브워드 분절
### 서브워드
- 단어보다 더 작은 의미 단위
- 많은 언어들에서, 단어는 더 작은 의미 단위들이 모여 구성됨
- 따라서 이러한 작은 의미 단위로 분절할 수 있다면 좋을 것
- 이를 위해선 언어별 subword 사전이 존재해야 할 것

### Byte Pair Encoding(BPE) 알고리즘
- 압축 알고리즘을 활용하여 subword segmenation을 적용
- 학습 코퍼스를 활용하여 BPE 모델을 학습 후, 학습/테스트 코퍼스에 적용
- tokenization을 수행하고, 기존 띄어쓰기와 구분을 위해 _ 삽입(U+2581)
- 장점:
	- 희소성을 통계에 기반하여 효과적으로 낮출 수 있다
	- *언어별 특성에 대한 정보 없이, 더 작은 의미 단위로 분절 할 수 있다 -> 통계적으로
	- OoV를 없앨 수 있다 -> 성능상 매우 큰 이점
- 단점:
	- 학습 데이터 별로 BPE 모델도 생성됨
- Training
	1. 단어 사전 생성(빈도 포함)
	2. Character 단위로 분절 후, pair별 빈도 카운트
	3. 최빈도 pair를 골라, merge 수행
	4. Pair별 빈도 카운트 업데이트
	5. 3번 과정 반복
- Applying
	1. 각 단어를 character 단위로 분절
	2. 단어 내에서 '학습 과정에서 merge에 활용된 pair의 순서대로' merge 수행
- 한국어의 경우
	- 띄어쓰기가 제멋대로인 경우가 많으므로, normalization 없이 바로 subword segmentation을 적용하는 것은 위험
	- 따라서 형태소 분석기를 통한 tokenization을 진행한 이후, subword segmentation을 적용하는 것을 권장

### OoV가 미치는 영향
- 입력 데이터에 OoV가 발생할 경우, <UNK> 토큰으로 치환하여 모델에 입력
	- e.g. 나는 학교에 가서 밥을 먹었다 -> 나는 <UNK>에 가서 <UNK>을 먹었디
- 특히, 이전 단어들을 기반으로 다음 단어를 예측하는 task에서 치명적
	- e.g. NLP
- 모르는 단어지만, 알고있는 subword들을 통해 의미를 유추해볼 수 있음
	- e.g. 버카충

## Detokenization
1. whitespace를 제거
2. __을 white space로 치환
3. _를 제거
-  1~3번 리눅스 코드
	- sed "s/ //g" | sed "s/__/ /g" | sed "s/_//g" 

## 병렬 코퍼스 정렬
### 왜 Parallel Corpus가 필요한가?
- 대부분의 경우, 문서 단위의 matching은 되어 있지만, 문장 단위는 되어 있지 않음 -> Champollion으로 해결
### Champollion
- 최초로 이집트 상형문자를 해독한 역사학자
- Perl로 제작됨
- ratio parameter의 역할
	- source 언어의 character 당 target 언어의 character 비율
- 단어 번역 사전에 기반하여 사전을 최대한 만족하는 sentence align을 찾는 방식
- 단어 사전이 필요 -> MUSE
### MUSE
- 페이스북에서 개발
- 전혀 상관없는 두 코퍼스에 대해 유사하게 만들어 준다
	- stories <> 이야기, song <> 노래

## 미니배치 만들기
### Read Text & Build Dictionary
- 빈도 순으로 단어 사전 정렬
- 필요에 따라 min_count 보다 작은 빈도를 갖는 단어는 제외
	- 또는 max_vocab에 따라 빈도순으로 어휘를 제외하기도 함
- 필요에 따라 특수 토큰도 어휘 사전에 포함
	- <BOS> Begin of Sentence, <EOS> End of Sentence, <UNK>, <PAD>

### Chunking and padding
- 미니배치 형태: (batch_size, length, |V|)
- One-hot 벡터를 다 저장할 필요가 없음
	- (batch_size, length, 1) = (batch_size, length)
- Sequence 차원의 크기는 미니배치 내의 가장 긴 문장에 의해 결정됨
	- 각 샘플별 모자라는 부분은 padding으로 대체, 따라서 <PAD> 토큰이 필요
	- PyTorch의 PackedSequence를 활용할 경우 <PAD> 생략 가능
- 문제점: 쓸 때 없이 패딩이 많이 채워졌을 때 학습시 비효율성 발생 -> 길이에 따라 미니배치 생성
	- 효율적인 학습이 가능한 미니배치 만들기 -> TorchText
		1. 코퍼스의 각 문장들을 길이에 따라 정렬
		2. 각 token들을 사전을 활용하여 str -> index 맵핑
		3. 미니배치 크기대로 chunking
		4. 각 미니배치 별 텐서 구성 및 padding
		5. 학습 시 미니배치 shuffling하여 iterative하게 반환

## 에러 해결 방법
* 우분투에서 윈도우10 디렉토리 접근하기 cd /mnt/c
* pip install -U "jpype1<1.1"
* 우분투 폴더 찾는 법: 탐색기에 \\wls$
* 파일 실행시 permission denied 뜰 때: chmod +x file_name
