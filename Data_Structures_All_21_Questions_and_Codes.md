# Data Structures Final List — All 21 Questions and C Codes

**Important:** Every program has its own `main()` function. Copy one program section at a time into a `.c` file to compile and run it.

## Program 1: 01 Stack Array

### Question

Question:
Write a C program to implement a stack as an Abstract Data Type (ADT) using an array.
Requirements: Define stack structure; implement initialize, push, pop and peek; check overflow and underflow; display appropriate messages.

### C Code

```c
#include <stdio.h>
#define MAX 5

int stack[MAX], top = -1;

void initialize() { top = -1; }

void push(int x) {
    if (top == MAX - 1)
        printf("Stack Overflow\n");
    else
        stack[++top] = x;
}

void pop() {
    if (top == -1)
        printf("Stack Underflow\n");
    else
        printf("Deleted: %d\n", stack[top--]);
}

void peek() {
    if (top == -1)
        printf("Stack Empty\n");
    else
        printf("Top: %d\n", stack[top]);
}

int main() {
    int ch, x;
    initialize();
    do {
        printf("\n1.Push 2.Pop 3.Peek 4.Exit: ");
        scanf("%d", &ch);
        switch (ch) {
            case 1:
                printf("Enter value: ");
                scanf("%d", &x);
                push(x);
                break;
            case 2: pop(); break;
            case 3: peek(); break;
        }
    } while (ch != 4);
    return 0;
}
```

## Program 2: 02 Stack Linked List

### Question

Question:
Write a C program to implement a stack as an ADT using a linked list.
Requirements: Implement initialize, push, pop and peek; handle memory allocation failure and underflow.

### C Code

```c
#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    struct Node *next;
};
struct Node *top = NULL;

void initialize() { top = NULL; }

void push(int x) {
    struct Node *n = malloc(sizeof(struct Node));
    if (n == NULL) {
        printf("Stack Overflow (allocation failed)\n");
        return;
    }
    n->data = x;
    n->next = top;
    top = n;
}

void pop() {
    if (top == NULL) {
        printf("Stack Underflow\n");
        return;
    }
    struct Node *t = top;
    printf("Deleted: %d\n", t->data);
    top = top->next;
    free(t);
}

void peek() {
    if (top == NULL) printf("Stack Empty\n");
    else printf("Top: %d\n", top->data);
}

int main() {
    int ch, x;
    initialize();
    do {
        printf("\n1.Push 2.Pop 3.Peek 4.Exit: ");
        scanf("%d", &ch);
        if (ch == 1) {
            printf("Enter value: ");
            scanf("%d", &x);
            push(x);
        } else if (ch == 2) pop();
        else if (ch == 3) peek();
    } while (ch != 4);
    return 0;
}
```

## Program 3: 03 Prefix To Postfix

### Question

Question:
Write a C program to convert a prefix expression into postfix using a stack.
Support +, -, * and /. This simple version assumes single-character operands.

### C Code

```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

char stack[100][100];
int top = -1;

int main() {
    char pre[100];
    printf("Enter prefix expression: ");
    scanf("%99s", pre);

    for (int i = (int)strlen(pre) - 1; i >= 0; i--) {
        if (isalnum((unsigned char)pre[i])) {
            stack[++top][0] = pre[i];
            stack[top][1] = '\\0';
        } else if (pre[i] == '+' || pre[i] == '-' ||
                   pre[i] == '*' || pre[i] == '/') {
            if (top < 1) {
                printf("Invalid expression\n");
                return 0;
            }
            char left[100], right[100], temp[100];
            strcpy(left, stack[top--]);
            strcpy(right, stack[top--]);
            strcpy(temp, left);
            strcat(temp, right);
            int len = (int)strlen(temp);
            temp[len] = pre[i];
            temp[len + 1] = '\\0';
            strcpy(stack[++top], temp);
        } else {
            printf("Invalid character\n");
            return 0;
        }
    }

    if (top == 0) printf("Postfix: %s\n", stack[top]);
    else printf("Invalid expression\n");
    return 0;
}
```

## Program 4: 04 Balanced Parentheses

### Question

Question:
Write a C program to check whether an expression has balanced (), {} and [] brackets using a stack.

### C Code

```c
#include <stdio.h>

int main() {
    char exp[200], stack[200];
    int top = -1, valid = 1;

    printf("Enter expression: ");
    scanf("%199s", exp);

    for (int i = 0; exp[i] != '\\0'; i++) {
        char c = exp[i];
        if (c == '(' || c == '{' || c == '[') {
            stack[++top] = c;
        } else if (c == ')' || c == '}' || c == ']') {
            if (top == -1) { valid = 0; break; }
            char open = stack[top--];
            if ((c == ')' && open != '(') ||
                (c == '}' && open != '{') ||
                (c == ']' && open != '[')) {
                valid = 0;
                break;
            }
        }
    }

    if (top != -1) valid = 0;
    printf("%s\n", valid ? "Balanced" : "Unbalanced");
    return 0;
}
```

## Program 5: 05 Queue Linked List

### Question

Question:
Implement a queue using a linked list. Perform enqueue, dequeue and display operations. The queue should grow dynamically.

