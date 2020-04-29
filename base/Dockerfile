FROM ubuntu:18.04

# 環境変数を設定
ENV DEBIAN_FRONTEND=noninteractive \
    TZ=Asia/Tokyo \
    USER=astre \
    PASS=astre \
    HOME=/home/astre \
    SHELL=/bin/bash \
    PATH=/home/astre/.yarn/bin:/home/astre/.config/yarn/global/node_modules/.bin:$PATH

# パッケージのインストール
RUN apt update && apt install -y \
        curl \
        git \
        sudo \
        tzdata \
        vim \
    && curl -SL https://deb.nodesource.com/setup_12.x | bash \
    && curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
    && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
    && apt update && apt install -y \
         nodejs \
         yarn \
    && apt clean \
    && rm -rf /var/lib/apt/lists/*

# 一般ユーザーアカウント設定
RUN useradd -m ${USER} \
    && gpasswd -a ${USER} sudo \
    && echo "${USER}:${PASS}" | chpasswd \
    && usermod -s ${SHELL} ${USER}

# 以降のRUN/CMDを実行するユーザー
USER ${USER}
# 以降の作業ディレクトリを指定
WORKDIR ${HOME}

# 開発用のディレクトリおよびGitレポジトリ用のディレクトリを作成
RUN mkdir ${HOME}/dev && mkdir ${HOME}/dev/repos

# Gitの設定
RUN git clone https://github.com/astre0816/gitconfig ${HOME}/dev/repos/gitconfig \
    && chmod 777 ${HOME}/dev/repos/gitconfig/gitconfig.sh \
    && ${HOME}/dev/repos/gitconfig/gitconfig.sh

# dotfilesのダウンロードおよび展開
RUN git clone https://github.com/astre0816/dotfiles ${HOME}/dev/repos/dotfiles \
    && chmod 777 ${HOME}/dev/repos/dotfiles/dotfiles.sh \
    && ${HOME}/dev/repos/dotfiles/dotfiles.sh

CMD ["/bin/bash"]
