---
layout: post
title: Dictionaries, hash tables and algorithmic analysis
date: 2024-03-20
description: Quick dive into dictionaries, their implementation and algorithmic analysis
tags: ds
categories: DSA
giscus_comments: true
related_posts: false
---

# Dictionary

A dictionary is an ADT that models a collection which stores key-value pairs. The following should be present:

- `put(key, value)`
- `get(key)`

## Hash Table 

A hash table is an implementation of the dictionary ADT. It uses a hash function to convert key to index values to then be stored in an array. This makes accessing a hashmap seemingly constant, but in theory it is `O(n)`, because there might be collisions with indices, since mostly a hash function maps a space of input values to a **smaller** space of output values. Some collision techniques include:

- Linear probing: Tries to find the next available slot in the data collection
- Seperate chaining: Each index in the array points to a linked list
- Double hashing: Use a second hash function when collision is encountered

This then naturally makes insertion and searching in theory `O(n)`, but in practice it is much faster.

# Algorithmic Analysis

The efficiency of algortihms is measured in the number of operations rather than in the amount of time. The performance is expected to depend on the size of in the input, and drescribed with the following functions:

- $$O(1)$$.
- $$O(\log{n})$$.
- $$O(n)$$.
- $$O(n\log{n})$$.
- $$O(n^c)$$.
- $$O(2^n)$$.

Some good rules take take into consideration, include:

- Different steps get added
- Drop constants
- Different inputs maps to different variables
- Drop non-dominate terms

# Practice

## Duplicates

> Describe an algorithm that checks if an array of integers contains at least `k` duplicates of any  element. The method should have a time complexity of  `O(n²)`  and a storage complexity of `O(1)`. 

```java
public static boolean containsKDuplicates(int[] arr, int k) {
    int count = 0;

    for (int i = 0; i < arr.length; i++) {
        for (int j = 0; j < arr.length; j++) {
            if (arr[i] == arr[j])
                count++;

            if (count == k)
                return true;
        }

        count = 0;
    }

    return false;
}
```

Now, try to improve a time complexity to `O(n)`, but the space complexity can also be `O(n)`.

```java
public static boolean containsKDuplicatesImproved(int[] arr, int k) {
    HashMap<Integer, Integer> count = new HashMap<Integer, Integer>();

    for (int i = 0; i < arr.length; i++) {
        if (count.containsKey(arr[i])) {
            count.put(arr[i], count.get(arr[i]) + 1);
        } else
            count.put(arr[i], 1);

        if (count.get(arr[i]) == k)
            return true;
    }

    return false;
}
```

## Let's implement a dictionary

Let's create a hashmap by implementing the dictionary ADT.

```java 
public interface Dictionary {
    void put(String key, int value);

    int get(String key);
}
```

Let's also define a entry as a data structure:

```java
static class Entry {
    String key;
    int value;

    Entry(String key, int value) {
        this.key = key;
        this.value = value;
    }
}
```

Now, let's create our hashmap:

```java
public class HashMap implements Dictionary {

    private final int capacity;
    private final Entry[] table;

    public HashMap(int capacity) {
        this.capacity = capacity;
        this.table = new Entry[capacity];
    }  

}
```

Firstly, let's begin by creating a custom hash function, that given a key outputs the index where the data should be place:

```java
private int hash(String key) {
    int hashValue = 0;
    for (int i = 0; i < key.length(); i++) {
        hashValue += key.charAt(i):
    }

    int index = hashValue % capacity;

    return index;
}
```

Something as simple as summing the values of the characters is enough to do the trick. Next, let's implement the `put()` and `get()` methods.

```java
public void put(String key, int value) {
    table[hash(key)] = new Entry(key, value);
}
```

```java
public int get(String key) {
    return table[hash(key)];
}
```

You might already be seeing a problem with this implementation. What happens if when hashing the key we override something that has already been placed? And you right, this implementation is imcomplete, it does not have collision handling. You might want to add it to the implementation, but unfortunately I currently do now have much time :D, so I will let this for you.