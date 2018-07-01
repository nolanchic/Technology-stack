# 快速排序

### 基本思想：
快速排序（Quicksort）是对冒泡排序的一种改进。他的基本思想是：通过一趟排序将待排记录分割成独立的两部分，其中一部分的关键字均比另一部分记录的关键字小，则可分别对这两部分记录继续进行快速排序，整个排序过程可以递归进行，以达到整个序列有序的目的。

### 基本算法步骤：
举个栗子： 
假如现在待排序记录是：

```
6   2   7   3   8   9
```

第一步、创建变量 $low 指向记录中的第一个记录，$high 指向最后一个记录，$pivot 作为枢轴赋值为待排序记录的第一个元素（不一定是第一个），这里：

```
$low = 0;
$high = 5;
$pivot = 6;
```
第二步、我们要把所有比 $pivot 小的数移动到 $pivot 的左面，所以我们可以开始寻找比6小的数，从 $high 开始，从右往左找，不断递减变量 $high 的值，我们找到第一个下标 3 的数据比 6 小，于是把数据 3 移到下标 0 的位置（$low 指向的位置），把下标 0 的数据 6 移到下标 3，完成第一次比较：

```
//这时候,$high 减小为 3
$low = 0;
$high = 3;
$pivot = 6;
```

第三步、我们开始第二次比较，这次要变成找比 $pivot 大的了，而且要从前往后找了。递加变量 $low，发现下标 2 的数据是第一个比 $pivot 大的，于是用下标 2 （$low 指向的位置）的数据 7 和 指向的下标 3 （$high 指向的位置）的数据的 6 做交换，数据状态变成下表：

```
3   2   6   7   8   9

//这时候,$high 减小为 3
$low = 2;
$high = 3;
$pivot = 6;
```

完成第二步和第三步我们称为完成一个循环。

第四步（也就是开启下一个循环）、模仿第二步的过程执行。 
第五步、模仿第三步的过程执行。

执行完第二个循环之后，数据状态如下：

```
3   2   6   7   8   9

//这时候,$high 减小为 3
$low = 2;
$high = 2;
$pivot = 6;
```

到了这一步，我们发现 $low 和 $high“碰头”了：他们都指向了下标 2。于是，第一遍比较结束。得到结果如下，凡是 $pivot(=6) 左边的数都比它小，凡是 $pivot 右边的数都比它大。

然后，对 、$pivot 两边的数据 {3，2} 和 {7，8，9}，再分组分别进行上述的过程，直到不能再分组为止。

注意：第一遍快速排序不会直接得到最终结果，只会把比k大和比k小的数分到k的两边。为了得到最后结果，需要再次对下标2两边的数组分别执行此步骤，然后再分解数组，直到数组不能再分解为止（只有一个数据），才能得到正确结果。

> Partition（）函数才是整段代码的核心，因为该函数的功能是：
选取当中的一个关键字，比如选择第一个关键字。然后想尽办法将它放到某个位置，
使得它左边的值都比它小，右边的值都比它大，我们将这样的关键字成为枢轴（pivot）。

## 算法实现：

### PHP实现

