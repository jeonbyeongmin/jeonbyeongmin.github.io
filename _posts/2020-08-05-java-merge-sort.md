---
title: Merge Sort(합병 정렬)에 대해 설명해보겠다
comments: false
layout: single
share: false
---

<br/>
> 간단한 알고리즘으로 구현할 수 있는 대표적인 정렬은 다음과 같다.

1. Insertion Sort -- 삽입정렬
2. Selection Sort -- 선택정렬
3. Bubble Sort -- 버블정렬

<br/>
 &nbsp;&nbsp; 버블정렬과 선택정렬은 모든 경우에 $$O(n²)$$시간이 걸리고 삽입정렬 또한 최악의 경우 $$O(n²)$$시간이 걸리기 때문에 매우 비효율적인 알고리즘이라는 것을 알 수 있다. 

<br/>

> 다음은 복잡한 알고리즘으로 구현할 수 있는 대표적인 정렬이다.


1. Quick Sort -- 퀵정렬
2. Heap Sort -- 힙정렬
3. **Merge Sort -- 합병정렬**

<br/>
&nbsp;&nbsp; 힙정렬은 합병정렬과 마찬가지로 `Binary Tree`로 접근하는 알고리즘이기 때문에 모든 경우에 $$O(nlogn)$$시간이 걸린다. 실제 시간은 보통의 경우 다른 $$O(nlogn)$$정렬법들에 비해 오래 걸린다.  
<br/>
&nbsp;&nbsp; 퀵정렬의 내부 루프는 대부분의 컴퓨터 아키텍처에서 효율적으로 작동하도록 설계되어 있다. 그 이유는 메모리 참조가 지역화되어 있기 때문에 CPU 캐시의 히트율이 높아지기 때문이다. 최악의 경우 $$O(n²)$$시간이 걸리지만, 대부분의 실질적인 데이터를 정렬할 때 제곱 시간이 걸릴 확률이 거의 없도록 알고리즘을 설계하는 것이 가능하다.  
<br/>
&nbsp;&nbsp; 심지어 합병정렬은 추가적인 메모리가 필요하기 때문에 추가적인 메모리 할당이 불가능한 경우에는 합병정렬을 사용할 수 없다. 또한 배열을 생성하는 시간 때문에 보통 퀵정렬보다 느리다. 그래서 <u>대부분의 상황에서 빠르고 추가 메모리 할당이 필요없는 퀵정렬을 사용한다.</u> 

<br/>
	
> 아니 그렇다면 도대체 합병정렬을 왜 쓰는 것일까? 

&nbsp;&nbsp; 	결론부터 말하자면, **<span style="color:green">합병정렬</span>**은 `LinkedList` 의 정렬을 구현할 때 퀵정렬보다 유용하다. 그 이유는 `ArrayList` 와 달리 `LinkedList` 는 접근 연산에서 `head` 부터 접근하기 때문에 퀵정렬과 같은 임의적인 접근은 오버헤드(Overhead)가 증가할 수밖에 없다. 자세한 내용은 이후 퀵정렬을 다루는 포스트에서 작성하도록 하겠다.

<br/>
여하튼 그러한 이유로 지금은 합병정렬에 대해 설명해보겠다.
	
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


&nbsp;&nbsp; 먼저 `mergeSort()`의 전체 코드를 보자. 이제 코드 하나하나 뜯어보겠다.

<br/>
<br/>


```java
            int mid = (start+end) / 2;
            mergeSort(start, mid);
            mergeSort(mid+1, end);
```

&nbsp;&nbsp; 재귀함수(Recursive function)를 사용하는 부분이다. 정렬되지 않은 리스트를 절반으로 잘라 비슷한 크기의 두 부분 리스트로 나눈다. 리스트의 길이가 1 이하가 될 때까지 반복한다.
> 즉, 더이상 리스트를 분할할 수 없을 때까지 계속 두 부분으로 나눈다.

<br/>
<br/>


```java
            int p = start;
            int q = mid + 1;
            int idx = p;
```
&nbsp;&nbsp; 분할된 두 리스트의 첫 번째 인덱스(Index)를 `p`와 `q`에 저장해줄 것이다. 리스트를 분할한다고 표현한 것이 바로 이 인덱스를 가리키며 한 말이다. 재귀함수이기 때문에 자기 자신을 계속 반복하며 `p`나 `q`에 할당되는 숫자가 달라진다. 이때 `p`와 `q`를 통해 이후에 나올 코드에서 정복(Conquer), 결합(Combine), 복사(Copy)를 할 수 있다.
<br/>
> 여기서 `int idx = p;`를 해주는 것은 두 리스트를 합병하기 위해 만들어진 리스트 `temp[]`의 인덱스 계산을 위해 필요하기 때문이다. 바로 아래의 코드를 참고하자.

<br/>
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
> 1. 분할된 두 리스트의 마지막 인덱스는 각각 `mid`와 `end`이다.
> 2. `<`이 아니라 `<=`인 것은 리스트이 요소가 하나일 때에도 `while문`을 돌려주기 위함이다.
> 3. `&&`이 아니라 `||`인 것은 둘 중에 하나라도 끝이 나지 않았다면 `while문`을 돌려주어야 하기 때문이다.

&nbsp;&nbsp; 이제 분할된 두 개의 리스트에서 각각의 요소를 **비교**하여 `temp[]`에 넣어줄 것이다. `if문`에 조건으로 들어가는 `q > end`는 `q`에서 더이상 가져올 요소가 없다는 뜻이며, `(p <= mid && src[p] < src[q])`는 `p`에서 가져올 요소가 존재할 때, 두 리스트의 요소 중 더 작은 것을 `temp[]`에 넣어주기 위함이다. 
> 1. `if문` 안에서 요소를 `temp[]`로 가져올 때마다 `q++`을 하기 때문에, `q > end` 라는 것이 곧 `q`에서 가져올 요소가 없다는 뜻이다.
> 2. `else`는 반대의 경우이다.
> 3. `q > end`와 `(p <= mid && src[p] < src[q])`를 `&&`이 아니라 `||`로 묶어주는 것은 `q`에서 가져올 것이 없으면 무조건 `p`에서 가져와야 하기 때문이다.

<br/>
<br/>


```java
            for (int i = start; i <= end; i++){
                src[i] = temp[i];
            }
```
&nbsp;&nbsp; 이제 `temp[]`에 저장된 요소들을 다시 `src[]`에 가져오면 된다.
>`while문`에서 `idx`로 인덱스 계산을 해주었기 때문에 `src[]`로 가져올 때는 따로 계산없이 그대로 가져와야 한다.

<br/>
<br/>

## 03. Merge Sort의 시간복잡도
<br/>

&nbsp;&nbsp; 합병정렬은 `Binary Tree`형태로 분할하기 때문에 가질수 있는 최대 깊이는 $$logn$$이 된다. 이때, 각 분할 별로 합병을 진행하기 때문에 총 시간복잡도는 $$O(nlogn)$$이 된다. 다음 그림을 참고하도록 하자.

<br/>
![](/assets/images/123.png){: width="100%" height="100%"}



<script type="text/javascript" 
src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS_HTML">
</script>