### C Code

```c
#include <stdio.h>
#include <stdlib.h>

struct Node { int data; struct Node *next; };
struct Node *front = NULL, *rear = NULL;

void enqueue(int x) {
    struct Node *n = malloc(sizeof(struct Node));
    if (n == NULL) { printf("Memory allocation failed\n"); return; }
    n->data = x; n->next = NULL;
    if (rear == NULL) front = rear = n;
    else { rear->next = n; rear = n; }
}

void dequeue() {
    if (front == NULL) { printf("Queue Underflow\n"); return; }
    struct Node *t = front;
    printf("Deleted: %d\n", t->data);
    front = front->next;
    if (front == NULL) rear = NULL;
    free(t);
}

void display() {
    struct Node *t = front;
    if (t == NULL) printf("Queue Empty\n");
    while (t != NULL) { printf("%d ", t->data); t = t->next; }
    printf("\n");
}

int main() {
    int ch, x;
    do {
        printf("\n1.Enqueue 2.Dequeue 3.Display 4.Exit: ");
        scanf("%d", &ch);
        if (ch == 1) { printf("Value: "); scanf("%d", &x); enqueue(x); }
        else if (ch == 2) dequeue();
        else if (ch == 3) display();
    } while (ch != 4);
    return 0;
}
```

## Program 6: 06 Circular Queue

### Question

Question:
Implement a circular queue using an array. Perform insertion, deletion and display. Handle full and empty conditions.

### C Code

```c
#include <stdio.h>
#define MAX 5

int q[MAX], front = -1, rear = -1;

void insert(int x) {
    if ((rear + 1) % MAX == front) { printf("Queue Full\n"); return; }
    if (front == -1) front = 0;
    rear = (rear + 1) % MAX;
    q[rear] = x;
}

void deleteItem() {
    if (front == -1) { printf("Queue Empty\n"); return; }
    printf("Deleted: %d\n", q[front]);
    if (front == rear) front = rear = -1;
    else front = (front + 1) % MAX;
}

void display() {
    if (front == -1) { printf("Queue Empty\n"); return; }
    int i = front;
    while (1) {
        printf("%d ", q[i]);
        if (i == rear) break;
        i = (i + 1) % MAX;
    }
    printf("\n");
}

int main() {
    int ch, x;
    do {
        printf("\n1.Insert 2.Delete 3.Display 4.Exit: ");
        scanf("%d", &ch);
        if (ch == 1) { scanf("%d", &x); insert(x); }
        else if (ch == 2) deleteItem();
        else if (ch == 3) display();
    } while (ch != 4);
    return 0;
}
```

## Program 7: 07 Printer Job Queue

### Question

Question:
Write a C program to manage printer jobs using a queue. Store job ID and page count; add jobs, process jobs, display pending jobs, and handle full/empty conditions.

### C Code

```c
#include <stdio.h>
#define MAX 5

struct Job { int id, pages; };
struct Job q[MAX];
int front = -1, rear = -1;

void addJob(int id, int pages) {
    if (rear == MAX - 1) { printf("Queue Full\n"); return; }
    if (front == -1) front = 0;
    q[++rear].id = id;
    q[rear].pages = pages;
}

void processJob() {
    if (front == -1) { printf("No pending jobs\n"); return; }
    printf("Printing Job %d, Pages: %d\n", q[front].id, q[front].pages);
    if (front == rear) front = rear = -1;
    else front++;
}

void display() {
    if (front == -1) { printf("No pending jobs\n"); return; }
    for (int i = front; i <= rear; i++)
        printf("Job ID: %d, Pages: %d\n", q[i].id, q[i].pages);
}

int main() {
    int ch, id, pages;
    do {
        printf("\n1.Add Job 2.Process Job 3.Display 4.Exit: ");
        scanf("%d", &ch);
        if (ch == 1) {
            printf("Enter job ID and pages: ");
            scanf("%d%d", &id, &pages);
            addJob(id, pages);
        } else if (ch == 2) processJob();
        else if (ch == 3) display();
    } while (ch != 4);
    return 0;
}
```

## Program 8: 08 Circular List Insert Begin Delete End Search

### Question

Question:
Write a menu-driven C program for a circular linked list with create, display, insert at beginning, delete from end, search and exit.

### C Code

