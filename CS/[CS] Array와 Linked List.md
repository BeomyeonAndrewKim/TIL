# Array vs Linked List

## Array

### 정의

배열이란 연관된 데이터를 하나의 변수에 그룹핑해서 관리하기 위한 방법입니다. 배열을 이용하면 하나의 변수에 여러 정보를 담을 수 있고, 반복문과 결합하면 많은 정보도 효율적으로 처리할 수 있습니다.

### 장점

- 논리적 저장 순서와 물리적 저장 순서가 일치합니다. 
- ```인덱스(index)```로 해당 원소(element)에 접근할 수 있습니다. 
- **random access** 가 가능합니다.

### 단점

- 삭제 또는 삽입의 과정에서는 해당 원소에 접근하여 작업을 완료한 뒤, 또 한 가지의 작업을 추가적으로 해줘야 하기 때문에, 시간이 더 걸립니다.
- 만약 배열의 원소 중 어느 원소를 삭제했다고 했을 때, 배열의 연속적인 특징이 깨지게 됩니다.. 즉 빈 공간이 생기는 것입니다.. 
- 삭제나 삽입을 할 시 원소보다 큰 인덱스를 갖는 원소들을 `shift`해줘야 하는 시간적 비용(cost)이 발생합니다.

#### Array List

Array List는 배열을 이용해서 리스트를 구현한 것을 의미한다. Array와 다른 점은 크기가 가변적입니다. 저장하는 데이터 수에 따라 크기가 변경됩니다.

## Linked List

### 정의

Linked List는 Array List와는 다르게 엘리먼트와 엘리먼트 간의 연결(link)을 이용해서 리스트를 구현한 것을 의미합니다. 그래서 이름도 linked list입니다.

### 장점

- 각각의 원소들은 자기 자신 다음에 어떤 원소인지만을 기억하고 있습니다. 
- Linked List는 Tree 구조의 근간이 되는 자료구조이며, Tree에서 사용되었을 때 그 유용성이 드러납니다.

### 단점 

- 원하는 위치에 삽입 혹은 삭제를 하고자 하면 원하는 위치를 Search 과정에 있어서 첫번째 원소부터 다 확인해봐야 합니다.
- 논리적 저장 순서와 물리적 저장 순서가 일치하지 않습니다.

Reference

- [Interview_Question_for_Beginner by Jbee](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/DataStructure)
- [초보몽키의 개발공부로그](https://wayhome25.github.io/cs/2017/04/17/cs-18-1/)
- [생활코딩](https://opentutorials.org/module/1335/8636)