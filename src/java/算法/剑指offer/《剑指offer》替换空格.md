# 《剑指offer》系列--替换空格

>*阳光总在风雨后，请相信一定有彩虹。*

### 题目

请实现一个函数，把字符串中的每个空格替换成"%20"，例如“We are happy.”，则输出“We%20are%20happy.”。

### 解析

```java
public class Test03 {
    public static String replaceSpace(String str){
        StringBuilder sb = new StringBuilder();
        for (int i = 0;i<str.length();i++){
            if (str.charAt(i) == ' '){
                sb.append("%20");
                continue;
            }
            sb.append(str.charAt(i));
        }
        return sb.toString();
    }

    public static void main(String[] args) {
        String str = "we are happy.";
        System.out.println(replaceSpace(str));
    }
}
```

输出结果：true