```c
#include <stdio.h>
#include <stdlib.h>

struct Node { int data; struct Node *next; };
struct Node *last = NULL;

void create() {
    int n, x;
    printf("Number of nodes: "); scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        scanf("%d", &x);
        struct Node *p = malloc(sizeof(struct Node));
        if (p == NULL) return;
        p->data = x;
        if (last == NULL) { last = p; p->next = p; }
        else { p->next = last->next; last->next = p; last = p; }
    }
}

void display() {
    if (last == NULL) { printf("Empty\n"); return; }
    struct Node *p = last->next;
    do { printf("%d ", p->data); p = p->next; } while (p != last->next);
    printf("\n");
}

void insertBeg(int x) {
    struct Node *p = malloc(sizeof(struct Node));
    if (p == NULL) return;
    p->data = x;
    if (last == NULL) { last = p; p->next = p; }
    else { p->next = last->next; last->next = p; }
}

void deleteEnd() {
    if (last == NULL) { printf("Empty\n"); return; }
    struct Node *p = last->next;
    if (p == last) { free(last); last = NULL; return; }
    while (p->next != last) p = p->next;
    p->next = last->next; free(last); last = p;
}

void search(int x) {
    if (last == NULL) { printf("Not Found\n"); return; }
    struct Node *p = last->next;
    do {
        if (p->data == x) { printf("Found\n"); return; }
        p = p->next;
    } while (p != last->next);
    printf("Not Found\n");
}

int main() {
    int ch, x;
    do {
        printf("\n1.Create 2.Display 3.Insert Beginning 4.Delete End 5.Search 6.Exit: ");
        scanf("%d", &ch);
        if (ch == 1) create();
        else if (ch == 2) display();
        else if (ch == 3) { scanf("%d", &x); insertBeg(x); }
        else if (ch == 4) deleteEnd();
        else if (ch == 5) { scanf("%d", &x); search(x); }
    } while (ch != 6);
    return 0;
}
```

## Program 9: 09 Circular List Insert End Delete Begin Count

### Question

Question:
Write a menu-driven C program for a circular linked list with create, display, insert at end, delete from beginning, count nodes and exit.

### C Code

```c
#include <stdio.h>
#include <stdlib.h>

struct Node { int data; struct Node *next; };
struct Node *last = NULL;

void create() {
    int n, x; scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        scanf("%d", &x);
        struct Node *p = malloc(sizeof(struct Node));
        if (p == NULL) return;
        p->data = x;
        if (last == NULL) { last = p; p->next = p; }
        else { p->next = last->next; last->next = p; last = p; }
    }
}

void display() {
    if (last == NULL) { printf("Empty\n"); return; }
    struct Node *p = last->next;
    do { printf("%d ", p->data); p = p->next; } while (p != last->next);
    printf("\n");
}

void insertEnd(int x) {
    struct Node *p = malloc(sizeof(struct Node));
    if (p == NULL) return;
    p->data = x;
    if (last == NULL) { last = p; p->next = p; }
    else { p->next = last->next; last->next = p; last = p; }
}

void deleteBeg() {
    if (last == NULL) { printf("Empty\n"); return; }
    struct Node *p = last->next;
    if (p == last) last = NULL;
    else last->next = p->next;
    free(p);
}

void count() {
    int c = 0;
    if (last != NULL) {
        struct Node *p = last->next;
        do { c++; p = p->next; } while (p != last->next);
    }
    printf("Nodes: %d\n", c);
}

int main() {
    int ch, x;
    do {
        printf("\n1.Create 2.Display 3.Insert End 4.Delete Beginning 5.Count 6.Exit: ");
        scanf("%d", &ch);
        if (ch == 1) create();
        else if (ch == 2) display();
        else if (ch == 3) { scanf("%d", &x); insertEnd(x); }
        else if (ch == 4) deleteBeg();
        else if (ch == 5) count();
    } while (ch != 6);
    return 0;
}
```

## Program 10: 10 Doubly List Insert Position Delete Begin Count

### Question

Question:
Write a menu-driven C program for a doubly linked list with create, display, insert at a given position, delete from beginning, count nodes and exit. Positions are 1-based.

### C Code

```c
#include <stdio.h>
#include <stdlib.h>

struct Node { int data; struct Node *prev, *next; };
struct Node *head = NULL;

void create() {
    int n, x; scanf("%d", &n);
    while (n--) {
        scanf("%d", &x);
        struct Node *p = malloc(sizeof(struct Node));
        if (p == NULL) return;
        p->data = x; p->next = NULL; p->prev = NULL;
        if (head == NULL) head = p;
        else {
            struct Node *t = head;
            while (t->next != NULL) t = t->next;
            t->next = p; p->prev = t;
        }
    }
}

void display() {
    struct Node *p = head;
    while (p != NULL) { printf("%d ", p->data); p = p->next; }
    printf("\n");
}

void insertPos(int x, int pos) {
    if (pos < 1) { printf("Invalid position\n"); return; }
    struct Node *p = malloc(sizeof(struct Node));
    if (p == NULL) return;
    p->data = x;
    if (pos == 1) {
        p->prev = NULL; p->next = head;
        if (head != NULL) head->prev = p;
        head = p; return;
    }
    struct Node *t = head;
    for (int i = 1; t != NULL && i < pos - 1; i++) t = t->next;
    if (t == NULL) { printf("Invalid position\n"); free(p); return; }
    p->next = t->next; p->prev = t;
    if (t->next != NULL) t->next->prev = p;
    t->next = p;
}

void deleteBeg() {
    if (head == NULL) { printf("Empty\n"); return; }
    struct Node *t = head; head = head->next;
    if (head != NULL) head->prev = NULL;
    free(t);
}

void count() {
    int c = 0; struct Node *p = head;
    while (p != NULL) { c++; p = p->next; }
    printf("Nodes: %d\n", c);
}

int main() {
    int ch, x, pos;
    do {
        printf("\n1.Create 2.Display 3.Insert Position 4.Delete Beginning 5.Count 6.Exit: ");
        scanf("%d", &ch);
        if (ch == 1) create();
        else if (ch == 2) display();
        else if (ch == 3) { printf("Value and position: "); scanf("%d%d", &x, &pos); insertPos(x, pos); }
        else if (ch == 4) deleteBeg();
        else if (ch == 5) count();
    } while (ch != 6);
    return 0;
}
```

