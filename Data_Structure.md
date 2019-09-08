# Data Structure



[TOC]

## Tree

 트리는 비선형자료구조이다. 계층적 관계를 표현한다. 

- 노드 : 트리를 구성하고 있는 각각의 요소
- 간선(Edge) : 트리를 구성하기 위해 노드와 노드를 연결하는 선
- Root Node : 최상위에 있는 노드
- Leaf Node(단말노드) : 하위에 다른 노드가 연결되지 않은 노드
- Internal Node : 단말노드를 제외한 모든 노드.



### Binary Tree

 루트 노드를 중심으로 두 개의 서브 트리로 나누어지는 트리. 각 서브 트리도 두 개의 서브트리가 있다. 

- 완전 이진 트리

  왼쪽에서 오른쪽 순서대로 차곡차곡 채워진 이진 트리를 완전 이진트리라고 한다. 배열로 구성했을 경우, 노드의 개수가 n개 일 때, i번째 노드에 대해 parent(i) = i/2, left_child(i) = 2i, right_child(i) = 2i+1의 인덱스 값을 갖는다.

### Binary Search Tree

 효율적인 탐색을 위한 저장방법에 해당한다. 이진 탐색 트리는 이진 트리의 일종이며 다음의 규칙을 따른다.

1. 이진 탐색 트리의 노드에서 저장된 키는 유일하다
2. 루트 노드의 키가 왼쪽 서브 트리를 구성하는 어떠한 노드의 키보다 크다.
3. 루트 노드의 키가 오른쪽 서브 트리를 구성하는 어떠한 노드의 키보다 작다.
4. 왼쪽과 오른쪽 서브트리도 이진 탐색 트리이다.

 O(log n)의 시간복잡도를 갖는다. 트리의 편향이 생길수 있다는 단점이 있다. 이미 정렬된 순서대로 트리에 삽입할 경우 한쪽 노드로만 저장이 될 것이다. 결국 시간복잡도는 O(n)이 된다.



``` java
// TreeNode.java
 
public class TreeNode {
    Object data;
    TreeNode left;
    TreeNode right;
    
    public TreeNode(){
        this.left = null;
        this.right = null;
    }
    
    public TreeNode(Object data){
        this.data = data;
        this.left = null;
        this.right = null;
    }
    
    public Object getData(){
        return data;
    }
}


출처: https://songeunjung92.tistory.com/31 [은져미]
```



``` java
// BinarySearchTree.java

public class BinarySearchTree {
    private TreeNode root = null;
    
    public TreeNode insertKey(TreeNode root, char x) {
        TreeNode p = root;
        TreeNode newNode = new TreeNode(x);
        
        if(p==null){ // 현재 노드에 데이터가 없을 경우 그 자리를 차지한다.
            return newNode;
        }else if(p.data>newNode.data){ // 현재 노드의 데이터보다 작을 경우 왼쪽으로 재귀. 
            p.left = insertKey(p.left, x);
            return p;
        }else if(p.data<newNode.data){ // 현재 노드의 데이터보다 클 경우 오른쪽으로 재귀.
            p.right = insertKey(p.right, x);
            return p;
        }else{ 
            return p;
        }
    }
    
    public void insertBST(char x){
        root = insertKey(root, x);
    }
    
    public TreeNode searchBST(char x){
        TreeNode p = root;
        while(p!=null){
            if(x<p.data) p = p.left;
            else if(x>p.data) p = p.right;
            else return p;
        }
        return p;
    }
    
    public void printBST(){
        inorder(root);
        System.out.println();
    }
}


출처: https://songeunjung92.tistory.com/31 [은져미]
```



## Binary Heap

 Heap은 기본적으로 완전 이진 트리이다. 힙을 사용하는 목적은 최소값, 최대값을 빨리 도출해내는 것이다. 그렇기 때문에 힙의 종류는 두 가지다. 최대힙, 최소힙. 최대힙과 최소힙의 원리는 같으니 최대힙 예제를 보자.

### Max Heap

  max heap의 기본 규칙은 다음과 같다

- 부모노드가 자식노드보다 큰 값이다.

그렇기 때문에 루트 노드는 항상 가장 큰 값이다.



힙 삽입의 그림은 다음과 같다.



