# Лекция: Структуры данных на основе указателей в C++

## Введение

Структуры данных на основе указателей широко используются в программировании на C++ для создания гибких и эффективных решений. Указатели позволяют напрямую работать с памятью, что дает возможность динамически управлять памятью и строить сложные структуры данных. В этой лекции мы рассмотрим основные структуры данных, использующие указатели, их реализацию и управление памятью.

## 1. Связный список

Связный список — это динамическая структура данных, где элементы списка (узлы) связаны указателями. Каждый узел содержит данные и указатель на следующий элемент.

- **Односвязный список**: каждый узел хранит указатель на следующий узел.

Пример реализации односвязного списка в C++:

```cpp
struct Node {
    int data;
    Node* next;

    Node(int data) {
        this->data = data;
        this->next = nullptr;
    }
};

class LinkedList {
    Node* head;

public:
    LinkedList() {
        head = nullptr;
    }

    void insert(int data) {
        Node* newNode = new Node(data);
        newNode->next = head;
        head = newNode;
    }

    void print() {
        Node* current = head;
        while (current != nullptr) {
            std::cout << current->data << " ";
            current = current->next;
        }
        std::cout << std::endl;
    }
};
```

В этом примере каждый узел содержит данные и указатель на следующий узел. Вставка в список осуществляется изменением указателя на следующий элемент.

- **Двусвязный список**: каждый узел хранит указатели на предыдущий и следующий узел, что позволяет перемещаться по списку в обоих направлениях.

Пример реализации двусвязного списка:

```cpp
struct Node {
    int data;
    Node* next;
    Node* prev;

    Node(int data) {
        this->data = data;
        this->next = nullptr;
        this->prev = nullptr;
    }
};

class DoublyLinkedList {
    Node* head;

public:
    DoublyLinkedList() {
        head = nullptr;
    }

    void insert(int data) {
        Node* newNode = new Node(data);
        if (head != nullptr) {
            head->prev = newNode;
            newNode->next = head;
        }
        head = newNode;
    }

    void printForward() const {
        Node* current = head;
        while (current != nullptr) {
            std::cout << current->data << " ";
            current = current->next;
        }
        std::cout << std::endl;
    }

    void printBackward() const {
        Node* current = tail;
        while (current != nullptr) {
            std::cout << current->data << " ";
            current = current->prev;
        }
        std::cout << std::endl;
    }
};
```

## 2. Стек и очередь

- **Стек** — это структура данных типа LIFO (Last In, First Out), где последний добавленный элемент извлекается первым. В C++ стек можно реализовать с помощью связного списка, используя указатели для управления элементами.

Пример реализации стека:

```cpp
class Stack {
    Node* top;

public:
    Stack() {
        top = nullptr;
    }

    void push(int data) {
        Node* newNode = new Node(data);
        newNode->next = top;
        top = newNode;
    }

    int pop() {
        if (top == nullptr) {
            throw std::runtime_error("Stack is empty");
        }
        int data = top->data;
        Node* temp = top;
        top = top->next;
        delete temp;
        return data;
    }

    bool isEmpty() {
        return top == nullptr;
    }
};
```

- **Очередь** — это структура данных типа FIFO (First In, First Out), где первым добавленным элементом является первый извлекаемый. Очередь также можно реализовать с использованием указателей и связного списка.

Пример реализации очереди:

```cpp
class Queue {
    Node* head;
    Node* tail;

public:
    Queue() {
        head = nullptr;
        tail = nullptr;
    }

    void enqueue(int data) {
        Node* newNode = new Node(data);
        if (tail != nullptr) {
            tail->next = newNode;
        }
        tail = newNode;
        if (head == nullptr) {
            head = newNode;
        }
    }

    int dequeue() {
        if (head == nullptr) {
            throw std::runtime_error("Queue is empty");
        }
        int data = head->data;
        Node* temp = head;
        head = head->next;
        if (head == nullptr) {
            tail = nullptr;
        }
        delete temp;
        return data;
    }

    bool isEmpty() {
        return head == nullptr;
    }
};
```

## 3. Деревья

**Дерево** — это иерархическая структура данных, где каждый узел может иметь несколько дочерних узлов. Наиболее часто используется **бинарное дерево**, в котором каждый узел имеет не более двух дочерних узлов (левый и правый).

Пример реализации бинарного дерева:

```cpp
struct TreeNode {
    int data;
    TreeNode* left;
    TreeNode* right;

    TreeNode(int data) {
        this->data = data;
        this->left = nullptr;
        this->right = nullptr;
    }
};

class BinaryTree {
    TreeNode* root;

public:
    BinaryTree() {
        root = nullptr;
    }

    void insert(int data) {
        root = insertRec(root, data);
    }

    TreeNode* insertRec(TreeNode* node, int data) {
        if (node == nullptr) {
            return new TreeNode(data);
        }
        if (data < node->data) {
            node->left = insertRec(node->left, data);
        } else {
            node->right = insertRec(node->right, data);
        }
        return node;
    }

    void inorder() {
        inorderRec(root);
        std::cout << std::endl;
    }

    void inorderRec(TreeNode* node) {
        if (node == nullptr) {
            return;
        }
        inorderRec(node->left);
        std::cout << node->data << " ";
        inorderRec(node->right);
    }
};

int main()
{
    BinaryTree* tree = new BinaryTree();

    tree->insert(5);
    tree->insert(6);
    tree->insert(7);
    tree->insert(2);
    tree->insert(1);
    tree->insert(5);

    tree->inorder();
}
```

## 4. Графы

**Граф** — это структура данных, состоящая из узлов (вершин) и рёбер, которые соединяют узлы. Графы могут быть реализованы с использованием указателей через списки смежности, где каждая вершина хранит указатель на список связанных с ней вершин.

Пример реализации графа через списки смежности:

```cpp
#include <vector>

class Graph {
    int V;
    std::vector<int>* adj;

public:
    Graph(int V) {
        this->V = V;
        adj = new std::vector<int>[V];
    }

    void addEdge(int v, int w) {
        adj[v].push_back(w);
        adj[w].push_back(v);
    }

    void printGraph() {
        for (int v = 0; v < V; ++v) {
            std::cout << "Vertex " << v << ":";
            for (auto x : adj[v])
                std::cout << " -> " << x;
            std::cout << std::endl;
        }
    }

    ~Graph() {
        delete[] adj;
    }
};
```