## Program 11: 11 Singly List Insert Position Delete Value

### Question

Question:
Write a menu-driven C program for a singly linked list with create, display, insert at a given position, delete the node by value and exit. Positions are 1-based.

### C Code

```c
#include <stdio.h>
#include <stdlib.h>

struct Node { int data; struct Node *next; };
struct Node *head = NULL;

void create() {
    int n, x; scanf("%d", &n);
    while (n--) {
        scanf("%d", &x);
        struct Node *p = malloc(sizeof(struct Node));
        if (p == NULL) return;
        p->data = x; p->next = NULL;
        if (head == NULL) head = p;
        else {
            struct Node *t = head;
            while (t->next != NULL) t = t->next;
            t->next = p;
        }
    }
}

void display() {
    struct Node *p = head;
    while (p != NULL) { printf("%d ", p->data); p = p->next; }
    printf("\n");
}

void insertPos(int x, int pos) {
    if (pos < 1) { printf("Invalid position\n"); return; }
    struct Node *p = malloc(sizeof(struct Node));
    if (p == NULL) return;
    p->data = x;
    if (pos == 1) { p->next = head; head = p; return; }
    struct Node *t = head;
    for (int i = 1; t != NULL && i < pos - 1; i++) t = t->next;
    if (t == NULL) { printf("Invalid position\n"); free(p); return; }
    p->next = t->next; t->next = p;
}

void deleteValue(int x) {
    struct Node *t = head, *prev = NULL;
    while (t != NULL && t->data != x) { prev = t; t = t->next; }
    if (t == NULL) { printf("Not Found\n"); return; }
    if (prev == NULL) head = t->next;
    else prev->next = t->next;
    free(t);
}

int main() {
    int ch, x, pos;
    do {
        printf("\n1.Create 2.Display 3.Insert Position 4.Delete Value 5.Exit: ");
        scanf("%d", &ch);
        if (ch == 1) create();
        else if (ch == 2) display();
        else if (ch == 3) { printf("Value and position: "); scanf("%d%d", &x, &pos); insertPos(x, pos); }
        else if (ch == 4) { printf("Value: "); scanf("%d", &x); deleteValue(x); }
    } while (ch != 5);
    return 0;
}
```

## Program 12: 12 Doubly List Insert Begin Delete End Search

### Question

Question:
Write a menu-driven C program for a doubly linked list with create, display, insert at beginning, delete from end, search element and exit.

### C Code

```c
#include <stdio.h>
#include <stdlib.h>

struct Node { int data; struct Node *prev, *next; };
struct Node *head = NULL;

void create() {
    int n, x; scanf("%d", &n);
    while (n--) {
        scanf("%d", &x);
        struct Node *p = malloc(sizeof(struct Node));
        if (p == NULL) return;
        p->data = x; p->next = NULL; p->prev = NULL;
        if (head == NULL) head = p;
        else {
            struct Node *t = head;
            while (t->next != NULL) t = t->next;
            t->next = p; p->prev = t;
        }
    }
}

void display() {
    struct Node *p = head;
    while (p != NULL) { printf("%d ", p->data); p = p->next; }
    printf("\n");
}

void insertBeg(int x) {
    struct Node *p = malloc(sizeof(struct Node));
    if (p == NULL) return;
    p->data = x; p->prev = NULL; p->next = head;
    if (head != NULL) head->prev = p;
    head = p;
}

void deleteEnd() {
    if (head == NULL) { printf("Empty\n"); return; }
    struct Node *t = head;
    while (t->next != NULL) t = t->next;
    if (t->prev == NULL) head = NULL;
    else t->prev->next = NULL;
    free(t);
}

void search(int x) {
    struct Node *p = head;
    while (p != NULL) {
        if (p->data == x) { printf("Found\n"); return; }
        p = p->next;
    }
    printf("Not Found\n");
}

int main() {
    int ch, x;
    do {
        printf("\n1.Create 2.Display 3.Insert Beginning 4.Delete End 5.Search 6.Exit: ");
        scanf("%d", &ch);
        if (ch == 1) create();
        else if (ch == 2) display();
        else if (ch == 3) { scanf("%d", &x); insertBeg(x); }
        else if (ch == 4) deleteEnd();
        else if (ch == 5) { scanf("%d", &x); search(x); }
    } while (ch != 6);
    return 0;
}
```

## Program 13: 13 Bst Inorder Preorder

### Question

Question:
Write a menu-driven C program for a BST to insert, delete and search nodes, perform inorder and preorder traversal, and exit.

Note: This shared BST code includes all required operations. The menu has extra options; demonstrate the options specified in this question.

### C Code

