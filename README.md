

# Repo 개요

본 repo는 [GENEXIS-AI/arXiv-PDF-Translator](https://github.com/GENEXIS-AI/arXiv-PDF-Translator) 의 Fork이며, 소소한 문제점 해결을 위해 아래의 update 작업을 진행하였습니다.

## 배경
제가 논문을 읽을 때 ChatGPT나 Perplexity 등으로부터 큰 도움을 받고 있습니다. 그런데 이들 LLM들이 점점 더 검색에 의존하는 경향이 커지고 있어서, 좀 오래된 논문들과 같은 경우에는 현재의 수준과는 매우 동떨어진 결과를 얻는 경우가 많았습니다. 아마도 논문 작성 시점의 블로그 리뷰 등이 검색되고 LLM 답변에 반영되는 것으로 보이는데, 이 때문에 결국 본 논문을 다시 확인해야 하더군요. 이럴 때 포맷이 유지된 형태로 번역된 논문을 빠르게 볼 수 있으면 좋을 것이라 생각했습니다.

아래는 제가 주로 작업하는 윈도 환경에서 gpt-4o를 사용해 얻은 번역 결과입니다. 보시다시피, 

1. 작성 시점(24.12월) 기준으로 일부 번역이 안되는 부분이 있으니 참고하시기 바랍니다.
2. 대략 10개 논문 중에서 7~8개 정도만 최종 pdf 생성에 성공합니다. (실패 원인에 대해 분석이나 추가 작업 계획이 없습니다. ㅠㅠ)
3. 실패 케이스나 결과물 품질을 생각하면, 시간 낭비라고 생각될 수도 있습니다.

https://github.com/secutron/arXiv-PDF-Translator-2411/blob/main/sample_0.jpg?raw=true

https://github.com/secutron/arXiv-PDF-Translator-2411/blob/main/sample_1.jpg?raw=true



## update: 
    1. dotenv로 api-key 분리
    2. openai의 api 변경내용 적용



# arXiv 논문 자동 번역 및 PDF 생성 도구

arXiv-PDF-Translator는 arXiv 논문을 다운로드하고, LaTeX 소스 파일을 자동 번역하여 PDF 파일로 생성하는 강력한 도구입니다. OpenAI의 GPT API를 활용하여 복잡한 LaTeX 문서를 한국어로 정확하게 번역하며, 이를 고품질 PDF 문서로 컴파일합니다.

arXiv-PDF-Translator는 연구자, 학계 종사자 및 기관을 위한 효율적이고 신뢰성 높은 도구로, 빠르고 정확한 논문 번역을 제공합니다.

## 동영상 가이드

이 프로젝트의 사용 방법에 대한 자세한 설명은 아래 동영상을 참고하세요:

![사용법](https://github.com/GENEXIS-AI/arXiv-PDF-Translator/blob/main/demo.mp4)]

## 기능

- 자동 워크플로우: arXiv 논문의 ID 또는 URL을 입력하면 논문 다운로드부터 번역, PDF 생성까지 모든 과정을 자동으로 처리합니다.
- 정확한 번역: OpenAI GPT API를 활용하여 복잡한 LaTeX 문서를 한국어로 번역하면서 LaTeX 명령어를 정확하게 보존합니다.
- 고품질 PDF 출력: 번역된 LaTeX 문서를 xelatex을 사용하여 전문적인 품질의 PDF로 컴파일합니다.
- 맞춤 설정 가능: 폰트 설정, LaTeX 명령어, 번역 옵션 등을 사용자의 필요에 맞게 쉽게 조정할 수 있습니다.

## 요구 사항

### 필수 패키지

이 프로젝트를 실행하기 위해서는 다음 Python 패키지가 필요합니다:

- `requests`
- `beautifulsoup4`
- `openai`
- `concurrent.futures` (내장 모듈)
- `tarfile` (내장 모듈)
- `subprocess` (내장 모듈)
- `shutil` (내장 모듈)
- `json` (내장 모듈)
- `time` (내장 모듈)
- `os` (내장 모듈)
- `re` (내장 모듈)
- `logging` (내장 모듈)
- `bs4`

패키지는 아래 명령어로 설치할 수 있습니다:

```bash
pip install requests beautifulsoup4 openai bs4 lxml
```

### LaTeX 설치

PDF 파일을 생성하기 위해서는 LaTeX이 시스템에 설치되어 있어야 합니다. 이 도구는 `xelatex`를 사용하여 PDF를 컴파일합니다. 다음과 같은 방법으로 LaTeX 배포판을 설치할 수 있습니다:

- **TeX Live** (Cross-platform): [TeX Live 다운로드 페이지](https://www.tug.org/texlive/)
- **MiKTeX** (Windows): [MiKTeX 다운로드 페이지](https://miktex.org/download)
- **MacTeX** (macOS): [MacTeX 다운로드 페이지](https://tug.org/mactex/)

### 폰트 설치

이 프로젝트에서 사용되는 한국어 폰트는 Noto Sans KR입니다. 이 폰트를 설치해야 합니다:

- [Noto Sans KR](https://fonts.google.com/noto/specimen/Noto+Sans+KR)

다운로드한 후 시스템에 설치해 주세요.

### OpenAI API 키

이 도구는 OpenAI GPT API를 사용하여 텍스트를 번역합니다. 따라서 OpenAI API 키가 필요합니다. API 키는 [OpenAI 홈페이지](https://platform.openai.com/)에서 얻을 수 있습니다. API 키를 환경 변수로 설정하거나, 스크립트에서 직접 설정할 수 있습니다.

## 사용 방법

1. 이 저장소를 클론하거나 소스 코드를 다운로드합니다.
2. Python 패키지와 LaTeX, 폰트를 설치합니다.
3. 스크립트를 실행하고, arXiv 논문 ID 또는 URL을 입력합니다.
4. 한국어로 번역된 PDF 파일이 현재 작업 디렉토리에 생성됩니다.

```bash
python script.py
```

실행 시, 다음과 같은 메시지가 출력됩니다:

```
Enter ArXiv ID or URL: 
```

여기에 arXiv 논문 ID 또는 URL을 입력하면 번역 및 PDF 생성이 자동으로 진행됩니다.


## 주의사항

- OpenAI API 키는 개인 정보이므로 공개되지 않도록 주의하세요.
- 번역 결과는 자동화된 번역이므로, 필요한 경우 수동으로 검토 및 수정이 필요할 수 있습니다.
- 생성된 PDF 파일의 품질은 원본 LaTeX 파일의 구조에 따라 다를 수 있습니다.

## 라이선스

이 프로젝트는 MIT 라이선스 하에 배포됩니다. 자세한 내용은 LICENSE 파일을 참조하세요.
