# 묵찌빠 게임 레퍼런스 코드 분석

## 묵찌빠 게임 개요
- 컴퓨터와 승부하는 간단한 묵찌빠 게임을 작성하세요.
- 컴퓨터는 매번 랜덤하게 제출합니다.
- START 버튼을 누르면 게임이 시작 되며 버튼은 사라집니다.
- 처음 가위바위보를 할 때는 3 → 2 → 1 카운트를 하고 시작합니다.
- 가위바위보의 승패 상태에 따라서, 게임이 진행됩니다.
- 가위바위보의 상태가 win인 참여자가 컴퓨터인 경우는 가위바위보 이후 랜덤한 시그널을 2초후에 제출합니다. 유저는 2초 +-0.5초 내로 내야 합니다.
- 가위바위보의 상태가 win인 참여자가 참여자인 경우는 3초내로 아무때나 제출 할 수 있습니다. 참여자가 내자마자 컴퓨터는 랜덤한 값을 바로 계산해서 내도록 합니다. (혹은 미리 낼 것을 계산해서 참여자가 버튼을 누르는 순간 승패를 결정합니다)
- 묵찌빠의 한 라운드의 승자가 결정이 나면 결과와 함께 TRY AGAIN 버튼을 출력하세요.

## 파일 구조
- index.html
- script.js
- css
    - style.css
    - style.css.map
    - style.scss
- images
    - computer_paper.png
    - computer_rock.png
    - computer_scissor.png
    - defeated.png
    - user_paper.png
    - user_rock.png
    - user_scissor.png
    - won.png
---
## script.js