```c
#include <stdio.h>
#include <stdlib.h>

struct Node { int data; struct Node *left, *right; };
struct Node *root = NULL;

struct Node *insert(struct Node *r, int x) {
    if (r == NULL) {
        r = malloc(sizeof(struct Node));
        if (r == NULL) return NULL;
        r->data = x; r->left = r->right = NULL;
    } else if (x < r->data) r->left = insert(r->left, x);
    else if (x > r->data) r->right = insert(r->right, x);
    return r;
}

struct Node *minimum(struct Node *r) {
    while (r != NULL && r->left != NULL) r = r->left;
    return r;
}

struct Node *maximum(struct Node *r) {
    while (r != NULL && r->right != NULL) r = r->right;
    return r;
}

struct Node *deleteNode(struct Node *r, int x) {
    if (r == NULL) return NULL;
    if (x < r->data) r->left = deleteNode(r->left, x);
    else if (x > r->data) r->right = deleteNode(r->right, x);
    else {
        if (r->left == NULL) {
            struct Node *t = r->right; free(r); return t;
        }
        if (r->right == NULL) {
            struct Node *t = r->left; free(r); return t;
        }
        struct Node *t = minimum(r->right);
        r->data = t->data;
        r->right = deleteNode(r->right, t->data);
    }
    return r;
}

void search(struct Node *r, int x) {
    if (r == NULL) printf("Not Found\n");
    else if (r->data == x) printf("Found\n");
    else if (x < r->data) search(r->left, x);
    else search(r->right, x);
}

void inorder(struct Node *r) {
    if (r) { inorder(r->left); printf("%d ", r->data); inorder(r->right); }
}
void preorder(struct Node *r) {
    if (r) { printf("%d ", r->data); preorder(r->left); preorder(r->right); }
}
void postorder(struct Node *r) {
    if (r) { postorder(r->left); postorder(r->right); printf("%d ", r->data); }
}

int main() {
    int ch, x;
    do {
        printf("\n1.Insert 2.Delete 3.Search 4.Inorder 5.Preorder");
        printf("\n6.Postorder 7.Minimum 8.Maximum 9.Exit: ");
        scanf("%d", &ch);
        switch (ch) {
            case 1: scanf("%d", &x); root = insert(root, x); break;
            case 2: scanf("%d", &x); root = deleteNode(root, x); break;
            case 3: scanf("%d", &x); search(root, x); break;
            case 4: inorder(root); printf("\n"); break;
            case 5: preorder(root); printf("\n"); break;
            case 6: postorder(root); printf("\n"); break;
            case 7:
                if (root == NULL) printf("Tree Empty\n");
                else printf("Minimum: %d\n", minimum(root)->data);
                break;
            case 8:
                if (root == NULL) printf("Tree Empty\n");
                else printf("Maximum: %d\n", maximum(root)->data);
                break;
        }
    } while (ch != 9);
    return 0;
}
```

## Program 14: 14 Bst Preorder Postorder

### Question

Question:
Write a menu-driven C program for a BST to insert, delete and search nodes, perform preorder and postorder traversal, and exit.

Note: This shared BST code includes all required operations. The menu has extra options; demonstrate the options specified in this question.

### C Code

```c
#include <stdio.h>
#include <stdlib.h>

struct Node { int data; struct Node *left, *right; };
struct Node *root = NULL;

struct Node *insert(struct Node *r, int x) {
    if (r == NULL) {
        r = malloc(sizeof(struct Node));
        if (r == NULL) return NULL;
        r->data = x; r->left = r->right = NULL;
    } else if (x < r->data) r->left = insert(r->left, x);
    else if (x > r->data) r->right = insert(r->right, x);
    return r;
}

struct Node *minimum(struct Node *r) {
    while (r != NULL && r->left != NULL) r = r->left;
    return r;
}

struct Node *maximum(struct Node *r) {
    while (r != NULL && r->right != NULL) r = r->right;
    return r;
}

struct Node *deleteNode(struct Node *r, int x) {
    if (r == NULL) return NULL;
    if (x < r->data) r->left = deleteNode(r->left, x);
    else if (x > r->data) r->right = deleteNode(r->right, x);
    else {
        if (r->left == NULL) {
            struct Node *t = r->right; free(r); return t;
        }
        if (r->right == NULL) {
            struct Node *t = r->left; free(r); return t;
        }
        struct Node *t = minimum(r->right);
        r->data = t->data;
        r->right = deleteNode(r->right, t->data);
    }
    return r;
}

void search(struct Node *r, int x) {
    if (r == NULL) printf("Not Found\n");
    else if (r->data == x) printf("Found\n");
    else if (x < r->data) search(r->left, x);
    else search(r->right, x);
}

void inorder(struct Node *r) {
    if (r) { inorder(r->left); printf("%d ", r->data); inorder(r->right); }
}
void preorder(struct Node *r) {
    if (r) { printf("%d ", r->data); preorder(r->left); preorder(r->right); }
}
void postorder(struct Node *r) {
    if (r) { postorder(r->left); postorder(r->right); printf("%d ", r->data); }
}

int main() {
    int ch, x;
    do {
        printf("\n1.Insert 2.Delete 3.Search 4.Inorder 5.Preorder");
        printf("\n6.Postorder 7.Minimum 8.Maximum 9.Exit: ");
        scanf("%d", &ch);
        switch (ch) {
            case 1: scanf("%d", &x); root = insert(root, x); break;
            case 2: scanf("%d", &x); root = deleteNode(root, x); break;
            case 3: scanf("%d", &x); search(root, x); break;
            case 4: inorder(root); printf("\n"); break;
            case 5: preorder(root); printf("\n"); break;
            case 6: postorder(root); printf("\n"); break;
            case 7:
                if (root == NULL) printf("Tree Empty\n");
                else printf("Minimum: %d\n", minimum(root)->data);
                break;
            case 8:
                if (root == NULL) printf("Tree Empty\n");
                else printf("Maximum: %d\n", maximum(root)->data);
                break;
        }
    } while (ch != 9);
    return 0;
}
```

