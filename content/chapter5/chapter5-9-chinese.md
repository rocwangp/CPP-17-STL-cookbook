# 生成输入序列的序列

当测试代码需要处理参数顺序不重要的输入序列时，有必要测试它是否对所有可能的输入产生相同的输出。当你自己实现了一个排序算法时，你就要写这样的测试代码来确定自己的实现是否正确。

std::next_permutation在任何时候都能帮我们将序列进行打乱。我们在可修改的范围中可以调用它，其会将以字典序进行置换。

## How to do it...

本节，我们将从标准输入中读取多个字符串，然后我们会使用std::next_permutation生成已排序的序列，并且打印这个序列：

1. 首先包含必要的头文件，并且声明所使用的命名空间。

   ```c++
   #include <iostream>
   #include <vector>
   #include <string>
   #include <iterator>
   #include <algorithm>
   
   using namespace std; 
   ```

2. 这里使用标准数组对vector进行初始化。接下来对vector进行排序：

   ```c++
   int main()
   {
       vector<string> v {istream_iterator<string>{cin}, {}};
       sort(begin(v), end(v));
   ```

3. 现在我们来打印vector中的内容。随后，我们会调用std::next_permutation。其会打乱已经排序的vector，然后再对其进行打印。直到 next_permutation返回false时，代表 next_permutation完成了其操作，循环结束：

   ```c++
       do {
           copy(begin(v), end(v),
           	ostream_iterator<string>{cout, ", "});
           cout << '\n';
       } while (next_permutation(begin(v), end(v)));
   }
   ```

4. 编译运行这个程序，会有如下的打印：

   ```c++
   $ echo "a b c" | ./input_permutations
   a, b, c,
   a, c, b,
   b, a, c,
   b, c, a,
   c, a, b,
   c, b, a,
   ```

## How it works...

 std::next_permutation算法使用起来有点奇怪。这是因为这个函数接受一组开始/结束迭代器，当其找到下一个置换时返回true；否则，返回false。不过“下一个置换”又是什么意思呢？

当std::next_permutation算法找到元素中的下一个字典序时，其会以如下方式工作：

1. 通过`v[i - 1] < v[i]`的方式找到最大索引i。如果这个最大索引不存在，那么返回false。
2. 再找到最大所以j，这里j需要大于等于i，并且`v[j] > v[i - 1]`。
3. 将位于索引位置j和i - 1上的值进行交换。
4. 将从i到范围末尾的元素进行反向。
5. 返回true。

每次单独的置换顺序，都会在同一个序列中呈现。为了看到所有置换的可能，我们先对数组进行了排序，因为如果我们输入“c b a”到算法中，算法会立即终止，因为每个元素都以反字典序排列。



