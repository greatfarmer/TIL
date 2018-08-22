# Lesson 1: Binary Gap

## Task description
A binary gap within a positive integer N is any maximal sequence of consecutive zeros that is surrounded by ones at both ends in the binary representation of N.

For example, number 9 has binary representation 1001 and contains a binary gap of length 2. The number 529 has binary representation 1000010001 and contains two binary gaps: one of length 4 and one of length 3. The number 20 has binary representation 10100 and contains one binary gap of length 1. The number 15 has binary representation 1111 and has no binary gaps. The number 32 has binary representation 100000 and has no binary gaps.

Write a function:

`class Solution { public int solution(int N); }`

that, given a positive integer N, returns the length of its longest binary gap. The function should return 0 if N doesn't contain a binary gap.

For example, given N = 1041 the function should return 5, because N has binary representation 10000010001 and so its longest binary gap is of length 5. Given N = 32 the function should return 0, because N has binary representation '100000' and thus no binary gaps.

Assume that:

- N is an integer within the range [1..2,147,483,647].

Complexity:

- expected worst-case time complexity is O(log(N));
- expected worst-case space complexity is O(1).

## Solution

### Java
> http://blue-shadow.tistory.com/75

```java
class Solution {
    public int solution(int N) {
    	int max = 0;
    	int gap = 0;
    	boolean flag = false;

    	while (N != 0) {
    		if (N % 2 == 1) {
    			if (gap > max) {
    				max = gap;
    			}
    			gap = 0;
    			flag = true;
    		}else {
    			if (flag) {
    				gap++;
    			}
    		}
    		N = N / 2;
    	}

    	return max;
    }
}
```

## Comment
문제를 접근할 때, 10진수를 2진수로 변경하고 변경된 2진수의 String값을 가지고 0의 갯수를 찾고자 했다. <br>
예를 들어, 1041 -> 10000010001에서 문자 "1"의 index를 찾아서 그 차이를 비교해 최대 binary gap을 구하고자 한 것이다. <br>
이러한 시도로 문제를 풀 수 있겠지만 효율적이지 않다고 판단해, 기존의 문제 풀이를 참고해서 이해하고자 했다.
