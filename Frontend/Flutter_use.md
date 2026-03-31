# Flutter Use

[Flutter](https://flutter.dev/)

## Flutter Quick Start

1. 개발 플랫폼 선택
   - 윈도우
   - macOS
   - 리눅스
   - ChromeOS

2. 필수 소프트웨어
   - Window용 Git
   - Visual Studio Code

3. Flutter를 설치하고 설정
   Git과 VS Code를 설치했으니, 이제 다음 단계를 따라 VS Code를 사용하여 Flutter를 설치하고 설정하세요
   1. VS code를 실행
        VS Code가 아직 열려 있지 않다면 Spotlight 검색을 이용하거나 설치된 디렉토리에서 직접 실행하여 열어주세요.
        설치된 디렉토리란 VS Code 프로그램 파일이 실제로 들어있는 폴더이다
        1. 운영체제별 VS Code 설치 경로 (기본 위치)
        macOS (맥):

            보통 응용 프로그램(Applications) 폴더 안에 있습니다.

            Finder를 열고 왼쪽 탭에서 '응용 프로그램'을 누르면 파란색 리본 모양의 Visual Studio Code 아이콘을 찾을 수 있습니다.

        Windows (윈도우):

            사용자별 설치 시: %LocalAppData%\Programs\Microsoft VS Code

            시스템 전체 설치 시: C:\Program Files\Microsoft VS Code

            간단히 [시작] 버튼을 누르고 VS Code를 검색해서 오른쪽 마우스를 클릭해 **'파일 위치 열기'**를 선택하면 해당 폴더가 바로 뜹니다.

   2. VS Code에 Flutter 확장 프로그램을 추가

   3. VS Code를 사용하여 Flutter를 설치
      1. VS Code에서 명령 팔레트를 엽니다.

        보기 > 명령 팔레트 로 이동 하거나 Control+ Shift+ 키를 누르세요 P.

      2. 명령 팔레트에 를 입력합니다 flutter.

      3. Flutter: 새 프로젝트를 선택하세요 .

      4. VS Code에서 컴퓨터에서 Flutter SDK를 찾으라는 메시지가 표시됩니다. "SDK 다운로드"를 선택하세요 .

      4. Flutter SDK 설치 폴더 선택 대화 상자 가 나타나면 Flutter를 설치할 위치를 선택하세요.

      5. Flutter 클론을 클릭하세요 .

        Flutter를 다운로드하는 동안 VS Code에 다음과 같은 팝업 알림이 표시됩니다.

        Downloading the Flutter SDK. This may take a few minutes.

        다운로드에는 몇 분 정도 소요됩니다. 다운로드가 멈춘 것 같으면 '취소'를 클릭한 후 설치를 다시 시작하세요.

      1. SDK를 PATH에 추가하려면 클릭하세요 .

        성공하면 알림이 표시됩니다.

        The Flutter SDK was added to your PATH

      1. VS Code에서 Google Analytics 알림이 표시될 수 있습니다.

동의하시면 확인을 클릭하세요 .

Flutter가 모든 터미널에서 사용 가능하도록 하려면 다음 단계를 따르세요.

터미널 창을 모두 닫았다가 다시 여세요.
VS Code를 다시 시작하세요.
메모
VS Code 설치 과정에서 Android Studio 설치를 확인할 수 있으며, 설치되어 있지 않은 경우 경고 메시지가 표시될 수 있습니다. 웹, iOS 또는 macOS와 같은 다른 플랫폼을 대상으로 하는 경우에는 이 경고를 무시해도 설치가 성공적으로 완료됩니다. 설치 후에는 명령어를 실행하여 flutter doctor설치를 확인하십시오.
