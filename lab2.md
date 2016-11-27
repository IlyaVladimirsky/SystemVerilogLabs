####[Task1](#t1) - [pgd](https://www.edaplayground.com/x/5tVQ)

> Записать ограничения на рандомизацию параметров транзакции в соответствии с требованиями спецификации:
- [x] Транзакция содержит поля data, addr, strob, mode, status
- [x] Ширина данных 128 бита (data)
- [x] Ширина адреса 32 бита (addr)
- [x] Ширина строба (strob) равна числу байт в шине данных
- [x] Поле mode имеет четыре значения Чтение, Запись, Ожидание, Ошибка
- [x] Поле strob имеет только непрерывное заполнение единицами, т.е. значения 0000, 1010, 1101 и др. недопустимы
- [x] Поле data принимает значения 0101….01 и 1010….10 с вероятностью 10%
- [x] Поле data принимает максимальное и минимальное значения с вероятностью 1%
- [x] Поле addr меньше переменной max_user_addr и меньше максимально возможного MAX_BUS_ADDR
- [x] addr должен иметь значения адресующие только по 8 байт, если значение поля mode равно Чтение
- [x] Заполнение 1 поля strob должно начинаться с бита, номер которого равен значению младших 3-разрядов адреса.
- [x] Поле статус всегда равно OK, если mode равен Запись, иначе UNDEF

