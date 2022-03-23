# Leveraging BERT for Extractive Text Summarization on Lectures. 
**Derek Miller** | Georgia Institute of Technology | Atlanta, Georgia | dmiller303@gatech.edu  
[원문보기](https://arxiv.org/abs/1906.04165)


## ABSTRACT
- 텍스트 임베딩 및 K-평균 클러스터링을 위해 BERT 모델을 사용하여 요약 선택을 위해 중심과 가장 가까운 문장을 식별하는 파이썬 기반 RESTful 서비스인 "강의 요약 서비스" 프로젝트  
- 학생들이 원하는 문장의 수에 따라 강의 내용을 요약할 수 있는 유틸리티 제공
- 요약 작업 외 강의 및 요약 관리, 클라우드 상에 콘텐츠를 저장하여 협업에 활용 가능 
- 추출 텍스트 요약에 BERT를 사용한 결과는 유망했지만, 여전히 모델이 어려움을 겪는 분야가 있어 향후 추가 개선을 위한 연구 제안
- [깃허브](https://github.com/dmmiller612/lecture-summarizer)  


## INTRODUCTION
### 자동 텍스트 요약 방식
1. **추출적 요약 Extractive Text Summarization**
- emulates human summarization in that it uses a vocabulary beyond the specified text
- difficult to automatically produce
- 새로운 단어를 사용하거나 다시 표현하는 것
2. **추상적 요약 Abstractive Text Summarization**
- utilizes the raw structures, sentences or phases of the text
- outputs a summarization, leveraging only the content from the source material
- 특정 메트릭을 기반으로 문서에서 문장의 순위를 지정하고 점수를 매긴 다음 입력 문서 의 대표적인 요약으로 상위 k 문장을 선택하는 작업 

## METHOD
1. 입력 단락을 깨끗한 문장으로 토큰화
2. 문장을 BERT 모델에 전달하여 임베딩을 출력
3. 임베딩 문장 을 클러스터링 목적을 위해 K-Means 에 대한 입력으로 사용하는 파이프라인
4. 클러스터링이 완료되면 요약 문장의 최종 세트는 해당 클러스터의 중심에 가장 가까운 문장으로 판단  
    → 결과가 유망하지만 여전히 어려움을 엮는 영역 존재
    → 몇가지 전처리 단계가 고려됨

### BERT
NLP에서 성능이 높은 BERT 구조 선정
여러 레이어에서 생성된 임베딩을 실험  
여러 임베딩 레이어에서 생성된 클러스터를 개별적으로 수동으로 분석  
- **(N-2)** 레이어의 평균 단어 임베딩 이 Udacity 데이터에 대한 클러스터를 가장 잘 나타내고 생성할 수 있음을 발견  
- 최상의 입력 표현을 얻기 위해 입력 문장의 최종 표현으로 <u>GPT-2 및 BERT 에서 임베딩된 최종 레이어[CLS]의 앙상블</u> 사용 제안
- 마지막 단계로 사용자가 K-Means 클러스터링의 K 값 입력 가능

### WEAKNESSES
**INITIAL WEAKNESSES**
큰 강의의 요약, 컨텍스트 워드의 처리의 어려움, 대화 언어의 처리와 같은 다른 방법론들이 가지고 있는 것과 동일  

**MODEL WEAKNESSES**
100개 이상의 문장이 들어오는 경우 대표성을 갖는 문장의 비율이 적다  
→ 가장 가까운 클러스터에 여러 문장을 포함하는 방법은 어떨까?  
⇢ 중심부가 수렴된 위치에 따라 대표성이 떨어질 가능성 O  
⇢ 요청된 것보다 더 많은 문장을 추가하고 도구에 대한 사용자 경험을 저하시킬 수 있다  
=> 서비스 포함 X  
  
"this", "these", "also"와 같이 추가 문맥이 필요한 문장이 선택되는 경우  
→ 이러한 단어를 포함하는 문장을 삭제 ⇢ 요약의 질을 크게 떨어뜨림  
→ (더 많은 시간이 주어진 경우) NLTK를 사용하여 언어 부분을 찾고 대명사와 키워드를 적절한 값으로 대체   
⇢ 일부 강의에서는 과거 2~3문장의 문맥을 언급했던 문맥 단어가 포함되어 있어 어떤 항목이 실제 문맥인지 판단하기 어려움  

### FUTURE IMPROVEMENTS
- 모델을 미세하게 조정
- 요약에서 누락된 문맥의 공백을 메우고 강의를 나타낼 최적의 문장 수를 자동으로 결정

### CONCLUSION 
- BERT라고 하는 최신 딥 러닝 NLP 모델을 활용하여, 컨텍스트를 가장 중요한 문장과 결합함으로써 요약 품질에서 TextRank와 같은 오래된 접근법이 꾸준히 개선되는 중
- 자동 추출 요약 서비스는 완벽하진 않았지만, 오래된 접근 방식에 비해 품질 면에서 높은 성능을 보임
