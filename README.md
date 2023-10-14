# stable diffusion, Ubuntu 20.04, GeForce GTX 1660 Ti

## はじめに
stable diffusionローカル環境構築備忘録です。
1111を使用します。
この分野は情報の更新が速く、自分の環境にfitした情報がなかなか見つかりませんでした。
Ubuntu 20.04.6 LTSにおけるstable diffusion環境を構築する手順を記します。
2023年10月14日時点での情報であることに注意してください。

![](https://raw.githubusercontent.com/yKesamaru/stable_diffusion/master/assets/eye_catch.png)

## 環境
```bash
inxi -SGm
System:    Host: user Kernel: 5.15.0-86-generic x86_64 bits: 64 Desktop: Gnome 3.36.9 
           Distro: Ubuntu 20.04.6 LTS (Focal Fossa) 
Memory:    RAM: total: 15.55 GiB used: 7.02 GiB (45.2%) 
           RAM Report: permissions: Unable to run dmidecode. Root privileges required. 
Graphics:  Device-1: NVIDIA TU116 [GeForce GTX 1660 Ti] driver: nvidia v: 525.85.12 
           Display: x11 server: X.Org 1.20.13 driver: fbdev,nouveau unloaded: modesetting,vesa resolution: 2560x1440~60Hz 
           OpenGL: renderer: NVIDIA GeForce GTX 1660 Ti/PCIe/SSE2 v: 4.6.0 NVIDIA 525.85.12 
```

- [stable diffusion, Ubuntu 20.04, GeForce GTX 1660 Ti](#stable-diffusion-ubuntu-2004-geforce-gtx-1660-ti)
  - [はじめに](#はじめに)
  - [環境](#環境)
  - [`sd`フォルダを作成](#sdフォルダを作成)
  - [依存ライブラリをインストール](#依存ライブラリをインストール)
  - [pyenvをインストール](#pyenvをインストール)
    - [`~/.bashrc`に追記](#bashrcに追記)
    - [python 3.10をインストール](#python-310をインストール)
    - [python 3.10.3をlocalに設定](#python-3103をlocalに設定)
    - [pipをアップグレード](#pipをアップグレード)
  - [cudaのバージョン確認](#cudaのバージョン確認)
  - [python依存ライブラリをインストール](#python依存ライブラリをインストール)
  - [`webui.sh`をダウンロード](#webuishをダウンロード)
  - [`webui-user.sh`を編集](#webui-usershを編集)
  - [`webui.sh`を実行](#webuishを実行)
  - [実行結果](#実行結果)


## `sd`フォルダを作成
```bash
mkdir sd
cd sd
```

## 依存ライブラリをインストール
```bash
# ビルドに必要なパッケージをインストール
sudo apt install -y \
  build-essential \
  libssl-dev \
  zlib1g-dev \
  libbz2-dev \
  libreadline-dev \
  libsqlite3-dev \
  wget \
  curl \
  llvm \
  libncurses5-dev \
  libncursesw5-dev \
  xz-utils \
  tk-dev \
  libffi-dev \
  liblzma-dev \
  python3-openssl \
  git

# その他のパッケージをインストール
sudo apt install -y \
  python3-venv \
  libgl1 \
  libglib2.0-0

```

## pyenvをインストール
```bash
curl https://pyenv.run | bash
```

### `~/.bashrc`に追記
```bash
export PATH="$HOME/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```
`.bashrc`を再読み込み
```bash
source ~/.bashrc
```

### python 3.10をインストール
この時点で最新バージョンの3.10.3がインストールされる。
```bash
pyenv install 3.10
```

### python 3.10.3をlocalに設定
```bash
pyenv local 3.10.13
```

### pipをアップグレード
このpipはpyenvでインストールしたpython3.10.3に紐づいている。
```bash
pip install -U pip
```

## cudaのバージョン確認
synapticを起動し、目視で確認した。→ cuda 12.0がインストールされている。

## python依存ライブラリをインストール
cuda 12.0に対応したpytorchをインストールする。
URLの最後が`cu120`であることに注意。
```bash
pip install torch==2.0.1 torchvision==0.15.2 --extra-index-url https://download.pytorch.org/whl/cu120
```

## `webui.sh`をダウンロード
```bash
wget -q https://raw.githubusercontent.com/AUTOMATIC1111/stable-diffusion-webui/master/webui.sh
```

## `webui-user.sh`を編集
GeForce GTX 1660 Tiを使用しているので、`--precision full --no-half --medvram`を追記する。
```bash
code '/home/user/bin/sd/stable-diffusion-webui/webui-user.sh'
```
```bash
# Commandline arguments for webui.py, for example: export COMMANDLINE_ARGS="--medvram --opt-split-attention"
export COMMANDLINE_ARGS="--precision full --no-half --medvram"
```

## `webui.sh`を実行
```bash
bash webui.sh
```
これで自動的にブラウザが起動する。

## 実行結果


![](https://raw.githubusercontent.com/yKesamaru/stable_diffusion/master/assets/2023-10-14-12-08-19.png)

![](https://raw.githubusercontent.com/yKesamaru/stable_diffusion/master/assets/2023-10-14-12-14-04.png)

`1 girl`で作成された画像

![](https://raw.githubusercontent.com/yKesamaru/stable_diffusion/master/assets/2023-10-14-12-15-58.png)

なにこれこわい。

もう一度実行

![](https://raw.githubusercontent.com/yKesamaru/stable_diffusion/master/assets/2023-10-14-12-26-27.png)

なにこれこわすぎるでしょ。完全に心霊写真です。
想定してたのと違う。

もう一度実行。

![](https://raw.githubusercontent.com/yKesamaru/stable_diffusion/master/assets/2023-10-14-12-28-22.png)

…。

とりあえず、動作確認を終了します。
