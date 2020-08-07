---
title: Counting Sort(카운팅 정렬)에 대해 설명해보겠다.
share: false
comments: false
---

<br/>

&nbsp; &nbsp; 정렬에 대한 알고리즘은 다양하다. 빠른 속도를 보여주는 정렬은 $$O(nlogn)$$시간이 걸리는 퀵 정렬 (Quick Sort), 힙 정렬 (Heap Sort), 합병 정렬 (Merge Sort)이 있고, 그 중에서 대부분의 경우에 퀵 정렬을 쓴다고 소개했었다.
<br/>

&nbsp; &nbsp;  하지만 오늘 설명할 <span style = "color:green"> **카운팅 정렬 (Counting Sort)**</span>의 시간복잡도는 $$O(n+k)$$이며 $$n > k$$인 경우에는 퀵 정렬보다 훨씬 빠른 성능을 자랑한다.

<br/>
> 그럼 왜 대부분의 경우에서 퀵 정렬을 사용할까

1.  카운팅 정렬은 추가적인 메모리 할당이 필요하다.
2.  정렬해야하는 수의 범위를 모른다면 사용하기가 곤란하다.
3.  수열의 크기$$(n)$$보다 정렬해야하는 수의 최대값$$(k)$$의 가 크다면 시간이 비약적으로 증가한다. 

<br/>

## 01. Counting Sort의 개념과 동작과정
<br/>

&nbsp; &nbsp; 보통 정렬을 할 때에는 비교를 하기 마련이다. 합병 정렬도 그렇고, 퀵 정렬도 그렇고, 힙 정렬도 마찬가지이다. 이런 비교 기반 정렬법들은 어지간하면 시간 복잡도가 $$O(nlogn)$$보다 짧아지기가 어려운데, 카운팅 정렬은 $$O(n+k)$$이다. 이게 어떻게 된 일일까.

> Counting Sort는 비교 기반 정렬이 아니다.

&nbsp; &nbsp; 카운팅 정렬은 기본적으로 비교 연산이 들어가는 정렬이 아니다. 카운팅 정렬의 동작 과정을 보며 카운팅 정렬의 정렬 방법을 알아보자. (변수의 이름은 밑의 애니메이션의 변수와 같으니 애니메이션을 참고하자)
1. 먼저 카운팅 정렬은 수를 세주는 배열 `c[]`가 필요하다. 정렬이 필요한 배열 `a[]`의 각 `index`를 방문하여 `a[index] = k`일때  `c[k]++`를 해주어 `k`가 몇 번 반복되었는 지 세줄 것이다.
2. 실제 정렬을 했을 때 그 숫자가 처음 시작하는 `index`를 알기 위하여 `c[i] = c[i] + c[i-1]`를 통해 카운트의 누적합을 적용한다.
3. 이제 `a[]`를 아래 애니메이션과 같은 과정으로 `b[]`에 정렬해줄 것이다.

![](https://spagnuolocarmine.github.io/assets/img/count.gif)

<br/>

## 02. Counting Sort JAVA 소스코드
<br/>

변수의 이름은 위 설명의 변수와 같으니 설명과 애니메이션을 참고하자

```java
public static void main(String[] args){

        int a[] = new int[10];
        int c[] = new int[20];
        int b[] = new int[a.length];

        for(int i = 0; i < a.length; i++) { // 00. a에 랜덤 원소 할당
            a[i] = (int)(Math.random()*20);
        }

        for (int i = 0; i < a.length; i++) { // 01. 카운팅
           c[a[i]]++;
        }
				
        for (int i = 1; i < c.length; i++){ // 02. 카운트 누적합
           c[i] = c[i] + c[i-1];
        }
				
        for (int i = a.length-1; i >= 0; i--){ // 03. 누적합를 통해 실질적인 정렬을 담당
           c[a[i]]--;
           b[c[a[i]]] = a[i];
        }


        // 출력
        for (int i = 0; i < a.length; i++){
            System.out.print(" | " + a[i]);
        }
        System.out.println();
        for (int i = 0; i < b.length; i++){
            System.out.print(" | " + b[i]);
        }
}
```

<br/>

만약 단순히 정렬된 출력이 필요하다면 2번, 3번, 출력 부분을 생략하고 다음과 같은 코드로 대신하여도 된다.

```java
        for (int i = 0; i < c.length; i++){
            while(c[i] > 0){
                System.out.println(i);
                count[i]--;
            }
        }
```

<br/>
## 03. Counting Sort의 시간복잡도
<br/>

&nbsp; &nbsp;  카운팅 정렬의 시간복잡도는 $$O(n+k)$$라고 하였다.  수열의 크기$$(n)$$보다 정렬해야하는 수의 최대값$$(k)$$이 크다면 시간이 비약적으로 증가한다. 이제 이 말이 무슨 의미인지 감이 잡힌다. 
<br/>

&nbsp; &nbsp;  예를 들어 최대값이 1억이고 수열의 크기가 5인 수열이 있다고 가정해보자. 이 수열을 정렬해야 하는데 5개의 수를 정렬하기 위해 크기가 1억인 배열을 만들어야 한다.
> 수열의 크기보다 수의 최대값이 크면 배열의 크기가 쓸때없이 커져버리기 때문에 비효율적일 수밖에 없다.

&nbsp; &nbsp;  그에 비해 퀵 정렬은 수의 최대값에 영향을 받지도 않고, 추가적인 메모리 공간이 필요하지도 않기 때문에 정렬이 필요한 대부분의 상황에서 퀵 정렬을 사용하는 것이다.


<script type="text/javascript" 
src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS_HTML">
</script>
