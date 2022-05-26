# Выполнил ВЕЧЕРИНИН А.А. группа ИУ8-22 
## Laboratory work III

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.

```shell
$ cat > CMakeLists.txt <<EOF
> cmake_minimum_required(VERSION 3.22)
> project (hw3)
> 
> set(CMAKE_CXX_STANDARD 11)
> set(CMAKE_CXX_STANDARD_REQUIRED ON)
> 
> add_library(formatter STATIC formatter.cpp)
>
> EOF

```
Сборка
```shell
$ cmake -H. -B_build
-- Configuring done
-- Generating done
-- Build files have been written to: /home/leonard/TIMP/hw3_/formatter_lib/_build

$ cmake --build _build
[ 50%] Building CXX object CMakeFiles/formatter.dir/formatter.cpp.o
[100%] Linking CXX static library libformatter.a
[100%] Built target formatter

```
Commit, push
```shell
git add .
git commit -m "libr formatter_lib"
git push origin master
```

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.

В formatter_ex.cpp нужно указать путь к файлу formatter.h
```shell
$ nano formatter_ex.cpp
#include "../formatter_lib/formatter.h"
```
Создание CMakeList.txt
```shell
$ cat > CMakeLists.txt <<EOF
> cmake_minimum_required(VERSION 3.22)
> project(hw3_1)
> 
> set(CMAKE_CXX_STANDART 11)
> set(CMAKE_CXX_STANDART_REQUIRED ON)
> 
> include_directories("../formatter_lib")
>
> add_library(formatter_ex_lib STATIC formatter_ex.cpp)
>
> target_link_libraries(formatter_ex_lib PUBLIC formatter_lib)
> EOF
```
Билд и линковка
```shell
$ cmake -H. -B _build
-- The C compiler identification is GNU 11.2.0
-- The CXX compiler identification is GNU 11.2.0
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
-- Build files have been written to: /home/leonard/TIMP/hw3_/formatter_ex_lib/_build
$ cmake --build _build
[ 50%] Building CXX object CMakeFiles/formatter_ex_lib.dir/formatter_ex.cpp.o
[100%] Linking CXX static library libformatter_ex_lib.a
[100%] Built target formatter_ex_lib

```
Commit, push
```shell
$ git add .
$ git commit -m "lib formatter_ex"
$ git push origin master
```
### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;

Создание CMakeLists.txt в директории hello_world_application
```shell
$ cat > CMakeLists.txt <<EOF
> cmake_minimum_required(VERSION 3.22)
> project(hw3)
>
> set(CMAKE_CXX_STANDART 11)
> set(CMAKE_CXX_STANDART_REQUIRED ON)
>
> add_library(hello_world_application hello_world.cpp)
>
> include_directories("../formatter_ex_lib")
> EOF
```
Билд
```shell
$ cmake -H. -B_build
-- The C compiler identification is GNU 11.2.0
-- The CXX compiler identification is GNU 11.2.0
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
-- Build files have been written to: /home/leonard/TIMP/hw3_/hello_world_application/_build
$ cmake --build _build
[ 50%] Building CXX object CMakeFiles/hello_world_application.dir/hello_world.cpp.o
[100%] Linking CXX static library libhello_world_application.a
[100%] Built target hello_world_application

```
* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.

Создание CMakeLists.txt в директории solver_lib

```shell
$ cat>CMakeLists.txt<<EOF
>cmake_minimum_required(VERSION 3.22)
>project (solver_lib)
>
>set(CMAKE_CXX_STANDARD 11)
>set(CMAKE_CXX_STANDARD_REQUIRED ON)
>
>add_library(formatter STATIC ${CMAKE_CURRENT_SOURCE_DIR}/solver.h ${CMAKE_CURRENT_SOURCE_DIR}/solver.cpp)
>EOF
```
Билд
```shell
$ cmake -H. -B_build
-- Configuring done
-- Generating done
-- Build files have been written to: /home/leonard/TIMP/hw3_/solver_lib/_build
$ cmake --build _build
[ 50%] Building CXX object CMakeFiles/formatter.dir/solver.cpp.o
[100%] Linking CXX static library libformatter.a
[100%] Built target formatter


```
Создание CMakeLists.txt в директории solver_application

```shell
$ cat>CMakeLists.txt<<EOF
>cmake_minimum_required(VERSION 3.22)
>project(lab03)
>
>set(CMAKE_CXX_STANDARD 11)
>set(CMAKE_CXX_STANDARD_REQUIRED ON)
>
>include_directories("../formatter_lib")
>include_directories("../formatter_ex_lib")
>include_directories("../solver_lib")
>add_library(formatter_lib STATIC "../formatter_lib/formatter.cpp")
>add_library(formatter_ex_lib STATIC "../formatter_ex_lib/formatter_ex.cpp")
>add_library(solver_lib STATIC "../solver_lib/solver.cpp")
>
>
>
>
>add_executable(lab03 "equation.cpp")
>target_link_libraries(lab03 solver_lib formatter_ex_lib formatter_lib)
>EOF
```

Билд
```shell
$ cmake -H. -B_build
-- Configuring done
-- Generating done
-- Build files have been written to: /home/leonard/TIMP/hw3_/solver_application/_build

$ cmake --build _build
[ 12%] Building CXX object CMakeFiles/formatter_lib.dir/home/leonard/TIMP/hw3_/formatter_lib/formatter.cpp.o
[ 25%] Linking CXX static library libformatter_lib.a
[ 25%] Built target formatter_lib
[ 37%] Building CXX object CMakeFiles/formatter_ex_lib.dir/home/leonard/TIMP/hw3_/formatter_ex_lib/formatter_ex.cpp.o
[ 50%] Linking CXX static library libformatter_ex_lib.a
[ 50%] Built target formatter_ex_lib
[ 62%] Building CXX object CMakeFiles/solver_lib.dir/home/leonard/TIMP/hw3_/solver_lib/solver.cpp.o
[ 75%] Linking CXX static library libsolver_lib.a
[ 75%] Built target solver_lib
[ 87%] Building CXX object CMakeFiles/lab03.dir/equation.cpp.o
[100%] Linking CXX executable lab03
[100%] Built target lab03

```
commit, push
```shell
$ git add .
$ git commit -m "fina"
$ git push origin master
```

```
Copyright (c) 2015-2021 The ISC Authors
```
