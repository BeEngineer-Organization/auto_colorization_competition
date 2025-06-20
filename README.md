# 運営方針

## 講師が行う初期設定

1. リーモートリポジトリで `main` からその日のコンペティション用のブランチ（`pages-PLACE-YYYY-MM-DD` の命名規則で作成したブランチ）を切る。ブランチを切るのは、コンペティションで作成されるリーダーボード用の `HTML` ファイルを `main` ブランチや他の日で開催されるコンペティションと混同したくないためである。例えば、`pages-kyoto-2025-04-21` のようなブランチ名。
2. GitHub Pages から `docs/index.html` を配信できるように当日行うコンペティションのブランチの `docs` を設定する。

3. 以下の `.github/workflows/main.yml` にあるブランチ名を先ほど設定したコンペティション用のブランチ名（例えば、`pages-kyoto-2025-04-21`）に変更し、コミットを反映させる。

   ```
   name: CI

   # ...

   env:
     P_BRANCH: pages-PLACE-YYYY-MM-DD # ← ココ

   # ...
   ```

## 生徒が行う初期設定

1. 以下のコマンドからコンペティション用のブランチ（pages-PLACE-YYYY-MM-DD）を `clone` する。

   ```
   git clone -b (コンペティションのブランチ名) https://github.com/BeEngineer-Organization/auto_colorization_competition.git
   ```

2. `main` ブランチから以下のコマンドのようにその授業のクラスで**一意である**ブランチを作成する。その名前がリーダーボードに掲載されるユーザー名になる。

   ```
   git checkout -b (一意なブランチ名)
   ```

3. 以下のコマンドで仮想環境を作成し、仮想環境内に入り、必要なモジュールをインストールする。

   ```
   python -m venv .venv
   source .venv/bin/activate
   pip install -r requirements.txt
   ```

4. 以下のディレクトリ構造のようにダウンロードしたモデルファイル（`best_model.keras`）を配置する。

   ```
   .
   ├── README.md
   ├── data
   │   ├── ground_truth
   │   └── test_gray
   ├── docs
   │   └── index.html
   ├── evaluate.py
   ├── generate_html.py
   ├── model
   │   └── best_model.keras
   ├── prediction.py
   └── requirements.txt
   ```

## 推論とリーダーボードへの反映

1. 初期設定が終わり、ダウンロードしたモデルを先ほどのディレクトリ構造のように配置すると、`prediction.py` で推論することができる。以下のコマンドから推論する。

   ```
   python prediction.py
   ```

   このコマンドを実行すると、`submission.csv` がルート直下に作成される。このファイルがこの後 `push` する用のファイルになる。スコアの値がターミナル上もしくは `submission.csv` に出力されるので、その値を確認し、リーダーボードを更新する場合には、この後の操作を行う。

2. 以下のコマンドから先ほど作成した一意なユーザー名ブランチに `submission.csv` の変更差分を `push` する。

   ```
   git add submission.csv
   git commit -m "submit score"
   git push origin (ユーザー名ブランチ)
   ```

3. `push` することで `.github/workflows/main.yml` が走り、`submission.csv` に従い、リーダーボードの更新が行われる。数十秒後、GitHub Pages で設定したページを更新すると下の画像のようにリーダーボードの変更が行われている。

   ![guro-ishiguro github io_AutoColorization_](https://github.com/user-attachments/assets/b8bc81dd-5385-4fa5-b8d8-fe1df3411c79)
