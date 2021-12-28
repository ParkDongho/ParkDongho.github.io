---
layout: article
title: Verilog HDL 2장
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

# Module & Instantiation

<!--more-->

> ## 목차
>
> __Chapter 1 : [Introduction](https://parkdongho.github.io/2021/12/16/verilogHDL_chapter1_introduction.html)__
>
> __Chapter 2 : [Module & Instantiation](https://parkdongho.github.io/2021/12/19/verilogHDL_chapter2_module_&_instantiation.html)__
>
> __Chapter 3 : [Combinational Logic](https://parkdongho.github.io/2021/12/21/verilogHDL_chapter3_combinational_logic.html)__
>
> __Chapter 4 : [Sequential Logic](https://parkdongho.github.io/2021/12/23/verilogHDL_chapter4_sequential_logic.html)__
>
> __Chapter 5 : [FSM](https://parkdongho.github.io/2021/12/25/verilogHDL_chapter5_FSM.html)__

디지털 회로를 설계할때 하나의 블럭안에 모든회로를 표현하면 매우 복잡합니다. __그림1__ 의 경우 4bit addition을 하는 간단한 회로임에도 매우 복잡해 보입니다.

![fig 1](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-19-verilogHDL_chapter2_module_%26_instantiation/시스템_반도체_설계_2장-figure_1.png)

__그림1__ 4bit Adder 예제



그래서 회로를 여러 서브블럭들로 나누어 설계를 합니다. __그림2__ 는 full-adder 4개를 연결하여 4bit adder를 만든 예시 입니다. 이때 4bit_adder 및 full_adder와 같은 설계 블록들을 `모듈(module)`이라고 합니다. 

![fig 2](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-19-verilogHDL_chapter2_module_%26_instantiation/시스템_반도체_설계_2장-figure_2.png)

__figure 2__ module instantiation의 예시

---

이제 full_adder라는 모듈의 구성을 살펴봅시다. 모듈은 외부와 데이터를 주고 받을 수 있는 입출력 인터페이스를 제공합니다. 이러한 인터페이스를 `포트(port)` 라고 합니다. __그림3__ 에서 확인할 수 있듯이 full_adder 모듈은 a, b, carry_in 이라는 입력 포트, out, carry_out 이라는 출력포트로 구성되어 있습니다.

![fig 3](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-19-verilogHDL_chapter2_module_%26_instantiation/시스템_반도체_설계_2장-figure_3.png)

__figure 3__ full adder 모듈



이제 정의한 서브모듈이 어떻게 보다 큰 모듈로 합쳐질 수 있는지 알아봅시다. 이를 이해하기 위해서는`instantiation` 의 개념을 이해할 필요가 있습니다. 먼저 각각의 모듈은 객체를 만들기 위한 템플릿을 제공합니다. 이러한 템플릿을 통하여 상위 레벨의 모듈에서 객체를 불러들입니다. 이때 각각의 객체는 고유한 이름, 변수, 파라미터, 입출력 인터페이스를 가집니다. 이러한 객체를 `instance`라고 하고 모듈 템플릿으로 부터 객체를 생성하는 것을 `instantization` 이라고 합니다.

이에 대한 예시로 __그림2__ 의 4bit adder 를 다시 확인해 봅시다.  먼저 full_adder 라는 모듈에서 서브모듈에 대한 템플릿을 정의 합니다. 이때 full_adder 모듈은 5개의 입출력 포트, 내부 로직에 대한 정의 등을 포함합니다. 이제 정의한 full_adder 모듈을 템플릿으로 하여 4bit_adder라는 상위 모듈에서 full_adder_0, full_adder_1, full_adder_2, full_adder_3 라는 고유한 이름의 객체 4개를 불러들입니다. 



![fig 2](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-19-verilogHDL_chapter2_module_%26_instantiation/시스템_반도체_설계_2장-figure_2.png)

__figure 2__ module instantiation의 예시



## 2.1 Module

위에서 모듈이 무었인지 알아보았습니다. 이제 이를 베릴로그에서 어떻게 기술하는지 알아봅시다.

```verilog
module 모듈명( //모듈 시작
  //port를 정의
)
  
  //내부로직을 정의
  
endmodule //모듈 끝
```

verilog의 모듈은`module`이라는 키워드로 시작하고 `endmodule`이라는 키워드로 모듈을 닫습니다.

`module`이라는 키워드 다음에는 `모듈명`을 정의합니다.

그리고 모듈명 다음에 나오는 괄호 `( )`에서는 포트를 정의 합니다.

---

![fig 3](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-19-verilogHDL_chapter2_module_%26_instantiation/시스템_반도체_설계_2장-figure_3.png)

```verilog
module full_adder( //module 시작
  //port를 정의
)
  //내부로직을 정의
endmodule //모듈 끝
```

다음은 full_adder에 대한 예시입니다. 본 예시에서 모듈이름이 full_adder로 정의 됨을 확인할 수 있습니다.



## 2.2 Port

모듈이 외부 및 다른 모듈과 신호를 주고 받으려면 입출력 단자가 필요합니다. 이러한 입출력 단자를 `포트`라고 합니다. 이러한 입력단자에서 신호를 받아 처리를 한후 출력단자로 내보냅니다. verilog에서는 총 3가지의 port가 있습니다.

* input : 모듈내로 데이터를 받는 포트
* output : 모듈밖으로 데이터를 내보내는 포트
* inout : 모듈 내로 데이터를 받을수도 있고 밖으로 데이터를 내보낼 수도 있는 양방향 포트

이때 각 포트는 __그림4__ 와 같이

* input 포트 : only net
* output 포트 : reg or net
* inout 포트 : only net

와 같은 타입으로 정의 될수 있습니다.

또한 각 포트에 instantiation으로 외부에서 연결되는 신호는 __그림4__ 와 같이 

* input 포트 : reg or net
* output 포트 : only net
* inout 포트 : only net

와 같은 타입으로 정의 될 수 있습니다.

![figure4](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-19-verilogHDL_chapter2_module_%26_instantiation/시스템_반도체_설계_2장-figure_4.png)

__figure 4__ module의 포트 연결 규칙

---

왜 이런 규칙이 존재하는지 생각해 볼까요? 이 부분은 베릴로그가 처음이신 분은 넘어가도 괜찮습니다.

먼저 모듈의 입력은 반드시 continuous하게 신호를 받아야 된다는 규칙이 있습니다. 이렇게 continuous하게 들어온 신호에 의하여 내부로직이 continuous(assign문)하게 혹은 procedual(always문)하게 연산된후 출력을 내보내게되죠. 따라서 출력은 continuous 혹은 procedual할 수 있습니다. 이때 continuous한 신호는 wire에 procedual한 신호는 reg타입에 대응됩니다. 

![figure5](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-19-verilogHDL_chapter2_module_%26_instantiation/시스템_반도체_설계_2장-figure_5.png)

__figure 5__ module의 포트 연결 규칙 with instantiation

모듈A를 instantiation한 모듈B를 가정해 봅시다. 모듈 B의 입장에서 모듈 A의 출력포트에 연결된 신호는 모듈B의 input입니다. input은 반드시 continuous 하여야 하므로 net 타입만이 올 수 있습니다. 이와 동일하게 모듈 B의 입장에서 모듈 A의 입력 포트에 연결된 신호는 모듈B의 output입니다. 따라서 reg 혹은 net타입이 올 수 있죠.

---

이제 포트를 어떻게 표현할 수 있는지 알아 봅시다.

포트는 아래와 같이

```verilog
module 모듈명(
  <포트_타입> <변수_타입> <입력_포트_명>
  <포트_타입> <변수_타입> <입력_포트_명>
);
  
  //여기에 내부 로직 기술
  
endmodule
```

`<포트_타입> [변수_타입] 입력_포트_명` 와 같은 형태로 표현 할 수 있습니다.

이때

`<포트_타입>` 은  `input`, `output`, `inout`이 올 수 있고 반드시 정의 해야 합니다.

`[변수_타입]`은 `wire`, `reg`가 올수 있으며 생략가능합니다. 만약 생략하였을시 기본적으로 `wire`가 오게 됩니다. 만약 출력값을 유지시켜줄 필요가 있다면 `reg`를 사용할 수 있습니다.

`입력_포트_명`은 포트의 이름을 정의 해줍니다.

---

full adder 예제를 확인 해봅시다.

![fig 3](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-19-verilogHDL_chapter2_module_%26_instantiation/시스템_반도체_설계_2장-figure_3.png)



```verilog
module full_adder( //module 키워드로 모듈시작
  input a,             //1bit wire input
  input wire b,        //1bit wire input
  input wire carry_in, //1bit wire input
  
  output reg out,      //1bit reg output
  output carry_out     //1bit wire output
);
  
  //mux의 내부 로직
  
endmodule //endmodule 키워드로 모듈 종료
```

__Example 2.1__

* 포트 a, b, carry_in은 입력 포트입니다. input 키워드를 통하여 입력으로 정의해 줍니다.
* 포트 out, carry_out은 출력 포트 입니다. output 키워드를 통하여 출력으로 정의해 줍니다.
* 포트 a는 변수 타입을 정의하지 않았습니다. 따라서 default값 wire형으로 정의 됩니다.
* 포트 b, carry_in은 wire 타입으로 정의해 주었습니다.
* 포트 out은 reg 타입으로 정의하였습니다.
* 포트 carry_out은 변수타입을 정의하지 않았습니다. 따라서 wire형으로 정의됩니다.

예제에서는 예시를 보여주기 위해 여러 종류의 포트 선언방식을 섞어 사용하였지만 실제로 설계할때는 아래와 같이 일정한 코딩 가이드라인에 따라주는 것이 좋습니다.

```verilog
module full_adder( //module 키워드로 모듈시작
  input wire a,        //1bit wire input
  input wire b,        //1bit wire input
  input wire carry_in, //1bit wire input
  
  output wire out,      //1bit wire output
  output wire carry_out //1bit wire output
);
  
  //mux의 내부 로직
  
endmodule //endmodule 키워드로 모듈 종료
```



### 2.2.1 Vector Form

이전까지는 1bit의 입출력까지만 다루었습니다. 하지만 numerical data를 표현하기 위해서는 n-bit의 binary data 형태로 표현할수 있으면 좋겠죠. 이를 위하여 verilog에서는 verctor 형태의 데이터 선언을 지원합니다.

![fig 6](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-19-verilogHDL_chapter2_module_%26_instantiation/시스템_반도체_설계_2장-figure_6.png)

__figure 6__ 4bit adder의 vector 형태 표현

---

Verilog에서의 vector 선언는 아래와 같이 표현합니다.

```verilog
<포트_타입> <변수_타입> [MSB : LSB] <변수명> //포트 선언
<변수_타입> [MSB : LSB] <변수명> //변수 선언
```

이전 1bit의 변수 선언에서 `<변수_타입>`과 `<변수명>` 사이에 `[MSB : LSB]` 가 추가된 형태입니다.

이때 MSB 최상위 비트, LSB는 최하위 비트를 의미합니다.

이를 4bit adder에 적용하면 아래와 같습니다.

```verilog
module nbit_adder(
  input wire [3:0] a,  //4bit input port
  input wire [3:0] b,  //4bit input port
  input wire carry_in, //1bit input port
  
  output wire [3:0] out, //4bit output port
  output wire carry_out  //1bit output port
);
  
  //instantiation
  
endmodule
```

위 예시에서는 포트 a, b, out를 4bit로 구성하였습니다.

4bit bus의 최상위 비트(MSB)는 `3` 최하위 비트(LSB)는 `0`이므로 

```verilog
<포트_타입> <변수_타입> [3:0] <변수명> //포트 선언
```

형태로 선언해주었습니다.

---

하지만 위와 같이 표현한다면 bus의 폭을 변화시키고 싶을시 모든 포트의 MSB를 수정해주어야 하는 문제가 있습니다.

또한 직관적이지 못한 문제도 있지요.

따라서 아래와 같이 parameter를 선언하여

최상위 비트(MSB)는 `DATA_WIDTH-1` 최하위 비트(LSB)는 `0`의 형태, 즉

```verilog
<포트_타입> <변수_타입> [DATA_WIDTH-1:0] <변수명> //포트 선언
```

형태로 많이들 선언합니다.

이를 4bit adder에 적용해보면 아래와 같습니다.

```verilog
module nbit_adder#(
  parameter DATA_WIDTH = 4
)(
  input wire [DATA_WIDTH-1:0] a,
  input wire [DATA_WIDTH-1:0] b,
  input wire carry_in,
  
  output wire [DATA_WIDTH-1:0] out,
  output wire carry_out
);
  
  //instantiation
  
endmodule
```



## 2.3 Instantiation

이제 상위모듈에서 하위모듈의 인스턴스를 불러들이는 instantiation을 해봅시다.

nbit_adder라는 모듈에서 4개의 full_adder 인스턴스를 불러들이는 형태이겠죠.

또한 불러들인 모듈에 wire를 연결해주는 작업이 필요합니다.



instantiation의 문법은 아래와 같습니다.

```verilog
<템플릿명> <인스턴스명>(.템플릿포트명(연결한_신호), .템플릿포트명(연결한_신호), ... , .템플릿포트명(연결한_신호));
```

* <템플릿명>은 원본 모듈의 명칭입니다.
* <인스턴스명>은 템플릿을 통하여 생성한 인스턴스들의 새로운 이름 입니다. (복사한 모듈의 이름)
* <템플릿포트명>은 원본 모듈의 포트 명칭입니다.
* <연결한_신호>는 인스턴스 모듈의 포트에 연결한 신호명 입니다.

---

먼저 nbit_adder라는 모듈에서 4개의 full_adder 인스턴스를 불러들여 봅시다.

![fig 7](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-19-verilogHDL_chapter2_module_%26_instantiation/시스템_반도체_설계_2장-figure_7.png)

```verilog
module nbit_adder#(
  parameter DATA_WIDTH = 4
)(
  input wire [DATA_WIDTH-1:0] a,
  input wire [DATA_WIDTH-1:0] b,
  input wire carry_in,
  
  output wire [DATA_WIDTH-1:0] out,
  output wire carry_out
);
  
  full_adder full_adder_0(.a(), .b(), .carry_in(), .out(), .carry_out());
  full_adder full_adder_1(.a(), .b(), .carry_in(), .out(), .carry_out());
  full_adder full_adder_2(.a(), .b(), .carry_in(), .out(), .carry_out());
  full_adder full_adder_3(.a(), .b(), .carry_in(), .out(), .carry_out());
  
endmodule
```

위 예제에서 `full_adder`는 템플릿인 full_adder 모듈명입니다.

이러한 템플릿을 통하여 `full_adder_0`, `full_adder_1`, `full_adder_2`, `full_adder_3` 이름의 4개의 인스턴스를 생성해주었습니다.

---

이제 `nbit_adder` 모듈의 포트와 `full_adder` 인스턴스의 포트를 연결해줍시다.

![fig 8](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-19-verilogHDL_chapter2_module_%26_instantiation/시스템_반도체_설계_2장-figure_8.png)

```verilog
module nbit_adder#(
  parameter DATA_WIDTH = 4
)(
  input wire [DATA_WIDTH-1:0] a,
  input wire [DATA_WIDTH-1:0] b,
  input wire carry_in,
  
  output wire [DATA_WIDTH-1:0] out,
  output wire carry_out
);
  
  full_adder full_adder_0(.a(a[0]), .b(b[0]), .carry_in(carry_in), .out(out[0]), .carry_out());
  full_adder full_adder_1(.a(a[1]), .b(b[1]), .carry_in(),         .out(out[1]), .carry_out());
  full_adder full_adder_2(.a(a[2]), .b(b[2]), .carry_in(),         .out(out[2]), .carry_out());
  full_adder full_adder_3(.a(a[3]), .b(b[3]), .carry_in(),         .out(out[3]), .carry_out(carry_out));
  
endmodule
```

```verilog
full_adder full_adder_0(.a(a[0]), .b(b[0]), .carry_in(carry_in), .out(out[0]), .carry_out());
```

`full_adder_0` 인스턴스의 `a` 포트에 `nbit_adder` 모듈의 `a[0]` 포트를 연결시켜준 예시입니다.

---

마지막으로 인스턴스들 사이의 신호를 연결해줍시다.

먼저 `nbit_adder` 모듈내에 내부신호를 선언해줍니다.

```verilog
wire carry_0_w;
wire carry_1_w;
wire carry_2_w;
```

그 다음 해당 신호들을 인스턴스에 연결해줍니다.

```verilog
full_adder full_adder_0(.a(a[0]), .b(b[0]), .carry_in(carry_in),  .out(out[0]), .carry_out(carry_0_w));
full_adder full_adder_1(.a(a[1]), .b(b[1]), .carry_in(carry_0_w), .out(out[1]), .carry_out(carry_1_w));
full_adder full_adder_2(.a(a[2]), .b(b[2]), .carry_in(carry_1_w), .out(out[2]), .carry_out(carry_2_w));
full_adder full_adder_3(.a(a[3]), .b(b[3]), .carry_in(carry_2_w), .out(out[3]), .carry_out(carry_out));
```

최종 완성본은 아래와 같습니다.

![fig 9](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-19-verilogHDL_chapter2_module_%26_instantiation/시스템_반도체_설계_2장-figure_9.png)

```verilog
module nbit_adder#(
  parameter DATA_WIDTH = 4
)(
  input wire [DATA_WIDTH-1:0] a,
  input wire [DATA_WIDTH-1:0] b,
  input wire carry_in,
  
  output wire [DATA_WIDTH-1:0] out,
  output wire carry_out
);
  
  wire carry_0_w;
  wire carry_1_w;
  wire carry_2_w;
  
  full_adder full_adder_0(.a(a[0]), .b(b[0]), .carry_in(carry_in),  .out(out[0]), .carry_out(carry_0_w));
  full_adder full_adder_1(.a(a[1]), .b(b[1]), .carry_in(carry_0_w), .out(out[1]), .carry_out(carry_1_w));
  full_adder full_adder_2(.a(a[2]), .b(b[2]), .carry_in(carry_1_w), .out(out[2]), .carry_out(carry_2_w));
  full_adder full_adder_3(.a(a[3]), .b(b[3]), .carry_in(carry_2_w), .out(out[3]), .carry_out(carry_out));
  
endmodule
```



