# Computer-Architecture/Project2
## 簡介
* 實作Tomasulo algorithm
* SETUP
  * Environment:WIN10
  * IDE:Pycharm 4.0 Python 3.7
  * Language:Python
### Parameter:
Initial state:
```py    
Register={'F1':1,'F2':1,'F3':2,'F4':None,'F5':None}
addcycle=2 #set cycle
mulcycle=10
divcycle=20
adder=3
multiplier=2
```
### Input: 
    ADD F3,F2,F3
    DIV F4,F2,F3
    MUL F5,F3,F2
    ADD F3,F5,F1
    ADD F2,F4,F2
### Output: 
    ------------------------------Cycle 1------------------------------
    -----Register-----       -------RS-------
    F1 1                      RS1 + 1 2
    F2 1                      RS2 None None None
    F3 2                      RS3 None None None
    F4 None                   (ADD)Buffer=Empty
    F5 None                   RS4 None None None
    -----RAT-----             RS5 None None None
    F1 None                   (MUL)Buffer=Empty
    F2 None
    F3 RS1
    F4 None
    F5 None

    

### Step:
#### 1.Set parameter:
##### Register value:F1,F2,F3,F4,F5 可給初值
##### Cycle time:
- addcycle(subcycle)
- mulcycle
- divcycle
##### Adder and Multiper:
- adder 加法器個數
- multiper 乘法器個數
#### 2.Read input file:
    ADD F3,F2,F3
    DIV F4,F2,F3
    MUL F5,F3,F2
    ADD F3,F5,F1
    ADD F2,F4,F2
#### 3.Create Table:
##### Issue table:根據input file create issue list
    Instruction1:[OP,Reg1,Reg2,Reg3]
    Instruction2:[OP,Reg1,Reg2,Reg3]
    Instruction3:[OP,Reg1,Reg2,Reg3]
                   .
                   .
                   .
##### Register Table and RAT Table:使用python dict()function創建
    Register={'F1':1,'F2':1,'F3':2,'F4':None,'F5':None}
    RAT={'F1':None,'F2':None,'F3':None,'F4':None,'F5':None}
##### RS table:創建二維陣列根據加法器和乘法器個數創建
 RS table     | OP  | REG1 |REG2 |Issue Time 
 | ---------- | :-----------:  | :-----------: | :-----------:  | :-----------: |
 RS1    |     |     |      |     |
 RS2    |     |     |      |     | 
 RS3    |     |     |      |     |  
 RS4    |     |     |      |     |   
 RS5    |     |     |      |     |  
    RS=[[None for i in range(5)] for j in range(multiplier+adder)]
#### 4.history() function會記錄前兩筆結果也就是Outcome
##### select() function 根據history選擇prediction要使用的2BC
##### prediction() function 根據選擇的2BC做預測 T or N
##### changeREG() function 根據history以及Outcome去修改上次使用的2BC 
    Selector=01,Pred=T,Outcome=T,Hit
#### 5.Loop
    for i in range (len(outcome))
#### 6.Result:根據miss次數計算misspredicton
    Misprediction=0.098039
