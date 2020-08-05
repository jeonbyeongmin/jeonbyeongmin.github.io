---
comments: false
layout: single
share: false
title: "[알고리즘] Merge Sort(합병 정렬)에 대해 설명해 보겠다"
---

<br/>
> 간단한 알고리즘으로 구현할 수 있는 대표적인 정렬은 다음과 같다.

1. Insertion Sort -- 삽입정렬
2. Selection Sort -- 선택정렬
3. Bubble Sort -- 버블정렬

<br/>

 &nbsp;&nbsp; 버블정렬과 선택정렬은 모든 경우에 `O(n²)`시간이 걸리고 삽입정렬 또한 최악의 경우 `O(n²)`시간이 걸리기 때문에 매우 비효율적인 알고리즘이라는 것을 알 수 있다. 

<br/>

> 다음은 복잡한 알고리즘으로 구현할 수 있는 대표적인 정렬이다.


1. Quick Sort -- 퀵정렬
2. Heap Sort -- 힙정렬
3. Merge Sort -- 합병정렬

<br/>

&nbsp;&nbsp; 퀵정렬은 이름만 보면 세상에서 가장 빠른 정렬같지만 사실은 그렇지 않다. 삽입정렬, 선택정렬, 버블정렬보다 평균의 경우 빠른 것은 사실이지만 최악의 경우 `O(n²)`시간이 걸리기 때문에 그 경우엔 비효율적인 알고리즘이라고 할 수 있다.
<br/>
&nbsp;&nbsp; 힙정렬은 합병정렬과 마찬가지로 `Binary Tree`로 접근하는 알고리즘이기 때문에 모든 경우에 `O(nlogn)`시간이라는 매우 짧은 시간이 걸린다. 하지만 어렵고, 가장 큰 값 몇 개를 찾을 때 더 유용한 알고리즘이다.

<br/>
&nbsp;&nbsp; 따라서 더 쉽고, 더 짧을 시간으로 구현할 수 있는 **합병정렬(Merge Sort)**을 알아두면 코딩할 때 짱좋다. 여튼 그런 이유로 지금부터 합병정렬에 대해 설명해 보겠다.
<br/>
<br/>
<br/>


## 01. Merge Sort 의 개념과 작동과정
<br/>

&nbsp;&nbsp; 합병정렬은 **분할정복(Divide and Conquer)**의 대표적인 알고리즘이다. 분할정복은 그대로 해결할 수 없는 문제를 작은 문제로 분할하여 문제를 해결하는 방법이나 알고리즘이다. 보통은 재귀함수(Recursive function)를 이용하여 구현한다.

<br/>

>합병정렬은 다음과 같이 작동한다.