```systemverilog
module top; 
  	class T;
      parameter int DATA_WIDTH = 128;
      parameter int ADDR_WIDTH = 32;
      parameter int STROB_WIDTH = DATA_WIDTH / 8;
      
      parameter bit [31:0] MAX_BUS_ADDR = '1;
      parameter bit [31:0] MAX_USER_ADDR = 10000;      
      
      rand int low_strob_index; // is equal 3 low bits of addr
      rand int left_edge; // index of the left one in strob
      rand int right_edge; // index of the right one in strob
      
      rand bit [DATA_WIDTH - 1:0] data;
      rand bit [ADDR_WIDTH - 1:0] addr;
      rand bit [STROB_WIDTH - 1:0] strob;
      rand enum {read, write, await, error} mode;
      rand enum {OK, UNDEF} status;
      
      constraint order {
        solve mode before addr, status;
        
        solve addr before low_strob_index;
        solve low_strob_index before left_edge;
        
        solve left_edge before right_edge;
        solve right_edge before strob;
      }
      
      constraint strob_index {
        low_strob_index == int'(addr[2:0]);
      }
      
      constraint edges {
        (left_edge >= low_strob_index) && (left_edge <= STROB_WIDTH - 1);
        (right_edge >= left_edge) && (right_edge <= STROB_WIDTH - 1);
      }
      
      constraint strob_value {
        foreach(strob[i]) { 
          if(i >= left_edge && i <= right_edge) {
            strob[i] == 1; 
          } else {
            strob[i] == 0;  
          }
        }
      }
      
      constraint addr_value {
        (addr < MAX_BUS_ADDR) && (addr < MAX_USER_ADDR);
      }
      
      constraint addr_byte {
        if(mode == read) {
          int'(addr) % 8 == 0;
        }
      }
      
      constraint status_value {
        (mode == write) -> (status == OK);
        (mode != write) -> (status == UNDEF);
      }
      
      constraint data_chance {
        data dist {
          [1 : ({DATA_WIDTH{1'b1}} - 1)] :/ 78,
                  
          {(DATA_WIDTH / 2){2'b01}} := 10,
          {(DATA_WIDTH / 2){2'b10}} := 10,
          
          0 := 1,  
          {DATA_WIDTH{1'b1}} := 1
        };
      }      
    endclass
  
    initial begin
      T t = new;
      
      for (int i = 0 ; i < 10; i ++) begin
        t.randomize();
        
        $display("************New random values************");
        $display("MAX_BUS_ADDR = %0b", t.MAX_BUS_ADDR);
        $display("MAX_USER_ADDRr = %0b", t.MAX_USER_ADDR);
        $display("mode = %s", t.mode.name());
        $display("status = %s", t.status.name());
        $display("addr = %0b", t.addr);
        $display("low_strob_index = %0d", t.low_strob_index);
        $display("left_edge = %0d", t.left_edge);
        $display("right_edge = %0d", t.right_edge);
        $display("strob = %b", t.strob);
        $display("data = %b", t.data);
      end
    end  
endmodule
```
####[Output:](#t1out)
```
# KERNEL: ************New random values************
# KERNEL: MAX_BUS_ADDR = 11111111111111111111111111111111
# KERNEL: MAX_USER_ADDRr = 10011100010000
# KERNEL: mode = read
# KERNEL: status = UNDEF
# KERNEL: addr = 101101100000
# KERNEL: low_strob_index = 0
# KERNEL: left_edge = 11
# KERNEL: right_edge = 12
# KERNEL: strob = 0001100000000000
# KERNEL: data = 01100100000101111000100111110010110101110000100101001011011110100101111001100000110100011010011011101111100111011010000101000000
# KERNEL: ************New random values************
# KERNEL: MAX_BUS_ADDR = 11111111111111111111111111111111
# KERNEL: MAX_USER_ADDRr = 10011100010000
# KERNEL: mode = read
# KERNEL: status = UNDEF
# KERNEL: addr = 1111001000
# KERNEL: low_strob_index = 0
# KERNEL: left_edge = 10
# KERNEL: right_edge = 10
# KERNEL: strob = 0000010000000000
# KERNEL: data = 01010101010101010101010101010101010101010101010101010101010101010101010101010101010101010101010101010101010101010101010101010101
# KERNEL: ************New random values************
# KERNEL: MAX_BUS_ADDR = 11111111111111111111111111111111
# KERNEL: MAX_USER_ADDRr = 10011100010000
# KERNEL: mode = error
# KERNEL: status = UNDEF
# KERNEL: addr = 1011010101011
# KERNEL: low_strob_index = 3
# KERNEL: left_edge = 4
# KERNEL: right_edge = 12
# KERNEL: strob = 0001111111110000
# KERNEL: data = 11010111000100000100111100000100111101100111000011100010000010101100001001110101110011001101010101100111011101011100111101001111
# KERNEL: ************New random values************
# KERNEL: MAX_BUS_ADDR = 11111111111111111111111111111111
# KERNEL: MAX_USER_ADDRr = 10011100010000
# KERNEL: mode = read
# KERNEL: status = UNDEF
# KERNEL: addr = 10010101000000
# KERNEL: low_strob_index = 0
# KERNEL: left_edge = 5
# KERNEL: right_edge = 6
# KERNEL: strob = 0000000001100000
# KERNEL: data = 01110011000110100010111110000001111011110110010000001000101011010111111010101111010111111000111100100010000011000011110110001001
# KERNEL: ************New random values************
# KERNEL: MAX_BUS_ADDR = 11111111111111111111111111111111
# KERNEL: MAX_USER_ADDRr = 10011100010000
# KERNEL: mode = read
# KERNEL: status = UNDEF
# KERNEL: addr = 1111010010000
# KERNEL: low_strob_index = 0
# KERNEL: left_edge = 12
# KERNEL: right_edge = 14
# KERNEL: strob = 0111000000000000
# KERNEL: data = 01111101111111101101000101100101001100110100000010110111000010011101100101110010111010001100001100111010011000110101100011011001
# KERNEL: ************New random values************
# KERNEL: MAX_BUS_ADDR = 11111111111111111111111111111111
# KERNEL: MAX_USER_ADDRr = 10011100010000
# KERNEL: mode = read
# KERNEL: status = UNDEF
# KERNEL: addr = 100001011000
# KERNEL: low_strob_index = 0
# KERNEL: left_edge = 5
# KERNEL: right_edge = 14
# KERNEL: strob = 0111111111100000
# KERNEL: data = 00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
# KERNEL: ************New random values************
# KERNEL: MAX_BUS_ADDR = 11111111111111111111111111111111
# KERNEL: MAX_USER_ADDRr = 10011100010000
# KERNEL: mode = write
# KERNEL: status = OK
# KERNEL: addr = 110000000100
# KERNEL: low_strob_index = 4
# KERNEL: left_edge = 10
# KERNEL: right_edge = 13
# KERNEL: strob = 0011110000000000
# KERNEL: data = 10101010101010101010101010101010101010101010101010101010101010101010101010101010101010101010101010101010101010101010101010101010
# KERNEL: ************New random values************
# KERNEL: MAX_BUS_ADDR = 11111111111111111111111111111111
# KERNEL: MAX_USER_ADDRr = 10011100010000
# KERNEL: mode = read
# KERNEL: status = UNDEF
# KERNEL: addr = 1100111111000
# KERNEL: low_strob_index = 0
# KERNEL: left_edge = 0
# KERNEL: right_edge = 1
# KERNEL: strob = 0000000000000011
# KERNEL: data = 10010000011010011101000001010110101111001101001110111011001100111111010100011000110001101011101101111000111011000111010011110110
# KERNEL: ************New random values************
# KERNEL: MAX_BUS_ADDR = 11111111111111111111111111111111
# KERNEL: MAX_USER_ADDRr = 10011100010000
# KERNEL: mode = await
# KERNEL: status = UNDEF
# KERNEL: addr = 1111011001110
# KERNEL: low_strob_index = 6
# KERNEL: left_edge = 9
# KERNEL: right_edge = 9
# KERNEL: strob = 0000001000000000
# KERNEL: data = 11001101111111111000100110101011101011011011110111000001111011011100110111000110111110001101010110011100111010110010001001000110
# KERNEL: ************New random values************
# KERNEL: MAX_BUS_ADDR = 11111111111111111111111111111111
# KERNEL: MAX_USER_ADDRr = 10011100010000
# KERNEL: mode = await
# KERNEL: status = UNDEF
# KERNEL: addr = 1100110001010
# KERNEL: low_strob_index = 2
# KERNEL: left_edge = 4
# KERNEL: right_edge = 15
# KERNEL: strob = 1111111111110000
# KERNEL: data = 10110101101011011100111101101111011111110001000010010011110000001110110001001101001111101100111100110101100100000011101101010100
```

