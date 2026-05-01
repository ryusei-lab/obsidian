GithubとObsidianを連系することにより、今までのデータのバックアップ?のようなものが取れる
今回はその連系方法を記録



## はじめに

ノートアプリ**Obsidian**（mac版）を利用するにあたって、自身のGitHubアカウントと連携し、ノートを自動でコミット&プッシュされるようにしたいと考えました。

しかし、検索して出てくる記事はスマホ向けの内容ばかりで、そのまま使える情報には辿り着けませんでした。

そのため本記事では、実際に動作した手順を備忘録としてまとめます。

## [](https://zenn.dev/ofurousagi/articles/250801_obsidian-github-sync#%E5%8F%82%E8%80%83%E3%81%AB%E3%81%97%E3%81%9F%E8%A8%98%E4%BA%8B%E3%81%A8%E3%81%AE%E9%81%95%E3%81%84)参考にした記事との違い

多くの記事で紹介されているスマホ向けの手順は以下の通りです。

1. GitHubのアカウントでパーソナルアクセストークンを発行
2. ObsidianでGitプラグインをインストール
3. Gitプラグインのsettingの、「`Authentication/Commit Author > Username`」にGitHubのユーザーネームを入力
4. 「`Authentication/Commit Author > Password/Personal access token`」にパーソナルアクセストークンを貼り付ける。

しかし、PC版のObsidianのGitプラグインには「Authentication/Commit Author」のような項目が存在せず、アクセストークンを入力する欄もありません。

## [](https://zenn.dev/ofurousagi/articles/250801_obsidian-github-sync#%E5%AE%9F%E9%9A%9B%E3%81%AB%E8%A1%8C%E3%81%A3%E3%81%9F%E9%80%A3%E6%90%BA%E6%89%8B%E9%A0%86)実際に行った連携手順

### [](https://zenn.dev/ofurousagi/articles/250801_obsidian-github-sync#%E5%89%8D%E6%8F%90%E7%9F%A5%E8%AD%98)前提知識

- ObsidianはVault（保管庫）という単位でノートを管理します。
- VaultのルートディレクトリをそのままGitリポジトリにする必要があります。
- PC版の方法ではアクセストークンは不要です。

### [](https://zenn.dev/ofurousagi/articles/250801_obsidian-github-sync#1.-vault%EF%BC%88%E4%BF%9D%E7%AE%A1%E5%BA%AB%EF%BC%89%E3%82%92%E4%BD%9C%E6%88%90)1. Vault（保管庫）を作成

!

すでにVault（保管庫）がある場合はスキップして構いませんが、Vault名はGitHubリポジトリ名として利用するため、アルファベットに変更しておきましょう。

今回は`Valut/my-obsidian-notes`ディレクトリを作成しました。  
Valutディレクトリ直下に保管庫を置く必要は特にはありませんが、今後保管庫を増やしたい・分けたい場合のことを考えてこのようなファイル構成にしました。

```
Valut
└─ my-obsidian-notes
```

この`my-obsidian-notes`はこのあとgithubのリポジトリ名として使用します。

### [](https://zenn.dev/ofurousagi/articles/250801_obsidian-github-sync#2.-github%E3%81%AB%E3%83%AA%E3%83%9D%E3%82%B8%E3%83%88%E3%83%AA%E3%82%92%E4%BD%9C%E6%88%90)2. GitHubにリポジトリを作成

ここで設定したリポジトリ名と、ObsidianのVault名は一致している方が管理しやすいため、今回は`my-obsidian-notes`としました。

```
https://github.com/username/my-obsidian-notes.git
```


### [](https://zenn.dev/ofurousagi/articles/250801_obsidian-github-sync#3.-%E3%83%AD%E3%83%BC%E3%82%AB%E3%83%AB%E3%81%AE%E4%BF%9D%E7%AE%A1%E5%BA%AB%E3%83%87%E3%82%A3%E3%83%AC%E3%82%AF%E3%83%88%E3%83%AA%E3%82%92%E3%83%AA%E3%83%9D%E3%82%B8%E3%83%88%E3%83%AA%E5%8C%96)3. ローカルの保管庫ディレクトリをリポジトリ化

```
cd /Users/username/vault/my-obsidian-notes/
git init
git remote add origin https://github.com/username/obsidian-notes.git
```

### [](https://zenn.dev/ofurousagi/articles/250801_obsidian-github-sync#4.-%E3%82%B3%E3%83%9F%E3%83%83%E3%83%88%26%E3%83%97%E3%83%83%E3%82%B7%E3%83%A5)4. コミット&プッシュ

