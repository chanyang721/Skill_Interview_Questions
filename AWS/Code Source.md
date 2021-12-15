### Code Source 란 ?

개발을 진행하면서 다양한 프로젝트를 진행하게 되고, 현재 거의 모든 프로젝트는 CI / CD를 위한 소스 코드를 관리하게 된다. Code Source는 보통 개발자들이 소스 코드를 업로드하는 GitHub와 연동되어 사용할 수 있게 설계되어 있다. 그리고 만약 해당 GitHub에 어떤 변화(push, merge ..)를 감지하여, 변화가 생기면 AWS의 Code Source와 연동된 GitHub Repository의 소스 코드를 가져오는 기능을 하게 된다.

### Code Source 설정 방법

우선 Code Source는 Code pipeline을 생성하는 첫번째 단계로 옆의 사진과 같이 등장한다. 

다양한 Source들을 사용할 수 있고 나는 GitHub의 Repository와 연동하여 사용할 예정이기 때문에 **GitHub(버전 2)**를 선택했다. 자신이 배포하고 싶은 코드가 저장된 장소를 옵션으로 선택하면 된다.

![스크린샷 2021-12-14 오전 11.47.33.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e7728f38-7f76-4a29-bd71-b28d083d3c2a/스크린샷_2021-12-14_오전_11.47.33.png)

---

---

**GitHub(버전 2)** 옵션을 선택하면 옆의 그림과 같이 추가 옵션을 선택 할 수 있게 된다.

- 현재 로그인된 GitHub와 연결
- 연결된 GitHub와 연결하고 싶은 Repository 선택
- 선택된 Repository에서 소스 코드로 사용할 branch 선택
- 변경 감지 옵션: 선택된 branch에서 소스 코드의 변경(push, merge .. 등)이 감지되면 선택된 소스 코드로 pipeline을 실행하는 옵션

![스크린샷 2021-12-14 오전 11.51.25.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7ccf917c-67d1-4f46-8f02-79e2e5dcd9fe/스크린샷_2021-12-14_오전_11.51.25.png)

---

---

위의 사진에 있는 **GitHub에 연결** 버튼을 누르면 옆의 사진과 같은 창이 나타나고 연결 이름은 자유롭게 설정하되, 만약 연결하려는 GitHub 아이디의 2개 이상의 레포를 사용하여 각각 다른 pipeline을 설정해야 한다면, 다른 pipeline에서 GitHub 앱란에 이전에 연결했던 연결 이름을 선택할 수 있기 때문에 연결 이름을 기억하는 것이 좋다. 

![스크린샷 2021-12-14 오전 11.54.26.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5012eea3-b7b1-4922-a8e8-4919b1a367a0/스크린샷_2021-12-14_오전_11.54.26.png)

---

---

연결 이름을 입력하고 **GitHub에 연결** 버튼을 누르면 다음 사진이 나온다. 여기서 설정한 연결 이름은 Code Source와 GitHub와 연결이고, 사용자의 컴퓨터에서 로그인 되어 있는 GitHub 아이디와의 연결 이름이다.

![스크린샷 2021-12-14 오전 11.59.40.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b9759edb-a3d8-48b5-88eb-854bf879fc75/스크린샷_2021-12-14_오전_11.59.40.png)

---

---

자신의 GitHub 아이디가 자동으로 나오고 소속된 Organizations도 옵션으로 나온다. 여기서 자신의 아이디를 클릭하면 아래와 같은 창으로 넘어가게 된다. 

![스크린샷 2021-12-14 오후 12.36.30.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/aa340be5-e49b-4a4a-aea8-e0faa89d430a/스크린샷_2021-12-14_오후_12.36.30.png)

---

---

옆의 사진의 **Repository Access**칸에서 연결된 GitHub 아이디의 모든 레포지토리**(All repositoies)**와의 Access를 허가하거나, 혹은 특정한 하나의 레포지토리**(Only select repository)**와만 연결할 수 있는 설정을 하고 **Save** 버튼을 누르면 다음 사진과 같이 숫자 형식으로 연결된다. 

![스크린샷 2021-12-14 오후 12.05.30.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ce3f0e56-c3d8-4ee1-8aed-1cc1a4dd5a27/스크린샷_2021-12-14_오후_12.05.30.png)

![스크린샷 2021-12-14 오후 12.11.00.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9912499b-7a00-4480-8f92-4f915072ba3c/스크린샷_2021-12-14_오후_12.11.00.png)

---

---

여기까지 설정되면 AWS의 pipeline에서 첫번째 단계인 Code Source부분의 설정은 끝났다.

AWS의 자동 빌드, 배포를 위해 사용된 소스 코드를 GitHub의 Repository와 연결하는 과정만 있기 때문에 특별히 어려운 점은 없다고 본다.  

옆의 사진에서 **다음** 버튼을 누르게 된다면, [Code Build](https://www.notion.so/Code-Build-e692acdae6c94d9aa65b362c261a30cf)를 설정하기 위한 페이지로 이동된다.

![스크린샷 2021-12-14 오후 12.11.31.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3f684f96-86b9-4751-94b3-6d978350d75f/스크린샷_2021-12-14_오후_12.11.31.png)