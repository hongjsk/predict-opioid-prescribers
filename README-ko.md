## Watson 스튜디오와 scikit-learn을 이용한 머신러닝으로 미국내 마약성 진통제 관련 의사 처방 예측하기

*다른 언어로 보기: [English](README.md).*

> Data Science Experience(DSX)가 이제 Watson Studio로 이름이 바뀌었습니다. 이 코드 패턴에 포함된 몇몇 이미지에 Data Science Experience라는 이전 이름이 보이더라도, 각 단계 및 절차는 정상 작동합니다.

이 코드 패턴은 `scikit learn`과 Watson Studio의 `python`으로 [2014 kaggle 데이터 집합](https://www.kaggle.com/apryor6/us-opiate-prescriptions/data)에 기반한 마약성 진통제인 오피오이드 처방의를 예측하는 데에 초점을 맞춰 안내합니다.

마약성 진통제인 오피오이드 처방전의 과다사용은 미국에서 점점 큰 문제가 되어가고 있으며, 심지어 최근의 비상 사태 선포를 야기했습니다. 비록 우리와 같은 데이터 사이언티스트들이 혼자서 이 문제를 해결할 수는 없지만, 데이터를 파고들어 정확하게 무슨 일인지 그리고 현재 상황을 고려해 앞으로 어떤 일이 발생할지 파악할 수는 있습니다.

이 코드 패턴은 다음을 목표로 하고 있습니다: 오피오이드 과다복용 사망자에 대해 주(州)별로 살펴보는 것 뿐만 아니라 서로 다른, 고유한 의사들, 그들의 자격 정보, 세부 전공, 2014년에 오피오이드 처방 여부, 그들이 처방한 약들의 구체적인 이름을 포함하는 케글 데이터를 살펴봅니다. Watson Studio 노트북의 데이터를 어떻게 탐색하는지, Pixie Dust를 이용하여 지리 정보를 포함한 다양한 방법으로 찾아낸 초기 결과물을 어떻게 시각화 하는지 따라 가봅니다. Pixie Dust는 여러분이 데이터 시각화를 매우 빨리 탐색할 때 사용 가능한 훌륭한 라이브러리 입니다. 문자 그대로 코드 한 줄 정도만 있으면 됩니다! 초기 탐색이 한 번 끝나고나면, 이 코드 패턴은 머신러닝 라이브러리로 scikit learn을 사용하여, 몇 가지 모델을 훈련 시키고 오피오이드 처방자들을 가장 정확하게 예측하는 모델을 파악하게 됩니다. 여러분에게 낯설지도 모르지만, Scikit learn은 데이터 사이언티스트들이 쉽게 사용할 수 있기에 흔하게 사용하는 머신러닝 라이브러리입니다. 특히, 라이브러리를 사용함으로써 여러분은 상대적으로 적은 수의 코드만 구현하는 다수의 머신러닝 분류기에 쉽게 접근 할 수 있게 됩니다. 더욱이, scikit learn은 여러분이 결과물을 시각화하고 보여줍니다. 이 때문에, 이 라이브러리가 머신러닝 수업에서 서로 다른 분류기의 동작의 차이를 설명할 때 종종 사용됩니다-이 코드 패턴이 강조하는 결과물 비교처럼 말이죠! 준비 되셨나요?

![](doc/source/images/architecture.png)

## 순서

1. IBM Watson Studio 서비스에 로그인하십시오.
2. Watson Studio에 data asset으로 데이터를 업로드 하십시오.
3. Watson Studio에 포함된 노트북을 실행하고 앞서 만든 data asset을 입력하십시오.
4. pandas로 데이터를 탐색하십시오
5. Pixie Dust로 데이터 시각화를 생성하십시오.
6. scikit learn으로 머신 러닝 모델을 훈련하십시오.
7. 예측 성능을 평가 하십시오.

## 포함된 구성요소

* [IBM Watson Studio](https://dataplatform.ibm.com): 매니지드 Spark와 같이 IBM의 value-added를 포함한 구성 및 협업 환경에서 제공되는 RStudio, Jupyter 그리고 Python을 이용하여 데이터를 분석하십시오.
* [Jupyter Notebook](http://jupyter.org/): 라이브 코드, 수식, 시각화 및 설명 텍스트를 생성하고 공유 할 수 있는 오픈 소스 웹 애플리케이션.
* [PixieDust](https://github.com/ibm-watson-data-lab/pixiedust): IPython 노트북용 Python 라이브러리를 제공합니다.

## 주요 기술

* [Data Science](https://medium.com/ibm-data-science-experience/): 지식과 인사이트를 추출하기 위해 정형, 비정형 데이터를 분석하는 시스템 및 과학적 방법.
* [Python](https://www.python.org/): Python은 여러분이 보다 빠르게 작업하고 효과적으로 시스템을 통합하게하는 프로그래밍 언어입니다.
* [pandas](http://pandas.pydata.org/): 데이터 구조를 손쉽게 사용 할 수 있는 고성능 Python 라이브러리리.

# 단계

이 코드 패턴은 두 가지 활동으로 구성되어 있습니다:

* [IBM Watson Studio에서 Jupyter 노트북을 이용하기 실행하기](#ibm-watson-studio에서-jupyter-노트북을-이용하기-실행하기).
* [데이터 분석 및 예측하기](#데이터-분석-및-예측하기).

## IBM Watson Studio에서 Jupyter 노트북을 이용하기 실행하기 

1. [Watson Studio 가입하기](#1-watson-studio-가입하기)
2. [Watson Studio 프로젝트 생성하기](#2-watson-studio-프로젝트-생성하기)
3. [노트북 생성하기](#3-노트북-생성하기)
4. [데이터 업로드하기](#4-데이터-업로드하기)
5. [노트북 실행하기](#5-노트북-실행하기)
6. [저장 및 공유하기](#6-저장-및-공유하기)

### 1. Watson Studio 가입하기

IBM [Watson Studio](https://dataplatform.ibm.com)에 로그인 또는 가입하십시오.

> 참고: 남아 있는 Watson Studio 설정 단계를 건너뛰고 완성된 노트북을 보고 따라가고 싶다면, 간단하게:
> * 완성된 [노트북](https://dataplatform.ibm.com/analytics/notebooks/c32975c1-3994-42cc-8e2d-3f579ceebf63/view?access_token=cdb14a077ed4746b09b1dbaa05aee70133589f001dbb7582ba4e7fcfdd73a905)과 그 결과를 확인하십시오.
> * 노트북을 보고 있는 동안, 여러분은 이를 선택적으로 다운로드 후 저장하여 나중에 사용 할 수 있습니다.
> * 완료되면, [데이터 분석 및 예측하기](#데이터-분석-및-예측하기) 섹션으로 넘어가 계속 이 코드 패턴을 계속하십시오.

### 2. Watson Studio 프로젝트 생성하기

* Watson Studio 초기화면에서 `New Project` 옵션과 `Data Science`을 선택하십시오

![](https://raw.githubusercontent.com/IBM/pattern-images/master/watson-studio/project_choices.png)

* Watson Studio에서 project를 생성하기 위해 project에 대한 이름과 함께 `Cloud Object Storage` 서비스를 새로 만들거나, 기존 IBM Cloud 계정으로 만든 것 중 하나를 선택해 주십시오.

![](https://raw.githubusercontent.com/IBM/pattern-images/master/watson-studio/new_project.png)

* project 생성이 성공하면, 여러분의 project에 대한 대시보드로 이동하게 됩니다. 외부 assets (데이터 집합과 노트북들) 그리고 IBM 클라우드 서비스를 project와 연결 될 때 사용 할 수 있도록 `Assets`와 `Settings` 탭에 대해 메모 하십시오.

![](https://raw.githubusercontent.com/IBM/pattern-images/master/watson-studio/project_dashboard.png)

### 3. 노트북 생성하기

* project 대시보드 화면에서, `Assets` 탭을 클릭하고, `+ New notebook` 버튼을 클릭하십시오.

![](https://raw.githubusercontent.com/IBM/pattern-images/master/watson-studio/new_notebook.png)

* 여러분의 노트북 이름을 입력하고 실행할 runtime을 선택하십시오, 이 패턴의 경우 연관된 Spark runtime을 사용하게 됩니다.

![](https://raw.githubusercontent.com/IBM/pattern-images/master/watson-studio/notebook_spark.png)

* 이제 이 저장소에 있는 노트북의 URL 지정하기 위해 `From URL` 탭을 선택하십시오.

![](https://raw.githubusercontent.com/IBM/pattern-images/master/watson-studio/notebook_with_url_spark.png)

* 다음 URL을 입력하십시오:

```
https://github.com/IBM/predict-opioid-prescribers/blob/master/notebooks/opioid-prescription-prediction.ipynb
```

* `Create` 버튼을 클릭하십시오.

### 4. 데이터 업로드하기

* project 대시보드 화면으로 돌아가서 `Assets` 탭을 선택하십시오.
* 이 project는 세 개의 데이터 집합을 가지고 있습니다. 이 세 데이터 집합을 data asset으로 여러분의 프로젝트에 업로드 하십시오. 각각의 데이터 집합을 오른쪽에 있는 팝업 섹션으로 로딩하여 진행 하십시오. 어떻게 보여져야 하는지 아래 스크린샷을 확인 하십시오.   
* 완료되었다면, 노트북의 편집 모드로 진입 하십시오 (대시보드에서 여러분 노트북 옆에 있는 연필 아이콘을 클릭하십시오)
* 오른쪽 상단의 ``1001`` 데이터 아이콘을 클릭하십시오. 데이터 파일이 보여야 합니다. 
* 각각을 클릭하고 ``Insert Pandas Data Frame``을 선택하십시오. 동작을 수행하면, 전체 코드가 첫 번째 셀에 표시됩니다. 
* 원본 노트북과 일치하도록 여러분의 ``opioids.csv``가 ``df_data_1``로, ``overdoses.csv``가 ``df_data_2``로 그리고 ``prescriber_info.csv``가 ``df_data_3``로 저장되어 있는지 확인 하십시오. 여러분의 데이터가 노트북으로 로드 될 때 남겨진 곳을 기준으로 데이터 프레임의 연속으로 정의되어 있을 수 있는데, 이 경우 수정이 필요합니다. 이는 여러분의 데이터 ``opioids.csv``가 ``df_data_4``로, ``overdoses.csv``가 ``df_data_5``와 같은 식으로 나타나는 것을 말합니다. 데이터 프레임의 이름을 원본과 같도록 조정(미리 로드된 데이터 위치까지 삭제 후 데이터 프레임 이름을 변경) 하거나 아래 코드에 따라 변경하십시오. 이렇게하면 코드가 실행되도록 할 수 있습니다!

 ![](doc/source/images/project-assets.png)

### 5. 노트북 실행하기

노트북이 실행될 때, 노트북에 포함된 각각의 코드 셀이 위에서 아래까지 순서대로 실행됩니다.

각각의 코드 셀은 선택 가능하고 왼쪽 여백에 태그가 달리게 됩니다. 태그 형식은 `In [x]:` 입니다. 노트북의 상태에 따라, `x`는 다음과 같이 될 수 있습니다:

* 공백, 이는 해당 셀이 한 번도 실행되지 않았음을 의미합니다.
* 숫자, 이 숫자는 이 코드 단계의 실행된 상대적인 순서를 말합니다.
* `*`, 이는 해당 셀이 현재 실행 중인 것을 나타냅니다.

여러분의 노트북에 있는 코드 셀을 실행하는 여러가지 방법이 있습니다:

* 한번에 한 셀을 실행하기.
  * 셀을 선택하고, 툴바의 `Play` 버튼을 누르십시오.
* 순차적인 배치 모드로 실행하기.
  * `Cell` 메뉴바에서 몇 가지 선택 사항이 있습니다. 예를 들어, 여러분의 노트북에서 `Run All`로 모든 셀을 실행하거나,
    `Run All Below`로 현재 선택된 셀 부터 실행하여 이후 모든 셀을 실행 할 수 있습니다.
* 스케쥴에 맞춰 실행하기.
  * 노트북 패널의 오른쪽 상단에 있는 `Schedule` 버튼을 클릭하십시오.
    여러분의 노트북을 미래의 어떤 시간에 한 번 실행하거나, 특정 주기로 반복해서 실행하도록 스케쥴을 할 수 있습니다.
    
### 6. 저장 및 공유하기

#### 작업물 저장하는 법:

`File` 메뉴아래, 여러분의 노트북을 저장 할 수 있는 몇 가지 방법이 있습니다:

* `Save`는 어떤 버젼 정보 없이, 단순하게 노트북의 현재 상태를 저장합니다.
* `Save Version`은 날짜 및 타임스탬프를 담고 있는 버젼 태그와 함께 노트북의 현재 상태를 저장합니다.
  노트북에 최대 10개의 버젼이 저장될 수 있으며, 각각은 `Revert To Version` 메뉴 아이템을 선택하여 가져올 수 있습니다.

#### 작업물 공유하는 법:

여러분의 노트북은 노트북 패널의 오른쪽 상단에 있는 ``Share`` 버튼을 선택하여 공유할 수 있습니다.
이 작업의 최종 결과는 여러분의 노트북에 대해 “읽기-전용” 버전을 표시하는 URL 링크가 됩니다.
여러분은 노트북에서 공유될 부분을 명확하게 하기 위한 몇 가지 선택 사항이 있습니디:

* `Only text and output`: 노트북 화면의 모든 코드 셀을 삭제 합니다.
* `All content excluding sensitive code cells`: *sensitive* 태그가 붙은 모든 코드 셀을 삭제합니다.
  예를 들어, `# @hidden_cell` 는 여러분의 dashDB 신임 정보가 공유되는 것을 막을 때 사용됩니다.
* `All content, including code`: 노트북의 그대로를 표시합니다.
* 메뉴에 `download as`에 대한 다양한 선택 사항이 있습니다.

## 데이터 분석 및 예측하기
 
1. Python, pandas 그리고 Pixie Dust를 이용하여 서로 다른 데이터 집합을 탐색하십시오. 다시 한번 [Watson Studio를 따라 가십시오](https://dataplatform.ibm.com/analytics/notebooks/c32975c1-3994-42cc-8e2d-3f579ceebf63/view?access_token=cdb14a077ed4746b09b1dbaa05aee70133589f001dbb7582ba4e7fcfdd73a905).

여러분의 데이터와 익숙해지도록, 시각적으로 데이터를 탐색하고 데이터의 하위 집합을 살표 보십시오. 예를 들어, 우리는 California가 가장 높은 과다 사용 지역임을 볼 수 있는데, 인구를 바로로 잡을 때 West Virginia가 1인당 과다 사용 비율이 가장 높은 곳임을 알 수 있습니다.

2. Python으로 데이터 정리하십시오.

모든 데이터 집합은 자체적으로 불완정성을 가지고 있습니다. 우리가 가진 정보에서 주를 일관되게 만들고 컬럼 정보를 정수로 사용할 수 있도록 변경합니다.

3. scikit learn을 이용하여 오피오이드 처방자를 예측하는 여러 모델을 실행 하십시오.

여러분은 노트북이나 아래 이미지의 결과를 확인 할 수 있습니다. 이 단계에서 오피오이드 처방자 예측에 있어 가장 효과적인 모델이 무엇인지 평하기 위해 여러 머신 러닝 모델을 실행합니다. 이 코드 패턴의 범위를 넘는 것이긴 하지만, 이와 같은 오피오이드 처방자를 예측하는 것으로 여러분은 어떤 유형의 의사가 오피오이드를 처방하는지를 예측하는 것과 같은 프레임워크를 마련하게 됩니다. 더구나, 몇 년 더 데이터를 갖게되면 (2014년 이후) 우리는 또한 미래의 과다 사용율을 예측 할 수 있게 됩니다. 이제, 모델들을 살펴보도록 하겠습니다.

4. 모델을 평가하십시오.

코드는, 로컬 [notebooks](notebooks) 아래 있는 노트북이나 [여기](https://dataplatform.ibm.com/analytics/notebooks/c32975c1-3994-42cc-8e2d-3f579ceebf63/view?access_token=cdb14a077ed4746b09b1dbaa05aee70133589f001dbb7582ba4e7fcfdd73a905)에 있는 노트북을 보십시오!

## 샘플 결과물

다양한 분류기를 실행하고나면, 우리는 Random Forest, Gradient Boosting 그리고 Ensemble 모델이 오피오이드 처방자 예측에 있어 최고의 성능을 갖는다는 것을 알 수 있게됩니다.

![](doc/source/images/Screen%20Shot%202017-12-06%20at%202.30.47%20PM.png)

![](doc/source/images/studio-output.png)

잘 따라하셨습니다! 이제 이를 시도해보고 더 활용하거나 다른 유스케이스에 적용 해 보십시오!

## 링크

 - Watson Studio: https://datascience.ibm.com/docs/content/analyze-data/creating-notebooks.html.
 - Pandas: http://pandas.pydata.org/
 - Pixie Dust: https://ibm-watson-data-lab.github.io/pixiedust/displayapi.html#introduction
 - Data: https://www.kaggle.com/apryor6/us-opiate-prescriptions/data
 - Scikit Learn: http://scikit-learn.org/stable/

# 자세히 보기

* **Data Analytics 코드 패턴**: 이 코드 패턴이 즐거우셨나요? 다른 [Data Analytics 코드 패턴](https://developer.ibm.com/code/technologies/data-science/)도 확인해 보세요
* **AI and Data 코드 패턴 목록**: 코드 패턴 비디오에 대한 모든 [재생 목록](https://www.youtube.com/playlist?list=PLzUbsvIyrNfknNewObx5N7uGZ5FKH0Fde)을 북마크 하세요
* **Watson Studio**: IBM [Watson Studio](https://dataplatform.ibm.com/)와 함께 데이터 사이언스 기술을 마스터 하세요
* **IBM 클라우드의 Spark**: Spark 클러스터가 필요하세요? IBM 클라우드에서 [Spark 서비스](https://console.bluemix.net/catalog/services/apache-spark)로 최대 30개의 Spark executor를 만들어보세요

# 라이센스
[Apache 2.0](LICENSE)
