# 선택정렬(Selection Sort) - O(n<sup>2</sup>)

> 출처: [엔지니어대한민국 YouTube](https://www.youtube.com/channel/UCWMAh9cSkEn8v42YRO90BHA)

[![Selection Sort](http://img.youtube.com/vi/uCUu3fF5Dws/0.jpg)](https://www.youtube.com/watch?v=uCUu3fF5Dws)

## Java
```java
public class SelectionSort {
	private static void selectionSort(int[] arr) {
		selectionSort(arr, 0);
	}
	private static void selectionSort(int[] arr, int start) {
		if(start < arr.length - 1) {
			int min_index = start;
			for(int i = start; i < arr.length; i++) {
				if(arr[i] < arr[min_index]) {
					min_index = i;
				}
			}
			swap(arr, start, min_index);
			selectionSort(arr, start + 1);
		}
	}
	private static void swap (int[] arr, int index1, int index2) {
		int tmp = arr[index1];
		arr[index1] = arr[index2];
		arr[index2] = tmp;
	}
	private static void printArray(int[] arr) {
		for(int data : arr) {
			System.out.print(data + ",");
		}
		System.out.println();
	}
	public static void main(String[] args) {
		int[] arr = {3,6,1,8,2,4};
		printArray(arr);
		selectionSort(arr);
		printArray(arr);
	}
}
```

```
3,6,1,8,2,4,
1,2,3,4,6,8,
```
