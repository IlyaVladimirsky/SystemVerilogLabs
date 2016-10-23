## Lab1

####[Task1](#t1) - [pgd](https://www.edaplayground.com/x/2FdB)
```systemverilog
module top;
	initial begin
      string sbin, sdec, shex;
      sbin.bintoa(8'b1000_0101);
      sdec.itoa(11);
      shex.hextoa('hff);
      
      $display("Binary format: %s", sbin);
      $display("Decimal format: %s", sdec);
      $display("Hex format: %s", shex);
      $display("String format: %s", "task 1 !!!");
      $display;
    end  
endmodule
```
[**Output:**](#t1out)
```
# KERNEL: Binary format: 10000101
# KERNEL: Decimal format: 11
# KERNEL: Hex format: ff
# KERNEL: String format: task 1 !!!
# KERNEL: 
```

[Task2](#t2) - [pgd](https://www.edaplayground.com/x/nJz)
```systemverilog
module top;
	initial begin
      typedef struct {
        int value;
        string description;
        bit [7:0] vector;
      } Structure_task_2;
      Structure_task_2 struct1;
      Structure_task_2 struct2;
      
      struct1.value = ~(struct1.value);
      
      $display("-----First Struct-----");
      $display("Value: %0d", struct1.value);
      $display("Description: %s", struct1.description);
      $display("Vector: %b", struct1.vector);
      
      $display("-----Second Struct-----");
      $display("Value: %0d", struct2.value);
      $display("Description: %s", struct2.description);
      $display("Vector: %b", struct2.vector);
    end
  
endmodule
```
[**Output:**](#t2out)
```
# KERNEL: -----First Struct-----
# KERNEL: Value: -1
# KERNEL: Description: 
# KERNEL: Vector: 00000000
# KERNEL: -----Second Struct-----
# KERNEL: Value: 0
# KERNEL: Description: 
# KERNEL: Vector: 00000000
```

