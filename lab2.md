####[Task1](#t1) - [pgd](https://www.edaplayground.com/x/5tVQ)

> Записать ограничения на рандомизацию параметров транзакции в соответствии с требованиями спецификации:
- [x] Транзакция содержит поля data, addr, strob, mode, status
- [x] Ширина данных 128 бита (data)
- [x] Ширина адреса 32 бита (addr)
- [x] Ширина строба (strob) равна числу байт в шине данных
- [x] Поле mode имеет четыре значения Чтение, Запись, Ожидание, Ошибка
- [ ] Поле strob имеет только непрерывное заполнение единицами, т.е. значения 0000, 1010, 1101 и др. недопустимы
- [x] Поле data принимает значения 0101….01 и 1010….10 с вероятностью 10%
- [x] Поле data принимает максимальное и минимальное значения с вероятностью 1%
- [x] Поле addr меньше переменной max_user_addr и меньше максимально возможного MAX_BUS_ADDR
- [ ] addr должен иметь значения адресующие только по 8 байт, если значение поля mode равно Чтение
- [ ] Заполнение 1 поля strob должно начинаться с бита, номер которого равен значению младших 3-разрядов адреса.
- [x] Поле статус всегда равно OK, если mode равен Запись, иначе UNDEF

```systemverilog
module top;
  	class T;
      bit [31:0] MAX_BUS_ADDR = '1;
      bit [31:0] max_user_addr = 2;
      
      rand bit [127:0] data;
      rand bit [31:0] addr;
      rand bit [7:0] strob [];
      rand enum {read, write, await, error} mode;
      rand enum {OK, UNDEF} status;

      constraint strob_size {
        strob.size inside {$size(data) / 8};
      }
      
      constraint addr_value {
        (addr < MAX_BUS_ADDR) && (addr < max_user_addr);
      }
      
      constraint status_value {
        (mode == write) -> (status == OK);
        (mode != write) -> (status == UNDEF);
      }
      
      constraint data_chance {
        data dist {          
          [128'b0:128'b0] := 1,
          [{4'b0101, 122'b0, 2'b01}:{4'b0101, ~122'b0, 2'b01}] :/ 10, 
          [{4'b1010, 122'b0, 2'b10}:{4'b1010, ~122'b0, 2'b10}] :/ 10,
          [~128'b0:~128'b0] := 1
        };
      }      
    endclass
endmodule
```
