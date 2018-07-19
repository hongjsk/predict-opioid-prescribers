## Watson 스튜디오와 scikit-learn을 이용한 머신 러닝으로 미국 내 오피오이드 처방자 예측하기

https://github.com/hongjsk/predict-opioid-prescribers/blob/master/README-ko.md

## Ubuntu Linux 실습 환경 설정

### python 및 jupyter notebook 설치

``` bash
sudo apt install python3
sudo apt install python3-pip
sudo apt install ipython3
pip3 install jupyter
pip3 install --user pixiedust scipy sklearn matplotlib pandas
```

### jupyter 설정

``` bash
$ jupyter notebook --generate-config
```

설정 파일 편집

```
$ vi ~/.jupyter/jupyter_notebook_config.py
```

``` python
...
c.NotebookApp.notebook_dir = '/jupyter'
...
```

### nginx 설정 파일 변경

```
$ sudo vi /etc/nginx/sites-available/dwmeetup
```

다음 내용을 추가 후 저장

``` conf
location /jupyter {
    proxy_pass http://localhost:8888;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header Host $http_host;
    proxy_http_version 1.1;
    proxy_redirect off;
    proxy_buffering off;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_read_timeout 86400;
}
```

### nginx 서비스 재시작

``` bash
$ sudo service nginx restart
```


### git 저장소 복제

``` bash
$ git clone -b dwmeetup https://github.com/hongjsk/predict-opioid-prescribers.git
$ cd predict-opioid-prescribers

```

### jupyter notebook 실행

``` bash
$ jupyter notebook
```

출력되는 메시지에서 토큰 정보 확인
``` bash
...
   http://localhost:8888/notebook/?token=<토큰>
...
```

### Jupyter Note 접속

웹 브라우저로 다음 URL 접속

```
https://<가상머신 주소>/jupyter/?token=<토큰>
```

## 참고

### ML Classifiers

* [Logistic Regression](http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html)
* [Gaussian Naive Bayes](http://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.GaussianNB.html)
* [Random Forests](http://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)
* [Gradient Boosting](http://scikit-learn.org/stable/modules/generated/sklearn.ensemble.GradientBoostingClassifier.html)
* [K-Nearest Neighbors Vote](http://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsClassifier.html)
* [Decision Tree](http://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeClassifier.html)
* [Linear Discriminant Analysis](http://scikit-learn.org/stable/modules/generated/sklearn.discriminant_analysis.LinearDiscriminantAnalysis.html)
* [Bagging Classifier](http://scikit-learn.org/stable/modules/generated/sklearn.ensemble.BaggingClassifier.html)
* [Voting/Majority Rule](http://scikit-learn.org/stable/modules/generated/sklearn.ensemble.VotingClassifier.html)

### Accuracy
* [A score by cross-validation](http://scikit-learn.org/stable/modules/generated/sklearn.model_selection.cross_val_score.html)
* [Accuracy classification score](http://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html)

### Evaluate

* [Confusion Matrix](http://scikit-learn.org/stable/modules/generated/sklearn.metrics.confusion_matrix.html)
* [Precision-recall pairs](http://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_recall_curve.html)
* [Average Precision](http://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)

