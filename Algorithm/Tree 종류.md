# :fist_right:Tree​ ​종류:fist_left:

## Tree란?

- Array, LinkedList, Stack, Queue는 Line처럼 생긴 일직선 데이터 구조

- Tree는 부모-자식 관계를 가지는 데이터 구조이며 계층, 그룹 존재

<img src="https://postfiles.pstatic.net/MjAyMTAzMjNfMjUy/MDAxNjE2NDcwMTk1ODYy.wzP5pSGqlg2lyr7qfZ52psyPq3Nji-2WkBV2b_gDnGYg.g_-FN8GMhrOvZk8ooJ1YB98Cw02zPKGsnXXHcwvmTBMg.PNG.adlguswjda/image.png?type=w773" alt="img" style="zoom:80%;" />



**balanced가 맞는 대표적인 tree**

- red-black-tree
- AVL tree



----

## Binary Tree (이진 트리)

- 자식 노드가 최대 2개까지만 붙는 Tree

<img src="C:\Users\multicampus\Desktop\git\CS\Algorithm\img\binary_tree.png" alt="binary_tree" style="zoom:50%;" />



### Binary Search Tree (이진 검색 트리)

- binary tree에서 자신의 노드와 비교했을 때 왼쪽 자식들은 데이터가 작아야하고 오른쪽 자식들은 데이터가 커야 한다.
- 찾고 싶은 값이 자신의 노드보다 작은 값이라면 왼쪽으로, 큰 값이라면 오른쪽으로 가서 찾으면 된다.

<img src="C:\Users\multicampus\Desktop\git\CS\Algorithm\img\binary_seach_tree.png" alt="binary_seach_tree" style="zoom:50%;" />



### Complete Binary Tree (완전 이진 트리)

- 모든 노드들이 레벨별로 왼쪽부터 채워진 tree

![complete_binary_tree](C:\Users\multicampus\Desktop\git\CS\Algorithm\img\complete_binary_tree.png)





### Full Binary Tree vs Perfect Binary Tree

![full_perfect_binary_tree](C:\Users\multicampus\Desktop\git\CS\Algorithm\img\full_perfect_binary_tree.png)



**Full Binary Tree** 

- 노드들의 자식 노드가 하나도 없거나 두 개로 구성된 tree



**Perfect Binary Tree**

- 모든 레벨에 노드가 비어 있지 않는 tree
- 완벽한 피라미드 구조
- 노드 개수 : 2^n -1





[이미지 참고](https://www.youtube.com/watch?v=LnxEBW29DOw&t=0s)
