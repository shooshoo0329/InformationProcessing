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
→ 클러스터링이 완료되면 요약 문장의 최종 세트는 해당 클러스터의 중심에 가장 가까운 문장ㅇ
