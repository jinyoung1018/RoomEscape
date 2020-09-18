## 방탈출 게임

* 방탈출 게임
  * 게임 소개
    * 조작 방법
  * 소스코드

**게임소개**

방탈 라이브러리를 이용해서 파이썬으로 만든 **숨겨진 단서**를 찾아 방을 **탈출** 하는 게임.

조작 방법: 게임을 시작하면 화분을 옯겨 숨겨진 열쇠를 찾아 문을 연다. 문을 열면 ROOM2에 진입하게 되고, 전원 스위치를 끄면 보이는 암호를 입력하면 문이 열려 탈출에 성공한다.
  
소스코드 

```python
from bangtal import *
scene1 = Scene('룸1', 'Images/배경-1.png')

door1 = Object('Images/문-오른쪽-닫힘.png')
door1.locate(scene1,800,270)
door1.show( )


key = Object('Images/열쇠.png')
key.setScale(0.2)
key.locate(scene1, 600,150)
key.show( )

flowerpot = Object('Images/화분.png')
flowerpot.locate(scene1,550,150)
flowerpot.show( )

scene2 = Scene('룸2','Images/배경-2.png')

door2 = Object('Images/문-왼쪽-열림.png')
door2.locate(scene2,320,270)
door2.show( )

door3 = Object('Images/문-오른쪽-닫힘.png')
door3.locate(scene2,910,270)
door3.show( )

keypad = Object('Images/키패드.png')
keypad.locate(scene2,885,420)
keypad.show( )

switch = Object('Images/스위치.png')
switch.locate(scene2,880,440)
switch.show( )

password = Object('Images/암호.png')
password.locate(scene2, 400, 100)


door1.closed = True
def door1_onMouseAction(x,y,action):
   
    if door1.closed:
        if key.inHand( ):
            door1.setImage('Images/문-오른쪽-열림.png')
            door1.closed = False
        else:
            showMessage('열쇠가 필요해요~')
    else:
        scene2.enter( )
door1.onMouseAction = door1_onMouseAction


def key_onMouseAction(x,y,action):
    key.pick( )

key.onMouseAction = key_onMouseAction

flowerpot.moved = False
def flowerpot_onMouseAction(x,y,action):
    if action == MouseAction.DRAG_LEFT:
        flowerpot.locate(scene1,450,150)
        flowerpot.moved = True
    elif action == MouseAction.DRAG_RIGHT:
        flowerpot.locate(scene1,650,150)
        flowerpot.moved = True
flowerpot.onMouseAction = flowerpot_onMouseAction

def door2_onMouseAction(x,y,action):
    scene1.enter( )

door2.onMouseAction = door2_onMouseAction

door3.locked = True
door3.closed = True
def door3_onMouseAction(x,y,action):
    if door3.locked:
        showMessage('문이 잠겨있다')
    elif door3.closed:
        door3.setImage('Images/문-오른쪽-열림.png')
        door3.closed = False
    else:
        endGame( )
door3.onMouseAction = door3_onMouseAction

def door3_onKeypad( ):
    door3.locked = False
    showMessage('철커덕!!! 문이 열렸다.')
door3.onKeypad = door3_onKeypad

def keypad_onMouseAction(x,y,action):
    showKeypad('BANGTAL', door3)
keypad.onMouseAction = keypad_onMouseAction

switch.lighted = True
def switch_onMouseAction(x,y,action):
    switch.lighted = not switch.lighted
    if switch.lighted:
       scene2.setLight(1)
       password.hide( )
    else:
        scene2.setLight(0.25)
        password.show( )
switch.onMouseAction = switch_onMouseAction

startGame(scene1)
```


