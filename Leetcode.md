## Leetcode

# 数组

## 统计数组中的元素

### 448：

1. 如何不新建数组：对原有数组进行+n的操作。（超出n的就是不符合的）
2. map的更改就是替换（用put方法，相同key值会进行替代）

### 442

1. ### :有数字出现两次就说明有数组没出现。所以利用出现两次的数字让她取代没有出现数字的位置，最后进行判断，位置不合理的就是缺少的。
2. 另外一种方法：类似于Map方法，使用正负号作为标记，由于 本身可能已经为负数，因此在将 作为下标或者放入答案时，需要取绝对值

### 41:

1. 假如出现大于n或小于等于0，答案就必定在1到n间（因为此时没法将1到n的数字都包括，并把这些数字都赋值为1（不会影响结果））
2. java数组判断包含某个数值：一般自己写一个方法

### 274

1. 第一种方法利用一般的排序（其时间复杂度为O（NLogN））
2. 计数排序（i代表该论文被引用的次数，nums【i】代表论文的数量），至于为什么从n开始循环，主要就是一步步试，从最大的H值开始试

## 数组的改变、移动

### 453

思路：每次操作既可以理解为使 n-1*n*−1 个元素增加 11，也可以理解使 11 个元素减少 11。显然，后者更利于我们的计算。

得到数组最小值：` Arrays.stream(nums).min().getAsInt()`

### 665

思路：试探法：如果不是非递减，就让前后两个数相等，试一下，要么都是钱一个数，要么都是后一个数

```java
for(int i = 0; i < nums.length-1; i++){
            int x = nums[i];
            int y= nums[i+1];
            if (nums[i] > nums[i+1]){
                nums[i] = y;
                if(isSorted(nums)){
                    return true;
                }
                nums[i] = x;
                nums[i+1] = x;
                return isSorted(nums);
            }
        }
```

**注意，不要数组溢出，这里有i+1**

### 283

思路：就是通过数组前移来实现，需要注意的是每次前移，都要让i--，因为原先的位置已经有新的位置前移，这个位置还要扫描一遍

```java
 int count = 0;
        int i = 0;
        while (i < nums.length - count) {
            if (nums[i] == 0) {
                int position = i;
                count++;
                //  开始前移
                for (int j = i; j < nums.length - count; j++) {
                    nums[j] = nums[j + 1];
                }
                nums[nums.length - count] = 0;
                i--;
            }
            i++;
        }
```

题解思路：双指针，左指针指向当前已经处理好的序列的尾部，右指针指向待处理序列的头部。每次右指针指向非零数，则将左右指针对应的数交换，同时左指针右移。

1. 左指针左边均为非零数；
2. 右指针左边直到左指针处均为零

```java
int n = nums.size(), left = 0, right = 0;
        while (right < n) {
            if (nums[right]) {
                swap(nums[left], nums[right]);
                left++;
            }
            right++;
        }

```

## 二维数组及滚动数组

### 119

```
public List<Integer> getRow(int rowIndex) {
    int a[][] = new int[rowIndex+1][rowIndex+1];
    List<Integer> list = new ArrayList<>();
    a[0][0] = 1;
    if (rowIndex == 0){
        list.add(1);
        return list;
    }
    a[1][0] = 1;
    a[1][1] = 1;
    if (rowIndex > 1){
        for (int i = 2; i < rowIndex + 1; i++){
            for (int j = 0 ; j < rowIndex + 1; j++){
                if (j == 0 || j == rowIndex){
                    a[i][j] = 1;
                }else{
                    a[i][j] = a[i-1][j-1] + a[i-1][j];
                }
            }
        }
    }
    for (int j = 0 ;j < rowIndex + 1; j++){
        list.add(a[rowIndex][j]);
    }
    return list;
}
```

1. 当时这里没有用双层循环，就导致最后都是赋值为1
2. 没有注意下标越界

### 661