## Program 15: 15 Bst Min Max

### Question

Question:
Write a menu-driven C program for a BST to insert, delete and search nodes, find the minimum and maximum values, and exit.

Note: This shared BST code includes all required operations. The menu has extra options; demonstrate the options specified in this question.

### C Code

```c
#include <stdio.h>
#include <stdlib.h>

struct Node { int data; struct Node *left, *right; };
struct Node *root = NULL;

struct Node *insert(struct Node *r, int x) {
    if (r == NULL) {
        r = malloc(sizeof(struct Node));
        if (r == NULL) return NULL;
        r->data = x; r->left = r->right = NULL;
    } else if (x < r->data) r->left = insert(r->left, x);
    else if (x > r->data) r->right = insert(r->right, x);
    return r;
}

struct Node *minimum(struct Node *r) {
    while (r != NULL && r->left != NULL) r = r->left;
    return r;
}

struct Node *maximum(struct Node *r) {
    while (r != NULL && r->right != NULL) r = r->right;
    return r;
}

struct Node *deleteNode(struct Node *r, int x) {
    if (r == NULL) return NULL;
    if (x < r->data) r->left = deleteNode(r->left, x);
    else if (x > r->data) r->right = deleteNode(r->right, x);
    else {
        if (r->left == NULL) {
            struct Node *t = r->right; free(r); return t;
        }
        if (r->right == NULL) {
            struct Node *t = r->left; free(r); return t;
        }
        struct Node *t = minimum(r->right);
        r->data = t->data;
        r->right = deleteNode(r->right, t->data);
    }
    return r;
}

void search(struct Node *r, int x) {
    if (r == NULL) printf("Not Found\n");
    else if (r->data == x) printf("Found\n");
    else if (x < r->data) search(r->left, x);
    else search(r->right, x);
}

void inorder(struct Node *r) {
    if (r) { inorder(r->left); printf("%d ", r->data); inorder(r->right); }
}
void preorder(struct Node *r) {
    if (r) { printf("%d ", r->data); preorder(r->left); preorder(r->right); }
}
void postorder(struct Node *r) {
    if (r) { postorder(r->left); postorder(r->right); printf("%d ", r->data); }
}

int main() {
    int ch, x;
    do {
        printf("\n1.Insert 2.Delete 3.Search 4.Inorder 5.Preorder");
        printf("\n6.Postorder 7.Minimum 8.Maximum 9.Exit: ");
        scanf("%d", &ch);
        switch (ch) {
            case 1: scanf("%d", &x); root = insert(root, x); break;
            case 2: scanf("%d", &x); root = deleteNode(root, x); break;
            case 3: scanf("%d", &x); search(root, x); break;
            case 4: inorder(root); printf("\n"); break;
            case 5: preorder(root); printf("\n"); break;
            case 6: postorder(root); printf("\n"); break;
            case 7:
                if (root == NULL) printf("Tree Empty\n");
                else printf("Minimum: %d\n", minimum(root)->data);
                break;
            case 8:
                if (root == NULL) printf("Tree Empty\n");
                else printf("Maximum: %d\n", maximum(root)->data);
                break;
        }
    } while (ch != 9);
    return 0;
}
```

## Program 16: 16 Graph Bfs

### Question

Question:
Write a menu-driven C program to create a graph by accepting its vertex count and adjacency matrix, display the matrix, and perform Breadth First Search (BFS) from a given vertex. Vertices are numbered 0 to n-1.

### C Code

