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
    
    public void insertBST(Object x){
        root = insertKey(root, x);
    }
    
    public TreeNode searchBST(Object x){
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



