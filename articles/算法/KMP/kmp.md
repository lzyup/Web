
### 背景
![image](https://raw.githubusercontent.com/lzyup/Web/master/articles/%E7%AE%97%E6%B3%95/KMP/KMP1.jpg)
- 如图，当模式串和主串匹配的过程中，把不能匹配的那个字符叫做**坏字符**，把已经匹配的那段字符串叫做**好字符**。

![image](https://raw.githubusercontent.com/lzyup/Web/master/articles/%E7%AE%97%E6%B3%95/KMP/KMP2.jpg)
- 如图，KMP算法就是试图在模式串去匹配主串的过程中，遇到坏字符后，对于已经对比过的好前缀，能否找到一种规律，将模式串一次性滑动很多位

### 1、核心思想

- 设s为目标串，t为模式串，并设i指针和j指针分别指示目标串和模式串中正待比较的字符，令i和j的初值均为0。若有si=tj，则i和j分别增1；否则，i不变，j退回到`j=next[j]` 的位置（即模式串右滑），比较si和tj,若相等则指针各增1，否则j在退回到下一个j=next[j]的位置(即模式串继续右滑)，再比较si和tj。以此类推，直到出现下列两种情况之一为止：一种情况是j退回到某个j=next[j]位置时有si=tj,则指针各增1后继续匹配；另一种情况是j退回到j=-1，此时令i,j指针各增1，即从si+1和t0开始继续匹配。


### 3、分析如何滑动很多位
![image](https://raw.githubusercontent.com/lzyup/Web/master/articles/%E7%AE%97%E6%B3%95/KMP/KMP3.jpg)
如图，我们观察到，只需要拿好前缀本身，在它的后缀子串中，查找最长的那个可以跟好前缀的前缀子串匹配的。
比如最长的可匹配的那部分前缀子串是{v},长度是k。我们把模式串一次性往后滑动j-k位，相当于，每次遇到坏字符的时候，我们把**j更新为k**,i不变，然后继续比较。

4、区分基本概念
![image](https://raw.githubusercontent.com/lzyup/Web/master/articles/%E7%AE%97%E6%B3%95/KMP/KMP4.jpg)
- 好前缀的所有后缀子串中，最长的可匹配前缀子串的那个后缀子串，叫做**最长可匹配后缀子串**；对应的前缀子串，叫做**最长可匹配前缀子串**


### 2、next数组
- 数组的下标是每个前缀结尾字符下标，数组的值是这个前缀的最长可以匹配前缀子串的结尾**字符下标**