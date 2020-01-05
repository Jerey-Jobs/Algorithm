### 问题
实现atoi方法，将string转换为integer

### 分析
看似乎简单，但实际上，需要考虑很多情况，包括正负，空格，等等


### Code
``` java
public class Add {

    public static void main(String[] args) {
        System.out.println(atoi(" -5774543432434"));
    }

    public static int atoi(String str) {
        Integer ret = 0;

        if (str == null || str.length() == 0) {
            return 0;
        }

        int i = 0;
        // trim start
        while (i < str.length() && str.charAt(i) == ' ') {
                i++;
        }

        int signFlag = 1;
        if (str.charAt(i) == '-') {
            signFlag = -1;
            i++;
        } else if (str.charAt(i) == '+') {
            signFlag = 1;
            i++;
        }

        for (; i < str.length(); i++) {
            int tmpChar = str.charAt(i) - '0';
            if (tmpChar < 0 || tmpChar > 9) {
                throw new IllegalArgumentException();
            }

            if (signFlag == 1 && ret > (Integer.MAX_VALUE / 10) && tmpChar > 7) {
                return Integer.MAX_VALUE;
            } else if (signFlag == -1 && ret > (Integer.MAX_VALUE / 10) && tmpChar > 8) {
                return Integer.MIN_VALUE;
            } else {
                ret = tmpChar + ret * 10;
            }
        }
        return ret * signFlag;
    }
}



```