```
//交换函数
function swap(array &$arr,$a,$b){
    $tmp = $arr[$a];
    $arr[$a] = $arr[$b];
    $arr[$b] = $tmp;
}

//选取数组当中的一个关键字，使得它处于数组某个位置时，左边的值比它小，右边的值比它大，该关键字叫做枢轴
//使枢轴记录到位，并返回其所在位置
function Partition(&$arr,$low,$hight){
    $pivot_value = $arr[$low];  //选取子数组第一个元素作为枢轴
    while ($low < $hight)   //从数组的两端交替向中间扫描（当 $low 和 $high 碰头时结束循环）
    {
        while ($low < $hight && $arr[$hight] > $pivot_value)  //如果改为$arr[$hight] >= $pivot_value，则最坏情况不但会堕落为O(n*n).而且除了每次比较的消耗外，还会产生n次交互的额外开销
        {
            $hight--;
        }
        swap($arr,$low,$hight);  //终于遇到一个比$pivot小的数，将其放到数组低端
        while ($low < $hight && $pivot_value > $arr[$low])    //如果改为$pivot_value >= $arr[$low]，则最坏情况不但会堕落为O(n*n).而且除了每次比较的消耗外，还会产生n次交互的额外开销
        {
            $low++;
        }
        swap($arr,$low,$hight);  //终于遇到一个比$pivot大的数，将其放到数组高端
    }

    return $low;  //返回high也行，毕竟最后low和high都是停留在pivot下标处
}

function Qsort(array &$arr, $low, $hight){
    //当 $low >= $hight 时表示不能再进行分组，已经能够得出正确结果了
    if($low >= $hight){
        return ;
    }
    $pivot_index = Partition($arr,$low,$hight);  //将$arr[$low...$high]一分为二，算出枢轴值
    Qsort($arr,$low,$pivot_index-1);    //对低子表（$pivot左边的记录）进行递归排序
    Qsort($arr,$pivot_index+1,$hight);  //对高子表（$pivot右边的记录）进行递归排序
    
}
//主函数
function QuickSort(array &$arr){
    $low = 0;
    $hight = count($arr) -1;
    Qsort($arr,$low,$hight);   //主函数中，由于第一遍快速排序是对整个数组排序的，因此开始是 $low=0,$high=count($arr)-1。
}

//调用
echo "<pre>";
$arr = [6,2,7,3,8,9];
QuickSort($arr);
print_r($arr);

```

### GOLANG实现
```
//选取数组当中的一个关键字，使得它处于数组某个位置时，左边的值比它小，右边的值比它大，该关键字叫做枢轴
//使枢轴记录到位，并返回其所在位置
func Partition(arr []int,low int , hight int) int{
	piovt_value := arr[low]
	for low<hight {
		for low < hight && arr[hight] > piovt_value{
			hight--
		}
		arr[low],arr[hight] = arr[hight],arr[low]
		for low < hight && arr[low] <  piovt_value{
			low++
		}
		arr[low],arr[hight] = arr[hight],arr[low]
	}

	return low
}

func Qsort(arr []int , low int ,hight int)  {
	if low >= hight {
		return
	}
	piovt_index := Partition(arr,low,hight)   //将$arr[$low...$high]一分为二，算出枢轴值
	Qsort(arr,low,piovt_index-1)    //对低子表（$pivot左边的记录）进行递归排序
	Qsort(arr,piovt_index+1,hight)  //对高子表（$pivot右边的记录）进行递归排序
}
func QuickSort(arr []int){
	low :=0
	hight := len(arr)-1
	Qsort(arr,low,hight);
}

func main() {
	arr := [...]int{6,3,8,2,9,1}
	slice := arr[:]
	fmt.Println(slice)
	fmt.Println()
	QuickSort(slice)
	fmt.Println(slice)
	
}
```


### PHP快速排序，代码简单但性能较低的一种实现
```

function quick_sort($arr) {
    //先判断是否需要继续进行
    $length = count($arr);
    if($length <= 1) {
        return $arr;
    }
    //如果没有返回，说明数组内的元素个数 多余1个，需要排序
    //选择一个标尺
    //选择第一个元素
    $base_num = $arr[0];
    //遍历 除了标尺外的所有元素，按照大小关系放入两个数组内
    //初始化两个数组
    $left_array = array();//小于标尺的
    $right_array = array();//大于标尺的
    for($i=1; $i<$length; $i++) {
        if($base_num > $arr[$i]) {
            //放入左边数组
            $left_array[] = $arr[$i];
        } else {
            //放入右边
            $right_array[] = $arr[$i];
        }
    }
    //再分别对 左边 和 右边的数组进行相同的排序处理方式
    //递归调用这个函数,并记录结果
    $left_array = quick_sort($left_array);
    $right_array = quick_sort($right_array);
    //合并左边 标尺 右边
    return array_merge($left_array, array($base_num), $right_array);

```





参考文档

 [PHP实现排序算法----快速排序（Quick Sort）、快排 - CSDN博客](https://blog.csdn.net/baidu_30000217/article/details/53311840)
 
 [快速排序的php实现 - 根号五 - 博客园](https://www.cnblogs.com/firstForEver/p/5155393.html)
 
 大话数据结构