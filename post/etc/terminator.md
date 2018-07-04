### 시작하며
리눅스 민트를 os로 사용하면서 터미널로 terminator 사용하게 됐다. 일단 처음 세팅한 설정과 기본적인 단축키를 알아놔야겠다.

###  설정
```
[global_config]
  tab_position = top
  handle_size = 0
  focus = system
[keybindings]
[profiles]
  [[default]]
    scrollbar_position = hidden
    use_system_font = True
    background_darkness = 0.9
    background_type = transparent
    background_image = None
    show_titlebar = False
[layouts]
  [[default]]
    [[[child1]]]
      type = Terminal
      parent = window0
    [[[window0]]]
      type = Window
      parent = ""
      size = 1000, 600
[plugins]
```

### 단축키
`F11` 풀 스크린 모드
`Ctrl + Shift+ O` 수평으로 터미널 나누기
`Ctrl + Shift+ E` 수직으로 터미널 나누기
`Ctrl + Shift+ W` 현재 탭 닫기
`Ctrl + Shift+ T` 새로운 탭 열기
`Alt + ↑` (화면 분할 시) 현재 터미널에서 윗쪽 터미널로 이동
`Alt + ↓` (화면 분할 시) 현재 터미널에서 아랫쪽 터미널로 이동
`Alt + ←` (화면 분할 시) 현재 터미널에서 왼쪽 터미널로 이동
`Alt + →` (화면 분할 시) 현재 터미널에서 오른쪽 터미널로 이동

### 마치며
추가할 설정이나 단축키가 있으면 내용을 추가해야겠다.

### 참고
http://awesometic.tistory.com/96  
http://programmingsummaries.tistory.com/361  
