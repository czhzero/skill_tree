##vim 常用命令总结


### 1.vi打开文件

```
vi filename :打开或新建文件，并将光标置于第一行首
vi +n filename ：打开文件，并将光标置于第n行首
vi + filename ：打开文件，并将光标置于最后一行首
vi +/pattern filename：打开文件，并将光标置于第一个与pattern匹配的串处
vi -r filename ：在上次正用vi编辑时发生系统崩溃，恢复filename
vi -o/O filename1 filename2 ... ：打开多个文件，依次进行编辑

```