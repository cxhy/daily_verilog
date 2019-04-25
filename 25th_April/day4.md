# day4

## 问题

1. 什么是竞争和冒险？

2. 设计一个2-4译码器。

3. 输入一个8bit数，输出其中1的个数。如果只能使用1bit全加器，最少需要几个？

4. 如果一个标准单元库只有三个cell：2输入mux(o = s ？a ：b;)，TIEH(输出常数1)，TIEL(输出常数0)，如何实现以下功能？

4.1 反相器inv
4.2 缓冲器buffer 
4.3 两输入与门and2
4.4 两输入或门or2
4.5 四输入的mux  mux4
4.6 一位全加器 fa

## 解答

1. 看图

   ![Race_condition](Race_condition.svg)

   实际上就是同一个信号经过一个反相器之后和自己经过一个逻辑门产生一个窄脉冲的问题。这个在前仿真中不会遇到，但是在后仿真和实际情况中会比较常见。如果这个信号是时钟信号就会产生严重的后果，所以我们需要对关键路径进行约束识得那些我们需要关注的路径不存在这种毛刺。

2. 就是写一个case语句

3. 全加器真值表如下

   ![snipaste_20190426_002029](E:\doc\daily_verilog\25th_April\snipaste_20190426_002029.png)

4.  4.1 assign o = s ? 1'b0 : 1'b1;

   4.2 assign o = s ？ 1'b1 : 1'b0;

   4.3 assign o = s1 ? (s2 ? 1'b1 : 1'b0) : 1'b0;

   4.4 assign o = s1 ? 1'b1 : (s2 ? 1'b1 : 1'b0);

   4.5 assign o = s1 ? 

   ​								s2 

module or2(a,b,o);
input a,b;
output o;
wire a,b,o;
  assign o = a ? 1'b1 : (b ? 1'b1  : 1'b0);
endmodule

module and2(a,b,o);
input a,b;
output o;
wire a,b,o;
  assign o = a ? (b ? 1'b1 : 1'b0) : 1'b0;
endmodule

module mux4(a,b,c,d,s1,s2,o);
input a,b,c,d,s1,s2;
output o;
wire a,b,c,d,s1,s2,o;
wire t1_1,,t1_2,t2_1,t2_2,t3_1,t3_2,t4_1,t4_2,o1,o2,o3;
  and2 x1_and2(
    .a (a),
    .b (~s1),
    .o (t1_1)
  );
  and2 x2_and2(
    .a (t1_1),
    .b (~s2),
    .o (t1_2)
  );

  and2 x3_and2(
    .a (b),
    .b (~s1),
    .o (t2_1)
  );
  and2 x4_and2(
    .a (t2_1),
    .b (s2),
    .o (t2_2)
  );

  and2 x5_and2(
    .a (c),
    .b (s1),
    .o (t3_1)
  );
  and2 x6_and2(
    .a (t2_1),
    .b (~s2),
    .o (t3_2)
  );

  and2 x7_and2(
    .a (d),
    .b (s1),
    .o (t3_1)
  );
  and2 x8_and2(
    .a (t3_1),
    .b (s2),
    .o (t3_2)
  );

  or2 x1_or2(
    .a (t1_2),
    .b (t2_2),
    .o (o1)
  );
  or2 x2_or2(
    .a (o1),
    .b (t3_2),
    .o (o2)
  );
  or2 x3_or2(
    .a (o2),
    .b (t4_2),
    .o (o)
  );

endmodule