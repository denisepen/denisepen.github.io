---
layout: post
title:      "Looking At Linked Lists"
date:       2019-02-11 15:10:34 +0000
permalink:  looking_at_linked_lists
---


A linked list is a data structure that includes a 'linked' sequence of nodes. Each node contains data.


In a singly linked list each node points to the next node in the linked list:

![Imgur](https://i.imgur.com/4Y6bLlM.png)


In a doubly-linked list each node points to the next node as well as the previous node:

![Imgur](https://i.imgur.com/sCGHclk.png)


In order to access a single node you must traverse the entire linked list until you land on that node.

For example, to find node 4 we must start at node 1, move to node 2, then node 3, and finally to node 4:

![Imgur](https://i.imgur.com/6ddcC2N.png)

This is very different from an array where each value can be found by using it's index, or a dictionary (hash/object) where a value can be found using it's key.

One benefit of a linked list is that you can add a node to the beginning of a linked list using constant time (O(1)). This means that subsequent nodes do not need to be re-indexed as they do in an array.  The newest node simly becomes the "head" of the linked list.

![Imgur](https://i.imgur.com/puV3Ehl.png)

Have you ever used linked lists in your applications?

If so why did you choose them over other data structures?
