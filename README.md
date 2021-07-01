# parallel-cpp
## Задачи
| Название           | День | Баллы  |
| ------------------ |:----:| ------:|
| is_prime           |  0   |    -/0 |
| hello_world        |  1   |    -/1 |
| deadlock           |  1   |    -/1 |
| philosophers       |  1   |    -/1 |
| reduce             |  1   |    -/1 |
| thread_safe_queue  |  2   |    -/1 |
| thread_safe_vector |  2   |    -/1 |
| unbuffered_channel |  2   |    -/2 |
| spin_lock          |  3   |    -/1 |
| ticket_lock        |  3   |    -/1 |
| mcs_lock           |  3   |    -/1 |
| rw_spin_lock       |  3   |    -/2 |
| mpsc_queue         |  4   |    -/2 |
| mpmc_queue         |  4   |    -/2 |
| Сумма              |  -   |   0/17 |



## Как начать работать?
Для начала необходимо установить `git`, `cmake` и `clang`:
### Ubuntu
`$ sudo apt update && sudo apt install git cmake clang`
### MacOS
`$ brew install git cmake`

Скачать проект:
```
$ git clone git@github.com:<login>/parallel-cpp.git
$ cd parallel-cpp
```

Собрать проект:
```
$ mkdir build
$ cd build
$ [СС="clang" CXX="clang++"] cmake ..
$ make [-j [N]]
```
CMake собирает информацию о проекте и подготавливает правила компиляции и линковки для компилятора. `СС="clang" CXX="clang++"` необходимо писать в Ubuntu для того, чтобы проект собирался именно clang, а не gcc. В MacOS используется clang по умолчанию.  
Make непосредственно компилирует и собирает проект. Параметр `-j [N]` говорит `make` о том, что проект должен собираться параллельно. Вместо `N` необходимо указать количество параллельных работ (или ничего не указывать для максимальной параллельности).  
**Make необходимо запускать после изменения файлов и перед запуском тестов. СMake достаточно запустить один раз.**

Также можно собрать проект с address или thread санитайзером (для ASAN аналогично):
```
$ mkdir tsan_build
$ cd tsan_build
$ [СС="clang" CXX="clang++"] cmake -DTSAN ..
$ make [-j [N]]
```

## Запуск тестов
**Тесты не являются гарантией того, что задача будет засчитана на полный балл. Они лишь помогают убедиться в том, что ваше решение хоть насколько-то верное.**

Для запуска всех тестов из директории `build` нужно выполнить команду `ctest`:
```
$ cd build
$ ctest
```

Чтобы запустить тесты конкретной задачи, нужно выполнить соответствующий файл.  
Например, для задачи `is_prime`:
```
$ cd build
$ ./is_prime/is_prime
```
Или же можно запустить конкретный тест:  
`$ ./is_prime/is_prime --gtest-filter="IsPrime.One"`  
Или:  
`$ ./is_prime/is_prime --gtest-filter="*One"` 

### ASAN и TSAN
Тесты проверяют, что ваш код выдает ожидаемый результат, однако они не проверяют, например, наличие data race в программе. Для этого есть ASAN и TSAN сборки.  
Чтобы убедиться, что вы все сделали верно, запустите тесты еще и для этих двух сборок. Запуск тестов аналогичен, но запускать их нужно из директорий `asan_build` и `tsan_build`.  
Обратите внимание, что в этих сборках код может работать в несколько раз медленнее - это нормально.

### Timeout
У каждого теста есть timeout, после которого кидается исключение с текстом "Timeout!". Если вы считаете, что все сделали правильно, но тест не проходит по времени, попробуйте увеличить/убрать этот лимит в файле `test.cpp` у соответствующего теста.  
Для ASAN и TSAN сборок timeout'ы выключены.
