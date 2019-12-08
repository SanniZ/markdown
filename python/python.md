# **Python 拾遗**  

---------------------------------------------

## **密码输入**  

### *使用getpass模块实现密码输入*  
```
import getpass
pwd = getpass.getpass('Input your passwd:')
```

---------------------------------------------

## **编译出.pyc**  
```
import py_compile
py_compile.compile('xxx/xxx.py')
```

---------------------------------------------

## **python调用C语言编译出来的.so**  
```
# main.py

import os
import sys
import getpass
from ctypes import cdll, c_char_p, create_string_buffer

LIBKEY = os.path.abspath('libkey.so')

if __name__ == '__main__':
    if not os.path.exists(LIBKEY):
        print("No found %s" % LIBKEY)
        sys.exit()
    lib = cdll.LoadLibrary(LIBKEY)
    lib.get_key.argtype = (c_char_p)
    lib.get_key.restype = (c_char_p)
    passwd = create_string_buffer(16)
    passwd.raw = str(getpass.getpass("Input your pwd:")).encode()
    key = lib.get_key(passwd)
    print('get key: ', key)

    //需要注意设定数据类型对齐: c_char_p -> char * , c_char -> char ...

---------------------------

// func.c

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define PWD "passwd123"
#define KEY "TxT-00-^&^-_-key"

char* get_key(char *pwd) {
    if (!strcmp(pwd, PWD))
        return KEY;
    else
	    return NULL;
}

make: gcc -o libkey.so func.c -share -fPIC -O -Wall

// Ref to: https://github.com/SanniZ/essentials/tree/master/python/c2py/
```



---------------------------------------------

## **C语言调用python脚本**  
```
# func.py

VERSION='1.0.0'

def testFunc(args):
    print(args)
    return "Hi main!"

-----------------------------------

// main.c


int run_python(void) {
    PyObject *pModule, *pFunc, *pArgs, *pVer, *pRet;
    char *pVersion = NULL;

    Py_Initialize();
    if (!Py_IsInitialized()) {
        printf("it is failed to initialize python!");
        return -1;
    }

    pModule = PyImport_ImportModule("func");
    if (!pModule) {
        printf("it is failed to import module");
        goto err;
    }

    pFunc = PyObject_GetAttrString(pModule, "testFunc");
    if (!pFunc) {
        printf("it is failed to get func");
        goto err; 
    }

    pVer = PyObject_GetAttrString(pModule, "VERSION");
    if (!pVer) {
        printf("it is failed to get version");
        goto err; 
    } else {
        PyArg_Parse(pVer, "s", &pVersion);
        printf("Version: %s\n", pVersion);
    }

    pArgs = PyTuple_New(1);
    PyTuple_SetItem(pArgs, 0, Py_BuildValue("s", "Say Hello to func: Hi testFunc!"));

    pRet = PyEval_CallObject(pFunc, pArgs);
    if (pRet) {
        char *result = NULL;
        PyArg_Parse(pRet, "s", &result);
        printf("get return from testFunc: %s\n", result);
    }

    Py_DECREF(pModule);
    Py_DECREF(pFunc);
    Py_DECREF(pArgs);
    Py_DECREF(pVer);
    Py_DECREF(pRet);

    Py_Finalize();

    return 0;

err:

    Py_Finalize();
    return -1;
}

make: gcc -o main main.c -L/usr/lib/python3.5 -lpython3.5m -ldl -Wall -O

// Ref to: https://github.com/SanniZ/essentials/tree/master/python/py2c/
```

---------------------------------------------