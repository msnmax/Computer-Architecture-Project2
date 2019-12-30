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
    -----Register-----
    F1 1
    F2 1
    F3 2
    F4 None
    F5 None
    -----RAT-----
    F1 None
    F2 None
    F3 RS1
    F4 None
    F5 None
    -------RS-------
    RS1 + 1 2
    RS2 None None None
    RS3 None None None
    (ADD)Buffer=Empty
    RS4 None None None
    RS5 None None None
    (MUL)Buffer=Empty

    ------------------------------Cycle 2------------------------------
    -----Register-----
    F1 1
    F2 1
    F3 2
    F4 None
    F5 None
    -----RAT-----
    F1 None
    F2 None
    F3 RS1
    F4 RS4
    F5 None
    -------RS-------
    RS1 + 1 2
    RS2 None None None
    RS3 None None None
    (ADD)Buffer RS1=1+2
    RS4 / 1 RS1
    RS5 None None None
    (MUL)Buffer=Empty

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
##### Issue table: 根據input file create issue list
    Instruction1:[OP,Reg1,Reg2,Reg3]
    Instruction2:[OP,Reg1,Reg2,Reg3]
    Instruction3:[OP,Reg1,Reg2,Reg3]
                   .
                   .
                   .
##### Register Table and RAT Table:使用python dict()function創建
    Register={'F1':1,'F2':1,'F3':2,'F4':None,'F5':None}
    RAT={'F1':None,'F2':None,'F3':None,'F4':None,'F5':None}
##### RS table: 創建二維陣列根據加法器和乘法器個數創建,最後的issue time是之後執行可使用到
    RS=[[None for i in range(5)] for j in range(multiplier+adder)]
 RS table     | OP  | REG1 |REG2 |Issue Time 
 | ---------- | :-----------:  | :-----------: | :-----------:  | :-----------: |
 RS1    |     |     |      |     |
 RS2    |     |     |      |     | 
 RS3    |     |     |      |     |  
 RS4    |     |     |      |     |   
 RS5    |     |     |      |     |  

#### 4.updateRS() updateRAT()根據issue table進行issue的動作且印出所有table,如issue成功則將RS及RAT update,每回合只能執行一次 
#### 5.判斷Buffer是否有在使用,如無使用可將RAT抓取下來並update Buffer,但currentCycle必須>Issue time,如抓取進去一樣印出所有table
#### 6.根據addcycle mulcycle divcycle判斷buffer計算是否結束,如結束writeresult() 會將RAT,RS,Register值進行update
#### 7.Loop step4~6,根據keepgoing及initialstate判斷是否繼續,initialstate初值為1,進行第一輪之後會設為0,keepgoint()則是判斷RAT,Buffer是否為空,如都為空則回傳0
    while (keepgoing()or(initialstate))==1:

