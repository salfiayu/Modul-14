
# <h1 align="center">Laporan Praktikum Struktur Data<br> Modul 14 Graph </h1>
<p align="center">Salfi Ayu Ramadhani / 103112430224</p>

## Dasar Teori

Graph merupakan struktur data non-linear yang terdiri dari himpunan simpul (vertex/node) dan garis penghubung (edge). Simpul merepresentasikan objek, sedangkan edge menyatakan hubungan antar objek tersebut. Graph dapat bersifat berarah maupun tidak berarah dan dapat direpresentasikan menggunakan beberapa metode, salah satunya adjacency list. Untuk menelusuri graph, digunakan algoritma Depth First Search (DFS) yang bekerja secara mendalam dan Breadth First Search (BFS) yang bekerja secara melebar. Graph banyak digunakan untuk memodelkan hubungan pada berbagai sistem seperti jaringan, navigasi, dan struktur data.

## Guided

### Soal 1

```cpp
#include <iostream>
#include <string>
using namespace std;

struct ChildNode
{
    string info;
    ChildNode *next;
};

struct ParentNode
{
    string info;
    ChildNode *childHead;
    ParentNode *next;
};

ParentNode *createParent(string info)
{
    ParentNode *newNode = new ParentNode;
    newNode->info = info;
    newNode->childHead = NULL;
    newNode->next = NULL;
    return newNode;
}

ChildNode *createChild(string info)
{
    ChildNode *newNode = new ChildNode;
    newNode->info = info;
    newNode->next = NULL;
    return newNode;
}

void insertParent(ParentNode *&head, string info)
{
    ParentNode *newNode = createParent(info);
    if (head == NULL)
    {
        head = newNode;
    }
    else
    {
        ParentNode *temp = head;
        while (temp->next != NULL)
        {
            temp = temp->next;
        }
        temp->next = newNode;
    }
}

void insertChild(ParentNode *head, string parentInfo, string childInfo)
{
    ParentNode *p = head;
    while (p != NULL && p->info != parentInfo)
    {
        p = p->next;
    }

    if (p != NULL)
    {
        ChildNode *newChild = createChild(childInfo);
        if (p->childHead == NULL)
        {
            p->childHead = newChild;
        }
        else 
        {
            ChildNode *c = p->childHead;
            while (c->next != NULL)
            {
                c = c->next;
            }
            c->next = newChild;
        }
    }
}

void printAll(ParentNode *head)
{
    ParentNode *p = head;
    while (p != NULL)
    {
        cout << p->info;
        ChildNode *c = p->childHead;
        if (c != NULL)
        {
            while (c != NULL)
            {
                cout << " -> " << c->info;
                c = c->next;
            }
        }
        cout << endl;
        p = p->next;
    }
}

int main()
{
    ParentNode *list = NULL;

    insertParent(list, "Parent Node 1");
    insertParent(list, "Parent Node 2");

    insertChild(list, "Parent Node 1", "Child Node A");
    insertChild(list, "Parent Node 1", "Child Node B");
    insertChild(list, "Parent Node 2", "Child Node C");

    printAll(list);

    return 0;
}
```

