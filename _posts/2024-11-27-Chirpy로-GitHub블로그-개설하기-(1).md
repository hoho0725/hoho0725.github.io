---
title: Chirpy로 GitHub블로그 개설하기 (1)
date: 2024-11-27 14:36:33 +09:00
categories:
  - 블로그
  - 블로그 개설
tags:
  - 블로그
  - Chirpy
---
# __Chirpy란?__
Chirpy는 jekyll에서 블로그를 만들어주는 테마입니다.  깃허브 블로그를 방문하다 보면 가장 자주 만나는 테마일 것입니다. 간단하게 만들 수 있으면서도 다양한 기능을 제공하기에 많은 사람들이 사용하는 것 같습니다. 이번에는 이 Chirry테마를 이용하여 깃허브 블로그를 만들어보겠습니다. 
# __블로그 만들기__
먼저 Chirpy 테마를 적용하기 위해서는 그 테마를 적용할 블로그가 있어야 합니다. 보통은 깃허브를 이용하여 자신의 페이지를 만들고 그 곳에 테마를 적용합니다. Chirpy테마를 적용하는데는 크게 Stater template를 활용하기, Chirpy를 fork하기, clone하기가 있습니다. Starter는 매우 쉽게 블로그를 만들 수 있지만 다른 방법들보다 커스터마이징을 하는데 제약이 있습니다. fork와 clone은 비슷하지만 fork는 업데이트하며 오류가 날 수 있으므로 여기서는 clone을 사용해 블로그를 만들도록 하겠습니다. 그럼 먼저 미리 준비해야 하는 것들을 알아보겠습니다. __2024년 11월 window11 기준으로 작성되었습니다.__
##  __0. 미리 준비하기__
_아마도 Ruby를 제외하면 이미 설치되어 있을 것 같습니다_
### 🚨 __필수 항목__
#### 1. [GitHub 회원가입](https://github.com/)
블로그를 깃허브를 통해 만들 것이므로 회원가입이 필수입니다. 깃허브 회원가입은 홈페이지에서 쉽게 할 수 있으므로 여기서는 생략하도록 하겠습니다.
#### 2. [Ruby](https://rubyinstaller.org/downloads/)
Chirpy는 jekyll에서 작동하는데 이 지킬이 루비로 이루어져 있습니다. 따라서 로컬에서 홈페이지를 테스트해보기 위해서 Ruby를 필수적으로 설치해야 합니다. 설치하기 위해서 위의 링크로 들어갑니다
![Ruby_install](assets/img/post/Github_blog_1/Ruby_install.png)
링크를 타고 들어가면 이 화면이 나오는데 여기서 저 `=>` 표시가 되어있는 화살표 부분을 설치하시면 됩니다. 설치후 파일을 실행하고 다 next눌러서 넘기시면 설치가 완료되고 `Start Command Prompt With Ruby`프로그램이 시작화면에 있다면 제대로 된 것입니다.
#### 3. [GitHub Desktop](https://desktop.github.com/download/)
깃허브를 로컬에서 쉽게 관리하기 위해서 반 필수 입니다. Git을 설치해서 관리해도 되지만 깃허브 데스크탑이 훨씬 편합니다. 깃허브 데스크탑 또한 링크를 타고 들어가 쉽게 설치할 수 있으므로 여기서는 생략하도록 하겠습니다. 

---
### ✨ __추천 항목__
#### 1. vscode
폴더관리와 깃 관리를 더 쉽게 만들어 줍니다. 굳이 설치하지 않아도 블로그를 만드는데 지장은 없지만 설치하면 훨씬 쉽게 관리하실 수 있을 것입니다. 