```java
public int[][] imageSmoother(int[][] img) {
    int a[][] = new int[img.length][img[0].length];
    //  行数
    for (int i = 0; i < img.length; i++){
        //  列数
        for (int j = 0; j < img[0].length; j++ ){
            int count = 0;
            int total = 0;
            for (int x = j -1; x <= j + 1; x++){
                for (int y = i-1; y <= i+1; y++){
                    if (x >=0 && x < img[0].length && y >= 0 && y < img.length){
                        count ++;
                        total += img[y][x];
                    }
                }
            }
            a[i][j] = total / count;
        }
    }
    return a;
}
```

1. 用双循环查看四周的元素
2. 注意下标，如果我是用x，y的话最后要用img[y][x]
3. 注意二维数组求两个维度的长度

### 598

```java
class Solution {
    public int maxCount(int m, int n, int[][] ops) {
        int a[][] = new int[m][n];
        int x = ops.length;
        for (int i = 0; i < x; i++){
            for (int j = 0; j < ops[i][0]; j++){
                for (int k = 0; k < ops[i][1]; k++){
                    a[j][k] ++;
                }
            }
        }
        int num = 0;
        int max = a[0][0];
        //  遍历
        for (int i = 0; i < m; i++){
            for (int j = 0; j < m; j++){
                if (a[i][j] > max){
                    max = a[i][j];
                    num = 1;
                }else if (a[i][j] == max){
                    num ++;
                }
            }
        }
        return num;
    }
}
```

我的方法超出了内存限制

```java
class Solution {
    public int maxCount(int m, int n, int[][] ops) {
        int mina = m, minb = n;
        for (int []op:ops){
            mina = Math.min(mina,op[0]);
            minb = Math.min(minb,op[1]);
        }
        return mina * minb;
    }
}
```

其实就是找一直被选中的那一块最小的地方

### 419

```java
class Solution {
    public int countBattleships(char[][] board) {
        int row = board.length;
        int col = board[0].length;
        int ans = 0;
        for (int i = 0; i < row; ++i) {
            for (int j = 0; j < col; ++j) {
                if (board[i][j] == 'X') {
                    board[i][j] = '.';
                    for (int k = j + 1; k < col && board[i][k] == 'X'; k++) {
                        board[i][k] = '.';
                    }
                    for (int k = i + 1; k < row && board[k][j] == 'X'; k++) {
                        board[k][j] = '.';
                    }
                    ans++;
                }
            }
        }
        return ans;
    }
}
```

因为战舰不能相邻，因此相邻的块一定属于同一个战舰

1. 注意下面的for循环的&& 是关键，只要不是就会退出循环

## 二维数组的旋转

### 189

1. 如果不开辟新的数组，是不能一个个换过去的，我一开始的思路其实一直在换动同一个元素

   ```java
   class Solution {
       public void rotate(int[] nums, int k) {
           int n = nums.length;
           int[] newArr = new int[n];
           for (int i = 0; i < n; ++i) {
               newArr[(i + k) % n] = nums[i];
           }
           System.arraycopy(newArr, 0, nums, 0, n);
       }
   }
   ```

2. 分开旋转

   ```java
   class Solution {
       public void rotate(int[] nums, int k) {
           k %= nums.length;
           reverse(nums, 0, nums.length - 1);
           reverse(nums, 0, k - 1);
           reverse(nums, k, nums.length - 1);
       }
   
       public void reverse(int[] nums, int start, int end) {
           while (start < end) {
               int temp = nums[start];
               nums[start] = nums[end];
               nums[end] = temp;
               start += 1;
               end -= 1;
           }
       }
   }
   ```


### 396

![image-20220908174046472](C:\Users\lyh\AppData\Roaming\Typora\typora-user-images\image-20220908174046472.png)

1. 暴力求解会超出限时时间
2. 使用迭代的方法，求出F（k）与F（k-1）的关系
3. 可以使用流直接计算数组的最大值
4. 求较大值也可以直接利用函数解决

```java
import java.util.Arrays;

class Solution {
    public int maxRotateFunction(int[] nums) {
        int f = 0, n = nums.length, numSum = Arrays.stream(nums).sum();
        for (int i = 0; i < n; i++) {
            f += i * nums[i];
        }
        int res = f;
        for (int i = n - 1; i > 0; i--) {
            f += numSum - n * nums[i];
            res = Math.max(res, f);
        }
        return res;
    }
}
```

