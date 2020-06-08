# calculater 
vue demo

> 拟物风计算器 

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```



## 效果图

![](https://user-gold-cdn.xitu.io/2020/6/8/172930a1ce89a59a?w=946&h=995&f=png&s=56196)


## 核心代码

* 样式代码
    ``` bash
  
    .calculator {
    --button-width: 80px;
    --button-height: 80px;

    # grid 布局
    display: grid;
    grid-template-areas:
        "result result result result"
        "ac plus-minus percent divide"
        "number-7 number-8 number-9 multiply"
        "number-4 number-5 number-6 subtract"
        "number-1 number-2 number-3 add"
        "number-0 number-0 dot equal";
    grid-template-columns: repeat(4, var(--button-width));
    grid-template-rows: repeat(6, var(--button-height));
    # 拟物风样式
    box-shadow: -8px -8px 16px -10px rgba(255, 255, 255, 1),
        8px 8px 16px -10px rgba(0, 0, 0, 0.15);
    padding: 24px;
    border-radius: 20px;
    }

    .calculator button {
    margin: 8px;
    padding: 0;
    border: 0;
    display: block;
    outline: none;
    border-radius: 24px;
    font-size: 24px;
    font-family: Helvetica;
    font-weight: normal;
    color: #999;
    
    # 拟物风样式
    background: linear-gradient(
        135deg,
        rgba(230, 230, 230, 1) 0%,
        rgba(246, 246, 246, 1) 100%
    );
    box-shadow: -4px -4px 10px -8px rgba(255, 255, 255, 1),
        4px 4px 10px -8px rgba(0, 0, 0, 0.3);
    }

    # 拟物风按钮点击样式
    .calculator button:active {
    box-shadow: -4px -4px 10px -8px rgba(255, 255, 255, 1) inset,
        4px 4px 10px -8px rgba(0, 0, 0, 0.3) inset;
    }

    ```


* 逻辑代码
  ``` bash
    # 重要参数
    equation: "0",
      isDecimalAdded: false, # 是否输入小数点
      isOperatorAdded: false, # 是否点击了运算符号，防止连续点击超过1个运算符
      isStarted: false # 判断计算器是否已经输入数字
  ```
  ``` bash
   # 检测参数中是否含有加减乘除运算字符
    isOperator(character) {
      return ["+", "-", "×", "÷"].indexOf(character) > -1;
    },
  ```
  ``` bash

     # when pressed Operators or Numbers
    append(character) {
      # start
      if (this.equation === "0" && !this.isOperator(character)) {
        if (character === ".") {
          this.equation += "" + character;
          this.isDecimalAdded = true;
        } else {
          this.equation = "" + character;
        }

        this.isStarted = true;
        return;
      }

      # If number
      if (!this.isOperator(character)) {

        if (character === "." && this.isDecimalAdded) {
          return;
        }
        if (character === ".") {
          this.isDecimalAdded = true;
          this.isOperatorAdded = true;
        }else{
          this.isOperatorAdded = false;
        }
        this.equation += "" + character;
      }

      # Added Operator
      if (this.isOperator(character) && !this.isOperatorAdded) {

        this.equation += "" + character;
        this.isDecimalAdded = false;
        this.isOperatorAdded = true;
      }
    },

    # when pressed '='
    calculate() {


      let result = this.equation
        .replace(new RegExp("×", "g"), "*")
        .replace(new RegExp("÷", "g"), "/");
        # 计算结果
      this.equation = parseFloat(eval(result).toFixed(9)).toString();
      this.isDecimalAdded = false;
      this.isOperatorAdded = false;
    },

  ```