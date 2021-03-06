# 터미널을 이용해 리눅스 제어하기

## Terminal

- 문자를 입력해서 명령을 내림(CLI: Command Line Interface)
  - 아이콘을 이용해서 명령을 내리는 방식은 GUI
- 명령은 현재 머물고 있는 디렉토리를 대상으로 내려짐
  - 명령을 내릴 때는 언제나 어떤 디렉토리에 머물고 있는지 기억하고 있어야 함

### 기본 명령어

#### pwd

- 현재 위치하고 있는 디렉토리를 알려주는 명령어
  - 예: `/Users/ggam` -> 현재 '최상위 디렉토리> Users> ggam' 에 위치
- 입력되는 명령들은 pwd명령어가 표시한 디렉토리 안에 포함되어있는 파일들을 대상으로 해서 실행이 된다.

#### ls

- 현재 디렉토리의 파일 목록을 출력하는 명령어
- `ls -l`은 현재의 디렉토리에 있는 파일과 디렉토리 리스트를 더 자세하게 보여준다.
  - 권한/생성자/용량/시간 등 자세하게 나옴
  - 앞에 d가 있으면 디렉토리, d가 없으면 파일
- `ls -a`은 현재의 디렉토리내에 숨김파일까지 모두 출력

#### mkdir

- 현재 디렉토리 하위에 새 디렉토리를 만드는 명령어
  - `mkdir 폴더명`
- `mkdir hello_linux`를 입력하면 'hello_linux'이름의 디렉토리가 생성됨
  - 현재 위치에서 'ls'명령어를 입력해보면 확인할 수 있음

#### touch

- 비어있는 파일을 생성하는 명령어
  - `touch 파일명`
- `touch empty_file.txt`를 입력하면 empty_file이름의 비어있는 txt파일이 저장된다.

#### cd

- change directory의 약자
- 현재 위치한 디렉토리에서 다른 디렉토리로 이동
- `cd 이동할 디렉토리의 경로명`
- 예: 현재 위치한 디렉토리에서 하위 디렉토리로 이동하기
  - `cd hello_linux`
  - pwd를 실행해보면 hello_linux디렉토리에 들어온 것을 확인할 수 있음
- 예: 현재 위치한 디렉토리에서 부모 디렉토리로 다시 이동하기
  - `cd ..`
  - cd 뒤에 부모디렉토리의 경로를 붙여넣어도(절대경로) 되지만 '..'을 붙이면 부모 디렉토리로 이동함(상대경로)

#### rm

- 파일 또는 디렉토리를 삭제하는 명령어
- 파일삭제: `rm 파일명`, 폴더삭제: `rm -r 폴더명`
- 예: `rm empty_file.txt`
- ls명령어로 확인해보면 삭제된 것을 볼 수 있음
- 'rm hello_linux'를 하면 오류메세지 나옴
  - rm명령어로 디렉토리를 삭제할 수는 없음
- 'rm -r hello_linux'를 하면 삭제됨
  - -r을 붙이면 해당 디렉토리는 물론 하위 디렉토리, 파일을 모두 지움

#### 명령어의 사용법 확인하기

##### --help

- `명령어 --help`
- 예를 들어 `rm --help` 또는 `mkdir --help`처럼 명령어 뒤에 '--help'를 붙이면 해당 명령의 사용 설명에 대한 내용이 출력됨

##### man

- `man 명령어`
- 예를 들어 `man ls`처럼 man 뒤에 명령어를 입력하면 전용 페이지가 열리며 상세한 설명이 나옴
- man화면에서 위,아래 화살표를 누르면 화면을 움직일 수 있다.
- 특정 키워드를 검색하고 싶으면 '/텍스트'를 입력하면 됨
- 빠져나갈 때는 알파벳Q

### 파일편집(nano)

#### 명령어 기반의 편집기 nano

1. 명령어창에 nano를 입력하면 nano입력창이 열린다.
2. 내용을 입력한다.
3. ^o(write out)입력 및 file name to write(파일명 쓰기)
4. ^x(exit)입력으러ㅗ 나가기
5. 파일을 수정하고 싶을 때 nano+파일명 입력

##### nano편집기에서 복사 붙여넣기 기능

- 기본적으로  ^k(cut), ^u(paste)로 잘라내기 붙여넣기를 한다.(nano에는 잘라내기가없음)
- 영역을 잘라내서 붙여넣고 싶으면
  - 복사영역선택(esc+6), ^k(cut), ^u(paste)