```
git add .
git commit -m "Initial commit"
git push -u origin main
```

### [](https://zenn.dev/ofurousagi/articles/250801_obsidian-github-sync#5.-%E3%82%B3%E3%83%9F%E3%83%A5%E3%83%8B%E3%83%86%E3%82%A3%E3%83%97%E3%83%A9%E3%82%B0%E3%82%A4%E3%83%B3%E3%81%8B%E3%82%89git%E3%82%92%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)5. コミュニティプラグインからGitをインストール

1. **`Setting > コミュニティプラグイン`を開く**
2. **「コミュニティプラグインを有効化」をクリックし、プラグインをインストールできるようにする**  
    ![コミュニティプラグインを有効化](https://static.zenn.studio/user-upload/deployed-images/ae818053a7a6773bad55aa47.png?sha=f0e74269f38448140a70c88bec244b55ea680e81)
3. **`閲覧 > Git`を検索しインストール&有効化**  
    ![閲覧](https://static.zenn.studio/user-upload/deployed-images/3c57cbb954f22e456d513fa6.png?sha=dda731a0094d16386406bdd0744d36c4c45f499b)![Git`を検索しインストール&有効化](https://static.zenn.studio/user-upload/deployed-images/cb315aea5fec03656f107798.png?sha=767aab4b8db678407128cceab973ea8e66862dca)

### [](https://zenn.dev/ofurousagi/articles/250801_obsidian-github-sync#6.-%E8%87%AA%E5%8B%95%E3%82%B3%E3%83%9F%E3%83%83%E3%83%88%26%E3%83%97%E3%83%AB%E3%80%81%E3%82%B3%E3%83%9F%E3%83%83%E3%83%88%E3%83%A1%E3%83%83%E3%82%BB%E3%83%BC%E3%82%B8%E3%81%AE%E8%A8%AD%E5%AE%9A)6. 自動コミット&プル、コミットメッセージの設定

1. **`setting > Gitプラグインの設定`を開く**
2. **自動コミット&プッシュの間隔を設定**
    - `Auto commit-and-sync interval(minutes)`に任意の数値を入力  
        デフォルトが0（自動でコミット&プッシュされない）になっているので、今回は10分ごとにコミット&プッシュされるように10と入力します。
3. **自動プルの間隔を設定**
    - `Auto pull interval(minutes)`に任意の数値を入力  
        デフォルトが0（自動でプルされない）になっているので、今回は10分ごとにプルされるように10と入力します。
4. **必要に応じてコミットメッセージを設定**  
    自動コミットと手動（マニュアル）コミットでコミットメッセージを変えることができます。  
    私は`auto commit`と`manual commit`でコミットメッセージを変えました。

![Auto pull interval(minutes)](https://static.zenn.studio/user-upload/deployed-images/850135d193cf4fd8dd3866e0.png?sha=bd1245d8d305de37e37496c29a4edc1fe83090fc)![Auto pull interval(minutes)](https://static.zenn.studio/user-upload/deployed-images/c824c2db1343c32fd146b82f.png?sha=a4e426327e6295fff04caad4d79cfe082fa20aa6)

他の項目は必要に応じて設定してください。

### [](https://zenn.dev/ofurousagi/articles/250801_obsidian-github-sync#7.-%E3%83%91%E3%82%B9%E3%82%92%E9%80%9A%E3%81%99)7. パスを通す

1. **`which`でコマンドのパスを確認**

```
which git
/usr/local/bin/git
```


2. **Gitプラグインにパスを記載**  
    ![Gitプラグインにパスを記載](https://static.zenn.studio/user-upload/deployed-images/65bec777081d0a2df84d8376.png?sha=6b562fe732966df9a1fc2d615f83e459e92d9622)

- ディレクトリを追加する必要があるため、`/usr/local/bin/git`の`git`の部分は不要
- `/usr/bin`は保険として追加（なくてもOK）

### [](https://zenn.dev/ofurousagi/articles/250801_obsidian-github-sync#8.-%E3%82%B3%E3%83%9F%E3%83%83%E3%83%88%26%E3%83%97%E3%83%83%E3%82%B7%E3%83%A5%E3%81%AE%E7%A2%BA%E8%AA%8D)8. コミット&プッシュの確認

Obsidianからファイルを編集して、設定した間隔後にGitHubに反映されているか確認します。

これでObsidianとGitHubの連携が完了しました。