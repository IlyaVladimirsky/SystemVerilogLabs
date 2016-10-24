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

####[Task9](#t9) - [pgd](https://www.edaplayground.com/x/43WC)
```systemverilog
module top;    
	initial begin
      typedef struct {
        int value;
        string description;
        bit [7:0] vector;
      } Structure_task_2;
      Structure_task_2 arr[10];
      Structure_task_2 q[$];
      
      foreach (arr[i]) begin
        arr[i].value = i;
        q.push_back(arr[i]);
      end
      
      q.pop_front();
      q.pop_front();
      q.pop_front();
      
      foreach (q[i]) begin
        $display("-------Struct-------");
        $display("Value: %0d", q[i].value);
      	$display("Description: %s", q[i].description);
      	$display("Vector: %b", q[i].vector);
      end
    end 
endmodule
```
[**Output:**](#t9out)
```
# KERNEL: -------Struct-------
# KERNEL: Value: 3
# KERNEL: Description: 
# KERNEL: Vector: 00000000
# KERNEL: -------Struct-------
# KERNEL: Value: 4
# KERNEL: Description: 
# KERNEL: Vector: 00000000
# KERNEL: -------Struct-------
# KERNEL: Value: 5
# KERNEL: Description: 
# KERNEL: Vector: 00000000
# KERNEL: -------Struct-------
# KERNEL: Value: 6
# KERNEL: Description: 
# KERNEL: Vector: 00000000
# KERNEL: -------Struct-------
# KERNEL: Value: 7
# KERNEL: Description: 
# KERNEL: Vector: 00000000
# KERNEL: -------Struct-------
# KERNEL: Value: 8
# KERNEL: Description: 
# KERNEL: Vector: 00000000
# KERNEL: -------Struct-------
# KERNEL: Value: 9
# KERNEL: Description: 
# KERNEL: Vector: 00000000
```

####[Task10](#t10) - [pgd](https://www.edaplayground.com/x/3YpF)
```systemverilog
module top;
    class AdvancedArr;
      rand int A[];
      int minAi;
      int maxAi;
      
      constraint c_A {
        A.size inside {10};
        foreach(A[j]) A[j] inside {[-10:10]};
      }
      
      function int getMax;
        maxAi = int'(A.max);
        
        return maxAi;
      endfunction : getMax
      
      function int getMin;
        minAi = int'(A.min);
        
        return minAi;
      endfunction : getMin
      
      virtual function printAll();
        foreach (A[i])
          $display("A[%0d] = %0d", i, A[i]);
        $display("maxAi = %0d", getMax());
        $display("minAi = %0d", getMin());
      endfunction
    endclass : AdvancedArr
        
	initial begin
      AdvancedArr arr = new;
      arr.randomize();
      arr.printAll();
      arr = null;
  	end  
endmodule
```
[**Output:**](#t10out)
```
# KERNEL: A[0] = -7
# KERNEL: A[1] = -9
# KERNEL: A[2] = -6
# KERNEL: A[3] = 9
# KERNEL: A[4] = -7
# KERNEL: A[5] = 10
# KERNEL: A[6] = 3
# KERNEL: A[7] = 7
# KERNEL: A[8] = -9
# KERNEL: A[9] = 7
# KERNEL: maxAi = 10
# KERNEL: minAi = -9
```

####[Task11](#t11) - [pgd](https://www.edaplayground.com/x/5m_E)
```systemverilog
module top;
    class AdvancedArr;
      rand int A[];
      int minAi;
      int maxAi;
      
      constraint c_A {
        A.size inside {10};
        foreach(A[j]) A[j] inside {[-100:100]};
      }
      
      function new;
      	randomize();        
      endfunction
      
      function int getMax;
        maxAi = int'(A.max);
        
        return maxAi;
      endfunction : getMax
      
      function int getMin;
        minAi = int'(A.min);
        
        return minAi;
      endfunction : getMin
      
      virtual function printAll();
        foreach (A[i])
          $display("A[%0d] = %0d", i, A[i]);
        $display("maxAi = %0d", getMax());
        $display("minAi = %0d", getMin());
      endfunction
    endclass : AdvancedArr
        
  	class AdvancedArrExt extends AdvancedArr;
      function printAll();
        $display("-----Created by %s-----", "Ilia Vladimirsky");
        super.printAll();        
      endfunction
    endclass : AdvancedArrExt
  
	initial begin
      AdvancedArrExt arr = new;
      arr.printAll();
      arr = null;
  	end  
endmodule
```
[**Output:**](#t11out)
```
# KERNEL: -----Created by Ilia Vladimirsky-----
# KERNEL: A[0] = -15
# KERNEL: A[1] = -75
# KERNEL: A[2] = 30
# KERNEL: A[3] = -40
# KERNEL: A[4] = 38
# KERNEL: A[5] = -15
# KERNEL: A[6] = -39
# KERNEL: A[7] = -71
# KERNEL: A[8] = -56
# KERNEL: A[9] = -73
# KERNEL: maxAi = 38
# KERNEL: minAi = -75
```
