# NVM (Node Version Manager)

## Install & Update Script

### For MacOS

##### 아래에 있는 script를 사용하는 cli 에 돌린다
나는 개인적으로 curl을 돌렸지만 사실 이것만이 방법이 아닌 깃허브 공식문서에 보면 몇가지의 방법이 더 있긴하다.

```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.1/install.sh | bash
```
```sh
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.1/install.sh | bash
```

Running either of the above commands downloads a script and runs it. The script clones the nvm repository to ~/.nvm, and attempts to add the source lines from the snippet below to the correct profile file (~/.bash_profile, ~/.zshrc, ~/.profile, or ~/.bashrc).

### For Windows 10

Make sure that you have Nodejs installed on your software.
Install NVM by going onto this [https://github.com/coreybutler/nvm-windows/releases](https://github.com/coreybutler/nvm-windows/releases) and download nvm-setup.zip file. Unzip the file and install NVM.

```sh
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

## Usage

최신 버전의 node를 다운로드, 컴파일, 인스톨 할경우:
```sh
nvm install node # "node" 는 가장 마지막 버전의 alias이다.
```

특정한 node 버전을 사용할 경우:

```sh
nvm install 6.14.4 # or 10.10.0, 8.9.1, etc
```

> 제일 처음 인스톨한 버전이 default 버전이 된다. 새로운 shell은 default로 지정된 node 버전으로 시작된다. nvm alias default 를 치면 어떤 버전이 default 로 지정되 있는지 알 수 있다.

사용 가능한 버전을 볼 경우:
```sh
nvm ls-remote
```

그리고 아무 새로운 쉘 에서 인스톨한 버전을 사용할 경우:
```sh
nvm use node
```

혹은 그냥 실행 시킬 경우:
```sh
nvm run node --version
```

##### 지금까지 nvm 의 기본적인 커맨드를 알아봤다. 이것 말고도 더 있지만 지금으로선 이 이상 사용하진 않을듯 싶다.

### Reference:

https://github.com/nvm-sh/nvm