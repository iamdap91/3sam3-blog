---
slug: linked-list
title: Linked List
authors: [3sam3]
tags: [algorithm, data-structure, linked-list, doubly-linked-list, typescript, javascript, programming]
keywords: [
  "연결 리스트", "링크드 리스트", "자료구조", "알고리즘", "TypeScript", "JavaScript", "노드", "포인터",
  "linked list", "data structure", "algorithm", "node", "pointer", "append", "delete", "search",
  "연결리스트 구현", "링크드리스트 예제", "자료구조 공부", "알고리즘 학습", "타입스크립트 자료구조",
  "프로그래밍 기초", "컴퓨터 과학", "원형 연결 리스트", "단방향 연결 리스트",
  "양방향 연결 리스트", "이중 연결 리스트", "더블 링크드 리스트", "previous 포인터", "tail 노드",
  "doubly linked list", "double linked list", "bidirectional linked list", "prev pointer", "tail node",
  "양방향 링크드리스트", "이중링크드리스트", "역방향 탐색", "양방향 탐색", "tail 포인터"
]
date: 2025-10-03T21:00:00
sidebar_position: 1
---


> If you prefer english, I recommend you to use immersive translation or google translate.

# 개요
데이터(노드 node)를 저장할 때 하나의 데이터와 그 다음 데이터로의 위치(보통 다음 노드의 주소나 참조)를 함께 저장하여 논리적으로 연결(link)하는 방식


## 코드
### 링크드 리스트
```typescript showLineNumbers
  class LinkedList<T> {
    head: Node<T>;

    constructor(head?: Node<T>) {
        this.head = head || null;
    }

    append(value: T) {
        if (!this.head) {
            this.head = new Node(value);
            return;
        }

        let cur = this.head;
        while (cur.next) {
            cur = cur.next;
        }

        cur.next = new Node(value);
    }

    printAll() {
        let cur = this.head;
        while (cur !== null) {
            console.log(cur.value);
            cur = cur.next;
        }
    }

    getNode(index: number) {
        let cur = this.head;
        let curIndex = 0;

        while (cur && curIndex !== index) {
            cur = cur.next;
            curIndex += 1;
        }

        return cur;
    }

    addNode(index: number, value: T) {
        const newNode = new Node(value);
        if (index === 0) {
            newNode.next = this.head;
            this.head = newNode;
            return;
        }

        const prevNode = this.getNode(index - 1);
        const nextNode = prevNode.next;
        prevNode.next = newNode;
        newNode.next = nextNode;
    }

    deleteNode(index: number) {
        if (index === 0) {
            this.head = this.head.next;
        }

        const prev = this.getNode(index - 1);
        const target = prev.next;

        prev.next = target.next;
    }
    
    getKthNodeFromLast(k) {
        let slow = this.head;
        let fast = this.head;

        for (let i = 0; i < k; i++) {
            fast = fast.next;
        }

        while (fast !== null) {
            slow = slow.next;
            fast = fast.next;
        }

        return slow;
    }
}

class Node<T> {
    constructor(value: T) {
        this.value = value;
    }

    value: T;
    next: Node<T> | null = null;
}
```

### 양방향 링크드 리스트
```typescript showLineNumbers
  class DoublyLinkedList<T> {
    head: Node<T> | null;
    tail: Node<T> | null;

    constructor(head?: Node<T>) {
        this.head = head || null;
        this.tail = head || null;
    }

    append(value: T) {
        const newNode = new Node(value);

        if (!this.head) {
            this.head = newNode;
            this.tail = newNode;
            return;
        }

        this.tail.next = newNode;
        newNode.prev = this.tail;
        this.tail = newNode;
    }

    printAll() {
        let cur = this.head;
        while (cur !== null) {
            console.log(cur.value);
            cur = cur.next;
        }
    }

    printReverse() {
        let cur = this.tail;
        while (cur !== null) {
            console.log(cur.value);
            cur = cur.prev;
        }
    }

    getNode(index: number) {
        let cur = this.head;
        let curIndex = 0;

        while (cur && curIndex !== index) {
            cur = cur.next;
            curIndex += 1;
        }

        return cur;
    }

    addNode(index: number, value: T) {
        const newNode = new Node(value);

        if (index === 0) {
            newNode.next = this.head;
            if (this.head) {
                this.head.prev = newNode;
            } else {
                this.tail = newNode;
            }
            this.head = newNode;
            return;
        }

        const prevNode = this.getNode(index - 1);
        if (!prevNode) return;

        const nextNode = prevNode.next;

        prevNode.next = newNode;
        newNode.prev = prevNode;
        newNode.next = nextNode;

        if (nextNode) {
            nextNode.prev = newNode;
        } else {
            this.tail = newNode;
        }
    }

    deleteNode(index: number) {
        if (index === 0) {
            this.head = this.head?.next || null;
            if (this.head) {
                this.head.prev = null;
            } else {
                this.tail = null;
            }
            return;
        }

        const target = this.getNode(index);
        if (!target) return;

        if (target.prev) {
            target.prev.next = target.next;
        }

        if (target.next) {
            target.next.prev = target.prev;
        } else {
            this.tail = target.prev;
        }
    }

    getKthNodeFromLast(k) {
        let slow = this.head;
        let fast = this.head;

        for (let i = 0; i < k; i++) {
            fast = fast.next;
        }

        while (fast !== null) {
            slow = slow.next;
            fast = fast.next;
        }

        return slow;
    }
}

class Node<T> {
    constructor(value: T) {
        this.value = value;
    }

    value: T;
    next: Node<T> | null = null;
    prev: Node<T> | null = null;
}
```

### 원형 구조로 만들기
```typescript
let lastNode = linkedList.head;
while (lastNode.next) {
    lastNode = lastNode.next;
}
lastNode.next = linkedList.head;
```
