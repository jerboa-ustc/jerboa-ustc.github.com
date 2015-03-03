---
layout: post
title: "素数算法"
categories: Algorithm
date: 2015-02-07 15:33:40
---

素数在计算机中经常被运用于计算机安全（密码相关的计算），所以研究一下素数的判断算法是相当有必要的。所以现在就来看一下两种比较常见的算法，试除法和Eratosthenes算法吧！
<!-- more -->

#### 1、试除法

用需要验证的数 N 逐个除以从 2 开始至 N-1 中的所有数，若能被一个数整除，表示它有一个因数，说明数 N 不是素数；若一直到 N-1 都不能被整除，则说明 N 是素数。（当然我们对于因数的判断不必计算到 N-1,只需要到 <img src="http://latex.codecogs.com/svg.latex?\sqrt{N}" border="0"/> 就可以了）

	{% highlight java %}
	public class Prime {

    public static boolean IsPrime(int num){
        for(int i=2;i*i<=num;++i){
            if(num % i==0){
                return false;
            }
        }
        return true;
    }

    public static void Usual(int size){
        int index = 0;
        for(int j=2;j<=size;++j){
            if(IsPrime(j)){
                index++;
                System.out.print(j + " ");
                if(index%10==0) System.out.print('\n');
            }
        }
    }

	public static void main(String[] args) {
        Usual(10000);
    }
	}
	{% endhighlight %}

#### 2、Eratosthenes算法

Eratosthenes算法的实现，其实就像是一个筛子，每次过滤掉合数，最后剩下的就是素数了，例如：如果要找出2~10000之间所有素数的算法，可以先过滤调用 2 的倍数，再过滤掉 3 的倍数，依次再5,7,11,13...97 就是<img src="http://latex.codecogs.com/svg.latex?\sqrt{10000}" border="0"/>以内的所有素数。剩下的就都是素数了。

#####数组的实现方法

	{% highlight java %}
	public class Prime {

    public static void Eratosthenes2(int size){
        boolean[] nums = new boolean[size];
        // false 代表是素数,默认是素数
        for(int i=2;i*i<=size;++i){
            if(!nums[i]){
                for(int j=i*i;j<nums.length;++j){
                    if(nums[j])continue;        //如果是合数就跳过
                    if(j%i==0){
                        nums[j]=true;
                    }
                }
            }
        }
        int index = 0;
        for(int i=2;i<size;++i){
            if(!nums[i]){
                index++;
                System.out.print(i + " ");
                if(index%10==0) System.out.print('\n');
            }
        }
    }

    public static void main(String[] args) {
        Eratosthenes2(10000);
    }
	}
	{% endhighlight %}

#####链表的实现方法

	{% highlight java %}
	package Algorithm;

	import java.util.LinkedList;

	public class Prime {

    public static void Eratosthenes(int size){
        LinkedList<Integer> nums = new LinkedList<Integer>();
        for(int i=2;i<= size;++i)nums.add(i);
        int ends = (int)Math.sqrt(size);
        for(int i=0;i<size-1;++i){
            if(nums.get(i) > ends)break;
            for(int j=i+1;j<size-1;++j){
                if(nums.get(j) % nums.get(i)==0){
                    nums.remove(j);
                    size--;
                }
            }
        }
        int index = 0;
        for(int i=0;i<size-1;++i){
            index++;
            System.out.print(nums.get(i) + " ");
            if(index%10==0) System.out.print('\n');
        }
    }

    public static void main(String[] args) {
        Eratosthenes(10000);
    }
	}
	{% endhighlight %}

按照时间复杂度来看，Eratosthenes算法的效率要高得多。但是在java中使用LinkedList的操作，效率却低得多，这就不是算法的问题了。。。。
