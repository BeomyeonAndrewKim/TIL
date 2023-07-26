# 정렬 알고리즘(Sorting Algorithm)

컴퓨너 분야에서 중요시되는 문제 가운데 하나로 어떤 데이터들이 주어졌을 때 이를 정해진 순서대로 나열하는 문제이다. 실제 컴퓨터 분야에서 사용하는 데이터의 경우 숫자의 순서나 어휘의 순서대로 정렬한 다음 사용해야 되는 경우가 거의 항상 발생하는데 이걸 얼마나 효과적으로 해결할 수 있느냐가 정렬 문제의 핵심이다.

데이터를 정렬해야 하는 이유는 탐색을 위해서이다. 사람은 수십에서 수백 개의 데이터를 다루는 데 그치지만 컴퓨터가 다뤄야 할 데이터는 보통이 백만 개 단위이며 데이터베이스 같은 경우 이론상 무한 개의 데이터를 다룰 수 있어야 한다. 탐색할 대상 데이터가 정렬되어 있지 않다면 순차 탐색 이외에 다른 알고리즘을 사용할 수 없지만 데이터가 정렬되어 있다면 이진 탐색이라는 강력한 알고리즘을 사용할 수 있다. 삽입과 삭제가 자주 되는 자료의 경우 정렬에 더 많은 시간이 들어가므로 순차 탐색을 하는 경우도 있지만 대부분의 경우 삼입/삭제보다는 데이터를 조회하는 것이 압도적으로 많고 조회에 필요한 것이 바로 검색이다.

이미  정렬된 데이터의 특징은 어떤 값을 임의로 집었을 때 해당 값을 집은 위치는 오른쪽에는 오른쪽에는 무조건 그것보다 크거나 값은 값이 있고, 그 위치의 왼쪽에는 무조건 그것보다 작거나 같은 값이 있다는 것이다. 따라서 컴퓨터가 어떤 값을 딱 집었을 때 찾고자 하는 값보다 집어올린 값이 작다면 그 위치보다 왼쪽은 볼 필요가 없다. 그보다 오른쪽만 보면 된다. 컴퓨터가 어떤 값을 집어오리는 위치가 후보군의 가운데인 탐색 알고리즘이 이진 탐색 알고리즘이다. 컴퓨터에서 정렬을 수행하는 이유 중 가장 큰 이유가 바로 이 이진 탐색이 가능한 데이터를 만들기 위해서다.

