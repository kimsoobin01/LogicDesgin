# Lab 09
## 실습 내용
### **IR Controller 확인**
#### **NEC 적외선 통신 규약** 
- Leader Code: 프레임 모드 선택
- Custom Code: 특정 회사 나타냄
- Data Code: 송신 데이터
#### **송신부** : 적외선 발광 다이오드
#### **수신부** : 포토 다이오드

## verilog 코드
### **1. dut**
 ` module ir_rx(
				o_data,
				i_ir_rxb,
				clk,
				rst_n); `
### **tx** : 송신
### **rx** : 발신


 ` parameter IDLE = 2'b00 ;
   parameter LEADCODE = 2'b01 ; 
   parameter DATACODE = 2'b10 ; 
   parameter COMPLETE = 2'b11 ;  `
### **4개의 state를 생성**
: NEC 적외선 통신 규약
### **이후 case 문으로 각 state를 정의해 줌**
 ` wire ir_rx ;
assign ir_rx = ~i_ir_rxb;
reg [1:0] seq_rx ;
always @(posedge clk_1M or negedge rst_n) begin
	if(rst_n == 1'b0) begin 
		seq_rx <= 2'b00;
	end else begin
		seq_rx <= {seq_rx[0], ir_rx};
	end
end  `

### **-> 리모콘으로부터 오는 신호**

`reg [15:0] cnt_h ;
reg [15:0] cnt_l ;
always @(posedge clk_1M or negedge rst_n) begin
	if(rst_n == 1'b0) begin
		cnt_h <= 16'd0;
		cnt_l <= 16'd0;
	end else begin
		case(seq_rx)
			2'b00 : cnt_l <= cnt_l + 1;
			2'b01 : begin
				cnt_l <= 16'd0;
				cnt_h <= 16'd0;
			end
			2'b11 : cnt_h <= cnt_h + 1;
		endcase
	end
end`

**2'b00에선 1씩 증가, 아닌 경우 high**
### **2. test bench**
####  **L_CODE, D_CODE0, D_CODE1을 랜덤 값으로 돌려주어 leader code를 실행시킴.**

## 결과 
### **Top Module 의 DUT/TestBench Code 및 Waveform 검증**
`![](https://github.com/kimsoobin01/LogicDesgin/blob/master/Practice09/figs/wave.jpg)`
### **FPGA 동작 사진 **
`Please fill up your source`
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQyNTAzODU5MywyMDc4NDAwMjg3XX0=
-->
