# dev-react-router-7

# 初めに

フロントエンド開発環境の土台を作成します。  

## 構築する環境

- volta    # nodeのバージョン管理
  - node
  - npm
- vite     # ビルドツール
- React Router v7
  - TypeScript
  - Tailwind CSS

また、開発用のスタブサーバ（APIサーバ）としてwiremockも準備する

## 1. 前提環境

以下の環境は構築されている前提となります.  
未構築の場合は、事前にご準備をお願いいたします.  
尚、現時点でMac OSでの環境整備は考えておりません.(Mac 持っていないので…)

- Windows + wsl2 (ubuntu-22.04)
- VSCode
- docker (wiremock用)

## 2. Node.js環境構築

初めにVoltaのインストールを行います.  
VoltaでNodeのバージョン管理を行うため、既に指定のNode.jsの環境を構築済みの方は飛ばしても良いです.  
但し、プロジェクト毎のNode&npmのバージョン管理をvoltaにて行うことを前提とするため、可能な限りインストールすることをお勧めします.  
Volta無しの場合は、各自で適切なNodeの環境を準備してください.  

### 2.1. Voltaインストール

``` shell
curl https://get.volta.sh | bash
source ~/.bashrc
volta -v
 2.0.2  # インストールに成功していれば voltaのバージョンが表示される
```

上記コマンドを実行してVoltaのインストールを行います.  
voltaのバージョンが表示されていればインストール成功です.  
もしも表示されなかった場合は、コマンド実行時に出ているエラーメッセージなどをご確認ください.  

尚、インストールに成功していれば、`.~/.bashrc` の末尾に以下の設定が書き込まれているハズです.

``` shell
export VOLTA_HOME="$HOME/.volta"
export PATH="$VOLTA_HOME/bin:$PATH"
```

### 2.2. node インストール

※本手順はプロジェクト新規立ち上げする時だけの手順です. **プロジェクトに途中参加する場合、実行不要**

新規にプロジェクトを立ち上げる場合、以下のコマンドでプロジェクトで使用するバージョンのnodeインストールと、voltaによるバージョン固定を行ってください

``` shell
volta install node@<指定するバージョン>
volta pin node@<指定するバージョン>
# node v20.18.1 の場合
# volta install node@20.18.1

# node 最新の場合
# volta install node@latest
```

この設定を行うことで、voltaがnodeのバージョンを管理してくれるようになります.

#### 以下は全員実行

``` shell
cd <プロジェクトフォルダ>
node -v
# プロジェクト指定バージョンのnodeがインストールされてる
```

voltaでバージョン指定されたプロジェクトフォルダ内に移動した後に、nodeを実行することでvoltaが自動的に指定バージョンのnodeをインストールします

## 3. プロジェクト作成

※ GitHubから作成済みプロジェクトをチェックアウトした場合は、本手順を飛ばして [『4. プロジェクト初期化』](#4-プロジェクト初期化) へ

``` shell
npm create vite@latest
> create-vite

│
◇  Project name:
│  front-app
│
◇  Select a framework:
│  React
│
◇  Select a variant:
│  React Router v7 ↗
Need to install the following packages:
create-react-router@7.2.0
Ok to proceed? (y) y

npm warn deprecated inflight@1.0.6: This module is not supported, and leaks memory. Do not use it. Check out lru-cache if you want a good and tested way to coalesce async requests by a key value, which is much more comprehensive and powerful.
npm warn deprecated glob@7.2.3: Glob versions prior to v9 are no longer supported

> npx
> create-react-router front-app


         create-react-router v7.2.0
      ◼  Directory: Using front-app as project directory

      ◼  Using default template See https://github.com/remix-run/react-router-templates for more
      ✔  Template copied

   git   Initialize a new git repository?
         No

  deps   Install dependencies with npm?
         Yes

      ✔  Dependencies installed

  done   That's it!

         Enter your project directory using cd ./front-app
         Check out README.md for development and deploy instructions.

         Join the community at https://rmx.as/discord

npm notice
npm notice New major version of npm available! 10.9.2 -> 11.2.0
npm notice Changelog: https://github.com/npm/cli/releases/tag/v11.2.0
npm notice To update run: npm install -g npm@11.2.0
npm notice
```

プロジェクトが作成できたので起動確認

``` shell
cd front-app
npm install
npm run dev
```

正しく起動出来たらvoltaの機能を使いnodeのバージョンをprojectに記録
projectフォルダ内にて以下のコマンドを実施

``` shell
cd front-app                         # 移動不要な場合は省略
volta pin node@<指定するバージョン>    # 事前に指定するnodeのバージョンをインストールしておくこと
```

## 4. プロジェクト初期化

[『3. プロジェクト作成』](#3-プロジェクト作成) 、若しくはGitHubからチェックアウト後にプロジェクトの初期化を行う

### 4.1. プロジェクトに必要なパッケージをインストールしテスト起動

``` shell
cd front-app                    # プロジェクトディレクトリに移動
npm install --save-dev          # 開発用モジュールのインストール
npm run dev                     # 開発環境にてアプリ起動
# 以下必要に応じて以下の手順でスタブサーバも起動してください
cd ../docker            # スタブサーバのディレクトリに移動
docker-compose up -d            # スタブサーバ起動
docker-compose down             # スタブサーバ終了(使い終わったら)
```

正常に起動できたのであれば、準備完了
