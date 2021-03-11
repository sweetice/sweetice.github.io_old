---
title: 'How to installation mpi4py in your ubuntu'
date: 2019-03-18
permalink: /posts/2019/03/install-mpi4py/
tags:
  - skills
---

## How to install mpi4py in your ubuntu computer


首先要说明的是，直接使用pip　安装并不可取．

## 1. 安装openmpi

- 1.1 下载
URL: http://www.open-mpi.org/software/ompi/v1.10/

```
wget https://www.open-mpi.org/software/ompi/v1.10/downloads/openmpi-1.10.2.tar.gz
tar xvzf openmpi-1.10.x.tar.gz
cd openmpi-xxx/
```

- 1.2 编译安装

首先编译:  
```
./configure
```

注意：不要使用  
```
bash ./configure
```

接下来安装:
```
sudo make all install
```


注意:这里必须要使用sudo,否则会提示你权限不足

- 1.3 添加环境变量

进入~目录后
```
sudo gedit .bashrc
```

在最后一行添加上:
```
#mpi4py
export LD_LIBRARY_PATH+=:/usr/local/lib
```

激活一下:  
```
source /etc/profile
```

- 1.4 进行测试

```
cd openmpi-1.10.2/examples
make
mpirun -np 4 hello_c
```

当你的电脑上面显示:
```
Hello, world, I am 0 of 4, (Open MPI v1.10.2, package: Open MPI mirror@agent Distribution, ident: 1.10.2, repo rev: v1.10.1-145-g799148f, Jan 21, 2016, 123)
Hello, world, I am 2 of 4, (Open MPI v1.10.2, package: Open MPI mirror@agent Distribution, ident: 1.10.2, repo rev: v1.10.1-145-g799148f, Jan 21, 2016, 123)
Hello, world, I am 3 of 4, (Open MPI v1.10.2, package: Open MPI mirror@agent Distribution, ident: 1.10.2, repo rev: v1.10.1-145-g799148f, Jan 21, 2016, 123)
Hello, world, I am 1 of 4, (Open MPI v1.10.2, package: Open MPI mirror@agent Distribution, ident: 1.10.2, repo rev: v1.10.1-145-g799148f, Jan 21, 2016, 123)

```
这就说明openmpi安装好了.  

## 2. 安装mpi4py

- 2.1 激活环境变量

```
source ~/.bashrc
```

- 2.2 安装mpi4py

```
pip install mpi4py
```

出现一下信息:

```
Looking in indexes: https://pypi.tuna.tsinghua.edu.cn/simple
Collecting mpi4py
  Using cached https://pypi.tuna.tsinghua.edu.cn/packages/55/a2/c827b196070e161357b49287fa46d69f25641930fd5f854722319d431843/mpi4py-3.0.1.tar.gz
Building wheels for collected packages: mpi4py
  Building wheel for mpi4py (setup.py) ... done
  Stored in directory: /home/mirror/.cache/pip/wheels/73/ef/7a/e81433083a06d8735f0b50e1e388168b39b88444fd81fe5f27
Successfully built mpi4py
Installing collected packages: mpi4py
Successfully installed mpi4py-3.0.1
You are using pip version 19.0.2, however version 19.0.3 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.

```
说明已经成功安装上了

- 2.3 检测是否成功安装

```
import mpi4py.MPI as MPI
```

之后再
```
dir(MPI)
```