## 特定顺序遍历二维数组 

### 54

1. 数组的初始化都是用的 `{}`
2. 注意边界的判定，这道题其实可以不用记录走过的路径‘
3. 答案的必须是++u，不能是u++

#### 我的代码：

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        //  主要分为4个动作，上下左右
        boolean[][] flag = new boolean[matrix.length][matrix[0].length];
        ArrayList list = new ArrayList();
        int k =0;
        while (true){
            if (!right(matrix,flag,k,k,list)){
                break;
            };
            if (!down(matrix,flag,k,matrix[0].length-1-k,list)) {
                break;
            }
            if (!left(matrix,flag,matrix.length-1-k,matrix[0].length-1-k,list)) {
                break;
            }
            if (!up(matrix,flag,matrix.length-1-k,k,list)) {
                break;
            }
            k++;
        }
        return list;
    }

    //  返回移动后的位置
    public boolean right(int[][] martix,boolean[][] flag,int row,int line,ArrayList list){
        if ( line  >= martix[0].length || flag[row][line] ){
            return false;
        }
        for (int j = line ; j < martix[0].length && !flag[row][j]; j++){
            list.add(martix[row][j]);
            //  将该位置设置为已访问
            flag[row][j] = true;
        }
        return true;
    }

    public boolean down(int[][] martix,boolean[][] flag,int row,int line,ArrayList list){
        if (row + 1 >= martix.length  || flag[row+1][line]  ){
            return false;
        }
        for (int j = row+1; j < martix.length && !flag[j][line]; j++){
            list.add(martix[j][line]);
            //  将该位置设置为已访问
            flag[j][line] = true;
        }
        return true;
    }

    public boolean left(int[][] martix,boolean[][] flag,int row,int line,ArrayList list){
        if (line - 1 < 0 || flag[row][line-1]){
            return false;
        }
        for (int j = line-1; j >= 0 && !flag[row][j]; j--){
            list.add(martix[row][j]);
            //  将该位置设置为已访问
            flag[row][j] = true;
        }
        return true;
    }

    public boolean up(int[][] martix,boolean[][] flag,int row,int line,ArrayList list){
        if (row - 1 < 0  ||flag[row-1][line]){
            return false;
        }
        for (int j = row-1; j >= 0 && !flag[j][line]; j--){
            list.add(martix[j][line]);
            //  将该位置设置为已访问
            flag[j][line] = true;
        }
        return true;
    }

    public static void main(String[] args) {
        int [][]matrix = {{1}};
        Solution solution = new Solution();
        List<Integer> integers = solution.spiralOrder(matrix);
        System.out.println(integers);
    }
}
```

#### 答案代码

```c++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector <int> ans;
        if(matrix.empty()) return ans; //若数组为空，直接返回答案
        int u = 0; //赋值上下左右边界
        int d = matrix.size() - 1;
        int l = 0;
        int r = matrix[0].size() - 1;
        while(true)
        {
            for(int i = l; i <= r; ++i) ans.push_back(matrix[u][i]); //向右移动直到最右
            if(++ u > d) break; //重新设定上边界，若上边界大于下边界，则遍历遍历完成，下同
            for(int i = u; i <= d; ++i) ans.push_back(matrix[i][r]); //向下
            if(-- r < l) break; //重新设定有边界
            for(int i = r; i >= l; --i) ans.push_back(matrix[d][i]); //向左
            if(-- d < u) break; //重新设定下边界
            for(int i = d; i >= u; --i) ans.push_back(matrix[i][l]); //向上
            if(++ l > r) break; //重新设定左边界
        }
        return ans;
    }
};

```

### 59

1. 注意判断break的条件容易写错，我当时第四个写错了
2. 注意数组的行和列，我当时弄反了

```java
class Solution {
    public int[][] generateMatrix(int n) {
        int a[][] = new int[n][n];
        int count = 1;
        //  定义四个边界
        int left = 0;
        int right = n - 1;
        int down = n -1 ;
        int up = 0;
        //  开始循环遍历
        while (true){
            //  向右遍历
            for (int i = left; i <= right; i++){
                a[up][i] = count;
                count ++;
            }
            if ( ++up > down){
                break;
            }
            //  向下遍历
            for (int i = up; i <= down; i++){
                a[i][right] = count;
                count ++;
            }
            if (left > --right){
                break;
            }
            //  向左遍历
            for (int i = right; i >= left;i--){
                a[down][i] = count;
                count ++;
            }
            if (up > --down){
                break;
            }
            //  向上遍历
            for (int i = down; i >= up; i--){
                a[i][left] = count;
                count ++;
            }
            if (++left > right){
                break;
            }
        }
        return a;
    }



}
```

### 498

1. 这道题特别关键的就是这个循环，根据对角线的数量进行循环，再根据奇偶来分

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public int[] findDiagonalOrder(int[][] mat) {
        int m = mat.length;
        int n = mat[0].length;
        int[] res = new int[m * n];
        int pos = 0;

        for (int i = 0; i < m + n - 1; i++) {
            if (i % 2 == 1) {
                int x = i < n ? 0 : i - n + 1;
                int y = i < n ? i : n - 1;
                while (x < m && y >= 0) {
                    res[pos] = mat[x][y];
                    pos++;
                    x++;
                    y--;
                }
            } else {
                int x = i < m ? i : m - 1;
                int y = i < m ? 0 : i - m + 1;
                while (x >= 0 && y < n) {
                    res[pos] = mat[x][y];
                    pos++;
                    x--;
                    y++;
                }
            }
        }
        return res;
    }
}
```

## 二维数组变换

### 566

1. 注意已知总数求行和列的方式，用的是num-1，不是line+1，求出来是不一样的
2. 注意读完条件，题目中有说不符合条件的处理方式
3. 可以直接写，不用通过num转换

```java
class Solution {
    public int[][] matrixReshape(int[][] mat, int r, int c) {
        if (r * c != mat[0].length * mat.length){
            return mat;
        }

        int [][]a = new int[r][c];
        for (int i = 0; i < r; i++){
            for(int j = 0; j < c; j++){
                //  算当前是第几个数字
                int num = i * c + j + 1;
                //  算在原数组中的位置
                int row = (num-1) / (mat[0].length) ;
                int line = num - row * mat[0].length - 1;
                a[i][j] = mat[row][line];
            }
        }
        return a;
    }


}
```

答案

```java
class Solution {
    public int[][] matrixReshape(int[][] nums, int r, int c) {
        int m = nums.length;
        int n = nums[0].length;
        if (m * n != r * c) {
            return nums;
        }

        int[][] ans = new int[r][c];
        for (int x = 0; x < m * n; ++x) {
            ans[x / c][x % c] = nums[x / n][x % n];
        }
        return ans;
    }
}

```

### 48

1. 注意使用辅助数组的时候，最后要拷贝到原数组中去，不要忘了
2. 不使用额外空间的方法
3. 可以利用水平+主对角线翻转
4. 要不利用额外空间的方法，只需要一次性找到每一次要变换的四个位置，只要让四个位置同时变，就不存在覆盖的问题了，解决覆盖的关键就在于，一次性让会覆盖的位置都换掉、



翻转

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        // 水平翻转
        for (int i = 0; i < n / 2; ++i) {
            for (int j = 0; j < n; ++j) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[n - i - 1][j];
                matrix[n - i - 1][j] = temp;
            }
        }
        // 主对角线翻转
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }
    }
}

```

#### 不利用额外数组

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        for (int i = 0; i < n / 2; ++i) {
            for (int j = 0; j < (n + 1) / 2; ++j) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[n - j - 1][i];
                matrix[n - j - 1][i] = matrix[n - i - 1][n - j - 1];
                matrix[n - i - 1][n - j - 1] = matrix[j][n - i - 1];
                matrix[j][n - i - 1] = temp;
            }
        }
    }
}

```

### 73

1. 我原来使用的是两个数组来记录需要清零的位置
2. 其实可以使用第一行和第一列来记录是否需要清零，再利用额外的两个变量来存储第一行和第一列是否需要清零



```java
class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        boolean flagCol0 = false, flagRow0 = false;
        for (int i = 0; i < m; i++) {
            if (matrix[i][0] == 0) {
                flagCol0 = true;
            }
        }
        for (int j = 0; j < n; j++) {
            if (matrix[0][j] == 0) {
                flagRow0 = true;
            }
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = matrix[0][j] = 0;
                }
            }
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }
        if (flagCol0) {
            for (int i = 0; i < m; i++) {
                matrix[i][0] = 0;
            }
        }
        if (flagRow0) {
            for (int j = 0; j < n; j++) {
                matrix[0][j] = 0;
            }
        }
    }
}

```

### 269

```java
if ((r < rows && r >= 0) && (c < cols && c >= 0) && (copyBoard[r][c] == 1)) {
                                liveNeighbors += 1;
                            }
```

1. 可以利用一个并的条件语句来判断
2. 根据原细胞是死还是活分成两个条件
3. 要做到同步更新就不能直接更新后填入原数组
4. 官方解答的妙处就在于可以看出该细胞之前的状态是什么，2状态的意义就在于揭示了他之前是死的状态，不然我们没法判断当前周围的活细胞数量

```java
class Solution {
    public void gameOfLife(int[][] board) {
        int[] neighbors = {0, 1, -1};

        int rows = board.length;
        int cols = board[0].length;

        // 创建复制数组 copyBoard
        int[][] copyBoard = new int[rows][cols];

        // 从原数组复制一份到 copyBoard 中
        for (int row = 0; row < rows; row++) {
            for (int col = 0; col < cols; col++) {
                copyBoard[row][col] = board[row][col];
            }
        }

        // 遍历面板每一个格子里的细胞
        for (int row = 0; row < rows; row++) {
            for (int col = 0; col < cols; col++) {

                // 对于每一个细胞统计其八个相邻位置里的活细胞数量
                int liveNeighbors = 0;

                for (int i = 0; i < 3; i++) {
                    for (int j = 0; j < 3; j++) {

                        if (!(neighbors[i] == 0 && neighbors[j] == 0)) {
                            int r = (row + neighbors[i]);
                            int c = (col + neighbors[j]);

                            // 查看相邻的细胞是否是活细胞
                            if ((r < rows && r >= 0) && (c < cols && c >= 0) && (copyBoard[r][c] == 1)) {
                                liveNeighbors += 1;
                            }
                        }
                    }
                }

                // 规则 1 或规则 3      
                if ((copyBoard[row][col] == 1) && (liveNeighbors < 2 || liveNeighbors > 3)) {
                    board[row][col] = 0;
                }
                // 规则 4
                if (copyBoard[row][col] == 0 && liveNeighbors == 3) {
                    board[row][col] = 1;
                }
            }
        }
    }
}

```

![image-20220914100831144](C:\Users\lyh\AppData\Roaming\Typora\typora-user-images\image-20220914100831144.png)

```java
class Solution {
public:
    void gameOfLife(vector<vector<int>>& board) {
        int neighbors[3] = {0, 1, -1};

        int rows = board.size();
        int cols = board[0].size();

        // 遍历面板每一个格子里的细胞
        for (int row = 0; row < rows; row++) {
            for (int col = 0; col < cols; col++) {

                // 对于每一个细胞统计其八个相邻位置里的活细胞数量
                int liveNeighbors = 0;

                for (int i = 0; i < 3; i++) {
                    for (int j = 0; j < 3; j++) {

                        if (!(neighbors[i] == 0 && neighbors[j] == 0)) {
                            // 相邻位置的坐标
                            int r = (row + neighbors[i]);
                            int c = (col + neighbors[j]);

                            // 查看相邻的细胞是否是活细胞
                            if ((r < rows && r >= 0) && (c < cols && c >= 0) && (abs(board[r][c]) == 1)) {
                                liveNeighbors += 1;
                            }
                        }
                    }
                }

                // 规则 1 或规则 3 
                if ((board[row][col] == 1) && (liveNeighbors < 2 || liveNeighbors > 3)) {
                    // -1 代表这个细胞过去是活的现在死了
                    board[row][col] = -1;
                }
                // 规则 4
                if (board[row][col] == 0 && liveNeighbors == 3) {
                    // 2 代表这个细胞过去是死的现在活了
                    board[row][col] = 2;
                }
            }
        }

        // 遍历 board 得到一次更新后的状态
        for (int row = 0; row < rows; row++) {
            for (int col = 0; col < cols; col++) {
                if (board[row][col] > 0) {
                    board[row][col] = 1;
                } else {
                    board[row][col] = 0;
                }
            }
        }
    }
};


```

## 前缀和数组

### 303

1. 如果朴素的利用复制数组会导致时间过长，所以直接存储总和

```java
class NumArray {
    int[] sums;
public NumArray(int[] nums) {
    int n = nums.length;
    sums = new int[n + 1];
    for (int i = 0; i < n; i++) {
        sums[i + 1] = sums[i] + nums[i];
    }
}

public int sumRange(int i, int j) {
    return sums[j + 1] - sums[i];
}
}
```
### 304

1. 注意sum是从下标1开始计数，因此无需对i=0进行讨论
2. 注意交叉区域怎么计算

```java
class NumMatrix {
    int sum[][];

    public NumMatrix(int[][] matrix) {
        sum = new int[matrix.length+1][matrix[0].length+1];
        for (int i = 0; i < matrix.length; i++){
            for (int j = 0; j < matrix[0].length; j++){
                sum[i+1][j+1] = sum[i][j+1] + sum[i+1][j] - sum[i][j] + matrix[i][j];
            }
        }
    }

    public int sumRegion(int row1, int col1, int row2, int col2) {
        return sum[row2+1][col2+1] - sum[row1][col2+1] - sum[row2+1][col1] + sum[row1][col1];
    }
}

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix obj = new NumMatrix(matrix);
 * int param_1 = obj.sumRegion(row1,col1,row2,col2);
 */
```

### 238

1. 关键就是利用前缀和后缀来实现
2. 转换除法就是不乘以那个数字

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        //  定义前缀数组
        int a[] = new int[nums.length];
        //  定义后缀数组
        int b[] = new int[nums.length];
        //  定义结果数组
        int result[] = new int[nums.length];
        //  赋初值
        a[0] = 1;
        b[nums.length-1] = 1;
        //  对前缀开始赋初值
        for (int i = 1;i < nums.length;i++){
            a[i] = a[i - 1] * nums[i-1];
        }

        for (int i = nums.length-2; i >=0; i--){
            b[i] = b[i+1] * nums[i+1];
        }

        //  对前缀数组和后缀数组进行相应的乘积
        for (int i = 0;i < nums.length;i++){
            result[i] = a[i] * b[i];
        }
        return result;
    }
}
```

# 字符串

## 字符

### 520

1. 注意这里字符串大小写相关的判断方法 `Character.isLowerCase`
2. 这里可以把条件简化，无论何种情况都需要第二个后面的字符和前面的字符相等，然后再排除一种不可能的情况就行

```java
class Solution {
    public boolean detectCapitalUse(String word) {
        if (word.length() >= 2 && Character.isLowerCase(word.charAt(0)) && Character.isUpperCase(word.charAt(1))){
            return false;
        }

        for (int i = 2; i < word.length(); ++i) {
            if (Character.isLowerCase(word.charAt(i)) ^ Character.isLowerCase(word.charAt(1))) {
                return false;
            }
        }
        return true;

    }
}
```

## 回文串的定义

### 125

1. 注意要全部转化为大写或者小写再进行判断
2. 对一些不是字母的需要去除，通过设置一个StringBuffer来接收符合要求的字符

```java
class Solution {
    public boolean isPalindrome(String s) {
        StringBuffer sgood = new StringBuffer();
        //  进行整体的大小写转换
        for (int i = 0; i < s.length() ; i++) {
            if (Character.isLetterOrDigit(s.charAt(i))){
                sgood.append(s.charAt(i));
            }
        }

        //  空字符判断
        if (s.equals("")){
            return true;
        }
        //  只需要判断前一半和后一半
        for (int i = 0; i < sgood.length() / 2;i++){
            if (Character.toUpperCase(sgood.charAt(i)) != Character.toUpperCase(sgood.charAt(sgood.length() - 1 - i))){
                return false;
            }
        }
        return true;
    }
}
```

## 公共前缀

### 14

1. 利用substring（i，j）截取字符串的部分字符，记住这里不会取尾
2. 如何避免取第一个字符串的长度作为循环的变量而导致多循环了，增加一个循环条件，如果循环到了某一个字符串的长度就直接退出

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        //  公共前缀
        StringBuffer s = new StringBuffer();
        //
        int count = strs.length;
        int length = strs[0].length();
        for (int i = 0; i <length ; i++) {
            char c = strs[0].charAt(i);
            for(int j = 1; j < count; j++){
                if (i == strs[j].length() || c != strs[j].charAt(i)){
                    return strs[0].substring(0,i);
                }
            }
        }
        return strs[0];
    }
}
```