## __1. 깃허브 레파지토리 만들기__
블로그를 처음 만들기 위해서는 깃허브에 블로그의 정보를 담는 레파지토리가 있어야 합니다. 일단 깃허브 로그인을 해줍니다. 로그인이 안되어있다면 회원가입을 해줍니다. 그 후 홈화면에서 new를 클릭하고 새 레파지토리를 생성합니다. 이때 레파지토리의 이름은 무조건 `[계정 이름].github.io` 형식으로 작성해야 합니다. 다른 설정은 바꾸지 않고 Create Repository를 누르면 됩니다.
![git](assets/img/post/Github_blog_1/GitHub_Repository_Create.png)
*저는 이미 저장소를 만들어둬서 경고가 뜨지만 처음 만드는 경우에는 초록색 글씨가 뜰거에요.*
## __2. 레파지토리 로컬로 이동__
만들어진 레파지토리는 깃허브 사이트에서는 수정하기 힘들기에 로컬로 옮겨서 작업하도록 하겠습니다. 로컬로 옮기기위해 깃허브 데스크탑으로 이동합니다. 이미 깃허브 데스크탑을 사용하고 있으신 분들은 잘 하실테니 여기서는 처음하시는분 기준으로 작성하겠습니다. 깃허브 데스크탑을 열고 로그인을 하시면 이 화면이 나타날 것입니다.
![GitHub_Desktop_Home](assets/img/post/Github_blog_1/GitHub_Desktop_home.png)
여기서 아까 만들어둔 레파지토리를 선택하고 Clone합니다. 이후 나오는 창에서 레파지토리를 클론한 로컬 위치를 설정하고 다시 Clone을 누르면 로컬에 레파지토리를 받아올 수 있습니다.
## __3. Chirpy 설치__
이제 이 레파지토리에 Chirpy테마를 채우도록 하겠습니다. 일단 Chirpy를 받아와야 합니다.

> [Chirpy 링크](https://github.com/cotes2020/jekyll-theme-chirpy)

링크로 들어가 초록색 Code버튼을 누르고 Download zip으로 파일들을 받아옵니다. 받아온 파일을 압축해제하고 폴더 안의 파일들을 아까 로컬에 받아온 레파지토리 안에 넣습니다. 
## __4. config 설정하기__
블로그가 제대로 작동하기 위해서는 config를 설정해야 합니다. 다운받은 폴더를 보면 가장 상위 폴더 안에 .config.yml파일을 찾을 수 있을 것입니다. 이 파일로 들어가서 몇 가지 사항을 수정하겠습니다. 
먼저 두번째로 보이는 lang입니다. 값이 en으로 되어있을텐데 ko-KR로 바꾸면 블로그의 영어로 작성되어있는 텍스트들이 한국어로 바뀝니다.
```yaml
lang: ko-KR
```

다음은 timezone입니다. Asia/Seoul로 바꾸시면 됩니다.
```yaml
timezone: Asia/Seoul
```

title은 블로그 이름입니다 원하는데로 작성해주세요
```yaml
title: Hoho
```

tagline은 블로그 이름 밑에 들어가는 설명입니다. 한 줄정도의 설명을 적어주시면 됩니다
```yaml
tagline: hoho의 개발 블로그
```

description는 검색에 사용되는 문장입니다. 보통 한글 80자 미만 정도로 작성하는것을 권장합니다.
```yaml
description: >- # used by seo meta and the atom feed
  hoho 개발 블로그 | programing, github
```

가장 중요한 url입니다. 이것이 제대로 설정되어 있지 않으면 정상작동 하지 않을 수 있습니다. 이곳에는 자신의 블로그 url을 적으면 됩니다. 블로그 url은 `https://[깃허브아이디].github.io`형식으로 적으시면 됩니다.
```yaml
url: "https://hoho0725.github.io"
```

소셜부분은 자신의 소셜미디어의 정보를 적으시면 됩니다.
## __4. 블로그 개설하기__
기본 설정을 완료하셨다면 이제 블로그를 개설할 차례입니다. 다시 깃허브 데스크탑으로 돌아와 지금까지 편집한 내용들을 커밋하고 푸쉬하여 깃허브로 전송합니다. 깃허브 사이트로 돌아가 몇가지 설정을 합시다.
![GitHub_Page_Setting](/assets/img/post/Github_blog_1/GitHub_Page_Setting.png)
레파지토리로 들어와 세팅에서 Pages로 들어갑니다. 그리고 Source가 Deploy from a brunch로 되어있다면 GitHub Actions로 바꿉니다. 그리고 직접 자신의 블로그 url을 작성하거나 Visit site를 눌러 자신의 블로그를 확인해 봅시다.
> Jekyll의 Chirpy Theme로 깃허브 블로그를 시작하는 방법을 소개합니다. 
{: .prompt-info }

![[Pasted image 20241128193237.png]]
