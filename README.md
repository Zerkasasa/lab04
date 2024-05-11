## Laboratory work III

Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake**

## Homework

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.
```bush

$ cmake --version
cmake version 3.22.1

```

```bush

cd formatter_lib
cat >> CMakeLists.txt << EOF
>cmake_minimum_required(VERSION 3.22.1)
>
>project(formatter_lib)
>
>set(CMAKE_CXX_STANDARD 20)
>set(CMAKE_CXX_STANDARD_REQUIRED ON)
>
>add_library(formatter_lib STATIC \${CMAKE_CURRENT_SOURCE_DIR}/formatter.cpp)
> 
> include_directories(\${CMAKE_CURRENT_SOURCE_DIR})
> 
> EOF

cmake -H. -B build

```

## Вывод:

```bush
-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/vboxuser/Zerkasasa/workspace/projects/lab03/formatter_lib/build

```
## Файл CMakeLists

```
cmake_minimum_required(VERSION 3.22.1)

project(formatter_lib)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(formatter_lib STATIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter.cpp)

include_directories(${CMAKE_CURRENT_SOURCE_DIR})

```

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.

```bush
cd ../formatter_ex_lib
 cat>> CMakeLists.txt << EOF
cmake_minimum_required(VERSION 3.22.1)

project(formatter_ex_lib)

set(CMAKE_CXX_STANDART 20)
set(CMAKE_CXX_STANDART_REQUIRED ON)
set(CMAKE_CURRENT_SOURCE_DIR /home/vboxuser/Zerkasasa/workspace/projects/lab03/formatter_ex_lib)

add_library(formatter_ex STATIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex.cpp)

include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib)

target_link_libraries(formatter_ex formatter)
> EOF

cmake -H. -B build

```

## Вывод:

```bush
- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/vboxuser/Zerkasasa/workspace/projects/lab03/formatter_e_lib/build

```


## Файл CMakeLists

```
cmake_minimum_required(VERSION 3.22.1)

project(formatter_ex_lib)

set(CMAKE_CXX_STANDART 20)
set(CMAKE_CXX_STANDART_REQUIRED ON)
set(CMAKE_CURRENT_SOURCE_DIR /home/vboxuser/Zerkasasa/workspace/projects/lab03/formatter_ex_lib)

add_library(formatter_ex STATIC /formatter_ex.cpp)

include_directories()
include_directories(/formatter_lib)

target_link_libraries(formatter_ex formatter)

```


### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;


```bush
cd ../hello_world_application

cat>> CMakeLists.txt << EOF
> cmake_minimum_required(VERSION 3.22.1)
project(hello_world)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(hello_world hello_world.cpp)

add_library(formatter_lib STATIC ../formatter_lib/formatter.cpp)
add_library(formatter_ex_lib STATIC ../formatter_ex_lib/formatter_ex.cpp)


target_include_directories(formatter_lib PUBLIC ../formatter_lib)
target_include_directories(formatter_ex_lib PUBLIC ../formatter_ex_lib ../formatter_lib)
target_include_directories(hello_world PUBLIC ../formatter_ex_lib ../formatter_lib)

target_link_libraries(hello_world formatter_ex_lib formatter_lib)
> EOF


cmake -H. -B build

```

## Вывод:

```bush
-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/vboxuser/Zerkasasa/workspace/projects/lab03/hello_world_application/build


```

## Файл CMakeLists

```

cmake_minimum_required(VERSION 3.22.1)
project(hello_world)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(hello_world hello_world.cpp)

add_library(formatter_lib STATIC ../formatter_lib/formatter.cpp)
add_library(formatter_ex_lib STATIC ../formatter_ex_lib/formatter_ex.cpp)


target_include_directories(formatter_lib PUBLIC ../formatter_lib)
target_include_directories(formatter_ex_lib PUBLIC ../formatter_ex_lib ../formatter_lib)
target_include_directories(hello_world PUBLIC ../formatter_ex_lib ../formatter_lib)

target_link_libraries(hello_world formatter_ex_lib formatter_lib)

```

* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.

```bush
cd ../hello_world_application

cat >> ./CMakeLists.txt << EOF
> cmake_minimum_required(VERSION 3.22.1)

project(solver)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)



add_library(formatter_lib STATIC ../formatter_lib/formatter.cpp)
add_library(formatter_ex_lib STATIC ../formatter_ex_lib/formatter_ex.cpp)
add_library(solver_lib STATIC ../solver_lib/solver.cpp)

include_directories(    ../formatter_lib
                        ../formatter_ex_lib
                        ../solver_lib)


add_executable(solver equation.cpp)

target_link_libraries(solver    solver_lib 
                                formatter_ex_lib 
                                formatter_lib)
> EOF

cmake -H. -B build

```

## Вывод:

```bush
-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/vboxuser/Zerkasasa/workspace/projects/lab03/solver_application/build


```

## Файл CMakeLists

```

cmake_minimum_required(VERSION 3.22.1)

project(solver)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)



add_library(formatter_lib STATIC ../formatter_lib/formatter.cpp)
add_library(formatter_ex_lib STATIC ../formatter_ex_lib/formatter_ex.cpp)
add_library(solver_lib STATIC ../solver_lib/solver.cpp)

include_directories(	../formatter_lib
			../formatter_ex_lib
			../solver_lib)


add_executable(solver equation.cpp)

target_link_libraries(solver 	solver_lib 
				formatter_ex_lib 
				formatter_lib)

```