> Screenshoot 
> ![Screenshot Soal 1](https://github.com/salfiayu/Modul-13/blob/main/Modul%2013/screenshoot/Screenshot%20(207).png)

Program itu berfungsi membuat multi linked list yang menyimpan data bertingkat, di mana setiap parent bisa punya banyak child. Fungsi insertParent menambah parent baru, insertChild menambah child ke parent tertentu, dan printAll menampilkan semua parent beserta child-nya. Program ini dipakai untuk mengelola data hierarkis seperti kategori–subkategori atau fakultas–jurusan.

---

## Unguided

### Soal 1

Buatlah ADT Graph tidak berarah file “graph.h”:
Type infoGraph: char
Type adrNode : pointer to ElmNode
Type adrEdge : pointer to ElmNode
Type ElmNode <
info : infoGraph
visited : integer
firstEdge : adrEdge
Next : adrNode
>
Type ElmEdge <
Node : adrNode
Next : adrEdge
>
Type Graph <
first : adrNode
>
procedure CreateGraph (input/output G : Graph)
procedure InsertNode (input/output G : Graph,
input X : infotype)
procedure ConnectNode (input/output N1, N2 : adrNode)
procedure PrintInfoGraph (input G : Graph)
Program 4 Graph.h

Buatlah implementasi ADT Graph pada file “graph.cpp” dan cobalah hasil implementasi ADT
pada file “main.cpp”.

## Soal 2
Buatlah prosedur untuk menampilkanhasil penelusuran DFS. prosedur PrintDFS (Graph G, adrNode N);

## Soal 3
Buatlah prosedur untuk menampilkanhasil penelusuran DFS. prosedur PrintBFS (Graph G, adrNode N);


## graph.h

```cpp
#ifndef GRAPH_H
#define GRAPH_H

#include <iostream>
using namespace std;

typedef char infoGraph;
typedef struct ElmNode *adrNode;
typedef struct ElmEdge *adrEdge;

struct ElmEdge {
    adrNode Node;
    adrEdge Next;
};

struct ElmNode {
    infoGraph info;
    int visited;
    adrEdge firstEdge;
    adrNode Next;
};

struct Graph {
    adrNode first;
};

void CreateGraph(Graph &G);
void InsertNode(Graph &G, infoGraph X);
adrNode FindNode(Graph G, infoGraph X);
void ConnectNode(adrNode N1, adrNode N2);
void PrintInfoGraph(Graph G);

void PrintDFS(Graph &G, adrNode N);
void PrintBFS(Graph &G, adrNode N);

#endif


```


### graph.cpp

```cpp
#include "graph.h"
#include <queue>

void CreateGraph(Graph &G) {
    G.first = NULL;
}

void InsertNode(Graph &G, infoGraph X) {
    adrNode P = new ElmNode;
    P->info = X;
    P->visited = 0;
    P->firstEdge = NULL;
    P->Next = G.first;
    G.first = P;
}

adrNode FindNode(Graph G, infoGraph X) {
    adrNode P = G.first;
    while (P != NULL) {
        if (P->info == X)
            return P;
        P = P->Next;
    }
    return NULL;
}

void ConnectNode(adrNode N1, adrNode N2) {
    adrEdge E1 = new ElmEdge;
    E1->Node = N2;
    E1->Next = N1->firstEdge;
    N1->firstEdge = E1;

    adrEdge E2 = new ElmEdge;
    E2->Node = N1;
    E2->Next = N2->firstEdge;
    N2->firstEdge = E2;
}

void PrintInfoGraph(Graph G) {
    cout << "Adjacency List Graph:" << endl;
    for (char c = 'A'; c <= 'H'; c++) {
        adrNode P = G.first;
        while (P != NULL && P->info != c) {
            P = P->Next;
        }
        if (P != NULL) {
            cout << P->info << " -> ";
            adrEdge E = P->firstEdge;
            while (E != NULL) {
                cout << E->Node->info << " ";
                E = E->Next;
            }
            cout << endl;
        }
    }
}

void PrintDFS(Graph &G, adrNode N) {
    if (N == NULL || N->visited == 1)
        return;

    cout << N->info << " ";
    N->visited = 1;

    adrEdge E = N->firstEdge;
    while (E != NULL) {
        PrintDFS(G, E->Node);
        E = E->Next;
    }
}

void PrintBFS(Graph &G, adrNode N) {
    queue<adrNode> Q;
    N->visited = 1;
    Q.push(N);

    while (!Q.empty()) {
        adrNode P = Q.front();
        Q.pop();
        cout << P->info << " ";

        adrEdge E = P->firstEdge;
        while (E != NULL) {
            if (E->Node->visited == 0) {
                E->Node->visited = 1;
                Q.push(E->Node);
            }
            E = E->Next;
        }
    }
}

```

### main.cpp

```cpp
#include "graph.h"

int main() {
    Graph G;
    CreateGraph(G);

    InsertNode(G, 'A');
    InsertNode(G, 'B');
    InsertNode(G, 'C');
    InsertNode(G, 'D');
    InsertNode(G, 'E');
    InsertNode(G, 'F');
    InsertNode(G, 'G');
    InsertNode(G, 'H');

    adrNode A = FindNode(G, 'A');
    adrNode B = FindNode(G, 'B');
    adrNode C = FindNode(G, 'C');
    adrNode D = FindNode(G, 'D');
    adrNode E = FindNode(G, 'E');
    adrNode F = FindNode(G, 'F');
    adrNode Gg = FindNode(G, 'G');
    adrNode H = FindNode(G, 'H');

    ConnectNode(A, B);
    ConnectNode(A, C);
    ConnectNode(B, D);
    ConnectNode(B, E);
    ConnectNode(C, F);
    ConnectNode(C, Gg);
    ConnectNode(D, H);
    ConnectNode(E, H);
    ConnectNode(F, H);
    ConnectNode(Gg, H);

    PrintInfoGraph(G);

    cout << "\nDFS Traversal (mulai dari A):" << endl;
    PrintDFS(G, A);

    adrNode P = G.first;
    while (P != NULL) {
        P->visited = 0;
        P = P->Next;
    }

    cout << "\n\nBFS Traversal (mulai dari A):" << endl;
    PrintBFS(G, A);

    return 0;
}


```

> Sreenshoot 
> ![Screenshot Soal 1](https://github.com/salfiayu/Modul-13/blob/main/Modul%2013/screenshoot/Screenshot%20(206).png)

Program ini mengimplementasikan graph tidak berarah menggunakan linked list. Node A sampai H dihubungkan dan ditampilkan dalam bentuk adjacency list. Penelusuran DFS dilakukan secara mendalam menggunakan rekursi, sedangkan BFS dilakukan secara melebar menggunakan queue. Hasil program menampilkan adjacency list serta urutan penelusuran DFS dan BFS.

1. https://www.trivusi.web.id/2022/07/struktur-data-graph.html (diakses 17-12-2025)
