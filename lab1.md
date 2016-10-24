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

####[Task12](#t12) - [pgd](https://www.edaplayground.com/x/6Ms4)
```systemverilog
package Pack;
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
    endclass
endpackage : Pack

module top;
  	import Pack::*;
  
  	class AdvancedArrOverriden extends Pack::AdvancedArrExt;
      function printAll();
        AdvancedArr::printAll();
      endfunction
    endclass
  
  	initial begin
      AdvancedArrOverriden arr = new;
      arr.printAll();
      arr = null;
  	end  
endmodule
```
####[Output:](#t12out)
```
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

####[Task13](#t13) - [pgd](https://www.edaplayground.com/x/ZEZ)
```systemverilog
module top;
	timeunit 1ms;
  
    class Tasker; 
      event startout;
      event endout;
      int i = 1;
      int bi = 1;
      
      // increment every 10ms instead of 1us to adapt the output
      task inc;        
        forever #10000000 begin 
          $display("%0d", i++);
          if (i > 1)
            ->startout;
        end
      endtask
      
      task biginc;
        forever #1000000 begin // 1ms tacting
          $display("%0d000us", bi++);
          if (bi == 51)
            ->endout;
        end
      endtask
      
      task handle;
        @(startout);
        $display("Start check processing...");
        
        @(endout);
        $display("End check processing...");
      endtask
    endclass
        
  	initial begin
      Tasker t = new;
      
      fork
        t.inc(); 
        t.biginc();
        t.handle();
      join_none
      
      #100 $finish;
  	end  
endmodule
```
####[Output:](#t13out)
```
# KERNEL: 1000us
# KERNEL: 2000us
# KERNEL: 3000us
# KERNEL: 4000us
# KERNEL: 5000us
# KERNEL: 6000us
# KERNEL: 7000us
# KERNEL: 8000us
# KERNEL: 9000us
# KERNEL: 1
# KERNEL: 10000us
# KERNEL: Start check processing...
# KERNEL: 11000us
# KERNEL: 12000us
# KERNEL: 13000us
# KERNEL: 14000us
# KERNEL: 15000us
# KERNEL: 16000us
# KERNEL: 17000us
# KERNEL: 18000us
# KERNEL: 19000us
# KERNEL: 2
# KERNEL: 20000us
# KERNEL: 21000us
# KERNEL: 22000us
# KERNEL: 23000us
# KERNEL: 24000us
# KERNEL: 25000us
# KERNEL: 26000us
# KERNEL: 27000us
# KERNEL: 28000us
# KERNEL: 29000us
# KERNEL: 3
# KERNEL: 30000us
# KERNEL: 31000us
# KERNEL: 32000us
# KERNEL: 33000us
# KERNEL: 34000us
# KERNEL: 35000us
# KERNEL: 36000us
# KERNEL: 37000us
# KERNEL: 38000us
# KERNEL: 39000us
# KERNEL: 4
# KERNEL: 40000us
# KERNEL: 41000us
# KERNEL: 42000us
# KERNEL: 43000us
# KERNEL: 44000us
# KERNEL: 45000us
# KERNEL: 46000us
# KERNEL: 47000us
# KERNEL: 48000us
# KERNEL: 49000us
# KERNEL: 5
# KERNEL: 50000us
# KERNEL: End check processing...
# KERNEL: 51000us
# KERNEL: 52000us
# KERNEL: 53000us
# KERNEL: 54000us
# KERNEL: 55000us
# KERNEL: 56000us
# KERNEL: 57000us
# KERNEL: 58000us
# KERNEL: 59000us
# KERNEL: 6
# KERNEL: 60000us
# KERNEL: 61000us
# KERNEL: 62000us
# KERNEL: 63000us
# KERNEL: 64000us
# KERNEL: 65000us
# KERNEL: 66000us
# KERNEL: 67000us
# KERNEL: 68000us
# KERNEL: 69000us
# KERNEL: 7
# KERNEL: 70000us
# KERNEL: 71000us
# KERNEL: 72000us
# KERNEL: 73000us
# KERNEL: 74000us
# KERNEL: 75000us
# KERNEL: 76000us
# KERNEL: 77000us
# KERNEL: 78000us
# KERNEL: 79000us
# KERNEL: 8
# KERNEL: 80000us
# KERNEL: 81000us
# KERNEL: 82000us
# KERNEL: 83000us
# KERNEL: 84000us
# KERNEL: 85000us
# KERNEL: 86000us
# KERNEL: 87000us
# KERNEL: 88000us
# KERNEL: 89000us
# KERNEL: 9
# KERNEL: 90000us
# KERNEL: 91000us
# KERNEL: 92000us
# KERNEL: 93000us
# KERNEL: 94000us
# KERNEL: 95000us
# KERNEL: 96000us
# KERNEL: 97000us
# KERNEL: 98000us
# KERNEL: 99000us
```

####[Task14](#t14) - [pgd](https://www.edaplayground.com/x/3qVz)
```systemverilog
module top;
  	bit c = 0;
  
  	initial begin      
      fork
        while (1) begin
          #10 c = ~c; // clock each 10ns
          $display("%0b", c);
        end
      join_none
      
      #100 $finish;
  	end  