####[Task2](#t2) - [pgd](https://www.edaplayground.com/x/3YiA)
> Сделать ковер-группу, которая проверяет, что все размеры очереди в режиме чтения и записи транзакции из Спецификации 4 были проверены в тесте.<br><br>
Транзакция 4:
- [x] Транзакция содержит поля data, addr, strob, mode
- [x] Ширина данных 64 бита, поле data - очередь из N данных
- [x] Ширина адреса 32 бита, поле addr - очередь из N адресов
- [x] Ширина строба равна числу байт в шине данных, strob - очередь из N стробов
- [x] Число элементов в очередях адреса, данных и стробов должно совпадать и должно быть не более 16.
- [x] Поле mode имеет два значения Чтение, Запись
- [x] Поле strob имеет только непрерывное заполнение единицами, с учетом очереди слов (т.е. Множество единиц в словах составленных друг за другом не должны перемешиваться 0, другими словами все стробы со 2-го и до предпоследнего всегда равны всем 1, если 1-й и последний стробы больше 0)
- [x] Поле data принимает значения 0101….01 и 1010….10 с вероятностью 25%
- [x] Поле data принимает максимальное и минимальное значения с вероятностью 6%

```systemverilog
module top; 
  	class T;
      parameter MAX_N = 4; // decreased to cover fully
      parameter int DATA_WIDTH = 64;
      parameter int ADDR_WIDTH = 32;
      parameter int STROB_WIDTH = DATA_WIDTH / 8;  
      
      rand int n;  
      
      rand int strob_leftedge; // index of first word with ones in strob
      rand int strob_rightedge; // index of last word with ones in strob    
      rand int word_leftedge; // index first one in the left edge word
      rand int word_rightedge; // index last one in the right edge word
      
      rand bit [DATA_WIDTH - 1:0] data[$];
      rand bit [ADDR_WIDTH - 1:0] addr[$];
      rand bit [STROB_WIDTH - 1:0] strob[$];
      rand enum {read, write} mode;
      
      function new;
        cg_modes_sizes = new;
      endfunction
      
      function display_transaction(); 
        string s = "";
      	string temps = "";
        
        $display("mode = %s", mode.name());
        $display("n = %0d", n);
        $display("addr = %0b", addr);
        foreach (strob[i]) begin
          temps.bintoa(strob[i]);
          s = {s, "|", temps, "|"};        
        end
        $display("strob = %s", s);
        $display("data = %0b", data[0]);
        $display("coverage = %0d", cg_modes_sizes.get_coverage());
      endfunction
      
      constraint order {
        solve n before data, addr, strob;
        
        solve strob_leftedge before strob_rightedge; 
        solve strob_rightedge before word_leftedge; 
        solve word_leftedge before word_rightedge; 
      }
            
      constraint n_par {
        (n >= 1) && (n <= MAX_N);
      }
      
      constraint queue_sizes { 
        (data.size == n) && (addr.size == n) && (strob.size == n);
      }
      
      constraint strob_edges {
        (strob_leftedge >= 0) && 
        (strob_leftedge <= strob.size() - 1);
        
        (strob_rightedge >= strob_leftedge) && 
        (strob_rightedge <= strob.size() - 1);
      }
      
      constraint word_edges {
        (word_leftedge >= 0) && 
        (word_leftedge <= STROB_WIDTH - 1);
        
        if(strob_leftedge == strob_rightedge) {
          (word_rightedge >= word_leftedge);
        } else {
          (word_rightedge >= 0);
        }
        (word_rightedge <= STROB_WIDTH - 1);
      }
      
      constraint strob_queue {
        foreach(strob[i, j]) { 
          if(strob_leftedge == strob_rightedge) {
            if(j >= word_leftedge && j <= word_rightedge &&
               i == strob_leftedge) {
              strob[i][j] == 1;
            } else {
              strob[i][j] == 0;
            }
          } else {          
            if(
              ((i == strob_leftedge && j <= word_leftedge) || 
               (i == strob_rightedge && j >= word_rightedge)) ||
              (i > strob_leftedge && 
               i < strob_rightedge)) {
              strob[i][j] == 1; 
            } else {
              strob[i][j] == 0;  
            }
          }
        }
      }
            
      constraint data_chance {
        foreach (data[i]) {
          data[i] dist { 
            [1 : ({DATA_WIDTH{1'b1}} - 1)] :/ 38,
                  
            {(DATA_WIDTH / 2){2'b01}} := 25,
            {(DATA_WIDTH / 2){2'b10}} := 25,
          
          	0 := 6,  
            {DATA_WIDTH{1'b1}} := 6
          };
        }
      }  
          
      covergroup cg_modes_sizes;
        SIZES : coverpoint n {
          bins sizes[MAX_N] = {[1:MAX_N]};
        }

        MODES : coverpoint mode {
          bins mode_bins[2] = {read, write};
        }

        CRS_SIZES_MODES : cross SIZES, MODES;
      endgroup
    endclass
  
    initial begin
      T t = new;
      int loop_count = 0;
      int show_loops = 10;      
       
      while(t.cg_modes_sizes.get_coverage() < 100) begin
        loop_count++;
        
        t.randomize();
        t.cg_modes_sizes.sample();
        
        if(loop_count < show_loops) begin
          $display("************New random values************"); 
          $display("loop number = %0d", loop_count); 
          t.display_transaction();
        end
      end
      
      $display("loops number to cover fully = %0d", loop_count);
    end  
endmodule
```
####[Output:](#t1out)
```
# KERNEL: ************New random values************
# KERNEL: loop number = 1
# KERNEL: mode = write
# KERNEL: n = 4
# KERNEL: addr = 11111111001100011000100100100010110111001011000
# KERNEL: strob = |11||11111111||11111100||0|
# KERNEL: data = 101100100110010011010000111100110111010101001101111000010010101
# KERNEL: coverage = 29
# KERNEL: ************New random values************
# KERNEL: loop number = 2
# KERNEL: mode = read
# KERNEL: n = 3
# KERNEL: addr = 11111111001100011000100100100010110111001011000
# KERNEL: strob = |0||0||10000000|
# KERNEL: data = 101010101010101010101010101010101010101010101010101010101010101
# KERNEL: coverage = 58
# KERNEL: ************New random values************
# KERNEL: loop number = 3
# KERNEL: mode = read
# KERNEL: n = 2
# KERNEL: addr = 11111111001100011000100100100010110111001011000
# KERNEL: strob = |0||110000|
# KERNEL: data = 101010101010101010101010101010101010101010101010101010101010101
# KERNEL: coverage = 71
# KERNEL: ************New random values************
# KERNEL: loop number = 4
# KERNEL: mode = write
# KERNEL: n = 1
# KERNEL: addr = 11111111001100011000100100100010110111001011000
# KERNEL: strob = |111|
# KERNEL: data = 111010011000110101100011011011100011001011110010001111100100100
# KERNEL: coverage = 83
# KERNEL: ************New random values************
# KERNEL: loop number = 5
# KERNEL: mode = write
# KERNEL: n = 4
# KERNEL: addr = 11111111001100011000100100100010110111001011000
# KERNEL: strob = |0||0||11111||11100000|
# KERNEL: data = 101010101010101010101010101010101010101010101010101010101010101
# KERNEL: coverage = 83
# KERNEL: ************New random values************
# KERNEL: loop number = 6
# KERNEL: mode = read
# KERNEL: n = 2
# KERNEL: addr = 11111111001100011000100100100010110111001011000
# KERNEL: strob = |11111110||0|
# KERNEL: data = 1110001100010100000110010100011101101110001011001011011000000001
# KERNEL: coverage = 83
# KERNEL: ************New random values************
# KERNEL: loop number = 7
# KERNEL: mode = read
# KERNEL: n = 4
# KERNEL: addr = 11111111001100011000100100100010110111001011000
# KERNEL: strob = |0||0||1111||11000000|
# KERNEL: data = 101010101010101010101010101010101010101010101010101010101010101
# KERNEL: coverage = 88
# KERNEL: ************New random values************
# KERNEL: loop number = 8
# KERNEL: mode = write
# KERNEL: n = 4
# KERNEL: addr = 11111111001100011000100100100010110111001011000
# KERNEL: strob = |0||0||0||1111100|
# KERNEL: data = 1010101010101010101010101010101010101010101010101010101010101010
# KERNEL: coverage = 88
# KERNEL: ************New random values************
# KERNEL: loop number = 9
# KERNEL: mode = write
# KERNEL: n = 4
# KERNEL: addr = 11111111001100011000100100100010110111001011000
# KERNEL: strob = |0||0||11111||11000000|
# KERNEL: data = 1111111111111111111111111111111111111111111111111111111111111111
# KERNEL: coverage = 88
# KERNEL: loops number to cover fully = 383
```
