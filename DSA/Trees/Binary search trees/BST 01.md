
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

### Insertion

Insertion in a BST is straight forward for a node to be inserted we need to find an empty place where this new node fits maintaining the BST property
### Implementation


```java
class Solution {

public TreeNode insertIntoBST(TreeNode root, int val) {
TreeNode result = insert(root,val);
return result;
}

public TreeNode insert(TreeNode root,int val){
	if(root == null) return new TreeNode(val);
		if(root.val > val){
			root.left = insert(root.left,val);
		}else{
			root.right = insert(root.right,val);
		}
		return root;
	}
}
```

### Deletion:


The deletion process in a BST is a bit complicated if we have children in the node because now we need to restructure the tree to maintain its property .

Recursive solution :

first we go to the branch in which we are expecting to find the node to be deleted and for each child node we call delete(node.left,value) or delete(node.right,val) if we find the node to be deleted first we check if its has 1 chid or 0 child that is straight forward we just need to delete the node and assign its child to the prev node ie return new subtree without that node

if it has left and right we will need to find th successor a successor for a node will always lie in right for a bst as right values are greater so do a inorder successor ie go left until we cannot on the right branch . once we have the successor set its value to the current node and then call delete on the right branch.

`Note:  as we have only assigned the value of the root to the succcesor the left is automatically preserved`

now that we have replaced we can return the node 


### Implementation:

```java
class Solution {

    public TreeNode deleteNode(TreeNode root, int key) {
        return delete(root,key);
    }
    public TreeNode getSuccessor(TreeNode root){
        root = root.right;
        while(root.left != null){
            root = root.left;
        }
        return root;
    }


    public TreeNode delete(TreeNode root, int x){
        if(root == null) return root;
        if(root.val > x){
            root.left = delete(root.left,x);
        }else if(root.val < x){
            root.right = delete(root.right,x);
        }else{
            if (root.left == null) return root.right;
            if (root.right == null) return root.left;
            TreeNode succ = getSuccessor(root);
            root.val = succ.val;
            root.right = delete(root.right, succ.val);
        }
        return root;
    }
}
```