## 单词

### 434

1. 注意判断的条件，主要通过出现的空格时和i==0开始判断，另一个条件就是后一个字符是字母

```java
class Solution {
    public int countSegments(String s) {
        int count = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) != ' ' && (i == 0 || s.charAt(i-1) == ' ') ){
                count++;
            }
        }
        return count;
    }
}
```

### 58

1. 注意这里要理清逻辑，既然是计算最后一个，那就从后面开始数，记下字符的开始和结束位置

```java
class Solution {
    public int lengthOfLastWord(String s) {
        int end = s.length() -1 ;
        while (s.charAt(end) == ' ' && end >= 0){
            end -- ;
        }
        if (end < 0){
            return 0;
        }
        int start = end ;
        while (start >= 0 && s.charAt(start) != ' ' ){
            start -- ;
        }
        return end - start;
    }
}
```

## 字符串的反转

### 541

1. 这里的问题特别多，首先是循环的中间到底是多少，对于字符串的翻转，如果是从0开始的，[0,(end+ 1) / 2]，那如果不是从0开始的，[start, (end + start+1)/2]
2. 如果对于字符串的对应，如果是0开始,s[i]对应s[end-i]，那如果不是从0开始的呢，s[i]对应s[end+start-i]

#### 自己的解法

```java
class Solution {
    public String reverseStr(String s, int k) {
        //  转化为字符数组来处理
        char[] chars = s.toCharArray();
        if (s.length() < 2 * k && s.length() >= k){
            reverse(chars,0,k-1);
        }else if (s.length() < k){
            reverse(chars,0,s.length()-1);
        }

        for (int i = 2 * k -1; i < s.length(); i += 2 * k){
            //  先将前k个字符进行翻转
            reverse(chars,i - 2 * k + 1,i - k);

            //  判断剩余字符
            if (s.length() - i - 1 < k){
                reverse(chars,i + 1,s.length()-1);
            }else if (s.length() - i - 1 >= k && s.length() - i - 1 < 2 * k){
                reverse(chars, i + 1,i + k );
            }
        }
        return new String(chars);
    }

    private void reverse(char[] s, int start,int end){
        for (int i = start; i < (start + end + 1) / 2  ; i++) {
            char temp = s[i];
            s[i] = s[end  + start- i];
            s[end + start - i] = temp;
        }
    }


}
```

#### 答案解法

```java
class Solution {
    public String reverseStr(String s, int k) {
        int n = s.length();
        char[] arr = s.toCharArray();
        for (int i = 0; i < n; i += 2 * k) {
            reverse(arr, i, Math.min(i + k, n) - 1);
        }
        return new String(arr);
    }

    public void reverse(char[] arr, int left, int right) {
        while (left < right) {
            char temp = arr[left];
            arr[left] = arr[right];
            arr[right] = temp;
            left++;
            right--;
        }
    }
}
```

