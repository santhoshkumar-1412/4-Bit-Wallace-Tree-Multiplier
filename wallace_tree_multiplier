`timescale 1ns/1ps
module ha(output s,c,input a,b);
assign s = a^b;
assign c = a&b;
endmodule
module fa(output s,cout,input a,b,cin);
assign s = a^b^cin;
assign cout = (a&b)|(b&cin)|(a&cin);
endmodule
module wallace_tree_multiplier(output [7:0]p,input [3:0]a,b);
wire [15:0] pp;
// Partial product generation
assign pp[3:0]   = a & {4{b[0]}};
assign pp[7:4]   = a & {4{b[1]}};
assign pp[11:8]  = a & {4{b[2]}};
assign pp[15:12] = a & {4{b[3]}};
// First reduction
wire p1,c1;
wire s2,c2;
wire p2,c3;
assign p[0] = pp[0];
ha h1(p1,c1,pp[1],pp[4]);
fa f1(s2,c2,pp[2],pp[5],pp[8]);
ha h2(p2,c3,s2,c1);
assign p[1] = p1;
assign p[2] = p2;
// Second reduction
wire s3a,c3a,s3b,c3b;
wire p3,c4;
fa f2(s3a,c3a,pp[3],pp[6],pp[9]);
fa f3(s3b,c3b,pp[12],c2,c3);
ha h3(p3,c4,s3a,s3b);
assign p[3] = p3;
// Third reduction
wire s4a,c4a,s4b,c4b;
wire p4,c5;
fa f4(s4a,c4a,pp[7],pp[10],pp[13]);
fa f5(s4b,c4b,c3a,c3b,c4);
ha h4(p4,c5,s4a,s4b);
assign p[4] = p4;
// Fourth reduction
wire s5a,c5a,s5b,c5b;
wire p5,c6;
fa f6(s5a,c5a,pp[11],pp[14],c4a);
ha h5(s5b,c5b,c4b,c5);
ha h6(p5,c6,s5a,s5b);
assign p[5] = p5;
// Fifth reduction
wire s6,c7;
wire p6,c8;
fa f7(s6,c7,pp[15],c5a,c5b);
ha h7(p6,c8,s6,c6);
assign p[6] = p6;
// Final output bit
wire unused_carry;
ha h8(p[7],unused_carry,c7,c8);
endmodule
