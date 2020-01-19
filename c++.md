# Enhancing the C++ Knowledge

1. ## **Min and Max Heap C++ STL**

   Both min and max heaps can be implemnted using a Priority Queue.
   ## MAX HEAP

   ```C++
   // Note that by default C++ creates a max-heap

    // for priority queue
    #include <iostream>
    #include <queue>
    using namespace std;

    void showpq(priority_queue <int> gq)
    {
        priority_queue <int> g = gq;
        while (!g.empty())
        {
            cout << '\t' << g.top();
            g.pop();
        }
        cout << '\n';
    }

    int main()
    {
        priority_queue <int> gquiz;//by default priority queue is a max heap
        priority_queue <int> gquiz;//by default priority queue is a max heap
        gquiz.push(10);
        gquiz.push(30);
        gquiz.push(20);
        gquiz.push(5);
        gquiz.push(1);

        cout << "The priority queue gquiz is : ";
        showpq(gquiz);

        cout << "\ngquiz.size() : " << gquiz.size();
        cout << "\ngquiz.top() : " << gquiz.top();


        cout << "\ngquiz.pop() : ";
        gquiz.pop();
        showpq(gquiz);

        return 0;
    }
    ```
    ## MIN HEAP

    ```C++
    // C++ program to demonstrate min heap
    #include <iostream>
    #include <queue>

    using namespace std;

    void showpq(priority_queue <int, vector<int>, greater<int>> gq)
    {
        priority_queue <int, vector<int>, greater<int>> g = gq;
        while (!g.empty())
        {
            cout << '\t' << g.top();
            g.pop();
        }
        cout << '\n';
    }

    int main ()
    {
        priority_queue <int, vector<int>, greater<int>> gquiz; //a little difficult to remember syntax
        gquiz.push(10);
        gquiz.push(30);
        gquiz.push(20);
        gquiz.push(5);
        gquiz.push(1);

        cout << "The priority queue gquiz is : ";
        showpq(gquiz);

        cout << "\ngquiz.size() : " << gquiz.size();
        cout << "\ngquiz.top() : " << gquiz.top();


        cout << "\ngquiz.pop() : ";
        gquiz.pop();
        showpq(gquiz);

        return 0
    }
    ```
    ## NOTE

    In case of numerical values we can multiply values by -1 and use the max heap to get effect of a min heap.

2.  ## **Sorting a Vector of Objects**

    ```C++
    struct MyStruct
    {
        int key;
        std::string stringValue;

        MyStruct(int k, const std::string& s) : key(k), stringValue(s) {}
    };

    struct less_than_key
    {
        inline bool operator() (const MyStruct& struct1, const MyStruct& struct2)
        {
            return (struct1.key < struct2.key);
        }
    };

    std::vector < MyStruct > vec;

    vec.push_back(MyStruct(4, "test"));
    vec.push_back(MyStruct(3, "a"));
    vec.push_back(MyStruct(2, "is"));
    vec.push_back(MyStruct(1, "this"));

    std::sort(vec.begin(), vec.end(), less_than_key());
    ```


