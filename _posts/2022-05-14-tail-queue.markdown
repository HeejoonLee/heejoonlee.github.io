---
title: "[Kernel DS] Tail Queue"
date: 2022-05-14 18:55:00 +0900
categories: [Data Structure, Linux]
tags: [Linux, kernel, queue]
---

### Header File
sys/queue.h

### TAILQ Definitions
#### TAILQ_HEAD
```c
#define TAILQ_HEAD(name, type)   \
struct name {                    \
    struct type *tqh_first;      \
    struct type **tqh_last;      \
}
```

**TAILQ_HEAD** defines a struct that stores information about the head and tail of a queue. One thing to note in the kernel implementation of a queue is that the *tqh_first* variable stores the address of the first element as expected, but the *tqh_last* variable(notice that *tqh_last* is of type *struct type \*\**) actually stores the address of the *last head variable* which stores the address of the next element(NULL in this case).

Such a structure is similarly used in **TAILQ_ENTRY**. Refer to the image below for clarification on how TAILQ structs are defined and used.

#### TAILQ_ENTRY
```c
#define TAILQ_ENTRY(type)      \
struct {                       \
    struct type *tqe_next;     \
    struct type **tqe_prev;    \
}
```

Similar to the TAILQ_HEAD struct, **TAILQ_ENTRY** defines two variables: *tqe_next* stores the address of the next element, and *tqe_prev* holds the address of the *tqe_next variable of the previous element*.

To use the kernel tail queue implementation in your code, TAILQ_ENTRY must be included in each element of your queue.

*type* should be equal to the struct identifier of the element of the queue.

### TAILQ Functions


### TAILQ Usage

For this example, we will implement a queue whose elements contain information about a person. 

Each element will store a person struct defined as:
```c
struct person {
    char name[256];
    int age;
};
```
To use the kernel tail queue implementation, a TAILQ_ENTRY should be defined along with the actual data inside each element. We will define an element struct as below:
```c
struct element {
    TAILQ_ENTRY(element) entry;
    struct person personal_data;
};
```

Test code:
```c
#include <stdio.h>
#include <sys/queue.h>

TAILQ_HEAD(_head, element) tailq_head;

int main()
{
    tailq_head head;
    struct element *e1, *e2, *e3;
    
    TAILQ_INIT(head);
    e1 = malloc(sizeof(struct element));
    e2 = malloc(sizeof(struct element));
    e3 = malloc(sizeof(struct element));
}
```
