# Forevereader Server Setting

## deploy를 편하게 

### 원격에 repository 만들기

```bash
# 이하 작업들은 원격에서 이루어 짐
$ mkdir project 
$ mkdir project.git && cd project.git # 관습적으로 프로젝트명.git으로 디렉토리명을 만들어주는 듯 하다.
$ git init --bare # --bare: repository를 bare repository로 만들어준다. GIT_DIR 환경변수가 설정되어 있지 않으면 최근 working directroy로 설정된다. 
```

### hook 설정(project.git/hooks)
로컬에서 원격으로 push를 한 뒤 실행되기 원하는 작업들을 설정한다.

거의 대부분 상황에서의 설정이 가능한 것 같고 여기서는 **post-receive**(없으면 생성) 파일의 설정이 필요하다.

내용은 그냥 쉘에서 실행하듯 추가하면 된다.(아마?)

```sh
#!/bin/sh

GIT_WORK_TREE=/path/to/project
export GIT_WORK_TREE # GIT_WORK_TREE 설정은 필수
git checkout -f
cd /path/to/project
npm install # 추가된 dependencies가 있을지 몰라 추가
forever restart app.js # 밑에 설명
```

### 로컬에서 원격 repository를 추가

```bash
# 이하 작업들은 로컬에서 이루어 짐
$ git remote add myserver ssh://username@myserver/path/to/project.git
$ git push myserver master # 처음에만 master를 붙이고 추후엔 git push myserver만 해도 된다
```
          
## process를 영원히                      

### forever
node.js 프로세스 관리툴로 [forever][0]를 사용한다.

```bash
$ npm install -g forever
```

사용방법 또한 쉽다.

```bash
  $ forever --help
  usage: forever [action] [options] SCRIPT [script-options]

  Monitors the script specified in the current process or as a daemon

  actions:
    start               Start SCRIPT as a daemon
    stop                Stop the daemon SCRIPT
    stopall             Stop all running forever scripts
    restart             Restart the daemon SCRIPT
    restartall          Restart all running forever scripts
    list                List all running forever scripts

  # 후략
```

[0]: https://github.com/nodejitsu/forever