```c
#include <stdio.h>
#define MAX 20

int a[MAX][MAX], n, visited[MAX], created = 0;

void create() {
    printf("Number of vertices: "); scanf("%d", &n);
    if (n < 1 || n > MAX) { created = 0; printf("Invalid count\n"); return; }
    printf("Enter adjacency matrix:\n");
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++) scanf("%d", &a[i][j]);
    created = 1;
}

void display() {
    if (!created) { printf("Create graph first\n"); return; }
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) printf("%d ", a[i][j]);
        printf("\n");
    }
}

void bfs(int start) {
    int q[MAX], front = 0, rear = 0;
    for (int i = 0; i < n; i++) visited[i] = 0;
    q[rear++] = start; visited[start] = 1;
    while (front < rear) {
        int v = q[front++]; printf("%d ", v);
        for (int i = 0; i < n; i++)
            if (a[v][i] && !visited[i]) { visited[i] = 1; q[rear++] = i; }
    }
    printf("\n");
}

int main() {
    int ch, start;
    do {
        printf("\n1.Create 2.Display 3.BFS 4.Exit: "); scanf("%d", &ch);
        if (ch == 1) create();
        else if (ch == 2) display();
        else if (ch == 3) {
            if (!created) { printf("Create graph first\n"); continue; }
            printf("Starting vertex: "); scanf("%d", &start);
            if (start >= 0 && start < n) bfs(start);
            else printf("Invalid vertex\n");
        }
    } while (ch != 4);
    return 0;
}
```

## Program 17: 17 Graph Dfs

### Question

Question:
Write a menu-driven C program to create a graph using an adjacency matrix, display it, and perform Depth First Search (DFS) from a given vertex. Vertices are numbered 0 to n-1.

### C Code

```c
#include <stdio.h>
#define MAX 20

int a[MAX][MAX], n, visited[MAX], created = 0;

void create() {
    printf("Number of vertices: "); scanf("%d", &n);
    if (n < 1 || n > MAX) { created = 0; printf("Invalid count\n"); return; }
    printf("Enter adjacency matrix:\n");
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++) scanf("%d", &a[i][j]);
    created = 1;
}

void display() {
    if (!created) { printf("Create graph first\n"); return; }
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) printf("%d ", a[i][j]);
        printf("\n");
    }
}

void dfs(int v) {
    visited[v] = 1; printf("%d ", v);
    for (int i = 0; i < n; i++)
        if (a[v][i] && !visited[i]) dfs(i);
}

int main() {
    int ch, start;
    do {
        printf("\n1.Create 2.Display 3.DFS 4.Exit: "); scanf("%d", &ch);
        if (ch == 1) create();
        else if (ch == 2) display();
        else if (ch == 3) {
            if (!created) { printf("Create graph first\n"); continue; }
            printf("Starting vertex: "); scanf("%d", &start);
            if (start >= 0 && start < n) {
                for (int i = 0; i < n; i++) visited[i] = 0;
                dfs(start); printf("\n");
            } else printf("Invalid vertex\n");
        }
    } while (ch != 4);
    return 0;
}
```

## Program 18: 18 Reorder Linked List

### Question

Question:
Given a singly linked list L0 → L1 → … → Ln, reorder it as L0 → Ln → L1 → Ln-1 → … . Do not change node values; only change links.

### C Code

```c
#include <stdio.h>
#include <stdlib.h>

struct Node { int data; struct Node *next; };

void reorder(struct Node *head) {
    if (head == NULL || head->next == NULL) return;

    struct Node *slow = head, *fast = head;
    while (fast->next != NULL && fast->next->next != NULL) {
        slow = slow->next;
        fast = fast->next->next;
    }

    struct Node *second = slow->next;
    slow->next = NULL;
    struct Node *prev = NULL, *next;

    while (second != NULL) {
        next = second->next;
        second->next = prev;
        prev = second;
        second = next;
    }
    second = prev;

    struct Node *first = head;
    while (second != NULL) {
        struct Node *t1 = first->next;
        struct Node *t2 = second->next;
        first->next = second;
        second->next = t1;
        first = t1;
        second = t2;
    }
}

int main() {
    struct Node *head = NULL, *tail = NULL;
    int n, x;
    printf("Number of nodes: "); scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        scanf("%d", &x);
        struct Node *p = malloc(sizeof(struct Node));
        if (p == NULL) return 1;
        p->data = x; p->next = NULL;
        if (head == NULL) head = tail = p;
        else { tail->next = p; tail = p; }
    }
    reorder(head);
    while (head != NULL) { printf("%d ", head->data); head = head->next; }
    printf("\n");
    return 0;
}
```

## Program 19: 19 Remove Duplicates Sorted List

### Question

Question:
Given the head of a sorted linked list, delete every value that occurs more than once, leaving only distinct values. Example: 1→2→2→3→4→4→5 becomes 1→3→5.

### C Code

```c
#include <stdio.h>
#include <stdlib.h>

struct Node { int data; struct Node *next; };

int main() {
    struct Node dummy;
    dummy.next = NULL;
    struct Node *tail = &dummy, *head = NULL, *end = NULL;
    int n, x;

    printf("Number of nodes: "); scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        scanf("%d", &x);
        struct Node *p = malloc(sizeof(struct Node));
        if (p == NULL) return 1;
        p->data = x; p->next = NULL;
        if (head == NULL) head = end = p;
        else { end->next = p; end = p; }
    }

    struct Node *p = head;
    while (p != NULL) {
        if (p->next != NULL && p->data == p->next->data) {
            int value = p->data;
            while (p != NULL && p->data == value) {
                struct Node *t = p; p = p->next; free(t);
            }
        } else {
            tail->next = p; tail = p; p = p->next;
        }
    }
    tail->next = NULL;

    p = dummy.next;
    if (p == NULL) printf("List is empty");
    while (p != NULL) { printf("%d ", p->data); p = p->next; }
    printf("\n");
    return 0;
}
```