![Heap insert](https://gmlwjd9405.github.io/images/data-structure-heap/maxheap-insertion.png)





``` java
/*
	위의 완전이진트리를 참고해보면, 루트 노드는 index 1에서 시작한다는 것을 알 수 있다.
	이후 왼쪽 자식 노드는 2*i, 오른쪽 자식 노드는 2*i + 1, 부모노드는 i/2 임을 알 수 있다.
	예제에서는 이러한 완전이진트리의 특성을 활용했다.
*/

// 최대힙 삽입
void insert_max_heap(int x){
	maxHeap[++heapSize] = x; // 힙 크기를 하나 증가하고 마지막 노드에 x를 넣는다.

	for (int i=heapSize; i>1; i/=2) {
     // 마지막 노드가 자신의 부모 노드보다 크면 swap
 	 if (maxHeap[i/2] < maxHeap[i]) {
    	swap(i/2, i);
  	 } else {
    	break;
  	 }
	}
}
// 최대힙 삭제(Get max value & Heapify)
int delete_max_heap(){
	if (heapSize == 0) // 배열이 빈 경우
	  return 0;

	int item = maxHeap[1]; // 루트 노드의 값을 저장한다.
	maxHeap[1] = maxHeap[heapSize]; // 마지막 노드의 값을 루트 노드에 둔다.
	maxHeap[heapSize--] = 0; // 힙 크기를 하나 줄이고 마지막 노드를 0으로 초기화한다.

	for (int i=1; i*2<=heapSize;) {
		// 마지막 노드(임시 루트노트)가 왼쪽 노드와 오른쪽 노드보다 크면 반복문을 나간다.
		if (maxHeap[i] > maxHeap[i*2] && maxHeap[i] > maxHeap[i*2+1]) {
    		break;
		}
		// 왼쪽 노드가 더 큰 경우, 왼쪽 노드와 마지막 노드를 swap
		else if (maxHeap[i*2] > maxHeap[i*2+1]) {
    		swap(i, i*2);
    		i = i*2;
  		}
  		// 오른쪽 노드가 더 큰 경우, 오른쪽 노드와 마지막 노드를 swap
  		else {
    		swap(i, i*2+1);
    		i = i*2+1;
  		}
	}
	return item;
}

출처: https://gmlwjd9405.github.io/2018/05/10/data-structure-heap.html
```



## Red Black Tree

 이해의 대부분을 블로그 포스팅 [알고리즘 ) Red-Black Tree](https://zeddios.tistory.com/237) 을 통해 했다.

 RBT는 Binary Search Tree를 기반으로 하는 트리 형식의 자료구조이다. Binary Search Tree가 편중될 수 있으므로 이를 보완하기 위해 만든 자료구조이며, Search, Insert, Delete에 대한 시간 복잡도가 O(log n)이다. 핵심은 동일한 노드 개수일 때, Height를 최소화 하는 것이다. 즉, 완전 이진 트리를 만들겠다는 것이다.

Red Black Tree는 다음의 규칙을 만족한다.

- 각 노드는 Red 또는 Black이라는 색을 갖는다.
- Root node와 Leaf node(NIL)는 Black이다.
- 어떤 노드의 색이 Red라면 두 개의 children 색은 모두 black이다.
- 노드로부터 descendant leaves까지의 단순 경로는 모두 같은 수의 black nodes를 포함하고 있다. 이를 해당 노드의 `Black-height`라고 한다.
  cf) `Black-Height` : 노드 x로부터 Leaf node까지의 simple path 상에 있는 black nodes의 개수



참고로 존나 어려움.

참고2) insert 시 새로운 노드의 색은 Red이다.

![Red Black Tree - resolve double red(restructuring)](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile3.uf.tistory.com%2Fimage%2F998F903359CF65861762F8)



![Red Black Tree - resolve double red(Recoloring)](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile23.uf.tistory.com%2Fimage%2F9956CA3359CF6587088DE0)

 Insert 시, double color 문제에 빠질 수 있다. 그럴 경우 uncle node(부모의 형제)의 색에 따라 double color를 해결하는 방법이 달라진다.

### Restructuring: Uncle node가 Black인 경우

 이 경우, 새로운 노드와, 부모, 부모의 부모(Grand parent)를 선택한다. 그리고 정렬한 후 노드를 재구성한다. 부모는 중간 값이 되겠고, 중간 값의 왼쪽에는 세 값 중 가장 작은 값, 오른쪽에는 가장 큰 값이 오게 된다. 또한 색도 중간 값이 Black, 다른 두 값은 Red가 된다. 또한 이전에 Uncle node였던 노드를 원래 위치에 그대로 넣어준다.

![Restructuring1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile4.uf.tistory.com%2Fimage%2F99F7BD3359CF679E2A66F5)

![Restructuring2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile29.uf.tistory.com%2Fimage%2F99C2FF3359CF699A316DCD)



 Restructuring은 다른 서브트리에 영향을 미치지 않기 떄문에 한번의 Restructuring이면 끝난다. 



### Recoloring : Uncle node가 Red인 경우

 이 경우, 새로운 노드의 부모와 그 형제를 Black으로 하고, Grand parent를 Red로 한다. 이후 Grand Parent가 Root node가 아닐 시 Double Red가 다시 발생할 수 있다. (Grand Parent의 부모노드가 Red일 경우) 이 경우에는 다시 Uncle node를 보고 Restructuring, Recoloring 중 해결 방법을 선택해야 한다. Grand parent가 루트 노드일 경우, Black으로 놔둔다. 

![Recoloring](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile9.uf.tistory.com%2Fimage%2F99FAA13359CF6ECA2E3656)

### RBT의 특징

 지금 당장 이해할 수는 없지만 RBT에는 다음과 같은 특징이 있다.

- Binary Search Tree의 특징을 모두 갖는다.
- Root node부터 Leaf node까지의 모든 경로 중 최소 경로와 최대 경로의 크기 비율은 1:2보다 크지 않다. 즉, 최대 경로가 최소 경로의 2배 이상이 되지 않는다. 이러한 상태를 balanced 상태라고 한다.
  그 이유는 Red가 두 번 연속 올 수 없고, 각 노드에서 Leaf node까지의 Black 개수가 같으므로 최대 경로와 최소 경로의 차이는 2배가 될 수 없기 때문이다.
- 노드의 child가 없을 경우 child를 가리키는 포인터는 NIL값을 저장한다. 이러한 NIL들을 leaf node로 간주한다.

