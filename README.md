# 📚  Data-structure-and-agorithms-a-education-notes

Personal notes and code examples covering core Data Structures & Algorithms — written while learning, organized by topic, and shared for anyone else on the same path.

Each file explains one concept in plain language: what it is, why it matters, its time/space complexity, and a working code example.

---

## 📖 Contents

- [Fundamentals](#-fundamentals)
- [Searching Algorithms](#-searching-algorithms)
- [Sorting Algorithms](#-sorting-algorithms)
- [Data Structures](#-data-structures)

---

## 🧩 Fundamentals

The building blocks every other topic in this repo relies on — how to measure an algorithm's efficiency, and how to think in terms of a function calling itself.

- **[Big O Notation](Fundamentals/BIG%20O%20NOTATION.md)** — How we measure and compare the time/space efficiency of algorithms, independent of hardware or programming language.
- **[Recursion](Fundamentals/Recursion.md)** — How a function can call itself to break a problem into smaller sub-problems, and why it's the backbone of algorithms like Binary Search and Merge Sort.

---

## 🔍 Searching Algorithms

Techniques for finding a target value inside a collection of data.

- **[Linear Search](Searching_Algorithms/Linear%20search.md)** — Checks every element one by one. Works on unsorted data and linked lists; O(n) time.
- **[Binary Search](Searching_Algorithms/Binary%20search.md)** — Repeatedly halves a *sorted* array to find a target much faster; O(log n) time.

---

## 🔃 Sorting Algorithms

Methods for arranging data into a defined order.

- **[Sorting Algorithms](Sorting_Algorithms/Sorting%20Algorithms.md)** — Covers Bubble, Insertion, and Selection Sort (the O(n²) basics), plus Merge, Quick, and Heap Sort (the O(n log n) advanced sorts), with a full complexity comparison table.

---

## 🌳 Data Structures

Ways of organizing and storing data so it can be used efficiently.

- **[Arrays & Linked Lists](Data_Structures/ARRAYS%20%26%20LINKED%20LISTS.md)** — The two fundamental ways to store a sequence of elements: contiguous memory (arrays) vs. linked nodes (linked lists).
- **[Stacks & Queues](Data_Structures/STACKS%20%26%20QUEUES.md)** — Restricted-access structures built on arrays/linked lists: LIFO (stack) and FIFO (queue).
- **[Trees – Binary Trees, BST, and Traversals](Data_Structures/TREES%20%E2%80%93%20BINARY%20TREES%2C%20BST%2C%20AND%20TRAVERSALS.md)** — Hierarchical, non-linear structures; covers Binary Trees, Binary Search Trees, and the traversal methods (in-order, pre-order, post-order) used to visit their nodes.
- **[Graphs](Data_Structures/GRAPHS.md)** — Nodes connected by edges, used to model networks, relationships, and paths.
- **[Hash Tables](Data_Structures/HASH%20TABLES.md)** — Key-value storage that gives near-instant (O(1) average) lookup, insertion, and deletion.

---

## 🎯 Purpose

This repo is a personal learning project built while studying algorithms and data structures. It's meant to reinforce my own understanding by writing things out clearly — and hopefully make these topics a bit easier for anyone else learning them too.