endmodule
```
####[Output:](#t14out)
```
# KERNEL: 1
# KERNEL: 0
# KERNEL: 1
# KERNEL: 0
# KERNEL: 1
# KERNEL: 0
# KERNEL: 1
# KERNEL: 0
# KERNEL: 1
```

####[Task15](#t15) - [pgd](https://www.edaplayground.com/x/nNV)
```systemverilog
module top;
  	function automatic equal(ref int ass1[int], ref int ass2[int]);
      if(ass1.num() != ass2.num())
        return 0;      
      
      foreach(ass1[i]) begin
        int res = 0;
        foreach(ass2[j])
          if(ass1[i] == ass2[j]) 
            res = 1;
        if (res != 1) return 0;
      end
      return 1;
    endfunction
  
  	initial begin  
      int ass[int] = '{2:5, 3:100, 5:2, -1:-1};
      int copy_of_ass[int] = '{-1:-1, 2:5, 3:100, 5:2};
      
      if(equal(ass, copy_of_ass))
        $display("Asses are really equal!");
      else
        $display("Asses are not equal!");
  	end 
endmodule
```
####[Output:](#t15out)
```
# KERNEL: Asses are really equal
```

####[Task16](#t16) - [pgd](https://www.edaplayground.com/x/nNf)
```systemverilog
module top;
  	function qfind_min(int q[]);
      int darr[];
      int min = 0;
      darr = new[q.size];
            
      for (int i = 0; i < q.size; i++)
        darr[i] = q[i];
      
      $display("Finded min value: %0d", int'(darr.min));
    endfunction
  
  	function qfind_max(int q[]);
      int max = q[0];
      foreach(q[i])
        max = (max < q[i]) ? q[i] : max;
      
      $display("Finded max value: %0d", max);
    endfunction
  
  	initial begin  
      int q[$] = {1, 10, -2, 5, 16, -6, 0, 2};
      
      qfind_min(q);
      qfind_max(q);   
  	end  
endmodule
```
####[Output:](#t16out)
```
# KERNEL: Finded min value: -6
# KERNEL: Finded max value: 16
```

####[Task17](#t17) - [pgd](https://www.edaplayground.com/x/nNr)
```systemverilog
module top;
  typedef int pair[2];
  
  function automatic is_intersected(pair q[4]);
      for (int i = 0; i < 4; i++) begin
        int min = (q[i][0] < q[i][1]) ? q[i][0] : q[i][1];
        int max = (q[i][0] > q[i][1]) ? q[i][0] : q[i][1];
        
      	for (int j = 0; j < 4; j++) begin
          if (i != j && ( 
            (min <= q[j][0] && max >= q[j][0]) ||
            (min <= q[j][1] && max >= q[j][1])))
            return 1;
        end
      end
      return 0;
    endfunction
  
  	initial begin 
      pair q[$];
      
      pair p = {10, 5};
      q.push_back(p);
      p = {1, -1};
      q.push_back(p);
      p = {-15, -12};
      q.push_back(p);
      p = {2, 3};
      q.push_back(p);
            
      if(is_intersected(q))
        $display("Ranges are intersected!");
      else
        $display("Ranges are not intersected!");
  	end  
endmodule
```
####[Output:](#t17out)
```
# KERNEL: Ranges are not intersected!
```

####[Task18](#t18) - [pgd](https://www.edaplayground.com/x/nNf)
```systemverilog
module top;
    class A;
      rand int data[];

      constraint c_data {
        data.size inside {5};
        foreach(data[j]) data[j] inside {[-100:100]};
      }

      function new;
        randomize();        
      endfunction
    endclass

    class B extends A;      
    endclass
  
  	class C;   
      function int sum(A a);
        int s = 0;
        foreach(a.data[i])
          s += a.data[i];

        return s;
      endfunction
    endclass

    initial begin
      C c = new;
      B b = new;
      
      $display("Array is %p", b.data);
      $display("Sum: %0d", c.sum(b));
      
      c = null;
      b = null;
    end  
endmodule
```
####[Output:](#t18out)
```
# KERNEL: Array is '{-15, -75, 30, -40, 38}
# KERNEL: Sum: -62
```
