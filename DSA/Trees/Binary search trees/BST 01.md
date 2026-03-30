
A ****Binary Search Tree**** (BST) is a special type of binary tree that maintains its elements in a sorted order. It is a non-linear, hierarchical data structure, where each node can have at most two children, and elements are organized in a parent-child relationship. For every node in the BST:

This property ensures that each comparison allows the operation to skip about half of the remaining tree, making BST operations much faster than linear structures like arrays or linked lists.
### Operations in BST

- [****Search****](https://www.geeksforgeeks.org/dsa/binary-search-tree-set-1-search-and-insertion/) ****:**** Finds whether a given key exists in the BST. Time Complexity on average is O(log n) and worst case is O(n)
- [****Insertion****](https://www.geeksforgeeks.org/dsa/insertion-in-binary-search-tree/) ****:**** Insert a new node while maintaining BST property. Compare key with current node and move left/right recursively or iteratively. Time Complexity on average is O(log n) and worst case is O(n)
- [****Deletion****](https://www.geeksforgeeks.org/dsa/deletion-in-binary-search-tree/) ****:**** Remove a node while keeping BST valid. Node has no children means remove directly. Node has one child means replace node with its child. Node has two children means replace node with inorder successor/predecessor and delete that successor/predecessor. Time Complexity: average O(logn) and O(n) worst case
- [****Traversals****](https://www.geeksforgeeks.org/dsa/tree-traversals-inorder-preorder-and-postorder/) ****:**** The four common tree traversals are Inorder (Left, Root, Right) which gives nodes in sorted order for a BST, Preorder (Root, Left, Right), Postorder (Left, Right, Root), and Level-order, which traverses the tree level by level using a queue.

### Searching

How to search:

To search for a element in the BST we do these steps
#### 1: compare root.val to value if === return root
#### 2: compare root.val to val if > go left so root = root.left
#### 3: compare root.val to value if <  go right root = root.right
#### 4: Repeat until found or you reach the end if found return true
#### 5: return false if node is null

### Implementation:

```java

class Solution {

public TreeNode searchBST(TreeNode root, int val) {
	while(root != null){
		if(root.val < val){
			root = root.right;
		}else if(root.val > val){
			root = root.left;
		}else{
			return root;
			}
		}
		return null;
	}
}
```