出现
```
['AINT', 'ANY_SOURCE', 'ANY_TAG', 'APPNUM', 'Add_error_class', 'Add_error_code', 'Add_error_string', 'Aint_add', 'Aint_diff', 'Alloc_mem', 'Attach_buffer', 'BAND', 'BOOL', 'BOR', 'BOTTOM', 'BSEND_OVERHEAD', 'BXOR', 'BYTE', 'CART', 'CHAR', 'CHARACTER', 'COMBINER_CONTIGUOUS', 'COMBINER_DARRAY', 'COMBINER_DUP', 'COMBINER_F90_COMPLEX', 'COMBINER_F90_INTEGER', 'COMBINER_F90_REAL', 'COMBINER_HINDEXED', 'COMBINER_HINDEXED_BLOCK', 'COMBINER_HVECTOR', 'COMBINER_INDEXED', 'COMBINER_INDEXED_BLOCK', 'COMBINER_NAMED', 'COMBINER_RESIZED', 'COMBINER_STRUCT', 'COMBINER_SUBARRAY', 'COMBINER_VECTOR', 'COMM_NULL', 'COMM_SELF', 'COMM_TYPE_SHARED', 'COMM_WORLD', 'COMPLEX', 'COMPLEX16', 'COMPLEX32', 'COMPLEX4', 'COMPLEX8', 'CONGRUENT', 'COUNT', 'CXX_BOOL', 'CXX_DOUBLE_COMPLEX', 'CXX_FLOAT_COMPLEX', 'CXX_LONG_DOUBLE_COMPLEX', 'C_BOOL', 'C_COMPLEX', 'C_DOUBLE_COMPLEX', 'C_FLOAT_COMPLEX', 'C_LONG_DOUBLE_COMPLEX', 'Cartcomm', 'Close_port', 'Comm', 'Compute_dims', 'DATATYPE_NULL', 'DISPLACEMENT_CURRENT', 'DISP_CUR', 'DISTRIBUTE_BLOCK', 'DISTRIBUTE_CYCLIC', 'DISTRIBUTE_DFLT_DARG', 'DISTRIBUTE_NONE', 'DIST_GRAPH', 'DOUBLE', 'DOUBLE_COMPLEX', 'DOUBLE_INT', 'DOUBLE_PRECISION', 'Datatype', 'Detach_buffer', 'Distgraphcomm', 'ERRHANDLER_NULL', 'ERRORS_ARE_FATAL', 'ERRORS_RETURN', 'ERR_ACCESS', 'ERR_AMODE', 'ERR_ARG', 'ERR_ASSERT', 'ERR_BAD_FILE', 'ERR_BASE', 'ERR_BUFFER', 'ERR_COMM', 'ERR_CONVERSION', 'ERR_COUNT', 'ERR_DIMS', 'ERR_DISP', 'ERR_DUP_DATAREP', 'ERR_FILE', 'ERR_FILE_EXISTS', 'ERR_FILE_IN_USE', 'ERR_GROUP', 'ERR_INFO', 'ERR_INFO_KEY', 'ERR_INFO_NOKEY', 'ERR_INFO_VALUE', 'ERR_INTERN', 'ERR_IN_STATUS', 'ERR_IO', 'ERR_KEYVAL', 'ERR_LASTCODE', 'ERR_LOCKTYPE', 'ERR_NAME', 'ERR_NOT_SAME', 'ERR_NO_MEM', 'ERR_NO_SPACE', 'ERR_NO_SUCH_FILE', 'ERR_OP', 'ERR_OTHER', 'ERR_PENDING', 'ERR_PORT', 'ERR_QUOTA', 'ERR_RANK', 'ERR_READ_ONLY', 'ERR_REQUEST', 'ERR_RMA_ATTACH', 'ERR_RMA_CONFLICT', 'ERR_RMA_FLAVOR', 'ERR_RMA_RANGE', 'ERR_RMA_SHARED', 'ERR_RMA_SYNC', 'ERR_ROOT', 'ERR_SERVICE', 'ERR_SIZE', 'ERR_SPAWN', 'ERR_TAG', 'ERR_TOPOLOGY', 'ERR_TRUNCATE', 'ERR_TYPE', 'ERR_UNKNOWN', 'ERR_UNSUPPORTED_DATAREP', 'ERR_UNSUPPORTED_OPERATION', 'ERR_WIN', 'Errhandler', 'Exception', 'FILE_NULL', 'FLOAT', 'FLOAT_INT', 'F_BOOL', 'F_COMPLEX', 'F_DOUBLE', 'F_DOUBLE_COMPLEX', 'F_FLOAT', 'F_FLOAT_COMPLEX', 'F_INT', 'File', 'Finalize', 'Free_mem', 'GRAPH', 'GROUP_EMPTY', 'GROUP_NULL', 'Get_address', 'Get_error_class', 'Get_error_string', 'Get_library_version', 'Get_processor_name', 'Get_version', 'Graphcomm', 'Grequest', 'Group', 'HOST', 'IDENT', 'INFO_ENV', 'INFO_NULL', 'INT', 'INT16_T', 'INT32_T', 'INT64_T', 'INT8_T', 'INTEGER', 'INTEGER1', 'INTEGER16', 'INTEGER2', 'INTEGER4', 'INTEGER8', 'INT_INT', 'IN_PLACE', 'IO', 'Info', 'Init', 'Init_thread', 'Intercomm', 'Intracomm', 'Is_finalized', 'Is_initialized', 'Is_thread_main', 'KEYVAL_INVALID', 'LAND', 'LASTUSEDCODE', 'LB', 'LOCK_EXCLUSIVE', 'LOCK_SHARED', 'LOGICAL', 'LOGICAL1', 'LOGICAL2', 'LOGICAL4', 'LOGICAL8', 'LONG', 'LONG_DOUBLE', 'LONG_DOUBLE_INT', 'LONG_INT', 'LONG_LONG', 'LOR', 'LXOR', 'Lookup_name', 'MAX', 'MAXLOC', 'MAX_DATAREP_STRING', 'MAX_ERROR_STRING', 'MAX_INFO_KEY', 'MAX_INFO_VAL', 'MAX_LIBRARY_VERSION_STRING', 'MAX_OBJECT_NAME', 'MAX_PORT_NAME', 'MAX_PROCESSOR_NAME', 'MESSAGE_NO_PROC', 'MESSAGE_NULL', 'MIN', 'MINLOC', 'MODE_APPEND', 'MODE_CREATE', 'MODE_DELETE_ON_CLOSE', 'MODE_EXCL', 'MODE_NOCHECK', 'MODE_NOPRECEDE', 'MODE_NOPUT', 'MODE_NOSTORE', 'MODE_NOSUCCEED', 'MODE_RDONLY', 'MODE_RDWR', 'MODE_SEQUENTIAL', 'MODE_UNIQUE_OPEN', 'MODE_WRONLY', 'Message', 'NO_OP', 'OFFSET', 'OP_NULL', 'ORDER_C', 'ORDER_F', 'ORDER_FORTRAN', 'Op', 'Open_port', 'PACKED', 'PROC_NULL', 'PROD', 'Pcontrol', 'Prequest', 'Publish_name', 'Query_thread', 'REAL', 'REAL16', 'REAL2', 'REAL4', 'REAL8', 'REPLACE', 'REQUEST_NULL', 'ROOT', 'Register_datarep', 'Request', 'SEEK_CUR', 'SEEK_END', 'SEEK_SET', 'SHORT', 'SHORT_INT', 'SIGNED_CHAR', 'SIGNED_INT', 'SIGNED_LONG', 'SIGNED_LONG_LONG', 'SIGNED_SHORT', 'SIMILAR', 'SINT16_T', 'SINT32_T', 'SINT64_T', 'SINT8_T', 'SUBVERSION', 'SUCCESS', 'SUM', 'Status', 'TAG_UB', 'THREAD_FUNNELED', 'THREAD_MULTIPLE', 'THREAD_SERIALIZED', 'THREAD_SINGLE', 'TWOINT', 'TYPECLASS_COMPLEX', 'TYPECLASS_INTEGER', 'TYPECLASS_REAL', 'Topocomm', 'UB', 'UINT16_T', 'UINT32_T', 'UINT64_T', 'UINT8_T', 'UNDEFINED', 'UNEQUAL', 'UNIVERSE_SIZE', 'UNSIGNED', 'UNSIGNED_CHAR', 'UNSIGNED_INT', 'UNSIGNED_LONG', 'UNSIGNED_LONG_LONG', 'UNSIGNED_SHORT', 'UNWEIGHTED', 'Unpublish_name', 'VERSION', 'WCHAR', 'WEIGHTS_EMPTY', 'WIN_BASE', 'WIN_CREATE_FLAVOR', 'WIN_DISP_UNIT', 'WIN_FLAVOR', 'WIN_FLAVOR_ALLOCATE', 'WIN_FLAVOR_CREATE', 'WIN_FLAVOR_DYNAMIC', 'WIN_FLAVOR_SHARED', 'WIN_MODEL', 'WIN_NULL', 'WIN_SEPARATE', 'WIN_SIZE', 'WIN_UNIFIED', 'WTIME_IS_GLOBAL', 'Win', 'Wtick', 'Wtime', '__builtins__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__pyx_capi__', '__spec__', '_addressof', '_handleof', '_keyval_registry', '_lock_table', '_set_abort_status', '_sizeof', '_typecode', '_typedict', '_typedict_c', '_typedict_f', 'get_vendor', 'memory', 'pickle']

```

这样子就说明安装成功了.