## Program 20: 20 Sorted List To Balanced Bst

### Question

Question:
Given a sorted singly linked list, convert it into a height-balanced binary search tree. This simple implementation copies list values into an array, then recursively chooses each range's middle element.

### C Code

```c
#include <stdio.h>
#include <stdlib.h>
#define MAX 20000

struct List { int data; struct List *next; };
struct Tree { int data; struct Tree *left, *right; };
int a[MAX], n = 0;

struct Tree *build(int l, int r) {
    if (l > r) return NULL;
    int mid = l + (r - l) / 2;
    struct Tree *root = malloc(sizeof(struct Tree));
    if (root == NULL) return NULL;
    root->data = a[mid];
    root->left = build(l, mid - 1);
    root->right = build(mid + 1, r);
    return root;
}

void inorder(struct Tree *root) {
    if (root != NULL) {
        inorder(root->left); printf("%d ", root->data); inorder(root->right);
    }
}

int main() {
    struct List *head = NULL, *tail = NULL;
    int count, x;
    printf("Number of nodes (0-%d): ", MAX); scanf("%d", &count);
    if (count < 0 || count > MAX) { printf("Invalid count\n"); return 1; }

    for (int i = 0; i < count; i++) {
        scanf("%d", &x);
        struct List *p = malloc(sizeof(struct List));
        if (p == NULL) return 1;
        p->data = x; p->next = NULL;
        if (head == NULL) head = tail = p;
        else { tail->next = p; tail = p; }
    }

    for (struct List *p = head; p != NULL; p = p->next) a[n++] = p->data;
    struct Tree *root = build(0, n - 1);
    printf("BST inorder: "); inorder(root); printf("\n");
    return 0;
}
```

## Program 21: 21 Skip List

### Question

Question:
Write a menu-driven C program for a skip list supporting insert, search, delete and display all levels. This implementation uses two levels and promotes every second base-level node to the upper level for predictable practical-exam behaviour.

### C Code

```c
#include <stdio.h>
#include <stdlib.h>

#define MAX_LEVEL 2
struct Node {
    int data;
    struct Node *next[MAX_LEVEL];
};
struct Node *head;
int count = 0;

struct Node *newNode(int x) {
    struct Node *p = malloc(sizeof(struct Node));
    if (p == NULL) return NULL;
    p->data = x;
    for (int i = 0; i < MAX_LEVEL; i++) p->next[i] = NULL;
    return p;
}

/* Rebuild upper-level links: every second base node is promoted. */
void rebuild() {
    struct Node *p = head->next[0], *upperTail = head;
    int index = 1;
    head->next[1] = NULL;
    while (p != NULL) {
        if (index % 2 == 0) {
            upperTail->next[1] = p;
            upperTail = p;
        }
        p = p->next[0];
        index++;
    }
    upperTail->next[1] = NULL;
}

void insert(int x) {
    struct Node *prev = head;
    while (prev->next[0] != NULL && prev->next[0]->data < x)
        prev = prev->next[0];

    if (prev->next[0] != NULL && prev->next[0]->data == x) {
        printf("Duplicate value\n");
        return;
    }

    struct Node *p = newNode(x);
    if (p == NULL) { printf("Memory allocation failed\n"); return; }
    p->next[0] = prev->next[0];
    prev->next[0] = p;
    count++;
    rebuild();
}

void search(int x) {
    struct Node *p = head;
    for (int level = MAX_LEVEL - 1; level >= 0; level--) {
        while (p->next[level] != NULL && p->next[level]->data < x)
            p = p->next[level];
    }
    p = p->next[0];
    if (p != NULL && p->data == x) printf("Found\n");
    else printf("Not Found\n");
}

void deleteNode(int x) {
    struct Node *prev = head;
    while (prev->next[0] != NULL && prev->next[0]->data < x)
        prev = prev->next[0];

    if (prev->next[0] == NULL || prev->next[0]->data != x) {
        printf("Not Found\n");
        return;
    }

    struct Node *t = prev->next[0];
    prev->next[0] = t->next[0];
    free(t);
    count--;
    rebuild();
}

void display() {
    printf("Level 1: ");
    for (struct Node *p = head->next[0]; p != NULL; p = p->next[0])
        printf("%d ", p->data);
    printf("\nLevel 2: ");
    for (struct Node *p = head->next[1]; p != NULL; p = p->next[1])
        printf("%d ", p->data);
    printf("\n");
}

int main() {
    int ch, x;
    head = newNode(-999999);
    if (head == NULL) return 1;

    do {
        printf("\n1.Insert 2.Search 3.Delete 4.Display 5.Exit: ");
        scanf("%d", &ch);
        if (ch == 1) { scanf("%d", &x); insert(x); }
        else if (ch == 2) { scanf("%d", &x); search(x); }
        else if (ch == 3) { scanf("%d", &x); deleteNode(x); }
        else if (ch == 4) display();
    } while (ch != 5);

    return 0;
}
```
