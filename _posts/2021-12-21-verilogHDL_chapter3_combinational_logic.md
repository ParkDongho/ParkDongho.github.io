# Combinational Logic

> ## 목차
>
> **[1부 - Verilog HDL를 활용한 디지털 회로 설계]()**
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

---

## 3.1 Gate Level Modeling

---

standard cell을 instantiation하는 방법

RTL을 합성한 결과임

---

## 3.2 Dataflow Modeling

---

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
module mux_2_to_1(
  input wire a,
  input wire b,
  input wire s,
  output wire z
);
  assign z = (a & ~s) | (a & s);
endmodule
```

> __Example 3.1__ continuous assignment를 활용한 2 to 1 mux

---

### 3.2.2 데이터 형

#### 3.2.2.1 4 state system

- 0 : 논리 0

- 1 : 논리 1

- z : 하이 임피턴스 상태

- x : unknown

---

#### 3.2.2.1 데이터 타입

- wire : 물리적인 연결선을 의미, 값을 저장 불가

  ```verilog
  module(input a, input b, input c, output out)
  	assign out = a + b + c;
  endmodule
  ```

- reg : 다음 값이 할당되기까지 현재값을 유지, 절차적 할당(procedural assignment)의 출력에 사용

  ```verilog
  module(input a, input b, input c, output out);
    reg out
    always(*) begin
      out = a + b + c;
    end
  endmodule
  ```

  

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

![figure 2](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-21-verilogHDL_chapter3_combinational_logic/시스템_반도체_설계_3장-figure_2.drawio.png)



---

#### 3.2.2.2 논리 연산자

```verilog
out = !a;
out = a && b;
out = a || b;
```

![figure 3](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-21-verilogHDL_chapter3_combinational_logic/시스템_반도체_설계_3장-figure_3.drawio.png)

---

#### 3.2.2.3 비교 연산자

```verilog
out = a > b;
out = a < b;
out = a >= b;
out = a <= b;
```

![figure 4](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-21-verilogHDL_chapter3_combinational_logic/시스템_반도체_설계_3장-figure_4.drawio.png)

---

#### 3.2.2.4 등가 연산자

```verilog
out = a == b;
out = a != b;
out = a === b;
out = a !== b;
```

![figure 5](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-21-verilogHDL_chapter3_combinational_logic/시스템_반도체_설계_3장-figure_5.drawio.png)



---

#### 3.2.2.5 비트단위 연산자

```verilog
out = ~a; //not
out = a & b; //and
out = a | b; //or
out = a ^ b; //xor
out = a ^~ b; 또는  a ~^ b; //xnor
```

![figure 6](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-21-verilogHDL_chapter3_combinational_logic/시스템_반도체_설계_3장-figure_6.drawio.png)

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

![figure 7](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-21-verilogHDL_chapter3_combinational_logic/시스템_반도체_설계_3장-figure_7.drawio.png)

---

#### 3.2.2.6 시프트 연산자

```verilog
out = a >> b; // a / 2^b
out = a << b; // a * 2^b
out = a >>> b; // a / 2^b
out = a <<< b; // a * 2^b
```

![figure 8](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-21-verilogHDL_chapter3_combinational_logic/시스템_반도체_설계_3장-figure_8.drawio.png)

---

![figure 9](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-21-verilogHDL_chapter3_combinational_logic/시스템_반도체_설계_3장-figure_9.drawio.png)

---

#### 3.2.2.7 결합 연산자

```verilog
a = 1'b1;
b = 2'b10;
c = 3'b011;

out = {a, b, c[1:0]}; //1 10 11
```

---

#### 3.2.2.8 반복 연산자

```verilog
a = 1'b1;
b = 2'b10;
c = 3'b011;

