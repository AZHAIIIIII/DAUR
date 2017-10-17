## R 数据结构

R 的数据结构屈指可数，无非是向量(Vector)、矩阵(Matrix)、数组(Array)、数据框(DataFrame)和列表(List)几种，他们主要区别在存储数据的类型和存储的方式上。

|        | **向量** | **矩阵** | **数组** | **数据框** | **列表**|
|:----------:|:----------:|:---------:|:----------:|:------------:|:--------:|
| 存储数据 | 同质 | 同质 | 同质 | 异质 | 异质|
| 存储方式 | 一维 | 二维 | 多维 | 二维 | 一维 |


### 向量 Vector

向量是最简单的数据结构^[数字或字符串等所谓的“标量”其实是只含一个元素的向量]，用来存储数值型(numeric)、字符型(character)或逻辑型(logical)数据，且一个向量只能存储一种类型的数据。向量使用`c()`函数进行创建。
```r
> vector_num <- c(1, 2, 3)                   # 创建数值型向量 vector_num
> vector_chr <- c("one", "two", "three")     # 创建字符型向量 vector_chr
> vector_log <- c(T, TRUE, F, FALSE)         # 创建逻辑型向量 vector_log
```
**提醒：**`T`与`F`是逻辑型数据`TRUE`与`FALSE`的缩写，二者等价，故定义变量名时尽量不要使用这两个大写字母。

`c(1, 2, 3)`与`c(1:3)`等价；还可使用类似`vector_num <- 1:5`的懒人命令，但更推荐规范的创建方式。

使用`vector_name[number]`的形式进行向量内元素的访问。
```r
> vector_chr <- c("one", "two", "three")
> vector_chr[2]
[1] "two"
> vector_chr[2:3]
[1] "two"   "three"
> vector_chr[c(1, 3)]
[1] "one"   "three"
```
以上三种方式分别访问了 vector_chr 向量的第2个元素、第2到第3个元素、第1和第3个元素。

使用`is.numeric()`、`is.character()`、`is.logical()`来判断向量是否为指定类型。
```r
> is.numeric(vector_num)
[1] TRUE
> is.character(vector_chr)
[1] TRUE
> is.logical(vector_log)
[1] TRUE
> is.numeric(vector_chr)
[1] FALSE
```

### 矩阵 Matrix

矩阵是仅包含同质数据的二维数据结构，可以简单理解为许多同类型、等长度向量的组合。矩阵使用`matrix()`函数进行创建。
```r
> matrix_01 <- matrix(1:6, nrow = 2, ncol = 3)
# 创建一个2行3列的矩阵，默认按列填充
> matrix_01
     [,1] [,2] [,3]
[1,]    1    3    5
[2,]    2    4    6

> matrix_02 <- matrix(1:6, nrow = 2, ncol = 3, byrow = TRUE)
# 创建按行填充的矩阵
> matrix_02
     [,1] [,2] [,3]
[1,]    1    2    3
[2,]    4    5    6

> cells <- c(1:6)
> rnames <- c("R1", "R2", "R3")
> cnames <- c("C1", "C2")
> matrix_03 <- matrix(cells, nrow = 3, ncol = 2, byrow = TRUE, dimnames = list(rnames, cnames))
# 创建按行填充、3行2列、行名为R1-3、列名为C1-2的矩阵
> matrix_03   
   C1 C2
R1  1  2
R2  3  4
R3  5  6

> vector_mat <- c(1, 3, 5, 7, 9, 11)
> dim(vector_mat) <- c(3, 2)
# 通过对向量添加 dim() 属性进行创建。
> vector_ma
     [,1] [,2]
[1,]    1    7
[2,]    3    9
[3,]    5   11
```

矩阵内元素的访问方式与向量类似。
```r
> matrix_04 <- matrix(1:8, nrow = 4)
> matrix_04
     [,1] [,2]
[1,]    1    5
[2,]    2    6
[3,]    3    7
[4,]    4    8
> matrix_04[2,]
[1] 2 6      # 访问第2行的元素
> matrix_04[,1]
[1] 1 2 3 4  # 访问第1列的元素
> matrix_04[4,2]
[1] 8        # 访问第4行第2个的元素
> matrix_04[c(1, 3),2]
[1] 5 7      # 访问第1和第3行的第2个元素
```

### 数组 Array

数组是矩阵的拓展，同样只能存储同质数据，但可有2个以上维度。数组使用`array()`函数创建。
```r
# 创建3x2x2、各维度各有命名的数组，并填充数字1到12
> dim1 <- c("a1","a2","a3")
> dim2 <- c("b1","b2")
> dim3 <- c("c1","c2")
> array_01 <- array(1:12, c(3, 2, 2), dimnames = list(dim1, dim2, dim3))
> array_01
, , c1
   b1 b2
a1  1  4
a2  2  5
a3  3  6

, , c2
   b1 b2
a1  7 10
a2  8 11
a3  9 12
```

```r
# 通过对向量添加 dim() 属性来创建
> vector_arr <- c(1, 3, 5, 7, 9, 11, 13, 15)
> dim(vector_arr) <- c(2, 2, 2)
> vector_arr
, , 1
     [,1] [,2]
[1,]    1    5
[2,]    3    7

, , 
     [,1] [,2]
[1,]    9   13
[2,]   11   15
```

数组中元素的访问方式跟矩阵相同。

### 数据框 DataFrame

数据框是 R 里应用最多的数据结构，能存储不同类型的数据，可视为许多等长度向量的组合，亦可视为解除了存储数据类型限制的矩阵。数据框使用`data.frame()`函数创建。

```r
# 创建一个学生信息数据框
> studentId <- c(1:4)
> stu_names <- c("a001", "a002", "a003", "a007")
> is_boy <- c(T, TRUE, F, FALSE)
> studentdata <- data.frame(studentId, stu_names, is_boy)
> studentdata
  studentId stu_names is_boy
1         1      a001   TRUE
2         2      a002   TRUE
3         3      a003  FALSE
4         4      a007  FALSE
```
当两个数据框行数相同时，可使用`cbind()`进行合并；当两个数据框列数相同且列名一一对应时，可使用`rbind()`进行合并。
```r
> cbind(studentdata, data.frame(age = c(18, 18, 17, 16)))
  studentId stu_names is_boy age
1         1      a001   TRUE  18
2         2      a002   TRUE  18
3         3      a003  FALSE  17
4         4      a007  FALSE  16
```
```r
> rbind(studentdata, data.frame(studentId = 5, stu_names = "a008", is_boy = T))
  studentId stu_names is_boy
1         1      a001   TRUE
2         2      a002   TRUE
3         3      a003  FALSE
4         4      a007  FALSE
5         5      a008   TRUE
```
上面两个例子是临时创建了两个未命名的数据框，将其合并到了已有数据框`studentdata`中。在数据框内，列表示变量，行表示观测，刚刚的合并分别是增添了一个变量和一组观测。

使用`dataframe_name[col_number]`的形式来读取数据框的元素。
```r
> studentdata[2:3]
# 读取第2到3列
  stu_names is_boy
1      a001   TRUE
2      a002   TRUE
3      a003  FALSE
4      a007  FALSE
> studentdata$studentId
# 利用 $ 符号直接选定指定列
[1] 1 2 3 4
```


