---
layout: article
title: VerilogHDL 2장
tags: Blog TeXt
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

# Module & Instantiation

<!--more-->

디지털 회로를 설계할때 그림1-(a)와 같이 하나의 블럭안에 모든회로를 표현하면 매우 복잡합니다.

![fig 1](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-19-chapter2_module_%26_instantiation/시스템_반도체_설계_2장-figure_1_1.drawio.png)

따라서 그림1-(b) 와 같이 블럭을 여러 그림1-(c)와 같은 서브블럭들로 나누어 설계를 합니다. 이러한 각각의 블럭을 모듈이라고 합니다.  

![fig 1](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-19-chapter2_module_%26_instantiation/시스템_반도체_설계_2장-figure_1.drawio.png)

__figure 1__ module instantiation의 예시



## 2.1 Module

위에서 모듈이 무었인지 알아보았습니다. 이제 이를 베릴로그에서 어떻게 기술하는지 알아봅시다.

```verilog
module 모듈명( //모듈 시작
  //port를 정의
)
  
  //내부로직을 정의
  
endmodule //모듈 끝
```

verilog의 module은`module`이라는 키워드로 시작하고 `endmodule`이라는 키워드로 모듈을 닫습니다.

이때 `module`이라는 키워드 다음에 `모듈명`을 정의합니다.



### 2.1.1 Port

모듈이 외부 및 다른 모듈과 신호를 주고 받으려면 입출력 단자가 필요합니다. 이러한 입출력 단자를 `포트`라고 합니다. 이러한 입력단자에서 신호를 받아 처리를 한후 출력단자로 내보냅니다. verilog에서는 총 3가지의 port가 있습니다.

* input : 모듈내로 데이터를 받는 포트
* output : 모듈밖으로 데이터를 내보내는 포트
* inout : 모듈 내로 데이터를 받을수도 있고 밖으로 데이터를 내보낼 수도 있는 양방향 포트

이때 각 포트의 변수타입을 결정하는 규칙이 있습니다. 아래 그림2를 보면

* input
* output
* inout

![](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-19-chapter2_module_%26_instantiation/시스템_반도체_설계_2장-figure_2.drawio.png)

__figure 2__ module의 포트 연결 규칙



왜 이런 규칙이 존재하는지 생각해 볼까요? 이 부분은 베릴로그가 처음이신 분은 넘어가도 괜찮습니다.

먼저 모듈의 입력은 반드시 continuous하게 신호를 받아야 된다는 규칙이 있습니다. 이렇게 continuous하게 들어온 신호에 의하여 내부로직이 continuous(assign문)하게 혹은 procedual(always문)하게 연산된후 출력을 내보내게되죠. 따라서 출력은 continuous 혹은 procedual할 수 있습니다. 이때 continuous한 신호는 wire에 procedual한 신호는 reg타입에 대응됩니다. 

![](https://raw.githubusercontent.com/ParkDongho/ParkDongho.github.io/master/assets/images/2021-12-19-chapter2_module_%26_instantiation/시스템_반도체_설계_2장-figure_3.drawio.png)

__figure 3__ module의 포트 연결 규칙 with instantiation

모듈A를 instantiation한 모듈B를 가정해 봅시다. 모듈 B의 입장에서 모듈 A의 출력포트에 연결된 신호는 모듈B의 input입니다. input은 반드시 continuous 하여야 하므로 net 타입만이 올 수 있습니다. 이와 동일하게 모듈 B의 입장에서 모듈 A의 입력 포트에 연결된 신호는 모듈B의 output입니다. 따라서 reg 혹은 net타입이 올 수 있죠.



이제 포트를 어떻게 표현할 수 있는지 알아 봅시다.

포트는 아래와 같이

```verilog
module 모듈명(
  <포트_타입> [변수_타입] 입력_포트_명
  <포트_타입> [변수_타입] 입력_포트_명
);
  
  //여기에 내부 로직 기술
  
endmodule
```

`<포트_타입> [변수_타입] 입력_포트_명` 와 같은 형태로 표현 할 수 있습니다.

이때

`<포트_타입>` 은  `input`, `output`, `inout`이 올 수 있고 반드시 정의 해야 합니다.

`[변수_타입]`은 `wire`, `reg`가 올수 있으며 생략가능합니다. 만약 생략하였을시 기본적으로 `wire`가 오게 됩니다. 만약 출력값을 유지시켜줄 필요가 있다면 `reg`를 사용할 수 있습니다.

`입력_포트_명`은 포트의 이름을 정의 해줍니다.



예제를 통해 확인해 봅시다.

```verilog
module ex_2_01_mux_2_to_1( //module 키워드로 모듈시작
  input A,      //1bit wire input
  input wire B, //1bit wire input
  input wire S, //1bit wire input
  
  output reg Z  //1bit reg output
);
  
  //mux의 내부 로직
  
endmodule //endmodule 키워드로 모듈 종료
```

__Example 2.1__

* 모듈의 이름은 ex_2_01_mux_2_to_1입니다.

* input 포트 A는 변수 타입을 생략하였으므로 wire 타입입니다.
* input 포트 B 및 S는 wire로 변수 타입이 선언 되었습니다.
* output 포트 Z는 reg 타입으로 선언되었습니다.



### 2.1.2 Vector Form





## 2.2 Instantiation











