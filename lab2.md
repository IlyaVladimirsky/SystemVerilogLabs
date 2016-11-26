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