a_3 = {3{a}}; //111
out = {{3{a}}, a_3, {2{c[1:0]}}} //111 111 1111
```

---

#### 3.2.2.9 3항 연산자

```verilog
out = sel ? a : b; //sel이 참일때 a를 out에 할당, 거짓일때 b를 out에 할당
out = sel ? a + b : a - b; //sel이 참일때 a + b를 out에 할당, 거짓일때 a - b를 out에 할당
```

- 2 to 1 mux의 표현에 적합함

![figure 10](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-21-verilogHDL_chapter3_combinational_logic/시스템_반도체_설계_3장-figure_10.drawio.png)

---

![figure 10](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-21-verilogHDL_chapter3_combinational_logic/시스템_반도체_설계_3장-figure_10.drawio.png)

```verilog
module mux_2_to_1(
  input wire a,
  input wire b,
  input wire sel,
  output wire out
);
  assign out = (sel==1'b0) ? a : b; //sel==1'b0이 참일 때(sel이 0일때) a가 out에 할당됨
endmodule
```

> __Example 3.2__ 3항 연산자를 활용한 2 to 1 mux

---

![figure 11](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-21-verilogHDL_chapter3_combinational_logic/시스템_반도체_설계_3장-figure_11.drawio.png)

```verilog
module mux_4_to_1(
  input wire a,
  input wire b,
  input wire c,
  input wire d,
  input wire [1:0] sel,
  output wire out
);
  assign out = (sel[1]==1'b0) ? ((sel[0]==1'b0) ? a : b) : ((sel[0]==1'b0) ? c : d);
endmodule
```

> __Example 3.3__ 3항 연산자를 활용한 4 to 1 mux

---

## 3.3 Behaviaral Modeling

---

### 3.3.1 always문

- time-step 마다 매번 실행됨

- sensitivity list 내부의 값에 변화가 발생시 `begin ~ end` 내부의 구문이 실행됩니다.

- sensitivity list 값의 구분은 `,` 를 통하여 합니다.
- always문 내부에서는 `procedural assignment`만 가능합니다. 즉 `assign` 문을 통한 `continous assignment`는 불가능합니다.

```verilog
always @(<sensitivity_list>, <sensitivity_list>, ... , <sensitivity_list>) begin
  //procedural assignment
end
```

---

![figure 10](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-21-verilogHDL_chapter3_combinational_logic/시스템_반도체_설계_3장-figure_10.drawio.png)

```verilog
always @(a, b, s) begin
  //procedural assignment
end
```

---

### 3.3.2 wildcard

- 아래 예시와 같이 매우 길고 복잡한 입력을 갖는 회로가 존재할수 있습니다. 

- 이러한 회로의 sensitivity list를 기술시 실수를 하기 쉽습니다.

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

### 3.3.3 procedural assignment

- `wire`형에만 할당 가능한 continuous assignment와 달리 procedural assignment은 `reg`형 객체에만 할당을 할 수 있습니다.
- 절차적 할당(procedural assignment)은 이벤트를 감지하지 않고 식에 도달하면 바로 실행을 하는 소프트웨어적인 특성이 있습니다. 
- 절차적 할당(procedural assignment)은 `blocking`과 `non-blocking`으로 나누어집니다. 
- blocking assignment
  - blocking은 combinational logic을 표현 하기 위해 사용
  - 표현식 :  `a = b`
- non-blocking assignment
  - non-blocking은 sequential logic을 표현하기 위해 사용
  - 표현식 :  `a <= b`

---

```verilog
module mux_2_to_1(
  input wire a, b,
  input wire s,
  output reg z
);
  always @(a, b, s) begin //sensitivity list
    //procedural assignment
    z = (a & ~s) | (a & s); //reg형이 와야함
  end
endmodule
```

> __Example 3.4__ always문 및 blocking assignment를 활용한 2 to 1 mux

- 2 to 1 mux의 input은 `a, b, s`가 있으므로 이를 sensitivity list에 넣어 주었습니다.

- sensitivity list의 값 `a, b, s` 에 변화가 발생하면 always 내부의 구문들을 실행합니다.

- 위의 예시에서는 blocking procedural assignment인  `z = (a & ~s) | (a & s);` 을 실행합니다.

---

### 3.3.4 blocking vs non-blocking assignment

__blocking assignment__

- RHS(우변 방정식)를 평가하고 blocking assignment의 LHS(좌변 방정식)를 업데이트합니다.

- blocking assignment의 활용 : Combinational Logic의 표현에 적합

```verilog
always @(a, b) begin //sensitivity list
  //non-blocking assignment
  a = 1;
  b = a + 1;
  c = b + 1;
end
```

---

__non-blocking assignment__

- time step의 시작 부분에서 non-blocking assignment의 RHS(우변 방정식를 평가합니다.

- time step의 끝에서 non-blocking assignment의 LHS(좌변 방정식)를 업데이트합니다.

- Non-blocking assignment의 활용 : sequential logic의 표현에 적합

```verilog
always @(posedge clk) begin //sensitivity list
  //non-blocking assignment
  a <= 1;
  b <= a + 1;
  c <= b + 1;
end
```

---

### 3.3.5 Initial문

- 전체 시뮬레이션에서 한번만 실행

- testbench나 메모리의 데이터를 초기화 할 때 사용

```verilog
  initial begin //전체 시뮬레이션 동안 한번만 실행
    //procedural assignment
    //procedural assignment
    //procedural assignment
  end
```

---

```verilog
`timescale 1ns/10ps  // 단위/해상도
module testbench();
  reg a_test;
  reg b_test;
  reg s_test;
  wire z_test;
  
  initial begin
    a_test = 1'b1;
    b_test = 1'b0;
    s_test = 1'b0;
    #5; //5ns delay
    a_test = 1'b1;
    b_test = 1'b1;
    s_test = 1'b0;
    #5; //5ns delay
    a_test = 1'b0;
    b_test = 1'b1;
    s_test = 1'b1;
    #5; //5ns delay
    a_test = 1'b1;
    b_test = 1'b1;
    s_test = 1'b0;
    #5; //5ns delay
    $finish;
  end
  
  mux_2_to_1 dut(.a(a_test), .b(b_test), .s(s_test), .z(z_test)); //instantization
  
endmodule
```

> __Example 3.5__ intial문을 활용한 testbench

---

### 3.3.6 if문

- c언어의 if문과 유사

- mux로 합성됨

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

#### 3.3.6.1 2 to 1 mux

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

> __Example 3.6__ if문을 활용한 2 to 1 mux

---



#### 3.3.6.2 4 to 1 mux

```verilog
module mux_4_to_1(
  input wire a,
  input wire b,
  input wire c,
  input wire d,
  input wire [1:0] s,
  output wire z
);
  reg z_r;
  always @(*) begin
    if (s == 2'b00) begin
      z_r = a;
    end
    else if (s == 2'b01) begin
      z_r = b;
    end
    else if (s == 2'b10) begin
      z_r = c;
    end
    else begin
      z_r = d;
    end
  end
  
  assign z = z_r;
  
endmodule
```

> __Example 3.7__ if문을 활용한 4 to 1 mux

---

```verilog
module mux_4_to_1(
  input wire a,
  input wire b,
  input wire c,
  input wire d,
  input wire [1:0] s,
  
  output wire z
);
  reg z_r;
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

> __Example 3.8__ if문을 활용한 4 to 1 mux

---

#### 3.3.6.3 if문 사용시 주의사항

```verilog
module mux_3_to_1(
  input wire a, b, c,
  input wire [1:0] s,
  output reg z
);
  always @(*) begin
    if (s == 2'b00)      
      z = a;
    else if (s == 2'b01) 
      z = b;
    else if (s == 2'b10) 
      z = c;
  end
endmodule
```

> __Example 3.9__ if문을 활용한 3 to 1 mux

- s가 2'b11일떄의 state가 규정되어 있지 않으므로 만약 s가 2'b11이라면 이전 timestep에서의 z값을 그대로 유지 할 것입니다. 이는 의도 하지 않은 latch의 합성을 의미합니다.

---

```verilog
module mux_3_to_1(
  input wire a, b, c,
  input wire [1:0] s,
  output reg z
);
  z = 1'b0;
  always @(*) begin
    if (s == 2'b00)      
      z = a;
    else if (s == 2'b01) 
      z = b;
    else if (s == 2'b10) 
      z = c;
  end
endmodule
```

> __Example 3.10__ if문을 활용한 3 to 1 mux

- 위의 예시의 경우는 if문을 초기화 하기전 출력을 초기화해주었습니다.

- 따라서 만약 s == 2'b11인 상황에서 z가 초기화한 값 0이 출력될 것입니다.

---

```verilog
module mux_3_to_1(
  input wire a, b, c,
  input wire [1:0] s,
  output reg z
);
  always @(*) begin
    if (s == 2'b00)      
      z = a;
    else if (s == 2'b01) 
      z = b;
    else                 
      z = c;
  end
endmodule
```

> __Example 3.11__ if문을 활용한 3 to 1 mux

- 혹은 if문을 반드시 else로 닫아주는 방법도 존재합니다.

- s == 2'b11인 상황에서 z = c가 출력되겠죠.

---

### 3.3.5 case문

- c 언어의 case문과 달리 break를 정의 하지 않는점에서 차이점이 존재
- 해당하는 condition의 statement를 끝까지 실행 
- 일종의 truth up table로 표현할 수 있는 회로 설계에 적합

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
always @(*) begin
  case(expression)
      condition : begin 
        statements;
        statements;
      end
      condition : begin 
        statements;
        statements;
      end
      condition : begin 
        statements;
        statements;
      end
      default   : begin
        statements;
        statements;
      end
  endcase
end
```

- 구문이 두줄 이상일시 `begin` ~ `end` 로 묶어 표현

---

```verilog
module mux_4_to_1(
  input wire a,
  input wire b,
  input wire c,
  input wire d,
  input wire [1:0] sel,
  
  output wire z
);
  reg z_r;
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

> __Example 3.12__ case문을 활용한 4 to 1 mux

---

#### 3.3.5.2 casex, casez

__casex__ : 'x', 'z', '?'를 don't care로 처리

__casez__ : 'z',  '?'를 don't care로 처리, 'z'대신에 ?로 표시 가능

```verilog
case(data_i)
  4'b1000, 4'b1001, 4'b1010, 4'b1011, 
  4'b1100, 4'b1101, 4'b1110, 4'b1111: 
    data_o_r = 3'b100;
```

```verilog
casez(data_i)
	4'b1??? : data_o_r = 3'b100;
```

---

```verilog
module priority_encoder(
  input wire [IN_WIDTH-1:0] data_i,
  output wire [OUT_WIDTH-1:0] data_o
);
  localparam IN_WIDTH = 4;
  localparam OUT_WIDTH = 3;
  reg [OUT_WIDTH-1:0] data_o_r;
  
  always @(*) begin
    casez(data_i)
      4'b1??? : data_o_r = 3'b100;
      4'b01?? : data_o_r = 3'b011;
      4'b001? : data_o_r = 3'b010;
      4'b0001 : data_o_r = 3'b001;
      4'b0000 : data_o_r = 3'b000;
      default: data_o_r = 0;
    endcase
  end
  assign data_o = data_o_r;
endmodule
```

> __Example 3.13__ Priority Encoder

---

#### 3.3.5.1 case문 사용시 주의사항 1 : 의도하지 않은 latch의 합성 - full case

case문 또한 모든 입력의 condition에 대하여 출력의 값이 규정되지 않으면 잘못된 래치가 합성될 수 있습니다.

```verilog
module mux_4_to_1(
  input wire a, b, c, d,
  input wire s,
  output wire z
);
  always @(*) begin
    case(sel)
        2'b00 : z_r = a;
        2'b01 : z_r = b;
        2'b10 : z_r = c;
        //2'b11 일때의 출력 z_r의 값이 정의 되지않음 //래치가 합성됨
    endcase
  end
  assign z = z_r;
endmodule
```

> __Example 3.14__ case문을 활용한 4 to 1 mux

---

```verilog
module mux_4_to_1(
  input wire a, b, c, d,
  input wire s,
  output wire z
);
  z_r = 0; //output 초기화
  always @(*) begin
    case(sel)
        2'b00 : z_r = a;
        2'b01 : z_r = b;
        2'b10 : z_r = c;
    endcase
  end
  assign z = z_r;
endmodule
```

> __Example 3.15__ case문을 활용한 4 to 1 mux

---

```verilog
module mux_4_to_1(
  input wire a, b, c, d,
  input wire s,
  output wire z
);
  always @(*) begin
    case(sel)
        2'b00 : z_r = a;
        2'b01 : z_r = b;
        2'b10 : z_r = c;
        default: z_r = 0; //default값을 정의하여 출력값을 초기화
    endcase
  end
  assign z = z_r;
endmodule
```

> __Example 3.16__ case문을 활용한 4 to 1 mux

---

#### 3.3.5.2 case문 사용시 주의사항 2 : parallel case

```verilog
module intctl2b (
  input wire [3:0] data_in, 
  output reg [4:0] data_out
);
  always @(data_in) begin
    data_out = 0;
    casez (data_in) //data_in == 4'b11xx, 4'bx11x, 4'bxx11, 4'b1xx1, ...
      4'b1??? : data_out = 1;
      4'b?1?? : data_out = 6;
      4'b??1? : data_out = 8;
      4'b???1 : data_out= 10;
    endcase
  end
endmodule
```

- 케이스 항목이 두번 이상 일치함(data_in == 4'b11xx, 4'bx11x, 4'bxx11, 4'b1xx1, ...)

- 이를 non-parallel case라 합니다.
- priority 라우팅 네트워크를 추론

---

```verilog
module intctl2b (
  input wire [3:0] data_in, 
  output reg [7:0] data_out
);
  always @(data_in) begin
    data_out = 0;
    casez (data_in)
      4'b1??? : data_out = 3;
      4'b01?? : data_out = 15;
      4'b001? : data_out = 63;
      4'b0001 : data_out = 255;
    endcase
  end
endmodule
```

- 각 케이스 항목이 고유함

- 이를 parallel case라 합니다.

- 멀티플렉싱 라우팅 네트워크를 추론

---

```verilog
module intctl2b (
  input wire [3:0] data_in, 
  output reg [4:0] data_out
);
  always @(data_in) begin
    data_out = 0;
    (* parallel_case *) //synthesis directives
    casez (data_in)
      4'b1??? : data_out = 3;
      4'b?1?? : data_out = 15;
      4'b??1? : data_out = 63;
      4'b???1 : data_out = 255;
    endcase
  end
endmodule
```

- 만약 non-parallel case임에도 각각의 케이스 항목이 mutually exclusive하게 실행됨을 설계자가 보장할 수 있다면, 즉 위 예시에서 data_in이 4'b1000, 4'b0100, 4'b0010, 4'b0001만이 입력됨을 보장할 수 있다면 `synthesis directives : (* parallel_case *)`를 추가하여 강제로 prallel하게 동작하도록 합성 할 수 있습니다.

---

### 3.3.6 if문 vs case문

![figure 12](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-21-verilogHDL_chapter3_combinational_logic/시스템_반도체_설계_3장-figure_12.drawio.png)

- priority encoding이 필요 할때는 if else문으로 표현하는 것이 유리하며

- priority encoding이 필요 없을 시 case문으로 표현하는 것이 유리합니다.

---

## 3.4 Case Study

---

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

### 3.4.2 7segment Decoder

```verilog
module decoder(
  input wire [IN_WIDTH-1:0] data_i,
  output wire [OUT_WIDTH-1:0] data_o
);
  localparam IN_WIDTH = 4;
  localparam OUT_WIDTH = 7;
  reg [WIDTH-1:0] data_o_r;
  
  always @(*) begin
    casez(data_i)
      4'b0000 : data_o_r = 8'b00000000; //0
      4'b0001 : data_o_r = 8'b00000001; //1
      4'b0010 : data_o_r = 8'b00000011; //2
      4'b0011 : data_o_r = 8'b00000111; //3
      4'b0100 : data_o_r = 8'b00001111; //4
      4'b0101 : data_o_r = 8'b00011111; //5
      4'b0110 : data_o_r = 8'b00111111; //6
      4'b0111 : data_o_r = 8'b01111111; //7
      4'b1000 : data_o_r = 8'b01111111; //8
      4'b1001 : data_o_r = 8'b01111111; //9
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

### 3.4.6 ALU

