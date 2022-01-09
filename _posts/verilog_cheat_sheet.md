### Verilog Cheat Sheet

#### Comment

```verilog
// One-liner 
/* Multiple lines */
```

---

#### Numeric Constants

```verilog
// The 8-bit decimal number 106: 
8'b_0110_1010 // Binary
8'o_152 // Octal
8'd_106 // Decimal
8'h_6A // Hexadecimal 
"j" // ASCII

// 78-bit high-impedance
78'bZ 
```

---

#### Nets and Variables

```verilog
wire [3:0] w; // Assign outside always blocks 
reg  [1:7] r; // Assign inside always blocks 
reg  [7:0] mem[31:0];

integer j; // Compile-time variable 
genvar k; // Generate variable
```

---

#### Parameter

```verilog
parameter  N     = 8;
localparam State = 2'd3;
```

---

#### Continuous Assignments

```verilog
assign Output = A * B;
assign {C, D} = {D[5:2], C[1:9], E};
```

---

#### Operators

```verilog
// These are in order of precedence... 
// Select
A[N] 
A[N:M]
// Reduction
&A ~&A |A ~|A ^A ~^A
// Compliment
!A ~A
// Unary
+A -A
// Concatenate
{A, ..., B}
// Replicate
{N{A}}
// Arithmetic
A*B A/B A%B
A+B A-B
// Shift
A<<B A>>B
// Relational
A>B A<B A>=B A<=B A==B A!=B
// Bit-wise
A&B
A^B A~^B
A|B
// Logical 
A&&B
A||B
// Conditional 
A ? B : C
```

---

#### Module

```verilog
module MyModule #(
  parameter WIDTH = 8 // Optional parameter
)(
  input  reset, clk, 
  input  wire in,
  output reg [WIDTH-1:0] out
);
// Module implementation
endmodule
```

---

#### Module Instantiation

```verilog
// Override default parameter: setting WIDTH = 13
MyModule #(.WIDTH(13)) MyModule1(.reset(reset), .clk(clk), .result(result));
```

---

#### If

```verilog
always @(*) 
  if(s == 1'b11) begin
    A = 8'd9;
  end
  else if(s == 1'b10) begin
    A = 2'd2: 
  end
  else begin
    A = 8'd2;
  end
end
```

---

#### Case

```verilog
always @(*) 
  case(Mux)
    2'd0: A = 8'd9;
    2'd1, 
    2'd3: A = 2'd2: 
    2'd2: A = 8'd2;
    A = default:;
  endcase 
end
```

---

```verilog
always @(*)
  casez(Decoded)
    4'b1???: Encoded = 2'd0; 
    4'b01??: Encoded = 2'd1; 
    4'b001?: Encoded = 2'd2; 
    4'b0001: Encoded = 2'd3; 
    default: Encoded = 2'd0;
  endcase
end
```

