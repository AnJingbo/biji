

java int 类整数的最大值是 2 的 31 次方 - 1 = 2147483648 - 1 = 2147483647

可以用 Integer.MAX_VALUE 表示它，即 int value = Integer.MAX_VALUE;

Integer.MAX_VALUE + 1 = Integer.MIN_VALUE = -2147483648

再大的数就要用 long （最大值 2 的 63 次方 - 1 ）或者 BigDecimal 表示

Java 八种基本类型 中表示整数的有：byte、short、int、long 这四种。

short 的 最小值是 -32768（-2^15）；
short 的 最大值是 32767（2^15 - 1）；
对应：
short max = Short.MAX_VALUE;
short min = Short.MIN_VALUE;