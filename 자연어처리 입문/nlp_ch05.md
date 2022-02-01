# ch 05 이론
## Text Classification
- 텍스트를 입력으로 받아 원하는 항목에 대한 수치를 출력하는 것
	-  e.g. 감성 분석(sentiment analysis), 주제 분류(topic classification)
	- 가격도 싸고 상품 품질은 괜찮은데 배송이 늦어서 화가 나네요 -> 품질:긍정, 배송:부정, 가격:긍정, 종합:부정
- 문장을 latent space에 projection하여 decision boundary를 찾는 것

### using RNN
- Many to One
- with Bidirectional Multi-layered RNN
- Non-autoregressive task이므로 입력을 한번에 받게 된다
	- 따라서 모든 time-step을 한번에 병렬로 처리 가능
- Feed-forward 과정
- - One-hot vectors -> Embedding Layer -> Multi-layered Bi-directional RNN -> Softmax Layer -> Distribution -> Cross Entropy Loss -> One-hot vectors
	1. One-hot vector를 입력으로 받아 embedding layer에 넣어준다
	2. Embedding vector를 RNN에 넣어 출력을 얻는다
	3. RNN의 출력값 중 마지막 time-step의 값을 잘라낸다
	4. 잘라낸 값을 softmax layer에 통과시켜 각 클래스별 확률값을 얻는다
- Problem Definition
	- INPUT 문장
		- input tensor shape: (n,length,|V|)
	- OUTPUT: 클래스
		- output tensor shape: (n,|C|)
	- 추후 CNN Classifier를 구현했을 때도 같은 인터페이스를 지닐 것
- Architecture
	- Embedding Layer -> LSTM -> Linear Layer -> Softmax

### using CNN
- 특정 패턴을 찾는다

### 결론
- 신경망은 텍스트를 입력으로 받아 context vector로 인코딩
	- RNN의 경우, 단어의 출현 여부와 순서에 따른 정보를 종합적으로 활용
	- CNN의 경우, 문구의 출현 여부를 종합적으로 활용
- Context vector는 y를 예측하기 위한 정보를 담고 있을 것
- 자연어 생성 미리보기:
	- Sequence-to-Sequence encoder의 경우, 문장을 생성하기 위한 context vector 생성

### BERT
- Google이 2018년에 공개한 사전 훈련된 모델
- 트랜스포머를 이용하여 구현되었으며, 위키피디아와BooksCorpus(8억 단어)와 같은 레이블이 없는 텍스트 데이터로 사전 훈련된 언어 모델
- Big-LM(굉장히 큰 네트워크)
- Masked Language Models(MLM)
	- Denoising Autoencoders
- Next Senetence Prediction(NSP)
- Bi-directional Sequential Modeling(using Transformer)
- 다른 작업에 대해서 파라미터 재조정을 위한 추가 훈련 과정을 **파인 튜닝(Fine-tuning)**

