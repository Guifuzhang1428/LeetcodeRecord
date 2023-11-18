#### KMP算法

​	1.定义前缀表：记录下标i（包括i)之前的字符串有多长的相同前后缀。

前缀：从首元素开始但不包含最后一个字符的连续子串

后缀：不包含第一个元素但以最后一个字符结束的连续子串



// 后缀数组的含义



前缀表是用来回退的，回退到当前不匹配字符上一个字符串中的最长匹配串（实际上这个长度就是重新开始匹配的索引位置（这里是以前缀表为next数组的情况）
$$
mathc\ str: a\ a\ b\ a\ a\ b\ a\ a\ f\ a \\
mode\ str:a \ a \ b \ a \ a \ f
$$
首先第一步求模式串的next数组，这里以前缀表为next数组，而不是以前缀表全体-1的写法

```C++
void getNext(int *next[], const string& s){
    next[0] = 0;   // 一个字符的最长相等前后缀为0（注意前后缀的定义）
    int j = 0;    // 表示以索引j结尾的前缀，next[j]表示s[0...j]中最长相同前后缀的长度
    for(int i = 1; i < s.size(); i++){  // i表示以s[i]结尾的后缀
        while(j > 0 && s[j] != s[i]){
            j = next[j-1];   // 索引j回退到上一字符匹配前缀的下一位置
        }
        
        if(s[i] == s[j]){
            j++;
        }
        
        next[i] = j;
        
    }
}
```

通过上述步骤，我们得到了 string s的前缀表，此时我们就可以利用它利用模式匹配了

```C++
int strStr(string haystack, string needle) {
    if(needle.size() == 0){ // 模式串为空，肯定匹配不上
        return 0;
    }
    int next[needle.size()];
    getNext(next, needle);  // 得到next数组
    int j = 0;
    for(int i = 0; i < haystack.size(); i++){  // 同样是回退的方法
        while(j > 0 && haystack[i] != needle[j]){
            j = next[j-1]; // 不匹配的时候利用上一个元素的匹配前缀，避免不必要的重复
        }
        
        if(haystack[i] == needle[j]){
            j++;
        }
        
        if(j == needle.size()){
            return i - needle.size() + 1; // 返回的是匹配的第一个位置索引
        }
    }
    return -1;
}
```

