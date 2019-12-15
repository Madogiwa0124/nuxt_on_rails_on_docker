# ベースイメージの設定
FROM ruby:2.6.5
ARG APP_NAME
# コンテナ上で関連パッケージのインストール
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev postgresql-client
RUN apt-get update -qq && apt-get install -y nodejs npm
RUN npm install -g yarn
RUN gem install bundler
# コンテナ上でフォルダを作成
RUN mkdir /${APP_NAME}
# 作業ディレクトの設定
WORKDIR /${APP_NAME}
# Gemfileをコンテナ上にコピー
ADD repos/${APP_NAME}/Gemfile Gemfile
ADD repos/${APP_NAME}/Gemfile.lock Gemfile.lock
# コンテナ上でbundle install
RUN bundle install
