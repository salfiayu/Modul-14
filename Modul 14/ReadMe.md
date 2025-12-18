
# <h1 align="center">Laporan Praktikum Struktur Data<br> Modul 14 Graph </h1>
<p align="center">Salfi Ayu Ramadhani / 103112430224</p>

## Dasar Teori

Graph merupakan struktur data non-linear yang terdiri dari himpunan simpul (vertex/node) dan garis penghubung (edge). Simpul merepresentasikan objek, sedangkan edge menyatakan hubungan antar objek tersebut. Graph dapat bersifat berarah maupun tidak berarah dan dapat direpresentasikan menggunakan beberapa metode, salah satunya adjacency list. Untuk menelusuri graph, digunakan algoritma Depth First Search (DFS) yang bekerja secara mendalam dan Breadth First Search (BFS) yang bekerja secara melebar. Graph banyak digunakan untuk memodelkan hubungan pada berbagai sistem seperti jaringan, navigasi, dan struktur data.

## Guided

### Soal 1

```cpp
#include <iostream>
#include <queue>
#include <stack>

using namespace std;

typedef char infoGraph;

struct ElmNode;
struct ElmEdge;

typedef ElmNode *adrNode;
typedef ElmEdge *adrEdge;

struct ElmNode
{
    infoGraph info;
    int visited;
    adrEdge firstEdge;
    adrNode next;
};

struct ElmEdge
{
    adrNode node;
    adrEdge next;
};

struct Graph
{
    adrNode first;
};

void CreateGraph(Graph &G)
{
    G.first = NULL;
}

adrNode AllocateNode(infoGraph X)
{
    adrNode P = new ElmNode;
    P->info = X;
    P->visited = 0;
    P->firstEdge = NULL;
    P->next = NULL;
    return P;
}

adrEdge AllocateEdge(adrNode N)
{
    adrEdge P = new ElmEdge;
    P->node = N;
    P->next = NULL;
    return P;
}

void InsertNode(Graph &G, infoGraph X)
{
    adrNode P = AllocateNode(X);
    P->next = G.first;
    G.first = P;
}

adrNode FindNode(Graph G, infoGraph X)
{
    adrNode P = G.first;
    while (P != NULL)
    {
        if (P->info == X)
            return P;
        P = P->next;
    }
    return NULL;
}

void ConnectNode(Graph &G, infoGraph A, infoGraph B)
{
    adrNode N1 = FindNode(G, A);
    adrNode N2 = FindNode(G, B);

    if (N1 == NULL || N2 == NULL)
    {
        cout << "Node tidak ditemukan!\n";
        return;
    }

    adrEdge E1 = AllocateEdge(N2);
    E1->next = N1->firstEdge;
    N1->firstEdge = E1;

    adrEdge E2 = AllocateEdge(N1);
    E2->next = N2->firstEdge;
    N2->firstEdge = E2;
}

void PrintInfoGraph(Graph G)
{
    adrNode P = G.first;
    while (P != NULL)
    {
        cout << "Node " << P->info << " terhubung dengan: ";
        adrEdge E = P->firstEdge;
        while (E != NULL)
        {
            cout << E->node->info << " ";
            E = E->next;
        }
        cout << endl;
        P = P->next;
    }
}

void ResetVisited(Graph &G)
{
    adrNode P = G.first;
    while (P != NULL)
    {
        P->visited = 0;
        P = P->next;
    }
}

void PrintDFS(Graph &G, adrNode N)
{
    if (N == NULL)
        return;

    N->visited = 1;
    cout << N->info << " ";

    adrEdge E = N->firstEdge;
    while (E != NULL)
    {
        if (E->node->visited == 0)
        {
            PrintDFS(G, E->node);
        }
        E = E->next;
    }
}

void PrintBFS(Graph &G, adrNode N)
{
    if (N == NULL)
        return;

    queue<adrNode> Q;
    Q.push(N);

    while (!Q.empty())
    {
        adrNode curr = Q.front();
        Q.pop();

        if (curr->visited == 0)
        {
            curr->visited = 1;
            cout << curr->info << " ";

            adrEdge E = curr->firstEdge;
            while (E != NULL)
            {
                if (E->node->visited == 0)
                {
                    Q.push(E->node);
                }
                E = E->next;
            }
        }
    }
}

int main()
{
    Graph G;
    CreateGraph(G);
    InsertNode(G, 'A');
    InsertNode(G, 'C');
    InsertNode(G, 'D');
    InsertNode(G, 'E');
    
    ConnectNode(G, 'A', 'B');
    ConnectNode(G, 'A', 'C');
    ConnectNode(G, 'B', 'D');
    ConnectNode(G, 'C', 'E');

    cout << "=== Struktur Graph (Adjacency List) ===\n";
    PrintInfoGraph(G);

    cout << "\n=== DFS dari Node A ===\n";
    ResetVisited(G);
    PrintDFS(G, FindNode(G, 'A'));

    cout << "\n\n=== BFS dari Node A ===\n";
    ResetVisited(G);
    PrintBFS(G, FindNode(G, 'A'));

    cout << endl;
    return 0;
}

```

> Screenshoot 
> ![Screenshot Soal 1](https://github.com/salfiayu/Modul-14/blob/main/Modul%2014/screenshoot/Screenshot%20(227).png)

Program ini membuat graph tak berarah menggunakan adjacency list di C++, di mana node dan edge disimpan dalam struct. Graph dapat ditambah node, dihubungkan antar node, dan ditampilkan strukturnya. Program juga menyediakan penelusuran DFS (rekursif) dan BFS (menggunakan queue) untuk menampilkan urutan kunjungan node dari node awal tertentu.

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
> ![Screenshot Soal 1](https://github.com/salfiayu/Modul-14/blob/main/Modul%2014/screenshoot/Screenshot%20(214).png)

Program ini mengimplementasikan graph tidak berarah menggunakan linked list. Node A sampai H dihubungkan dan ditampilkan dalam bentuk adjacency list. Penelusuran DFS dilakukan secara mendalam menggunakan rekursi, sedangkan BFS dilakukan secara melebar menggunakan queue. Hasil program menampilkan adjacency list serta urutan penelusuran DFS dan BFS.

1. https://www.trivusi.web.id/2022/07/struktur-data-graph.html (diakses 17-12-2025)
