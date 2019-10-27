```
/**
 * 给出一个32bit的整型，将其逆序为一个整型
 */
public class ReverseInteger {

    public static void main(String[] args) {
        System.out.println(reverse(-12345));
    }


    public static final int reverse(int num) {
        long ret = 0;

        while (num != 0) {
            ret = ret * 10 + num % 10;
            num /= 10;
        }

        if (ret > Integer.MAX_VALUE) {
            return Integer.MAX_VALUE;
        } else if (ret < Integer.MIN_VALUE) {
            return Integer.MIN_VALUE;
        } else {
            return (int) ret;
        }
    }
}
```