![sorting algorithms](https://i.imgur.com/fq0A8hx.gif)

source: https://www.toptal.com/developers/sorting-algorithms

## 1. 버블 정렬(Bubble Sorting)

1번째와 2번째 원소를 비교하여 정렬하고, 2번째와 3번째, ..., n-1번째와 n번째를 정렬한 뒤 다시 처음으로 돌아가 이번에는 n-2번째와 n-1번째까지, ...해서 최대 n(n-1)/n번 정렬한다. 한 번 돌 때마다 마지막 하나가 정렬되므로 원소들이 거품이 올라오는 것처럼 보여 거품정렬이다.

거의 모든 상황에서 최악의 성능을 보여준다. 단, 이미 정렬된 자료에서는 1번만 돌면 되기 때문에 최선의 성능을 보여준다. 가장 손쉽게 구현하여 사용할 수 있지만, 만들기가 쉽고 직관적일 뿐이지 알고리즘적 과점에서 보면 대단히 비효율적인 정렬방식이다.

![Bubble Sorting](http://codepumpkin.com/wp-content/uploads/2017/10/BubbleSort_Avg_case.gif)

source: [code pumbkin](http://codepumpkin.com/bubble-sort/)

## 2. 선택 정렬(Selection Sorting)

버를 정렬이 비교하고 바로 바꿔 넣는 걸 반복한다면 이쪽은 일단 1번째부터 끝까지 훑어서 가장 작은게 1번째, 2번째부터 끝까지 훑어서 가장 적은게 2번째, ...해서 (n-1)번 반복한다. 어떻게 정렬이 되어 있든 일관성 있게 n(n-2)/2 에 비례하는 시간이 걸린다는 게 특징. 또한, 버블 정렬보다 두 배 정도 빠르다

![Selection Sorting](http://codepumpkin.com/wp-content/uploads/2017/10/SelectionSort_Avg_case.gif)

source: [code pumpkin](http://codepumpkin.com/selection-sort-algorithms/)

## 2. 삽입 정렬(Insertion Sorting)

k번째 원소를 1부터 k-1까지와 비교해 적절한 위치에 끼워넣고 그 뒤의 자료를 한 칸씩 뒤로 밀어내는 방식이다. 자료구조에 따라서는 뒤로 밀어내는 시간이 큰 편이다. 다만 이미 정렬되어 있는 자료구조에 자료를 하나씩 삽입/제거하는 경우에는 현실적으로 최고의 정렬 알고리즘이 된다.

![Insertion Sorting](https://cdn-images-1.medium.com/max/1600/1*IK3Q4NBRLthllMINV3OxpQ.gif)

source: [Tony](https://medium.com/@fiv3star/%EC%A0%95%EB%A0%AC%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-sorting-algorithm-%EC%A0%95%EB%A6%AC-8ca307269dc7)

## 3. 병합 정렬(Merge Sorting)

원소 개수가 1 또는 0이 될 때까지 두 부분으로 쪼개서(두 개씩이 될때까지) 자른 순서의 역순으로 크기를 비교해 병합해 나간다. 병합된 부분 안은 이미 정렬되어 있으므로 전부 비교하지 않아도 제자리를 찾을 수 있다. 정렬되어 있는 두 배열을 합집할 할 때 이 알고리즘의 마지막 단계만을 이용하면 가장 빠르게 정렬된 상태로 합칠 수 있다.

![Merge Sorting](http://codepumpkin.com/wp-content/uploads/2017/10/MergeSort_Avg_case.gif)

source: [code pumpkin](http://codepumpkin.com/merge-sort-sorting-algorithm/)

## 4. 힙 정렬(Heap Sorting)

원소들을 전부 힙에 삽입한다. 그 후 힙의 루트에 있는 값은 남은 수들 중에서 최솟값(혹은 최댓값)을 가지므로 루트를 출력하고 힙에서 제거한다. 힙이 빌 때까지 이 과정을 계속 반복한다.

선택 정렬과 거의 유사한 알고리즘으로 단지 가장 큰 원소를 뒤로 보내는데에 매번 쭉 돌면서 알아내느냐 힙을 사용하여 알아내느냐가 유일한 차이점이다. 

![Heap Sorting])(https://blog.kakaocdn.net/dn/bzfBwF/btqFOM16NBX/AlAJkIe4uwtXHd6hThT7Kk/img.gif)

source: [나무위키](https://namu.wiki/w/%EC%A0%95%EB%A0%AC%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)

## 5. 퀵 정렬(Quick Sorting)

퀵이라는 이름에서 알 수 있듯이 평균적인 상황에서 최고의 성능을 나타낸다. 컴퓨터로 가장 많이 구현된 정렬 알고리즘 중 하나이다. 방식은 적절한 원소 하나를 기준(피벗, pivot)으로 삼아 그보다 작은 것을 앞으로 빼내고 그 뒤에 피벗을 옮겨 피벗보다 작은, 큰 것을 나눈뒤 나누어진 각각에서 다시 피벗을 잡고 정렬해서 각각의 크기가 0이나 1이 될 때까지 정렬한다.

현존하는 컴퓨터 아키텍처 상에서 비교 연산자를 이용하여 구현된 정렬 알고리즘 중 가장 고성능인 알고리즘이 바로 이 퀵정렬이다. 

![Quick Sorting](https://www.tutorialspoint.com/data_structures_algorithms/images/quick_sort_partition_animation.gif)

source: [tutorialspoint](https://www.tutorialspoint.com/data_structures_algorithms/quick_sort_algorithm.htm)

Reference [나무위키 정렬알고리즘](https://namu.wiki/w/%EC%A0%95%EB%A0%AC%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)
