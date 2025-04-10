## GOLF: GPT Organized Log Finder
기본적인 챗봇 기능에 RAG를 통한 대화 기록 검색 도우미 챗봇을 추가하는 프로젝트입니다.

![GOLF overview](/images/overview.png)  
[Youtube link](https://youtu.be/wx9slMmwu0k)  
[PDF](부트캠프_PPT_최종본.pdf)  

## 기술 스택
### Language
![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)

### Frontend
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=flat-square&logo=streamlit&logoColor=white)

### Backend
![LangChain](https://img.shields.io/badge/LangChain-121212?style=flat-square)

### Database
![SQLite](https://img.shields.io/badge/SQLite-003B57?style=flat-square&logo=sqlite&logoColor=white)

### Distribute
![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat-square&logo=amazon-aws&logoColor=white)

## 기능 설명
- 로그인
    - 각 유저의 이메일과 비밀번호를 DB에 저장하여 회원가입, 로그인, 로그아웃, 탈퇴 기능을 구현
- LLM과의 대화
    - - Azure OpenAI의 GPT4와 대화 기능을 구현하여 기본 ChatGPT의 기능을 재현함
    - langchain을 이용해서 대화 히스토리를 저장하고 이 히스토리와 유저 쿼리를 보내 답변을 받는 방식을 에이전트로 구현해 효율적으로 대화 기능을 구현함
- 대화 기록
    - 각 대화마다 별개의 히스토리를 두고 에이전트는 유저가 선택한 대화에 대해서 별개의 히스토리를 사용함
    - 각 대화마다 UUID를 할당하고 이를 유저 DB에 추가하여 각 유저마다 가진 대화 UUID를 통해서 대화를 대화 DB에서 불러올 수 있게 함
    - 이를 통해서 대화를 한 곳에 모아두는 동시에 유저 DB와 연결할 수 있게 함
- RAG를 이용한 대화 검색
    - 대화 DB에 대화를 저장할 때 대화를 LLM을 이용해서 요약하여 저장
    - 유저가 대화를 검색할 때 요약본 전체를 불러오고 유저 쿼리에서 키워드를 골라 내어 요약본을 대상으로 키워드를 검색하여 적절한 대화 UUID 리스트를 가져옴
    - 이 UUID 리스트는 LLM이 답변을 내기 전에 유저 쿼리와 같이 보내어 답변을 만듦
    - 웹 어프리케이션에선 LLM 답변에 나타난 UUID 리스트를 각 대화 버튼으로 대체하여 유저가 원하는 대화로 이동하게 함
- AWS에 서버를 올리고 도메인을 이용해 배포
    - EC2 리눅스에 웹 어플리케이션 서버를 업로드하고 쉘 파일을 이용해 구동시킨 후 Route와 도메인을 연결하여 배포함

## 설치 및 실행방법
### 1. 필요 라이브러리 설치
다음 명령어로 프로그램 실행에 필요한 라이브러리 설치  
```bash
pip install -r requirements.txt
```

### 2. .env 설정
.env 파일을 생성  
그 안에 다음 내용 채우기
```bash
api_key = AZURE_OPENAI_API_KEY
api_version = OPENAI_API_VERSION
azure_endpoint = AZURE_OPENAI_ENDPOINT
model = AZURE_OPENAI_DEPLOYMENT
```

### 3. 실행
다음 명령으로 streamlit 앱으로 실행
```bash
streamlit run streamlit_app.py
```