![](https://upload.wikimedia.org/wikipedia/commons/c/cc/Merge-sort-example-300px.gif){: width="100%" height="100%"}

<br/>

1. 리스트의 길이가 1 이하라면 이미 정렬된 것으로 본다. 그렇지 않은 경우에는
2. 분할(Divide) : 정렬되지 않은 리스트를 절반으로 잘라 비슷한 크기의 두 부분 리스트로 나눈다.
3. 정복(Conquer) : 각 부분 리스트를 재귀적으로 합병 정렬을 이용해 정렬한다.
4. 결합(Combine) : 두 부분 리스트를 다시 하나의 정렬된 리스트로 합병한다. 이때 정렬 결과가 임시배열에 저장된다.
5. 복사(Copy) : 임시 배열에 저장된 결과를 원래 배열에 복사한다.

<br/>
<br/>
## 02.  Merge Sort의  JAVA 소스코드
<br/>

```java
public static int[] src;
public static int[] temp;
public static void mergeSort(int start, int end){
        if (start < end){

            int mid = (start+end) / 2;
            mergeSort(start, mid);
            mergeSort(mid+1, end);

            int p = start;
            int q = mid + 1;

            int idx = p;

            while (p <= mid || q <= end){ 
                if (q > end || (p <= mid && src[p] < src[q])){ 
                    temp[idx++] = src[p++];
                } else{
                    temp[idx++] = src[q++];
                }
            }

            for (int i = start; i <= end; i++){
                src[i] = temp[i];
            }
        }
    }
```


&nbsp;&nbsp; 먼저 `mergeSort()`의 전체 코드를 보자. 이렇게 보면 코린이 눈에는 별거 아닌 것처럼 보일 수 있겠지만 정말 별거 없다. 코드 하나하나 뜯어보겠다.

<br/>

```java
            int mid = (start+end) / 2;
            mergeSort(start, mid);
            mergeSort(mid+1, end);
```

&nbsp;&nbsp; 재귀함수(Recursive function)를 사용하는 부분이다. 정렬되지 않은 리스트를 절반으로 잘라 비슷한 크기의 두 부분 리스트로 나눈다. 리스트의 길이가 1 이하가 될 때까지 반복한다.
> 즉, 더이상 리스트를 분할할 수 없을 때까지 계속 두 부분으로 나눈다.

<br/>

```java
            int p = start;
            int q = mid + 1;
            int idx = p;
```
&nbsp;&nbsp; 분할된 두 리스트의 첫 번째 **Index**를 `p`와 `q`에 저장해줄 것이다. 리스트를 분할한다고 표현한 것이 바로 이 Index를 가리키며 한 말이다. 재귀함수이기 때문에 자기 자신을 계속 반복하며 `p`나 `q`에 할당되는 숫자가 달라진다. 이때 `p`와 `q`를 통해 이후에 나올 코드에서 정복(Conquer), 결합(Combine), 복사(Copy)를 할 수 있다.
<br/>
> 여기서 `int idx = p;`를 해주는 것은 두 리스트를 합병하기 위해 만들어진 리스트 `temp[]`의 인덱스 계산을 위해 필요하기 때문이다. 바로 아래의 코드를 참고하자.

<br/>

```java
            while (p <= mid || q <= end){
                if (q > end || (p <= mid && src[p] < src[q])){
                    temp[idx++] = src[p++];
                } else{
                    temp[idx++] = src[q++];
                }
```
&nbsp;&nbsp; `while문`의 조건에 `(p <= mid || q <= end)`이 들어가는 이유는 다음과 같다.
> 1. 분할된 두 리스트의 마지막 `index`는 각각 `mid`와 `end`이다.
> 2. `<`이 아니라 `<=`인 것은 리스트이 요소가 하나일 때에도 `while문`을 돌려주기 위함이다.
> 3. `&&`이 아니라 `||`인 것은 둘 중에 하나라도 끝이 나지 않았다면 `while문`을 돌려주어야 하기 때문이다.

&nbsp;&nbsp; 이제 분할된 두 개의 리스트에서 각각의 요소를 **비교**하여 `temp[]`에 넣어줄 것이다. `if문`에 조건으로 들어가는 `q > end`는 `q`에서 더이상 가져올 요소가 없다는 뜻이며, `(p <= mid && src[p] < src[q])`는 `p`에서 가져올 요소가 존재할 때, 두 리스트의 요소 중 더 작은 것을 `temp[]`에 넣어주기 위함이다. 
> 1. `if문` 안에서 요소를 `temp[]`로 가져올 때마다 `q++`을 하기 때문에, `q > end` 라는 것이 곧 `q`에서 가져올 요소가 없다는 뜻이다.
> 2. `else`는 반대의 경우이다.
> 3. `q > end`와 `(p <= mid && src[p] < src[q])`를 `&&`이 아니라 `||`로 묶어주는 것은 `q`에서 가져올 것이 없으면 무조건 `p`에서 가져와야 하기 때문이다.

<br/>

```java
            for (int i = start; i <= end; i++){
                src[i] = temp[i];
            }
```
&nbsp;&nbsp; 이제 `temp[]`에 저장된 요소들을 다시 `src[]`에 가져오면 된다.
>`while문`에서 `idx`로 Index 계산을 해주었기 때문에 `src[]`로 가져올 때는 따로 계산없이 그대로 가져와야 한다.

<br/>
<br/>

## 03. Merge Sort의 시간복잡도
<br/>

&nbsp;&nbsp; 합병정렬은 `Binary Tree`형태로 `Divide`하기 때문에 가질수 있는 최대 깊이는 `logn`이 된다. 이때, 각 분할 별로 합병을 진행하기 때문에 총 시간복잡도는 `O(nlogn)`이 된다. 다음 그림을 참고하도록 하자.

<br/>
![](/assets/images/123.png){: width="100%" height="100%"}
