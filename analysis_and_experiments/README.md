# analysis_and_experiments

分析及び実験環境作成のテンプレート。
改善余地多々あり。

## 事前環境
- pyenv: pythonバージョン設定
- poetry: ライブラリ管理
- Docker, docker-compose: コンテナ作成

※Dockerfileのベースイメージのpythonバージョンとpyenvで指定するバージョンはまで合わせておく。


## 導入済みライブラリ

- pandas = 1.1.5
- numpy = 1.19.4
- matplotlib = 3.3.3
- scikit-learn = 0.23.2
- mlflow = 1.12.1
- kedro = 0.16.6
- jupyterlab = 2.2.9
- kedro-viz = 3.8.0
- lightgbm = 3.1.1


## コンテナ起動の手順
 
1. `git clone {このリポジトリ}`
2. `cd {このリポジトリ}`
6. `pyenv local {python version}`：pythonバージョン設定 
    - 現在は`pyenv local 3.8.5`
7. `poetry install`:ライブラリインストール
    - ライブラリ追加`poetry add`
8. `docker-compose build`：初回イメージビルド
9. `docker-compose up -d`：コンテナ起動
10. `docker exec -it laboratory /bin/bash` ：コンテナ入室
11. `exit`：コンテナ退室
12. `docker-compose stop`：コンテナ停止
13. 再起動する際は6

## 実験プロジェクト作成の手順

**コンテナ入室済み**

1. `cd kedro_project`:ディレクトリ移動
2. `kedro new`:プロジェクト作成→[Create a new project interactively][kedro_new]
    - 名前や説明等をつける
3. `cd example`：ディレクトリ移動

- 以降はnotebooksで試行錯誤したものをnode、pipelineに落とし込むを繰り返す。
- Example Project [kedro Iris Dataset][iris_data]が参考になります。

[kedro_new]:https://kedro.readthedocs.io/en/stable/02_get_started/04_new_project.html

[iris_data]:https://kedro.readthedocs.io/en/stable/02_get_started/05_example_project.html

## kedro+mlflowを使用した流れ

多分使い方としてはjupyterlabで分析をして、硬い実装をpiplinesで行う。


1. データを`data/01_raw`に配置
2. `conf/base/catalog.yml`にデータを記載
3. `notebooks`で分析と処理を試し、関数化
    - `klab`でjupyterLabを起動
    - http://0.0.0.0:8888/
5. `src/{プロジェクト名}/pipelines`配下にデータ加工と学習のパイプラインを作成
    - data_engineering
        - node:関数化されたデータ加工処理
        - pipeline: データ加工をパイプライン化
    - data_science
        - node: 関数化されたモデルの訓練と検証
        - pipeline: モデルの訓練をパイプライン化
        - mlflow.log_XXXはnodeの関数内に記載
6. pipelinsをhooks.pyの`register_pipelines`で結合
6. `kedro_project/{プロジェクト名}`直下で`kedro run`
7. mlflowでログを確認
    - `mfui`でmlflow uiを起動
    - http://0.0.0.0:5000/
8. パイプラインを確認
    - `kviz`でkedro vizを起動
    - http://0.0.0.0:4141/

## alias
`kedro_project/{プロジェクト名}`直下で使用

- klab='kedro jupyter lab --ip=0.0.0.0 --allow-root'
- mfui='mlflow ui --host 0.0.0.0'
- kviz='kedro viz --host 0.0.0.0'




# 参考


- [Kedro MLflow (Basic limited-YAML example of PipelineX)](https://github.com/Minyus/kedro_mlflow)
- [Iris dataset example project](https://kedro.readthedocs.io/en/stable/02_get_started/05_example_project.html)
- [pipelineライブラリKedroを使ってみた話](https://socinuit.hatenablog.com/entry/2020/02/08/210423)
- [ML Pipeline事始め – kedro(+notebook)とMLflow Trackingで始めるpipeline入門 –](https://recruit.gmo.jp/engineer/jisedai/blog/kedro_and_mlflow_tracking/)
- [機械学習ワークフロー管理ツール Kedroを使ってTitanic生存予測](https://qiita.com/tatamiya/items/42067d3777d19e9a2c9e)
- [kedro_titanic](https://github.com/tatamiya/kedro_titanic)
- [Deploying and Versioning Data Pipelines at Scale](https://medium.com/quantumblack/deploying-and-versioning-data-pipelines-at-scale-942b1d81b5f5)
- [Visualise pipelines](https://kedro.readthedocs.io/en/stable/03_tutorial/06_visualise_pipeline.html)
- [Kedroで機械学習パイプライン入門](https://qiita.com/noko_qii/items/2395d3a3dbcd9410e5e7)
