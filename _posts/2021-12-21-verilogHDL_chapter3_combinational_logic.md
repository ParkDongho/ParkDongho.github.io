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



#### 3.2.2.2 논리 연산자

```verilog
out = !a;
out = a && b;
out = a || b;
```



#### 3.2.2.3 비교 연산자

```verilog
out = a > b;
out = a < b;
out = a >= b;
out = a <= b;
```



#### 3.2.2.4 등가

```verilog
out = a == b;
out = a != b;
out = a === b;
out = a !== b;
```



#### 3.2.2.5 비트단위

```verilog
out = ~a;
out = a & b;
out = a | b;
out = a ^ b;
out = a ^~ b; 또는  a ~^ b;
```



#### 3.2.2.5 축소

```verilog
out = &a;
out = ~&a;
out = |a;
out = ~|a;
out = ^a
out = ^~a; 또는  ~^a;
```



#### 3.2.2.6 시프트

```verilog
out = a >> b;
out = a << b;
out = a >>> b;
out = a <<< b;
```




## 3.3 Behaviaral Modeling

### 3.3.1 always문

sensitivity list 내부의 값에 변화가 발생시 `begin ~ end` 내부의 구문이 실행됩니다.

sensitivity list 값의 구분은 `,` 를 통하여 합니다.

```verilog
always @(<sensitivity_list>, <sensitivity_list>, ... , <sensitivity_list>) begin
  //procedural assignment
end
```

아래 1bit 2 to 1 mux의 예제입니다.

```verilog
always @(a, b, s) begin
  //procedural assignment
  z = (a & ~s) | (a & s);
end
```

__Example 3.2__ always문을 활용한 2 to 1 mux

---

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

### 3.3.2 procedural assignment



---

### 3.3.3 if문

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
  always @(*) begin
    if (s == 1'b0) begin
      z = a;
    end
    else begin //s == 1'b1
      z = b;
    end
  end
endmodule
```

__Example 3.3__ if문을 활용한 2 to 1 mux





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
```

__Example 3.4__ if문을 활용한 4 to 1 mux



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
        z = a;
      end
      else begin
        z = b;
      end
    end
    else begin //s[1] == 1'b1
      if (s[0] == 1'b0) begin
        z = c;
      end
      else begin
        z = d;
      end
    end
  end
endmodule
```

__Example 3.5__ if문을 활용한 4 to 1 mux



### 3.3.4 case문

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



__4 to 1 mux__

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
        2'b00 : z = a;
        2'b01 : z = b;
        2'b10 : z = c;
        2'b11 : z = d;
        default   : z = 0;
    endcase
  end
endmodule
```

__Example 3.5__ case문을 활용한 4 to 1 mux





## 3.4 Case Study

### 3.4.1 4-to-1 Multiplexer

### 3.4.2 ROM

### 3.4.3 Decoder

### 3.4.4 Ripple Carry Adder

### 3.4.5 Subtractor

### 3.4.6 Comparator

### 3.4.7 Shifter

### 3.4.8 ALU

### 3.4.9 IEEE754 Adder

### 3.4.10 IEEE754 Multiplier

### 3.4.11 Carry Look Ahead Adder

### 3.4.12 Pre-Fix Adder

