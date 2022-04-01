# 数学

## **丑数**
<https://www.nowcoder.com/practice/6aa9e04fc3794f68acf8778237ba065b?tpId=117>

```
int GetUglyNumber_Solution(int idx) {  
      int i=0,j=0,k=0,now;//i,j,k分别为指向下一个*2,*3,*5可能成为下一个丑数的数的位置的指针  
      vector<int> v(1,1);//放入1个1  
      while(v.size()<idx){//v中的数量为为idx时候，停止循环  
	  now=min(v[i]*2,min(v[j]*3,v[k]*5));//三个指针运算的结果中找，下一个丑数  
	  v.push_back(now);//将下一个丑数入队  
	  if(v[i]*2==now)i++;//下一个丑数可以由v[i]*2得到，则i指针后移  
	  if(v[j]*3==now)j++;//下一个丑数可以由v[j]*3得到，则j指针后移  
	  if(v[k]*5==now)k++;//下一个丑数可以由v[k]*5得到，则k指针后移  
	  //此处不能写if -else ，因为可能存在v[i]*2==v[j]*3这种情况  
	  //那么在下一次循环中，v[j]*3就会被再次选中，这样就会造成v中有重复元素出现  
      }  
      return v[idx-1];//此处元素不能写now,当idx==1时被hack  
  }  
```

## 统计质数
https://blog.csdn.net/suoxd123/article/details/104116131

统计所有小于非负整数 n 的质数的数量。

* 解法一（检测）
  >详情查看链接
* 解法二（赋值）
  ```
    public class Solution {
        public int CountPrimes(int n) {
            bool [] r = new bool[n];
            for(int i=0;i<n;i++)
            {
                r[i] = false;
            }
            int c = 0;
            for(int i=2;i<n;i++)
            {
                if(r[i] == false)
                {
                    c++;
                    for(int j=i*2;j<n;j+=i)
                    {
                        r[j] = true;
                    }
                }
            }
            return c;
        }
    }
  ```
