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

####[Task2](#t2) - [pgd](https://www.edaplayground.com/x/nJz)
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

####[Task3](#t3) - [pgd](https://www.edaplayground.com/x/4nHn)
```systemverilog
module top;
	initial begin
      string str;
      str = "I'm a programmer!";
      
      $display("First char is '%s'", str.getc(0));
      $display("Last char is '%s'", str.getc(str.len - 1));
    end  
endmodule
```
[**Output:**](#t3out)
```
# KERNEL: First char is 'I'
# KERNEL: Last char is '!'
```

####[Task4](#t4) - [pgd](https://www.edaplayground.com/x/5_bY)
```systemverilog
module top;
	initial begin
      string str = "To throw the cat among the pigeons";
      string arr[10];
      
      string word = "";
      int word_counter = 0;
      foreach (str[i])
        if (str[i] == 32 || i == str.len - 1) begin
          arr[word_counter] = word;
          if (str[i] != 32)
            arr[word_counter] = {word, string'(str[i])};
          $display("%s", arr[word_counter]);
          word = "";
        end else
          word = {word, string'(str[i])};
    end 
endmodule
```
[**Output:**](#t4out)
```
# KERNEL: To
# KERNEL: throw
# KERNEL: the
# KERNEL: cat
# KERNEL: among
# KERNEL: the
# KERNEL: pigeons
```

####[Task5](#t5) - [pgd](https://www.edaplayground.com/x/q_f)
```systemverilog
module top;    
	initial begin
      int matrix [5][10];
      int temp [10];
      
      foreach (matrix[i, j]) begin
        matrix[i][j] = $urandom_range(100,1);
      end
      
      foreach (matrix[i, j]) begin
        temp[j] = matrix[i][j];
        if(j == 9) begin
          temp.sort();
          foreach(temp[k])
            matrix[i][k] = temp[k];
        end        
      end
    end 
endmodule
```

####[Task6](#t6) - [pgd](https://www.edaplayground.com/x/4Wub)
```systemverilog
module top;    
	initial begin
      bit [3:0][7:0] matrix [0:4][0:9];
      int tempint;
      string s = "";
      string temp = "";
      
      foreach (matrix[i, j]) begin
        matrix[i][j] = $urandom_range(100,1);        
      end      
      
      foreach (matrix[i, j]) begin
        temp.itoa(matrix[i][j]);
        s = {s, temp};
        if(j == 9) begin
          $display(s);
          s = "";
        end        
      end
    end 
endmodule
```
[**Output:**](#t6out)
```
# KERNEL: 3322820388840647272
# KERNEL: 839376559256685448
# KERNEL: 3963292887595759455
# KERNEL: 63906492222175355085
# KERNEL: 50768019827316805461
```

####[Task7](#t7) - [pgd](https://www.edaplayground.com/x/2QsV)
```systemverilog
module top;    
	initial begin
      byte darray [];
      darray = new[10000];    
      darray = new[100](darray);                                  
                                 
      $display("darray has %0d elements", darray.size());
      darray.delete();     
    end 
endmodule
```
[**Output:**](#t7out)
```
# KERNEL: darray has 100 elements
```
