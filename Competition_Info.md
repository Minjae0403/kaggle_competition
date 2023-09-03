CAFA 5 Protein Function Prediction 
================================
(Predict the biological function of a protein)

https://www.kaggle.com/competitions/cafa-5-protein-function-prediction/data

기간 
------------------
2023-06-04 ~ 2023-08-25

진행방식
--------------------
3명에서 기초 공부 후 각자의 방식을 연구 후 일주일에 한번 미팅으로 논의 및 질의 후 모델 개발

인원 
-----------------
3명

제공 Data
-----------------
train_sequences.fasta - amino acid sequences for proteins in training set
train_terms.tsv - the training set of proteins and corresponding annotated GO terms
train_taxonomy.tsv - taxon ID for proteins in training set
go-basic.obo - ontology graph structure
testsuperset.fasta - amino acid sequences for proteins on which the predictions should be made
testsuperset-taxon-list.tsv - taxon ID for proteins in test superset (Note: you may need to use encoding="ISO-8859-1" to read this file in pandas)
IA.txt - Information Accretion for each term. This is used to weight precision and recall (see Evaluation)
sample_submission.csv - a sample submission file in the correct format

분석 방법
--------------------
(기존에 사용)
1. 서열 유사성 검색: 주어진 아미노산 서열을 데이터베이스에 있는 이미 알려진 단백질 서열과 비교하여 유사한 서열을 찾는 방법입니다. 대표적인 도구로는 BLAST (Basic Local Alignment Search Tool)가 있습니다. 이를 사용하여 유사한 서열을 찾아 단백질의 기능을 유추할 수 있습니다.

2. 단백질 도메인 예측: 아미노산 서열에서 단백질 도메인을 예측하는 방법입니다. 도메인은 단백질 내에서 기능적인 단위로 작용하며, 서로 다른 도메인들의 조합으로 단백질의 기능이 결정됩니다. 대표적인 도구로는 Pfam, SMART, InterPro 등이 있습니다.

3. 기계 학습과 인공지능: 기계 학습과 인공지능 기법을 활용하여 아미노산 서열과 단백질 기능 간의 관계를 학습하고 예측하는 방법입니다. 이를 위해 주어진 아미노산 서열과 기능 정보로 구성된 데이터셋을 사용하여 모델을 훈련시킬 수 있습니다. 주로 사용되는 기법으로는 서포트 벡터 머신(SVM), 신경망(Neural Networks), 랜덤 포레스트(Random Forest) 등이 있습니다.

4. 구조 기반 예측: 아미노산 서열로부터 단백질의 3차원 구조를 예측한 뒤, 구조를 기반으로 기능을 예측하는 방법입니다. 이는 단백질 구조와 기능 간의 관련성을 활용하여 예측합니다. 대표적인 도구로는 I-TASSER, SWISS-MODEL, Phyre2 등이 있습니다.


선택한 분석방법
------------------

서열 유사성 검색 + 딥러닝 + pro-bert/T5 + LSTM

주어진 데이터는 아미노산 서열, 기능, 종 등이 주어졌습니다.

단백질 도메인 또는 구조기반 예측을 하고자 하면 추가 데이터를 외부에서 찾아서 매칭 시켜야 하기 때문에 전처리 및 모델을 구성하는데 드는 시간이 부족할 것이라고 생각이 되어서 딥러닝 방법과 융합 하여 사용하는 것을 선택 했습니다.

임베딩 방식은 bert를 finetuning한 probert와 T5방식 두 가지를 선택하였습니다.
bert임베딩 방식은 양방향에서 읽고 위치 정보까지 임베딩하기 때문에 위치 정보가 중요한 아미노산 배열에 어울리며, 자연어 처리에 사용한다는 장점까지 있어서 사용하였습니다.
T5 모델은 bert와 비슷한 PLM모델이며 작은 모델인것에 비해 NLP에 좋은 성능을 가지고 있다는 평이 있어서 사용을 하였습니다.

모델은 LSTM을 사용하였습니다.
RNN의 기울기 소실 문제를 해결할 수 있으며 자연어 처리(NLP)분야에서 많이 사용되고 있습니다.
아미노산 서열도 자연어와 같이 위치 정보가 중요하기 때문에 어울린다고 생각을 하여 사용을 하였습니다.

