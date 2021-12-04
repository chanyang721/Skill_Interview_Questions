### VScode Configuration setting
1. VScode에서 F1을 누르면 옆의 그림처럼 입력창에 `configuration` 을 입력한다. 
2. `"Remote-SSH: Open Configuration File..."` 을 클릭한다.
3. 두번째 그림과 같이 `~/.ssh/config` 와 같은 옵션이 보이면 클릭해준다.
4. 그러면 `config` 파일이 열리게 되는데 아래와 같이 생성한 EC2의 정보를 입력해준다
    - 실제 예시
        
        ```jsx
        Host Scrooge
            HostName 1.12.123.123
            User ubuntu
            Port 22
            IdentityFile ~/.ssh/ec2_pair_key.pem
        ```
        
    
    ```jsx
    Host "원하는 이름"
        HostName "EC2의 Public IP"
        User ubuntu
        Port "EC2의 인바운드 설정의 SSH PORT"
        IdentityFile "EC2의 Pair Key 위치"
    ```

### AWS EC2 옵션을 확인하는 방법

1. `AWS`에 접속하여 `EC2`로 들어가 `인스턴스`를 클릭한다. 
2. 접속을 원하는 EC2의 `인스턴스 ID`를 클릭하면 옆의 그림과 같은 창이 나온다.
3. `연결` 을 클릭하면 두번째 그림이 나오는데 `EC2 인스턴스 연결` 탭에서 EC2의 `Public IP`(퍼플릭 IP 주소)와 `HostName`(사용자 이름)을 확인할 수 있다. 
4. 그리고 `SSH 클라이언트` 탭에서  Pair Key의 파일 권한을 변경하는 `chmod 400 ~` 명령어가 나오게 되는데 저장한 EC2 Pair Key가 저장된 곳으로 터미널로 이동하여 해당 코드를 복붙하여 엔터를 쳐준다.

### VSCode에서 EC2연결

EC2 인스턴스의 옵션을 전부 VScode의 config파일에 작성하고 저장했다면, 다시 VScode로 돌아와서 F1을 눌러 `Remote-SSH: connect to HOST` 을 누르면 위의 `Host` 에 작성한 이름(Scrooge)이 나타나고 클릭 시 EC2로 접속하게 된다. VScode의 왼쪽 아래에 `SSH: [Host]` 와 같이 나오게 된다면 연결된것이다.

연결된 후 `Ctrl + o`  를 누르게 된다면 해당 인스턴스의 파일 리스트를 볼 수 있고, 선택한 파일을 CLI 편집기(Vim, nano)를 사용하지 않고 VSCode에서 변경 가능하다.

VScode에서 테트리스 블럭 아래의 모니터 모양을 클릭하면 원격 접속 가능한 SSH Targets 리스트를 확인 가능하다.

### Error Handling

```jsx
could not establish connection to permission denied (publickey,gssapi-keyex,gssapi-with-mic).
```

위와 같은 문구를 가진 경고창이 나온다면, 검색 결과 **아래 두가지 문제가 대표적인 해결법**이다.

1. **EC2 Pair Key의 권한 설정 변경을 않은 경우** 
2. **EC2를 생성할 때, 서브넷을 따로 설정하는 경우**

위의 두가지를 다시 설정하고 난 뒤 안된다면 에러 메시지로 구글링을 하기 바란다.

### Reference

[https://code.visualstudio.com/blogs/2019/07/25/remote-ssh](https://code.visualstudio.com/blogs/2019/07/25/remote-ssh)