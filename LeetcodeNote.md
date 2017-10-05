---
title: LeetcodeNote
date: 2017-07-01 00:18:41
tags:
- 算法
- Leetcode
---

算法培训课程基本模型汇总笔记

<!-- more -->

# 线

## 基本模型

### 数学归纳法

# 树

## 基本模板

- Draw/Equation -> Tree shape
- Define TreeNode 
  - 本点信息必然是辅助变量，计入TreeNode
  - 孩子信息决定TreeNode的形状
  - 任何第一次走的节点，如果不能走，一定要画出来打一把叉

### Binary Search

```java
Public int func(T[] array, V tartget ){
	int pos = -1;
	int start = 0;
	int end array.length - 1;
	while ( start <= end ){
		int mid = start + (end - start)/2;
		if ( f(a[mid]) <= target ){
			pos = mid;
			start = mid + 1;
		} else {
			end = mid - 1;
	   }
	}
	return pos;
}
```

### Bottom up - Recursion

```java
public <T_P> func(T_v_1, v1 …){
	checkhastreeNode();
	return helper(root(T_v_1, v_1, …))
}

private <T_P> helper(T_v_1, v1, …){
	resultchildfirst = helper(childFirst);
	…
	resultchildlast = helper(childLast);
	
	-> result by childs
	//generate cur node's result;
	
	return result;
}
```

### DFS

```java
public class DFSTree {
	public Type_R func(T_1, e1, T_2, e2){
		checkrootexists();
		
		TreeNode[] array = new TreeNode[TREE_HEIGHT];
		
		Stack<TreeNode> stack = Stack<>();
		stack.push(root);
		while (!stack.Empty()){
			TreeNode curNode = stack.pop();
			
			Operation at node;
			
			stack.push(childLast);
			…
			stack.push(childFirst);
		}
		
		return result;
	}
	
	private class TreeNode{
		T_V_1 field_1;
		…
		T_V_q field_q;
		
		int _height;
	}

```

### BFS

```java
public class BFS {
	public TypeR func(T_1 v_1, T_p, v_p) {
		checkexistroot();
		
		Queue<TreeNode> queue = new LinkedList<>();
		queue.add(root);
		
		while ( !queue.isEmpty() ){
		
			int size = queue.size();
			for ( int I = 0; I < size; i++ ){
			TreeNode node = queue.remove();
			
			op at node;
			
			queue.add(childFirst);
			…
			queue.add(childLast);
			}
			
			update var_l,…,var_k for next level
		}
		
		return result;
		
	}
	
	private class TreeNode{
		T_1 field_1;
		…
	}
}
```

# 图

## 基本模板

