---
layout: article
title: Verilog HDL 2장 
tags: Hardware Verilog
mode: immersive
header:
  theme: dark
article_header:
  type: overlay
  theme: dark
  background_color: '#203028'
  background_image:
    gradient: 'linear-gradient(135deg, rgba(34, 139, 87 , .4), rgba(139, 34, 139, .4))'
    src: /background_mountain.jpg


---

# Module & Instantization
<!--more-->

## 2.1 Module

반도체 전체 회로도를 한번 생각해 보면 아래와 같이 적게는 수천개 많게는 수백억개 까지의 논리회로로 구성되어 있을 것입니다. 

4bit adder와 4bit mux로 구성된 매우 간단한 회로를 생각해봅시다.

<img src="/Users/parkdongho/Documents/공부/시스템_반도체_설계_스터디/3주차/img/fig_2_4.png" alt="mux" style="zoom:50%;" />

단순한 기능을 하는 회로임에도 매우 복잡하죠....



따라서 아래 그림과 같이 회로를 작은 부분부분으로 쪼개서 설계할 필요가 있습니다.

<img src="/Users/parkdongho/Documents/공부/시스템_반도체_설계_스터디/3주차/img/fig_2_5.png" alt="mux" style="zoom:50%;" />

이렇게 회로를 분해한 기본 단위를 module 이라고 합니다.



verilog에서 module은 아래와 같이 기술 합니다.

```verilog
module 모듈명(
  input 입력_포트_명, //1bit input
  input 입력_포트_명, //1bit input
  input 입력_포트_명, //1bit input
  
  output 출력_포트_명 //1bit output
);
  
  //여기에 내부 로직 기술
  
endmodule
```



아래는 2 to 1 mux의 module 예시 입니다.

<img src="/Users/parkdongho/Documents/공부/시스템_반도체_설계_스터디/3주차/img/fig_2_6.png" alt="mux" style="zoom:50%;" />

2 to 1 mux는 데이터 입력이 2포트(A, B), 데이터 선택 포트(S), 출력이 1포트(Z)를 가집니다.

__Example 2.1__

```verilog
module ex_2_01_mux_2_to_1(
  input A, //1bit input
  input B, //1bit input
  input S, //1bit input
  
  output Z //1bit output
);
  
  //여기에 내부 로직 기술
  
endmodule
```

모듈명은 : ex_2_01_mux_2_to_1

입력포트는 : A, B, S

출력 포트는 : Z

입니다.



베릴로그에서 포트를 선언할 때 데이터 타입을 생략할 수 있습니다. 만약 데이터 타입을 생략 한다면 wire 타입을 가집니다. 하지만 설계를 명확하게 하기 위해 아래와 같이 wire 타입 임에도 타입을 선언 하는 것을 권장합니다.

```verilog
module 모듈명(
  input 변수_타입 입력_포트_명, //1bit input
  input 변수_타입 입력_포트_명, //1bit input
  input 변수_타입 입력_포트_명, //1bit input
  
  output 변수_타입 출력_포트_명 //1bit output
);
  
  //여기에 내부 로직 기술
  
endmodule
```

__Example 2.2__

```verilog
module ex_2_02_mux_2_to_1(
  input wire A, //1bit input
  input wire B, //1bit input
  input wire S, //1bit input
  
  output reg Z //1bit output
);
  
  //여기에 내부 로직 기술
  
endmodule
```



아래 그림과 같이 n-bit의 입출력을 갖는 회로도 존재합니다.  

n-bit의 포트는 아래와 같이 기술 할 수 있습니다.

![mux](/Users/parkdongho/Documents/공부/시스템_반도체_설계_스터디/3주차/img/fig_2_7.png)

```verilog
module 모듈명(
  input 변수_타입 [MSB:LSB] 입력_포트_명, //nbit input
  input 변수_타입 [MSB:LSB] 입력_포트_명, //n-bit input
  input 변수_타입 [MSB:LSB] 입력_포트_명, //n-bit input
  
  output 변수_타입 [MSB:LSB] 출력_포트_명 //n-bit output
);
  
  //여기에 내부 로직 기술
  
endmodule
```

__Example 2.3__

```verilog
module ex_2_03_mux_2_to_1(
  input wire  [3:0] A, //4bit input
  input wire  [3:0] B, //4bit input
  input wire        S, //1bit input
  
  output reg [3:0] Z //4bit output
);
  
  //여기에 내부 로직 기술
  
endmodule
```



이때 bitwidth를 parameter를 통하여 [WIDTH-1:0]와 같은 형태로 flexible하게 기술하는 것을 권장합니다.

```verilog
module 모듈명#(
  parameter WIDTH = 4 //bitwidth가 4bit 
)(
  input 변수_타입 [WIDTH-1:0] 입력_포트_명, //1bit input
  input 변수_타입 [WIDTH-1:0] 입력_포트_명, //1bit input
  input 변수_타입 [WIDTH-1:0] 입력_포트_명, //1bit input
  
  output 변수_타입 [WIDTH-1:0] 출력_포트_명 //1bit output
);
  
  //여기에 내부 로직 기술
  
endmodule
```

__Example 2.4__

```verilog
module ex_2_04_mux_2_to_1 #(
  parameter WIDTH = 4  
)(
  input wire  [WIDTH-1:0] A, //4bit input
  input wire  [WIDTH-1:0] B, //4bit input
  input wire              S, //1bit input
  
  output reg [WIDTH-1:0] Z //4bit output
);
  
  //여기에 내부 로직 기술
  
endmodule
```

