### 내가 개발한 웹 서비스에 주소를 입력해 접속해보기

IDE에서 개발한 서비스가 로컬 환경을 넘어서 실제 운영환경을 공부해보고 싶어서 시도한 프로젝트입니다. 현재는 AWS free tier가 종료됨에 따라 비용문제로 서버가 올라가 있지 않습니다. 프로젝트의 내용은 간단한 CRUD 기능을 가진 게시판형태의 서비스입니다. 

서버는 EC2 서버의 t2 micro 요금제를 사용했고, 저장소는 S3, DB는 AWS RDS를 사용했습니다.

Travis와 Amazond code deploy를 활용해 기초적인 CI/CD를 구현하였고, NginX를 활용하여 서버를 이중화하여 하나의 서버가 다운되거나 패치가 필요할 때 무중단 배포 서비스를 구축하였습니다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1f5ffc5a-9a86-457e-94e1-6573b3aab017/Untitled.png)

CI/CD의 전체 과정은 IDE에서 GITHUB 저장소에 소스코드를 push하면 Travis CI가 Build들 진행합니다. Build가 성공하면 S3에 빌드된 jar 파일이 전달되고 AWS CodeDeploy에서 EC2 서버에 배포를 진행합니다. EC2 내부에서는 이중화된 서버 중 서브 서버에서 새로운 jar 파일을 실행하여 패치하고NginX는 이제부터 새로운 요청이 들어오면 업데이트 된 포트로 연결시킵니다. 그리고 아직 패치가 완료되지 않은 서버를 내리고 업데이트를 진행합니다. 일련의 과정을 통해 무중단 배포 서비스를 구현하였고, IDE에서 push 한 코드가 곧바로 운영서버에 반영되기 때문에 테스트 코드를 작성해 정상적인 서비스가 될 수 있도록 하였습니다.
