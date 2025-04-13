# 1、MPI的6个基本函数

## MPI_Init

任何MPI程序都应该首先调用该函数。 此函数不必深究，只需在MPI程序开始时调用即可（必须保证程序中第一个调用的MPI函数是这个函数）。

C和C++需要将main函数里的两个参数传进去，因此在写main函数的主程序时，应该加上这两个形参。

```cpp
int main(int *argc,char* argv[]) 
{ 
    MPI_Init(&argc,&argv); 
}
```

## MPI_Finalize

任何MPI程序结束时，都需要调用该函数。 该函数同第一个函数，都不必深究，只需要求格式去写即可。

```cpp
MPI_Finalize() //C++ 
```

## MPI_COMM_RANK

```cpp
int MPI_Comm_Rank(MPI_Comm comm, int *rank) 
MPI_Comm_Rank(MPI_COMM_WORLD,&myid);
```

该函数是获得当前进程的进程标识，如进程0在执行该函数时，可以获得返回值0。可以看出该函数接口有两个参数，前者为进程所在的通信域，后者为返回的进程号。通信域可以理解为给进程分组，比如有0-5这六个进程。可以通过定义通信域，来将比如[0,1,5]这三个进程分为一组，这样就可以针对该组进行“组”操作，比如规约之类的操作。这类概念会在之后的MPI进阶一章中讲解。MPI_COMM_WORLD是MPI已经预定义好的通信域，是一个包含所有进程的通信域，目前只需要用该通信域即可。

## MPI_COMM_SIZE

该函数是获取该通信域内的总进程数，如果通信域为MPI_COMM_WORLD，即获取总进程数，使用方法和MPI_COMM_RANK相近。

```cpp
int MPI_Comm_Size(MPI_Comm, int *size) 
```

## MPI_SEND

该函数为发送函数，用于进程间发送消息，如进程0计算得到的结果A，需要传给进程1，就需要调用该函数。

```cpp
int MPI_Send(type* buf, int count, MPI_Datatype, int dest, int tag, MPI_Comm comm) 
```

该函数参数过多，不过这些参数都很有必要存在。

这些参数均为传入的参数，其中buf为你需要传递的数据的起始地址，比如你要传递一个数组A，长度是5，则buf为数组A的首地址。

count即为长度，从首地址之后count个变量。

datatype为变量类型，注意该位置的变量类型是MPI预定义的变量类型，比如需要传递的是C++的int型，则在此处需要传入的参数是MPI_INT，其余同理。

dest为接收的进程号，即被传递信息进程的进程号。

tag为信息标志，同为整型变量，发送和接收需要tag一致，这将可以区分同一目的地的不同消息。比如进程0给进程1分别发送了数据A和数据B，tag可分别定义成0和1，这样在进程1接收时同样设置tag0和1去接收，避免接收混乱。

## MPI_RECV

该函数为MPI的接收函数，需要和MPI_SEND成对出现。

```cpp
int MPI_Recv(type* buf, int count, MPI_Datatype, int source, int tag, MPI_Comm comm, MPI_Status *status)
```

参数和MPI_SEND大体相同，不同的是source这一参数，这一参数标明从哪个进程接收消息。最后多一个用于返回状态信息的参数status。

在C和C++中，status的变量类型为MPI_Status，分别有三个域，可以通过status.MPI_SOURCE，status.MPI_TAG和status.MPI_ERROR的方式调用这三个信息。这三个信息分别返回的值是所收到数据发送源的进程号，该消息的tag值和接收操作的错误代码。

SEND和RECV需要成对出现，若两进程需要相互发送消息时，对调用的顺序也有要求，不然可能会出现死锁或内存溢出等比较严重的问题。





