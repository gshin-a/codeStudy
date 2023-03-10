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
        > map 파일과 scss에 대한 설명    
        https://github.com/gshin-a/TIL/blob/main/2303/230310.md#%EC%9D%B4%EC%8A%88
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

- 사이트 동작(onload)
    - handleLoadWindow
        - 페이지가 로딩되면 실행되는 onload 이벤트 함수
        - 페이지의 모든 요소들이 준비되어야 호출됨    
            > window.onload에 대한 설명 : https://itworldyo.tistory.com/101
        - 동작    
            - 사이트에 들어오면 게임 설명문이 나타나게 함
- 게임 설명 동작(onclick)
    - handleClickDocumentWrap
        - 게임 설명문의 onclick 이벤트 함수
        - 동작
            - 게임 설명문을 클릭하면 사라짐
- 게임 동작
    ```mermaid
    graph LR
    A[handleClickStart]-->init
    A-->B[startTimer]
    A-->showVersusWrapAndUserSelectionWrap
    A-->C[startRound]

    B[startTimer]-->|startCount*1000|B_d[delay]

    C[startRound]-->|true|setUserSelectionButtonDisabled
    C-->setSignal
    C-->D[fight]

    D[fight]-->|currentWinner가 'user'일 때|E_1[유저가이기고있을때규칙]
    D-->|currentWinner가 'user'가 아닐 때|E_2[컴퓨터가이기고있을때규칙]
    D-->|term|D_d[delay]
    D-->mukChiPaRule
    D-->F[decideWinner]

    E_1[유저가이기고있을때규칙]-->|false|E_1_s[setUserSelectionButtonDisabled]
    E_1-->|ms,setComputerStatus|E_1_d[delay]
    E_1_d-->E_1_d_s[setComputerStatus]-->|computerStatus,'computer'|setThirdSignalInnerHtml

    E_2[컴퓨터가이기고있을때규칙]-->|ms-term,setUserSelectionButtonDisabled:false|E_2_d_1[delay]
    E_2_d_1-->|false|E_2_d_1_s[setUserSelectionButtonDisabled]
    E_2-->|term,setComputerStatus|E_2_d_2[delay]
    E_2_d_2-->E_2_d_2_s[setComputerStatus]-->|computerStatus,'computer'|setThirdSignalInnerHtml

    F[decideWinner]-->|winner가 'computer'일 때 : 'defeated'|showResult
    F-->|winner가 : 'user'일 때 won'|showResult
    F-.->|winner가 'computer'와 'user'가 아닐 때|C
    ```

    - 이벤트 함수(onclick)
        - handleClickStart
            - start button의 onclick 이벤트 함수
            - 게임을 시작시키는 함수
            - 동작
                - 변수 초기화(init)
                - 타이머를 동작시킴(startTimer)
                - 게임 화면을 띄움(showVersusWrapAndUserSelectionWrap)
                - 게임을 시작함(startRound)
        - setUserStatus : 'rock' / 'scissors' / 'paper'
            - 가위, 바위, 보 버튼 중 하나를 클릭하면 실행되는 함수
            - 동작
                - 유저가 누른 버튼에 따라 유저의 가위바위보 상태를 결정하고 이미지로 보여줌
                - 유저의 가위바위보 상태에 따라 세 번째 신호로 띄움(setThirdSignalInnerHtml)
    - init
        - 변수들을 초기화시키는 함수
    - startTimer
        - 타이머를 동작시키는 함수
        - 동작
            - setInterval로 타이머를 작동시키고 delay()로 3초의 딜레이를 준 후 timer를 중지시킨 후 timer 제거함
    - delay
        - 딜레이를 주는 함수
        - 동작
            - ms와 callback 함수를 파라미터로 받아서 callback 함수가 있으면 ms 후에 실행시키고 없으면 그냥 종료
    - showVersusWrapAndUserSelectionWrap
        - 게임화면과 유저가 가위바위보를 선택하는 버튼을 보여주는 함수
    - startRound
        - 게임 라운드를 시작시키는 함수
        - 동작
            - 유저 가위바위보 선택 버튼을 사라지게 함(setUserSelectionButtonDisabled(true))
            - 헤더에서 가위바위보 신호를 줌(setSignal)
            - 가위바위보를 진행함(fight)
    - setUserSelectionButtonDisabled
        - 유저가 가위바위보를 선택하는 버튼을 보이지 않게 하는 함수
    - setSignal
        - 가위바위보 신호를 보여주는 함수
        - 동작
            - 기존의 신호를 제거함
            - 현재 winner에 해당하는 라운드 시간에 맞춰 신호 시간을 설정함
            - 신호를 보여줌
    - fight
        - 가위바위보 게임을 진행하는 함수
        - 동작
            - 현재 winner가 유저이면 유저가 이기고 있을 때의 규칙을 적용하고 아니면 컴퓨터가 이기고 있을 때의 규칙을 적용함(유저가이기고있을때규칙 or 컴퓨터가이기고있을때규칙)
            - 묵찌빠 규칙을 적용함(mukChiPaRule)
            - 승자를 결정함(decideWinner)
    - 유저가이기고있을때규칙
        - 동작
            - 유저 선택 버튼을 보여줌(setUserSelectionButtonDisabled(false)) 
            - 딜레이 이후 컴퓨터가 선택하게 함(delay <- setComputerStatus)
    - 컴퓨터가이기고있을때규칙
        - 동작
            - 유저 선택 버튼을 보여줌(setUserSelectionButtonDisabled(false)) 단, 유저가 이기고 있을 때보다 짧은 시간만큼만.
            - 딜레이 이후 컴퓨터가 선택하게 함(delay <- setComputerStatus)
    - mukChiPaRule
        - 게임의 현재 상태를 고려해 게임을 진행시키는 함수
        - 동작
            - 게임의 승자를 결정함
                1. 현재 게임이 가위바위보
                    1. 승자가 존재함 -> 현재 게임을 묵찌빠로 변경하고 가위바위보 승자에 따라 신호를 다르게 줌
                    2. 승자가 null임(비김)
                    3. 승자가 undefined임(유저가 가위바위보를 선택하지 않음)
                        - 최종 승자를 컴퓨터로 저장함
                2. 현재 게임이 묵찌빠
                    1. 승자가 null임(비김)
                        - 현재 승자가 최종 승자임
                    2. 승자가 존재함
                        - 승자가 가위바위보에서 선택한 걸 기준으로 묵찌빠 신호줌(마지막 신호는 제외함 -> 유저나 컴퓨터의 묵찌빠 선택에 따라 신호가 달라짐)
    - decideWinner
        - 승자에 따라 결과를 보여주거나 다음 라운드를 진행시키는 함수
        - 동작
            1. 승자가 컴퓨터일 경우 패배했다는 결과를 보여줌(showResult('defeated'))
            2. 승자가 유저일 경우 승리했다는 결과를 보여줌(showResult('won))
            3. 승자가 없는 경우 다음 라운드를 진행함(startRound)
    - showResult
        - 승자에 따라 결과를 보여주고 TRY AGAIN 함수를 보여주는 함수
    - setComputerStatus
        - 컴퓨터의 상태를 결정하는 함수
        - 동작
            - 랜덤값을 추출해 컴퓨터의 가위바위보 상태를 결정하고 이미지로 보여줌
            - 컴퓨터의 가위바위보 상태에 따라 세 번째 신호로 띄움(setThirdSignalInnerHtml)
    - setThirdSignalInnerHtml
        - 현재 게임이 묵찌빠일 때 세 번째 신호를 보여줌
- 게임 진행 예시
    - 가위바위보에서 유저가 승리하고 묵찌빠에서 유저와 컴퓨터의 선택이 같아 유저가 최종승리했을 경우
        1. 유저가 start button을 클릭하여 게임을 실행함
        2. 3초의 타이머가 실행된 후 가위바위보 시그널과 함께 버튼이 생성됨
        3. 유저가 가위바위보를 선택하여 컴퓨터와 대결하여 승리함(유저는 '가위', 컴퓨터는 '보'로 가정함)
        4. 게임이 가위바위보에서 묵찌빠로 변경됨
        5. 묵찌빠 신호가 나오며 버튼이 생성됨
        6. 유저의 선택과 컴퓨터의 랜덤값이 일치하여 유저가 게임에서 최종승리함(유저는 '빠', 컴퓨터는 '빠'로 가정함)
        7. TRY AGAIN 버튼이 생성됨
        ```mermaid
        graph TD
        user(유저startbutton클릭)-->handleClickStart
        handleClickStart-->init
        handleClickStart-->startTimer
        handleClickStart-->showVersusWrapAndUserSelectionWrap
        handleClickStart-->startRound:가위바위보

        startTimer-->|3초|delay

        startRound:가위바위보-->|true|setUserSelectionButtonDisabled
        startRound:가위바위보-->setSignal
        startRound:가위바위보-->fight

        fight-->컴퓨터가이기고있을때규칙
        fight-->|0.5초|f_d[delay]
        fight-->mukChiPaRule
        fight-->decideWinner

        컴퓨터가이기고있을때규칙-->|2.5초,setUserSelectionButtonDisabled|컴_d[delay]
        컴_d[delay]-->|false|컴_d_se[setUserSelectionButtonDisabled]
        컴퓨터가이기고있을때규칙-->|0.5초,setComputerStatus|컴_d2[delay]
        컴_d2[delay]-->setComputerStatus
        setComputerStatus-->|'보','computer'|setThirdSignalInnerHtml

        decideWinner-->startRound:묵찌빠

        startRound:묵찌빠-->|true|s_2[setUserSelectionButtonDisabled]
        startRound:묵찌빠-->sg_2[setSignal]
        startRound:묵찌빠-->f_2[fight]

        f_2[fight]-->유저가이기고있을때규칙
        유저가이기고있을때규칙-->|false|유_se[setUserSelectionButtonDisabled]
        유저가이기고있을때규칙-->|3초,setComputerStatus|유_d[delay]
        유_d[delay]-->유_d_se[setComputerStatus]
        유_d_se[setComputerStatus]-->|'빠','computer'|유_d_se_third[setThirdSignalInnerHtml]
        f_2[fight]-->|0.5초|f_2_d[delay]
        f_2[fight]-->f_2_muk[mukChiPaRule]
        f_2[fight]-->f_2_de[decideWinner]
        f_2_de[decideWinner]-->|'won'|showResult
        showResult-->result(종료 : 유저가승리함)
        ```
- 사용된 메서드
    - event.stopPropagation()    
        - 이벤트의 전파를 중단시킴. 즉, bubbling과 capturing 중단
        - bubbling: 상위요소로 event가 전파되는 것
        - capturing: 하위요소로 event가 전파되는 것