# kaggle_competition
CAFA 5 Protein Function Prediction competition

## 1. 목적: 단백질의 go term 예측.
- 단백질 서열을 통해 해당 단백질의 molecular function, cellular component, biological process를 예측하는 모델 구축.
- Train Data: 14만개, Test Data: 14만개.

## 2. Library
- Numpy, Pandas, Pytorch, Tensorflow, SeqIO

## 3. 예측 방법
1) Fully connected neural network를 활용한 예측 모델.
2) 단백질 벡터간 유사도를 통한 예측 모델.
3) 두 가지 모델을 융합한 예측 모델.

##4. 예측 방법
4.1 Fully connected neural network를 활용한 예측 모델.
: 이민재님 작성

##4.2 단백질 벡터간 유사도를 통한 예측 모델.
4.2.1 원리: 단백질 벡터화 -> 유사도 측정 -> go term 예측
- 비슷한 아미노산 서열을 갖는 단백질은 비슷한 기능을 수행함 (molecular function).
- 비슷한 기능을 수행한다면 세포 내에서 위치 (cellular component)와 생물학적 기작(biological process)이 유사할 것으로 판단.
- 따라서, 기존에 알고있는 단백질 벡터와 예측하고자 하는 단백질 벡터간 유사도(거리)를 측한 후, 특정 거리 이내에 있는 단백질은 같은 go term(같은 기능)을 공유한다고 판단한다. 

##4.2.2 장단점
1) 장점
- 비슷한 아미노산 sequence를 갖는 단백질에 대한 예측도는 높을 것으로 예상된다.
- 벡터간 거리를 기준으로 하여 연산이 단순하다.

2) 단점
- (보완 가능) 비슷한 아미노산 서열이라도 3D 구조가 다르면 잘못 예측할 가능성이 높음.
- (보완 가능) 수만개의 단백질 벡터간 유사도를 계산 하려면 높은 computing 능력이 요구된다. 
- (보완 불가능) 기존 데이터에 없는 새로운 아미노산 sequnce를 갖는 단백질은 예측할 수 없음. 

##4.2.3 설계 단계 
1) 단백질 서열 벡터화
- 단백질 아미노산 서열을 벡터화 할 시 3차원 구조가 벡터에 잘 반영이 되는게 핵심 기술임.
- 자연어처리 모델의 경우 아미노산 서열의 3차원 구조가 반영이 될수 있도록 벡터화 한다고 알려져 있음.
- 따라서, 자연어 처리 모델 기반인 벡터화 모델을 사용함: ProtBERT, ESM-2 등.   
- 14만개 데이터 중 2만개에 대한 단백질 sequence 벡터화. (RAM 용량의 한계로 2만개만 샘플링).

2) 유사도 측정 
- 측정 모델: Euclidean distance
- 2만개의 test 데이터와 14만개의 test 데이터간 각각의 벡터간 거리를 측정.
- 특정 임계값 이하의 거리를 갖는 단백질은 비슷한 단백질이라고 판단하고 go term을 가져옴.   

##5. 참고 문헌 및 사이트
- Maria Littmann, Embeddings from deep learning transfer GO annotations beyond homology., Scientific Reports (2021) 
- Nadav Brandes, ProteinBERT: a universal deep-learning model of protein sequence and function., Bioinformatics (2022)
- Zeming Lin, Evolutionary-scale prediction of atomic level protein structure with a language model., bioRxiv (2022)
- https://huggingface.co/facebook/esm2_t36_3B_UR50D