1. 总的来说就是两种情况，在选中的字符串中全部翻转，或者是翻转k个

### 557

1. 对于字符串不可以进行更改的情况，除了将其转化为字符数组外，还可以新建一个StringBuffer类来进行存储字符串

```java
class Solution {
    public String reverseWords(String s) {
        StringBuffer ret = new StringBuffer();
        int length = s.length();
        int i = 0;
        while (i < length) {
            int start = i;
            while (i < length && s.charAt(i) != ' ') {
                i++;
            }
            for (int p = start; p < i; p++) {
                ret.append(s.charAt(start + i - 1 - p));
            }
            while (i < length && s.charAt(i) == ' ') {
                i++;
                ret.append(' ');
            }
        }
        return ret.toString();
    }
}
```

### 151

1. 第一种方法利用java内置的api，如去除开头和结尾的字符串，将字符串用空格符分割从而转化为List，然后再利用List的倒置，再利用join函数加入空格
2. 第二种方法，第一个函数负责去除多余的空格，第二个函数负责倒置整个字符串，第三个函数负责倒置各个单词。
3. 首先将使得空格正确化的这个函数，首先去除开头和结尾的空格，然后开始扫描字符串，出现一个非空格就加入，如果是空格，则要比对StringBuilder对象的最后一个字符是否是空格，再选中是否要加入该空格
4. 注意这里特别关键的是不要用两个while循环，会时间超了，
5. 对于倒转各个单词的方法，还是用while循环，直到找到这个单词的尾位置

#### 代码1

```java
class Solution {
    public String reverseWords(String s) {
        // 除去开头和末尾的空白字符
        s = s.trim();
        // 正则匹配连续的空白字符作为分隔符分割
        List<String> wordList = Arrays.asList(s.split("\\s+"));
        Collections.reverse(wordList);
        return String.join(" ", wordList);
    }
}
```

#### 代码2

```java
import java.util.Arrays;
import java.util.Collections;
import java.util.List;

class Solution {
    public String reverseWords(String s) {
        StringBuilder stringBuilder =trimSpace(s);

        //  翻转整个字符串
        reverse(stringBuilder,0,stringBuilder.length()-1);

        reverseWord(stringBuilder);
        return stringBuilder.toString();
    }

    private StringBuilder trimSpace(String s){
        int left = 0;
        int right = s.length() - 1;
        StringBuilder sb= new StringBuilder();
        while (left <= right && s.charAt(left) == ' ') {
            ++left;
        }

        // 去掉字符串末尾的空白字符
        while (left <= right && s.charAt(right) == ' ') {
            --right;
        }

        // 将字符串间多余的空白字符去除
        while (left <= right) {
            char c = s.charAt(left);

            if (c != ' ') {
                sb.append(c);
            } else if (sb.charAt(sb.length() - 1) != ' ') {
                sb.append(c);
            }

            ++left;
        }
        return sb;
    }

    private void reverse(StringBuilder sb,int left,int right){
        while (left < right){
            char temp = sb.charAt(left);
            sb.setCharAt(left++,sb.charAt(right));
            sb.setCharAt(right--,temp);
        }
    }

    private void reverseWord(StringBuilder sb){
        int n = sb.length();
        int start = 0,end = 0;
        while (start < n){
            while (end < n && sb.charAt(end) != ' '){
                end++;
            }
            reverse(sb,start,end-1);
            start = end + 1;
            end++;
        }
    }
}
```

#### 字符串类的区别

##### String

> 字符串***\*常量\****，但是它具有[**不可变性**](http://blog.csdn.net/daheiantian/archive/2010/12/20/6097353.aspx)，就是一旦创建，对它进行的任何修改操作都会创建一个**新的**字符串对象。

##### StringBuffer

> 字符串***\*可变量\****，是***\*线程安全\****的，和StringBuilder类提供的方法完全相同。

##### StringBuilder

> 字符串***\*可变量\****，是***\*线程不安全\****的。这个类是在JDK 5才开始加入的，是StringBuffer的**单线程**等价类。（String和StringBuffer类都是JDK 1.0开始）
