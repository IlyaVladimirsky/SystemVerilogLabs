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
      bit [31:0] MAX_BUS_ADDR = '1;
      bit [31:0] max_user_addr = 10000;      
      
      rand int low_strob_index; // is equal 3 low bits of addr
      rand int left_edge; // index of the left one in strob
      rand int right_edge; // index of the right one in strob
      
      rand bit [127:0] data;
      rand bit [31:0] addr;
      rand bit [128 / 8 - 1:0] strob;
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
        left_edge >= low_strob_index && left_edge <= 15;
        right_edge >= left_edge && right_edge <= 15;
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
        (addr < MAX_BUS_ADDR) && (addr < max_user_addr);
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
          [128'b0:128'b0] := 1,
          [{127'b0, 1'b1}:{4'b0101, 124'b0}] :/ 39,
          [{4'b0101, 122'b0, 2'b01}:{4'b0101, ~122'b0, 2'b01}] :/ 10, 
          [{4'b1010, 122'b0, 2'b10}:{4'b1010, ~122'b0, 2'b10}] :/ 10,
          [{4'b1010, ~124'b0}:{~127'b0, 1'b0}] :/ 39,
          [~128'b0:~128'b0] := 1
        };
      }      
    endclass
  
    initial begin
      T t = new;
      
      for (int i = 0 ; i < 10; i ++) begin
        t.randomize();
        
        $display("************New random values************");
        $display("MAX_BUS_ADDR = %0b", t.MAX_BUS_ADDR);
        $display("max_user_addr = %0b", t.max_user_addr);
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
# KERNEL: max_user_addr = 10011100010000
# KERNEL: mode = read
# KERNEL: status = UNDEF
# KERNEL: addr = 101101100000
# KERNEL: low_strob_index = 0
# KERNEL: left_edge = 11
# KERNEL: right_edge = 12
# KERNEL: strob = 0001100000000000
# KERNEL: data = 11000011001000001011110001001111100101101011100001001010010110111101001011110011000001110100011010011011101111100111011010000100
# KERNEL: ************New random values************
# KERNEL: MAX_BUS_ADDR = 11111111111111111111111111111111
# KERNEL: max_user_addr = 10011100010000
# KERNEL: mode = read
# KERNEL: status = UNDEF
# KERNEL: addr = 10001101101000
# KERNEL: low_strob_index = 0
# KERNEL: left_edge = 10
# KERNEL: right_edge = 10
# KERNEL: strob = 0000010000000000
# KERNEL: data = 01011101100000000111000110111010111111110011010011110000100010110101001110110110111110000100110110101111000001100101110100010110
# KERNEL: ************New random values************
# KERNEL: MAX_BUS_ADDR = 11111111111111111111111111111111
# KERNEL: max_user_addr = 10011100010000
# KERNEL: mode = read
# KERNEL: status = UNDEF
# KERNEL: addr = 1110101010000
# KERNEL: low_strob_index = 0
# KERNEL: left_edge = 10
# KERNEL: right_edge = 14
# KERNEL: strob = 0111110000000000
# KERNEL: data = 00001000010111100001001000110011101111100111000000101011110011100001100110010111001011110110011001110110011110100001010111000101
# KERNEL: ************New random values************
# KERNEL: MAX_BUS_ADDR = 11111111111111111111111111111111
# KERNEL: max_user_addr = 10011100010000
# KERNEL: mode = await
# KERNEL: status = UNDEF
# KERNEL: addr = 1110011110000
# KERNEL: low_strob_index = 0
# KERNEL: left_edge = 13
# KERNEL: right_edge = 13
# KERNEL: strob = 0010000000000000
# KERNEL: data = 10111000100101100110001001100001111010100010110011100001111000111100001001011100000010000011000110000111001110011110010100111010
# KERNEL: ************New random values************
# KERNEL: MAX_BUS_ADDR = 11111111111111111111111111111111
# KERNEL: max_user_addr = 10011100010000
# KERNEL: mode = read
# KERNEL: status = UNDEF
# KERNEL: addr = 11111011000
# KERNEL: low_strob_index = 0
# KERNEL: left_edge = 4
# KERNEL: right_edge = 14
# KERNEL: strob = 0111111111110000
# KERNEL: data = 11110011001010111101101010101110100010100110101111010000010101000001100000001010101000101110101110100011001111101101110110011110
# KERNEL: ************New random values************
# KERNEL: MAX_BUS_ADDR = 11111111111111111111111111111111
# KERNEL: max_user_addr = 10011100010000
# KERNEL: mode = await
# KERNEL: status = UNDEF
# KERNEL: addr = 10000110110111
# KERNEL: low_strob_index = 7
# KERNEL: left_edge = 8
# KERNEL: right_edge = 10
# KERNEL: strob = 0000011100000000
# KERNEL: data = 10111100001111001111010100100101011110011000010010100110111010101111010011011111000101001011001011010100000101000010111001000000
# KERNEL: ************New random values************
# KERNEL: MAX_BUS_ADDR = 11111111111111111111111111111111
# KERNEL: max_user_addr = 10011100010000
# KERNEL: mode = read
# KERNEL: status = UNDEF
# KERNEL: addr = 1110011111000
# KERNEL: low_strob_index = 0
# KERNEL: left_edge = 0
# KERNEL: right_edge = 1
# KERNEL: strob = 0000000000000011
# KERNEL: data = 11110001110001001011111011000001001110011110010101010001001010110100010000010110001001111011110111100101010111000110001100110100
# KERNEL: ************New random values************
# KERNEL: MAX_BUS_ADDR = 11111111111111111111111111111111
# KERNEL: max_user_addr = 10011100010000
# KERNEL: mode = await
# KERNEL: status = UNDEF
# KERNEL: addr = 1011011111101
# KERNEL: low_strob_index = 5
# KERNEL: left_edge = 5
# KERNEL: right_edge = 11
# KERNEL: strob = 0000111111100000
# KERNEL: data = 10110011111001010111001000001100010110100101101101001100101001010110111011000011100001010011010100110011110100001011011111100001
# KERNEL: ************New random values************
# KERNEL: MAX_BUS_ADDR = 11111111111111111111111111111111
# KERNEL: max_user_addr = 10011100010000
# KERNEL: mode = read
# KERNEL: status = UNDEF
# KERNEL: addr = 101011001000
# KERNEL: low_strob_index = 0
# KERNEL: left_edge = 10
# KERNEL: right_edge = 15
# KERNEL: strob = 1111110000000000
# KERNEL: data = 10110111101111011101010001111101111000111101000001100000111110111111101110001001100110110000110101011010110010010011001110101111
# KERNEL: ************New random values************
# KERNEL: MAX_BUS_ADDR = 11111111111111111111111111111111
# KERNEL: max_user_addr = 10011100010000
# KERNEL: mode = write
# KERNEL: status = OK
# KERNEL: addr = 110101001
# KERNEL: low_strob_index = 1
# KERNEL: left_edge = 2
# KERNEL: right_edge = 14
# KERNEL: strob = 0111111111111100
# KERNEL: data = 10110001101110100000111110010010110011100001111111011010000101001110001110001110000001000110101111101110011101111100111101101110
```
