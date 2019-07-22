# Fabenial Core [ru]

[Английская версия README](https://github.com/fabenial/FabenialCoin/blob/master/README.md)

## В кратце

Рад приветствовать вас в главном репозитории Fabenial Coin. Здесь вы найдёте исходный код проекта, инструкции и подсказки.

Contents
* Сборка на Linux (В качестве примера взята Ubuntu 18.04.2 LTS x86_64)
* Сборка на Mac OSX
* Сборка на Windows
* Сборка на других платформах

## Сборка на Linux

Данные команды расчитаны на ОС Ubuntu, на других дистрибутивах команды могут оличаться. Если у Вас не получется собрать код на каком либо дистрибутиве, то буду рад помочь решить эту проблему.

### Сборка со стандартными опциями

Создайте директорию `fabenial` и перейдите в неё:
```
$> mkdir fabenial
$> cd fabenial
```

Убедитесь, что в системи имеются необходимые инструменты. Версия компилятора gcc должна быть не ниже чем 5.4.

* Установка `build-essential`:
    ```
    $> sudo apt-get install build-essential
    ```

* Установка CMake (3.5 необходима версия 3.5 или выше):
    ```
    $> sudo apt-get install cmake
    $> cmake --version
    ```
    Если не получается установить требуемую версию, то помощь можно найти на [оффициальном сайте](https://cmake.org/download/).


* Этот метод установки Boost необходим если **не получается** стандартными методами установить версию выше 1.65:
    Все операции надо пробелывать в директории  `fabenial`. 
    ```
    $fabenial> wget -c 'https://dl.bintray.com/boostorg/release/1.65.1/source/boost_1_65_1.tar.gz'
    $fabenial> tar xf boost_1_65_1.tar.gz
    $fabenial> mv boost_1_65_1 boost
    $fabenial> cd boost
    $fabenial/boost> ./bootstrap.sh
    $fabenial/boost> ./b2 link=static cxxflags="-fPIC" linkflags="-pie" -j 8 --build-dir=build64 --stagedir=stage
    cd ..
    ```

Git-clone (или git-pull) исходный код монеты Fabenial в эту директориию:
```
$fabenial> git clone https://github.com/fabenial/FabenialCoin fabenial_core
```

Положите код LMDB в директориюю `fabenial`:
```
$fabenial> git clone https://github.com/fabenial/lmdb.git
```

Создайте директорию `build` внутри `fabenial`, перейдите в неё, а после соберите проект:
```
$fabenial> mkdir fabenial_core/build
$fabenial> cd fabenial_core/build
$fabenial/fabenial_core/build> cmake ..
$fabenial/fabenial_core/build> time make -j4
```



### Сборка со специальными опциями

Установите OpenSSL в `fabenial/openssl`. (Используйте замену `linux-x86_64-clang` на `linux-x86_64` если используете clang.)
```
$fabenial> git clone https://github.com/openssl/openssl.git
$fabenial> cd openssl
$fabenial/openssl> ./Configure linux-x86_64 no-shared
$fabenial/openssl> time make -j4
$fabenial/openssl> cd ..
```

Загрузите [SQLite 3](https://www.sqlite.org/download.html) и распакуйте в `fabenial/sqlite`.

Команды представленые ниже включают или отключают использование OpenSSL, LMDB или SQLite для CMake:

```
$fabenial> mkdir fabenial_core/build
$fabenial> cd fabenial_core/build
$fabenial/fabenial_core/build> cmake -DUSE_SSL=1 -DUSE_SQLITE=1 ..
$fabenial/fabenial_core/build> time make -j4
```

## Сборка на Mac OSX

### Стандартная сборка (10.11 El Capitan или новее)

Вам необходимы инструменты командной строки. Для этого установите XCode из магазина приложений или исполните 'xcode-select --install' в терминале и следуйте дальнейшим инструкциям. Первым делом необходимо установить [Homebrew](https://brew.sh).

Теперь откройте терминал и установите CMake и Boost:

* `brew install cmake`
* `brew install boost`

Создайте директорию `fabenial` и перейдите в неё:
```
$~/Downloads> mkdir <path-to-fabenial-folder>
$~/Downloads> cd <path-to-fabenial-folder>
```

Git-clone (или git-pull) исходный код монеты Fabenial в эту директориию:
```
$fabenial> git clone https://github.com/fabenial/FabenialCoin fabenial_core
```

Положите код LMDB в директориюю `fabenial`:
```
$fabenial> git clone https://github.com/fabenial/lmdb.git
```

Создайте директорию `build` внутри `fabenial`, перейдите в неё, а после соберите проект:
```
$fabenial> mkdir fabenial_core/build
$fabenial> cd fabenial_core/build
$fabenial/fabenial_core/build> cmake -DUSE_SSL=0 ..
$fabenial/fabenial_core/build> time make -j4
```

### Сборка со специальными опциями

Файлы собранные на вашем компьютере будут работать только на нём, в случае исли вы хотите что бы они работали везде, вам нужно будет использовать El Capitan SDK.

Загрузите [Mac OSX 10.11 SDK](https://github.com/phracker/MacOSX-SDKs/releases) и распакуйте его в `Downloads`.

Загрузите и распакуйте [Boost](https://boost.org) в `Downloads`.

Теперь установите Boost:
```
$~> cd ~/Downloads/boost_1_67_0/
$~/Downloads/boost_1_67_0> ./bootstrap.sh
$~/Downloads/boost_1_67_0> ./b2 -a -j 4 cxxflags="-stdlib=libc++ -std=c++14 -mmacosx-version-min=10.11 -isysroot/Users/user/Downloads/MacOSX10.11.sdk -fPIC" linkflags="-pie" install`
```

Установите OpenSSL в `fabenial/openssl` :
```
$~/Downloads/fabenial> git clone https://github.com/openssl/openssl.git
$~/Downloads/fabenial> cd openssl
```

Если вы хотите что бы файлы запускались на системах начиная от El Capitan, то сборку нужно ориентировать под El Capitan SDK.
```
$fabenial/openssl> ./Configure darwin64-x86_64-cc no-shared -mmacosx-version-min=10.11 -isysroot/Users/user/Downloads/MacOSX10.11.sdk
```
В противном случае используйте:
```
$fabenial/openssl> ./Configure darwin64-x86_64-cc no-shared
```
Сборка OpenSSL:
```
$fabenial/openssl> time make -j4
$fabenial/openssl> cd ..
```

Загрузите и распакуйте [SQLite 3](https://www.sqlite.org/download.html) в `fabenial/sqlite`.

Добавить поддержку OpenSSL, LMDB или SQLite:

```
$fabenial> mkdir fabenial_core/build
$fabenial> cd fabenial_core/build
$fabenial/fabenial_core/build> cmake -DUSE_SSL=1 -DUSE_SQLITE=1 ..
$fabenial/fabenial_core/build> time make -j4
```

## Сборка для Windows

Установите Microsoft Visual Studio Community 2017. [Download](https://www.visualstudio.com/vs/) с опциями `C++`, `git`, `cmake integration`.
Запустите `Visual Studio x64 command prompt`.

Создайте директорию `fabenial` и перейдите в неё:
```
$C:\> mkdir fabenial
$C:\> cd fabenial
```

Загрузите [Boost](https://boost.org), распакуйте его внутри `fabenial` и переименуйте `boost_*_**_*` в `boost`.

Соберите (собирайте 32-битную версию boost только в том случае если нужны 32-битные бинарные файлы монеты).
```
$> cd boost
$C:\fabenial\boost> bootstrap.bat
$C:\fabenial\boost> b2.exe address-model=64 link=static -j 8 --build-dir=build64 --stagedir=stage
$C:\fabenial\boost> b2.exe address-model=32 link=static -j 8 --build-dir=build32 --stagedir=stage32
cd ..
```

Git-clone (или git-pull) исходный код монеты Fabenial в эту директориию:
```
$C:\fabenial> git clone https://github.com/fabenial/FabenialCoin fabenial_core
```

Положите код LMDB в директориюю `fabenial`:
```
$C:\fabenial> git clone https://github.com/fabenial/lmdb.git
```

Вам необходимо установить openssl, перед этим потребуется установить ActivePerl (выберете "add to PATH" опцию и перезапустите консоль ):
```
$C:\fabenial> git clone https://github.com/openssl/openssl.git
$C:\fabenial> cd openssl
$C:\fabenial\openssl> perl Configure VC-WIN64A no-shared no-asm
$C:\fabenial\openssl> nmake
$C:\fabenial\openssl> cd ..
```
Если вам нужны 32-битные бинарные файлы, то потребуется собрать 32-битную версию openssl:
```
$C:\fabenial> git clone https://github.com/openssl/openssl.git openssl32
$C:\fabenial> cd openssl32
$C:\fabenial\openssl> perl Configure VC-WIN32 no-shared no-asm
$C:\fabenial\openssl> nmake
$C:\fabenial\openssl> cd ..
```

Запустите Visual Studio, и в меню File выберете `Open Folder`, затем откройте `C:\fabenial\fabenial_core` folder.
Дождитесь загрузки CMake finishes running and `Build` appears in main menu.
Select `x64-Debug` or `x64-Release` from standard toolbar, and then `Build/Build Solution` from the main menu.

You cannot add options to CMake running inside Visual Studio so just edit `CMakeLists.txt` and set `USE_SSL` or `USE_SQLITE` to `ON` if you wish to build with them.

## Building on 32-bit x86 platforms, iOS, Android and other ARM platforms

Fabenial Core works on 32-bit systems if SQLite is used instead of LMDB (we've experienced lots of problems building and running with lmdb in 32-bit compatibility mode, especially on iOS).

Therefore SQLite option is automatically selected by CMake on 32-bit platforms and you must have SQLite downloaded as explained in appropriate sections above.

We build official x86 32-bit version for Windows only, because there is zero demand for 32-bit version for Linux or Mac.

Building source code for iOS, Android, Raspberry PI, etc is possible but requires major skills on your part. __TBD__

## Building on Big-Endian platforms

Currently it is impossible to run Fabenial Core on any Big-Endian platform, due to lots of endianess-dependent code. This may be fixed in the future. If you wish to run on Big-Endian platform, please contact us.

## Building with parameters

If you want to use tools like `clang-tidy`, run `cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=ON ..` instead of `cmake ..`
