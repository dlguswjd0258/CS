# :fist_right: Binary Tree 3가지 순회:fist_left:

<img src="C:\Users\multicampus\Desktop\git\CS\Algorithm\img\binary_tree.png" alt="binary_tree" style="zoom:50%;" />



### 1. Inorder

- root 노드를 시작으로 **left - > root -> right** 순으로 순회
- 자신의 노드 중심으로 자식 노드 순회 중에(in) 순회
- 위 사진 inorder : 4  6  12  8  9  10



### 2. Preorder

- root 노드를 시작으로 **root -> left -> right**순으로 순회

- pre를 before라고 생각하면 자식 노드들을 순회하기 전 자신의 노드를 먼저 순회
- 위 사진 preorder : 8  6  4  12  9  10



### 3. Postorder

- root 노드를 시작으로 **left -> right -> root**순으로 순회
- post를 after의 의미로 자식 노드들을 순회하고난 후 순회
- 위 사진 preorder : 4  12  6  10  9  8

