# 数据结构 优先队列


<!--more-->

## 介绍

## 大顶堆

```java
class MaxPQ {
    private int[] data;
    private int n; // 当前元素个数，因为索引从 1 开始，也是最后一个元素的下标

    public MaxPQ(int maxN) {
        data = new int[maxN + 1];
    }

    public MaxPQ(int[] nums) {
        data = new int[nums.length + 1];
        for (int x : nums) add(x);
    }

    public void add(int x) {
        if (n == data.length - 1) return;
        data[++n] = x;
        swim(n);
    }

    public int getMax() {
        if (isEmpty()) return -1;
        return data[1];
    }

    public int pollMax() {
        if (isEmpty()) return -1;
        swap(1, n--);
        sink(1);
        return data[n + 1];
    }

    public int size() {
        return n;
    }

    public boolean isEmpty() {
        return n == 0;
    }

    private void swim(int k) {
        if (k <= 1 || k > n) return;
        while (k > 1 && data[k / 2] < data[k]) {
            swap(k / 2, k);
            k /= 2;
        }
    }

    private void sink(int k) {
        if (k < 1 || k > n / 2) return;
        while (k * 2 <= n) {
            int i = k * 2;
            if (i + 1 <= n && data[i] < data[i + 1]) i++;
            if (data[k] >= data[i]) break;
            swap(k, i);
            k = i;
        }
    }

    private void swap(int i, int j) {
        int temp = data[i];
        data[i] = data[j];
        data[j] = temp;
    }
}
```

## 小顶堆

```java
class MinPQ {
    private int[] data;
    private int n; // 当前元素个数，因为索引从 1 开始，也是最后一个元素的下标

    public MinPQ(int maxN) {
        data = new int[maxN + 1];
    }

    public MinPQ(int[] nums) {
        data = new int[nums.length + 1];
        for (int x : nums) add(x);
    }

    public void add(int x) {
        if (n == data.length - 1) return;
        data[++n] = x;
        swim(n);
    }

    public int getMin() {
        if (isEmpty()) return -1;
        return data[1];
    }

    public int pollMin() {
        if (isEmpty()) return -1;
        swap(1, n--);
        sink(1);
        return data[n + 1];
    }

    public int size() {
        return n;
    }

    public boolean isEmpty() {
        return n == 0;
    }

    private void swim(int k) {
        if (k <= 1 || k > n) return;
        while (k > 1 && data[k / 2] > data[k]) {
            swap(k / 2, k);
            k /= 2;
        }
    }

    private void sink(int k) {
        if (k < 1 || k > n / 2) return;
        while (k * 2 <= n) {
            int i = k * 2;
            if (i + 1 <= n && data[i] > data[i + 1]) i++;
            if (data[k] <= data[i]) break;
            swap(k, i);
            k = i;
        }
    }

    private void swap(int i, int j) {
        int temp = data[i];
        data[i] = data[j];
        data[j] = temp;
    }
}
```

## 参考


