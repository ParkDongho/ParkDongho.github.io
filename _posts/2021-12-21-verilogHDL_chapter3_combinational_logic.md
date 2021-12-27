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

좌항의 net형 변수 객체에 우항의 값을 할당

절치작

```verilog
assign a = b;
```



### 3.2.2 연산자

#### 3.2.2.1 1항 연산자



#### 3.2.2.2 2항 연산자



#### 3.2.2.3 3항 연산자



## 3.3 Behaviaral Modeling

### 3.3.1 always문

```verilog
always @(<sensitivity_list>, <sensitivity_list>, ... , <sensitivity_list>) begin
  _assignment 기술_
end
```





```verilog
always @(*) begin
  _assignment 기술_
end
```



### 3.3.2 procedural assignment



### 3.3.3 if문



### 3.3.4 case문

### 



## 3.4 Case Study

### 3.4.1 4-to-1 Multiplexer

### 3.4.2 ROM

### 3.4.3 BCD to 7 segment

### 3.4.4 Ripple Carry Adder

### 3.4.5 Subtractor

### 3.4.6 Comparator

### 3.4.7 Shifter

### 3.4.8 ALU

### 3.4.9 IEEE754 Adder

### 3.4.10 IEEE754 Multiplier

### 3.4.11 Carry Look Ahead Adder

### 3.4.12 Pre-Fix Adder

### 





