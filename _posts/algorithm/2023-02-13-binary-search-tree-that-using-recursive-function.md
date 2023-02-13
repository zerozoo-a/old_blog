---
layout: post
title: binary-search-tree-that-using-recursive-function
date: 2023-02-13 21:52 +0900
---
<img src="https://images.unsplash.com/photo-1620421680010-0766ff230392?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=898&q=80" loading="lazy">
<a href="https://unsplash.com/ko/%EC%82%AC%EC%A7%84/2d0Mk82TGI8">cc. Glitch Lab App</a>

<!--break-->
## index 
--- 

- [index](#index)
- [문제](#문제)
- [풀이](#풀이)
- [recap](#recap)

<br>

## 문제 
--- 
<br>
<br>

binary search tree 구현하기

## 풀이 
--- 
<br>
<br>

javascript에는 기본적으로 자료구조 형태 지원이 많이 없습니다.

뭐.. 있다고 하더라도 그 구조를 그대로 쓰는.. 사람이 많을지는 모르겠습니다


아무튼 구현을 해봅시다.

bst에서 중요한 부분은 아마 트리 구조이겠죠 트리 구조를 통해 빠르게 원하는 데이터까지 접근한다라는게
핵심이니까요

아무튼 위에서 언급한대로 자료구조를 언어차원에서 지원해주지 않다보니 암기하기 쉬운 재귀형태를 선호합니다.
이게 뭐... 콜스택을 늘리다보니 불안정하다는건 당연합니다. 

하지만 실제 프로덕션에서는 검증된 라이브러리의 코드를 가져다 쓸 것이므로 

구조에 대한 이해와 문제풀이 정도에는 사용 할 수 있겠습니다.

insert와 order만을 가진 클래스를 만들어보죠


```js
class Node {
    constructor(data = null){
        this.data = data;
        this.left = null;
        this.right = null;
    }
}
```
기본적인 노드 클래스입니다. 여기서 브라우저에서 사용하실 경우 조심하셔야 할 부분이 있습니다.

브라우저의 전역객체에는 이미 Node라는 기본 생성자가 있습니다. DOM Tree의 Node인데요 이름을
다른 이름으로 지어줘야 합니다. 


```js
class BinarySearchTree {
  constructor(keys = []) {
    this.root = null;
    for (const key of keys) {
      this.root = this.insert(this.root, key);
    }
  }

  insert(targetNode = null, key) {
    if (targetNode === null) return new Node(key);
    if (targetNode.data > key) {
      targetNode.left = this.insert(targetNode.left, key); // key
    } else {
      targetNode.right = this.insert(targetNode.right, key);
    }
    return targetNode;
  }
}

```
나머지는 BST 클래스입니다.

트리에서 가장 복잡하다고 느끼는 insert가 제법 간결해졌군요


핵심 아이디어는 이겁니다.

재귀를 돌면서 각 노드를 확인하는 해당 메서드는

1. 타겟 노드가 비어있다면 노드를 만들어서 반환합니다.
2. 타겟 노드가 비어있지 않고 data(node에 있는 값)이 현재 집어넣을 값인 key보다 크다면 왼쪽 자식 leaf에 노드 객체를 생성해서 붙입니다.
3. 크다면 오른쪽 자식 노드에 노드를 생성해 붙입니다.

이를 계속 반복합니다.

위 코드에서 값이 같을 경우에 대한 핸들링이 없이 모두 else로 분기처리를 합니다.
그냥.. 이렇게 해도 문제는 별로 없습니다.


또 하나의 특징은 객체를 생성할 때 데이터를 넣어주는 시점에 트리가 생성된다는 것입니다.

이건 뭐.. 취향차이 같습니다. 데이터가 모두 뽑힌 상태에서 하나 하나씩 넣어서 하나 뭐..
원하시는대로 수정 할 수 있겠죠

나머지는 order method입니다.

```js
class BinarySearchTree {
  constructor(keys = []) {
    this.root = null;
    for (const key of keys) {
      this.root = this.insert(this.root, key);
    }
  }

  insert(targetNode = null, key) {
    if (targetNode === null) return new Node(key);
    if (targetNode.data > key) {
      targetNode.left = this.insert(targetNode.left, key); // key
    } else {
      targetNode.right = this.insert(targetNode.right, key);
    }
    return targetNode;
  }

  inorder(node = null) {
    if (node === null) return;

    this.inorder(node.left);
    console.log(`DATA: ${node.data}`);
    this.inorder(node.right);
  }
}

```

```mermaid
{% include theme.html %}

graph TD;
A["ROOT: 3"]
B["LEFT: 1"]
C["RIGHT: 5"]

A -->|"(= true ROOT > LEFT)"| B 
A -->|"(= true ROOT < RIGHT)"| C 
```

트리는 위의 그래프와 같은 형태로 생성됩니다.

그럼 이를 어떻게 정렬하면 될까요?

트리는 사실 정렬이라기보다는 정렬된 것 처럼 찾아가는겁니다.

A, B, C 는 순서대로입니다. 정렬이 되어 있죠

C, B, A 는 역순입니다. 이 또한 정렬이 되어 있습니다.

B, C, A 는 뭘까요? 정렬이 되어 있지 않습니다.
하지만 위 B, C, A를 꺼낼 때, 순서대로 꺼낸다면 외부의 객체들은 이게 정렬이 되어 있는지 어쩐지 모릅니다.

트리는 이미 저 상태로 정렬이 되어 있는 것입니다.

A -> B -> C의 순서대로 트리에서 꺼낼 수 있다면 이는 정렬이 되어 있다고 볼 수 있습니다. 이게 트리의 정렬이죠.


```js
  inorder(node = null) {
    if (node === null) return;

    this.inorder(node.left);
    console.log(`DATA: ${node.data}`);
    this.inorder(node.right);
  }
```
위 모습은 <a href="https://zerozoo-a.github.io/algorithm/2022/12/10/binary-search-tree-inorder-traverse.html">이 글 🦅</a> 에서도 언급한 바와 같이

왼쪽의 서브트리를 주욱 타고 내려가다가 기저조건에 닿으면 해당 노드의 데이터를 출력합니다 그리고 오른쪽 서브트리를 순회하며 내려갑니다.

이게 전부입니다.


## recap 
--- 
<br>
<br>

트리든 뭐든 알고리즘은 결국 무작위 순회라는 부분에 닿게 되어 있나 봅니다.
