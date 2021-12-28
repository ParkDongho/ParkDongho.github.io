---
layout: article
title: Verilog HDL 3장
tags: Hardware Verilog VerilogHDL강좌
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
key: post
---

# Combinational Logic

<!--more-->

> ## 목차
>
> **[1부 - Verilog HDL]()**
>
> **Chapter 1 : [Introduction](https://parkdongho.github.io/2021/12/16/verilogHDL_chapter1_introduction.html)**
>
> **Chapter 2 : [Module & Instantiation](https://parkdongho.github.io/2021/12/19/verilogHDL_chapter2_module_&_instantiation.html)**
>
> **Chapter 3 : [Combinational Logic](https://parkdongho.github.io/2021/12/21/verilogHDL_chapter3_combinational_logic.html)**
>
> **Chapter 4 : [Sequential Logic](https://parkdongho.github.io/2021/12/23/verilogHDL_chapter4_sequential_logic.html)**
>
> **Chapter 5 : [FSM](https://parkdongho.github.io/2021/12/25/verilogHDL_chapter5_FSM.html)**
>
> **[2부 - SoC Design]()**
>
> **Chapter 1 : [Introduction]()**
>
> **Chapter 2  : [Memory Map]()**
>
> **Chapter 3  : [Bus Protocol]()**
>
> **Chapter 4 : [DMA]()**
>
> **Chapter 5 : [GPIO IP]()**
>
> **Chapter 6 : [Image Processing IP]()**
>
> **[3부 - NPU Design]()**
>
> **Chapter 1 : [Introduction]()**
>
> **Chapter 2 : [Datapath]()**
>
> **[부록]()**
>
> **Chapter A : [Vivado 설치 방법]()** 
>
> **Chapter B : [Vivado 사용 방법]()** 
>
> **Chapter C : [Drawio 사용방법]()**
>
> **Chapter D : [WaveDrom 사용방법]()**

---

## 3.1 Gate Level Modeling

standard cell을 instantiation하는 방법

RTL을 합성한 결과임



## 3.2 Dataflow Modeling

### 3.2.1 Continuous Assignment

연속 할당문은 net형 변수 객체에 우항의 연산결과를 할당합니다. 

이때 `우항의 연산결과가 변화가 발생`하면 바로 좌항에 값을 할당하는 특성을 가집니다.

이러한 특성은 combinational logic의 표현에 적합합니다.

```verilog
assign a = b; //b값에 변화가 발생시 a에 할당
assign a = b & c & d; //b & c & d의 연산결과에 변화가 발생시 a에 결과를 할당
```

---

```verilog
module(
  input wire a,
  input wire b,
  input wire s,
  output wire z
);
  assign z = (a & ~s) | (a & s);
endmodule
```

__Example 3.1__ continuous assignment를 활용한 2 to 1 mux

---

### 3.2.2 연산자

#### 3.2.2.1 산수 연산자

```verilog
out = a * b;
out = a / b;
out = a + b;
out = a - b;
out = a % b;
out = a ** b;
```

![figure 2](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-21-verilogHDL_chapter3_combinational_logic/시스템_반도체_설계_3장-figure_2.png)



---

#### 3.2.2.2 논리 연산자

```verilog
out = !a;
out = a && b;
out = a || b;
```

![figure 3](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-21-verilogHDL_chapter3_combinational_logic/시스템_반도체_설계_3장-figure_3.png)

---

#### 3.2.2.3 비교 연산자

```verilog
out = a > b;
out = a < b;
out = a >= b;
out = a <= b;
```

![figure 4](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-21-verilogHDL_chapter3_combinational_logic/시스템_반도체_설계_3장-figure_4.png)

---

#### 3.2.2.4 등가 연산자

```verilog
out = a == b;
out = a != b;
out = a === b;
out = a !== b;
```

![figure 5](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-21-verilogHDL_chapter3_combinational_logic/시스템_반도체_설계_3장-figure_5.png)



---

#### 3.2.2.5 비트단위 연산자

```verilog
out = ~a;
out = a & b;
out = a | b;
out = a ^ b;
out = a ^~ b; 또는  a ~^ b;
```

![figure 6](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-21-verilogHDL_chapter3_combinational_logic/시스템_반도체_설계_3장-figure_6.png)

---

#### 3.2.2.5 축소 연산자

```verilog
out = &a;
out = ~&a;
out = |a;
out = ~|a;
out = ^a
out = ^~a; 또는  ~^a;
```

![figure 7](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-21-verilogHDL_chapter3_combinational_logic/시스템_반도체_설계_3장-figure_7.png)

---

#### 3.2.2.6 시프트 연산자

```verilog
out = a >> b;
out = a << b;
out = a >>> b;
out = a <<< b;
```

![figure 2](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-21-verilogHDL_chapter3_combinational_logic/시스템_반도체_설계_3장-figure_8.png)

---


## 3.3 Behaviaral Modeling

### 3.3.1 always문

sensitivity list 내부의 값에 변화가 발생시 `begin ~ end` 내부의 구문이 실행됩니다.

sensitivity list 값의 구분은 `,` 를 통하여 합니다.

```verilog
always @(<sensitivity_list>, <sensitivity_list>, ... , <sensitivity_list>) begin
  //procedural assignment
end
```

---

### 3.3.2 blocking procedural assignment

procedural assignment는 `blocking`과 `non-blocking`으로 나누어 지며 blocking은 combinational logic을 non-blocking은 sequential logic을 표현하기 위해 사용됩니다. 본장에서는 combinational logic의 합성을 위해 사용되는 blocking assignment에 대해 중점적으로 알아볼것입니다.

* continuous assignment는 `우항의 연산결과에 변화가 발생`하는 이벤트 바로 좌항에 값을 할당하는 특성을 가졌습니다. 이와 반면에 procedural assignment는 이벤트를 감지하지 않고 문장이 실행되면 바로 할당을 합니다. 

* `wire`형에만 할당 가능한 continuous assignment와 달리 procedural assignment은 `reg`형 객체에만 할당을 할 수 있습니다.

* 절차적 할당은 `always` 문 혹은 `intial`문 내부에서 사용됨

이를 아래의 2 to 1 mux에 대한 예시를 통하여 더 자세히 알아봅시다.

```verilog
always @(a, b, s) begin //sensitivity list
  //procedural assignment
  z = (a & ~s) | (a & s);
end
```

__Example 3.2__ always문을 활용한 2 to 1 mux

2 to 1 mux의 input은 a, b, s가 있으므로 이를 sensitivity list에 넣어 주었습니다.

sensitivity list의 값 `a, b, s` 들에 변화가 존재하면 always 내부의 구문들을 실행합니다.

위의 예시에서는 blocking procedural assignment인  `z = (a & ~s) | (a & s);` 을 실행합니다.

### 3.3.3 blocking vs non-blocking assignment

* 값을 순서대로 할당

시뮬레이션 측면에서는 blocking assignment는 구문의 실행과 동시에 할당이 이루어 집니다.

```verilog
a = 1;
b = a;
```

* 값을 동시에 할당

시뮬레이션상에서는 할당할 계산결과를 queue에 저장후 iteration이 종료된 후 queue에 저장된 값을 할당을 합니다.

```verilog
a <= 1;
b <= a;
```

__Non-blocking assignment의 활용 : sequential logic을 합성 할 수 있음__

```verilog
always @(posedge clk) begin //sensitivity list
  //non-blocking assignment
  a <= 1;
  b <= a;
end
```

---

### 3.3.3 wildcard

하지만 아래 예시와 같이 매우 길고 복잡한 입력을 갖는 회로가 존재할수 있습니다. 

이러한 회로의 sensitivity list를 기술시 실수를 하기 쉽겠죠.

```verilog
always @(a, b, c, d, e, f, g, h, i, j, k, h, i, j, k) begin
  //procedural assignment
  z = a & (b | c);
  y = d + e + (f * j);
  x = (g * k) - h * i;
end
```

---

이런 상황에서 wildcard `*` 를 이용하여 모든 입력신호를 자동적으로 sensittivity list에 추가 해줄 수 있습니다.

```verilog
always @(*) begin //wild card *
  //procedural assignment
  z = a & (b | c);
  y = d + e + (f * j);
  x = (g * k) - h * i;
end
```



---

### 3.3.4 if문

```verilog
if(expression) begin
  statement;
end
else if (expression) begin
  statement;
end
else begin
  statement;
end
```



---



```verilog
module mux_2_to_1(
  input wire a,
  input wire b,
  input wire s,
  output wire z
);
  reg z_r;
  always @(*) begin
    if (s == 1'b0) begin
      z_r = a;
    end
    else begin //s == 1'b1
      z_r = b;
    end
  end
  
  assign z = z_r;
  
endmodule
```

__Example 3.3__ if문을 활용한 2 to 1 mux

---



```verilog
module mux_4_to_1(
  input wire a,
  input wire b,
  input wire c,
  input wire d,
  input wire s,
  
  output wire z
);
  reg z_r;
  always @(*) begin
    if (s == 2'b00) begin
      z = a;
    end
    else if (s == 2'b01) begin
      z = b;
    end
    else if (s == 2'b10) begin
      z = c;
    end
    else begin
      z = d;
    end
  end
  
  assign z = z_r;
  
endmodule
```

__Example 3.4__ if문을 활용한 4 to 1 mux

---

```verilog
module mux_4_to_1(
  input wire a,
  input wire b,
  input wire c,
  input wire d,
  input wire s,
  
  output wire z
);
  always @(*) begin
    if (s[1] == 1'b0) begin
      if (s[0] == 1'b0) begin
        z_r = a;
      end
      else begin
        z_r = b;
      end
    end
    else begin //s[1] == 1'b1
      if (s[0] == 1'b0) begin
        z_r = c;
      end
      else begin
        z_r = d;
      end
    end
  end
  assign z = z_r;
endmodule
```

__Example 3.5__ if문을 활용한 4 to 1 mux

---

### 3.3.5 case문

```verilog
always @(*) begin
  case(expression)
      condition : statements;
      condition : statements;
      condition : statements;
      default   : statements;
  endcase
end
```

---

```verilog
module mux_4_to_1(
  input wire a,
  input wire b,
  input wire c,
  input wire d,
  input wire s,
  
  output wire z
);
  always @(*) begin
    case(sel)
        2'b00 : z_r = a;
        2'b01 : z_r = b;
        2'b10 : z_r = c;
        2'b11 : z_r = d;
        default: z_r = 0;
    endcase
  end
  assign z = z_r;
endmodule
```

__Example 3.5__ case문을 활용한 4 to 1 mux

---

### 3.3.6 if문 vs case문



---

## 3.4 Case Study

### 3.4.1 8bit 4-to-1 Multiplexer

```verilog
module mux_4_to_1#(
  parameter WIDTH = 8
)(
  input wire [WIDTH-1:0] a, //nbit input data
  input wire [WIDTH-1:0] b, //nbit input data
  input wire [WIDTH-1:0] c, //nbit input data
  input wire [WIDTH-1:0] d, //nbit input data
  input wire [1:0] s, //2bit input select
  
  output wire [WIDTH-1:0] z //nbit output data
);
  reg [WIDTH-1:0] z_r;
  always @(*) begin
    case(sel)
      2'b00 : z_r = a;
      2'b01 : z_r = b;
      2'b10 : z_r = c;
      2'b11 : z_r = d;
      default: z_r = 0;
    endcase
  end
  assign z = z_r;
endmodule
```

---

### 3.4.2 Decoder

```verilog
module decoder(
  input wire [IN_WIDTH-1:0] data_i,
  output wire [OUT_WIDTH-1:0] data_o
);
  localparam IN_WIDTH = 4;
  localparam OUT_WIDTH = 8;
  reg [WIDTH-1:0] data_o_r;
  
  always @(*) begin
    casez(data_i)
      4'b?000 : data_o_r = 8'b00000000;
      4'b?001 : data_o_r = 8'b00000001;
      4'b?010 : data_o_r = 8'b00000011;
      4'b?011 : data_o_r = 8'b00000111;
      4'b?100 : data_o_r = 8'b00001111;
      4'b?101 : data_o_r = 8'b00011111;
      4'b?110 : data_o_r = 8'b00111111;
      4'b?111 : data_o_r = 8'b01111111;
      default: data_o_r = 0;
    endcase
  end
  assign data_o = data_o_r;
endmodule
```

---

### 3.4.3 Adder

```verilog
module adder(
  input wire [DATA_IN_WIDTH-1:0] data_a_i,
  input wire [DATA_IN_WIDTH-1:0] data_b_i,
  input wire carry_in_i,
  output wire [DATA_OUT_WIDTH-1:0] data_o
);
  assign data_o = data_a_i + data_b_i + carry_in_i;
endmodule
```

---

### 3.4.4 Subtractor

```verilog
assign data_o = data_a_i - data_b_i;
```

```verilog
assign data_o = data_a_i + ((~data_b_i) + 1);
```



```verilog
module substractor(
  input wire [DATA_IN_WIDTH-1:0] data_a_i,
  input wire [DATA_IN_WIDTH-1:0] data_b_i,
  input wire carry_in_i,
  output wire [DATA_OUT_WIDTH-1:0] data_o
);
  assign data_o = data_a_i + ((~data_b_i) + 1);
endmodule
```

---

### 3.4.5 Comparator

```verilog
assign data_o = data_a_i < data_b_i;
```

```verilog
assign data_o = (data_a_i - data_b_i) < 0;
```

```verilog
assign result = data_a_i - data_b_i;
assign data_o = result[MSB]; //sign bit == MSB
```



```verilog
module comparator(
  input wire [DATA_IN_WIDTH-1:0] data_a_i,
  input wire [DATA_IN_WIDTH-1:0] data_b_i,
  input wire carry_in_i,
  output wire data_o
);
  wire [DATA_OUT_WIDTH-1:0] result;
  assign result = data_a_i + ((~data_b_i) + 1);
  assign data_o = result[DATA_OUT_WIDTH-1];
endmodule
```

---

### 3.4.6 Shifter

### 3.4.7 ALU

### 3.4.8 IEEE754 Adder

### 3.4.9 IEEE754 Multiplier

### 3.4.10 Carry Look Ahead Adder

### 3.4.11 Pre-Fix Adder

