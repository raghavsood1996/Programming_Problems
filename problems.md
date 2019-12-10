
# PROGRAMMING PROBLEMS

1. ## Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.</br>[Leetcode](https://leetcode.com/problems/search-in-rotated-sorted-array/)

    (i.e., $[0,1,2,4,5,6,7]$ might become $[4,5,6,7,0,1,2]$).

    You are given a target value to search. If found in the array return its index, otherwise return -1.

    You may assume no duplicate exists in the array.

    Your algorithm's runtime complexity must be in the order of O(log n).

    **Example 1:**
    **Input**: $\mathbf{nums = [4,5,6,7,0,1,2]}$, **target** = 0.
    **Output**: 4 </br>

    **Example 2:**
    **Input:** $\mathbf{nums = [4,5,6,7,0,1,2]}$, **target** = 3 **Output:** -1

    ```C++
        class Solution {
        public:
            int search(vector<int>& nums, int target) {
                int start = 0, end = nums.size()-1;
                while(start <= end){
                    cout<<start<<" "<<end<<endl;
                    int mid = (start + end)/2;
                    if(target == nums[mid]){
                        return mid;
                    }
                    else if(nums[mid] >= nums[start]){ //unrotated part of the array
                        if(target >= nums[start] && target < nums[mid]) 
                            end = mid-1;
                        else
                            start = mid+1;
                    }
                    else{ //rotated part of the array
                        if(target <= nums[end] && target > nums[mid])
                            start = mid+1;
                        else
                            end = mid-1;
                    }
                }
                cout<<start<<" "<<end<<endl;
                return -1;
            }
        };
    ```

    ### **Notes**

    * Binary Search Problem
    * Figure out what part of the array the middle element and target belong to and accordingly change start and end of the binary search.

2. ## **DFS GRAPH**

   ```C++
       #include <iostream>
       #include <list>
        #include<memory>
        using namespace std;

        class Node{
            public:
            int val;
            int stat; //0 if unvisited 1 if visited and -1 if visiting
            list<Node*> neighbors;

            Node(int val){
                this->val = val;
                this->stat = 0;
            }
        };

        class Graph{
            public:
            list<Node*> vertices;
        };

        bool dfs_visit(Node* n,int target){
            if(n->stat == 1){
                return 0;
            }
            if(n->val == target){

                return 1;
            }

            n->stat = -1; //visiting
            for(auto neigh: n->neighbors){
                if(dfs_visit(neigh,target)){
                    return 1;
                };
            }

            n->stat = 1; //visited
            return 0;

        }
        int main()
        {

            Node n1(1),n2(5),n3(6),n4(8),n5(7),n6(9);
            Graph le_graph;
            n1.neighbors.push_back(&n2);
            n1.neighbors.push_back(&n3);
            n2.neighbors.push_back(&n4);
            n3.neighbors.push_back(&n4);
            n3.neighbors.push_back(&n5);
            n4.neighbors.push_back(&n6);

            le_graph.vertices.push_back(&n1);
            le_graph.vertices.push_back(&n2);
            le_graph.vertices.push_back(&n3);
            le_graph.vertices.push_back(&n4);
            le_graph.vertices.push_back(&n5);
            le_graph.vertices.push_back(&n6);
            int target = 100;

            if(dfs_visit(&n1,target)){
                cout<<"target found";
            }
            else{
                cout<<"not found"<<"\n";
            }

            return 0;
        }
    ```

3. ## **BFS GRAPH**

    ```C++
    #include <iostream>
    #include <list>
    #include<memory>
    #include<queue>
    using namespace std;

    class Node
    {
        public:
        int val;
        int stat; //0 if unvisited 1 if visited and -1 if visiting
        list<Node*> neighbors;

        Node(int val){
            this->val = val;
            this->stat = 0;
        }
    };

    class Graph
    {
        public:
        list<Node*> vertices;
    };
  
    void bfs(Node* start)
    {
        if(start->stat == 1|| start== NULL)
        {
            return;
        }

    queue<Node*> q;
    q.push(start);
    start->stat = -1;
        while(!q.empty())
        {
            Node* current = q.front();
            q.pop();

            std::cout<<current->val<<"\n"; //processing the current node
            for(auto neigh: current->neighbors)
            {
                if(neigh->stat == 0) //only processing the unvisited nodes
                {
                    q.push(neigh);
                    neigh->stat = -1;
                }
            }
            current->stat = 1;
        }
     return;
    }

    int main()
    {

        Node n1(1),n2(5),n3(6),n4(8),n5(7),n6(9);
        Graph le_graph;
        n1.neighbors.push_back(&n2);
        n1.neighbors.push_back(&n3);
        n2.neighbors.push_back(&n4);
        n3.neighbors.push_back(&n4);
        n3.neighbors.push_back(&n5);
        n4.neighbors.push_back(&n6);

        le_graph.vertices.push_back(&n1);
        le_graph.vertices.push_back(&n2);
        le_graph.vertices.push_back(&n3);
        le_graph.vertices.push_back(&n4);
        le_graph.vertices.push_back(&n5);
        le_graph.vertices.push_back(&n6);
        int target = 100;


        bfs(&n1);

        return 0;
     }
    ```

4. ## **Checking if Graph is Bipartite**

    ```C++
    class Solution {
    public:

        bool isBipartite(vector<vector<int>>& graph) {

            vector<int> stat(graph.size(),0); //maintain status of a node

            for(int i =0 ;i <graph.size();i++){ // go through all the components of the graph

                if(stat[i] == 1) continue;
                int start = i;

                vector<int> level(graph.size(),-1); //maintain bfs level of the expansions
                level[start] = 0;
                queue<int> bfs_q;
                bfs_q.push(start);
                stat[start] = -1;
                int curr_level = 0;

                while(!bfs_q.empty()) //bfs visit
                {
                    int curr_node = bfs_q.front();

                    bfs_q.pop();


                    curr_level = level[curr_node] + 1;
                    for(int ngb: graph[curr_node])
                    {

                        if(stat[ngb] == 1) continue;


                        if(level[ngb] == level[curr_node]) //odd cycle
                        {

                            return 0;
                        }


                        bfs_q.push(ngb);
                        stat[ngb] = -1;

                        level[ngb] = curr_level;

                    }

                    stat[curr_node] = 1; //done processing current node
                }
            }
            return 1;
        }
    };
    ```

    ### **Notes**
    * Bipartite Graph is a graph in which we can seperate the nodes in two groups such that there are no edges between node of a group.
    * Usually asked for undirecte graphs.
    * Implented using **BFS** by keeping track of the level of nodes.
    * If the level a parent and a neighbour node is same then there is an odd cycle in the graph and the graph is not Bipartite.

5. ## **Finding Groups in Bipartite Graphs**</br> [LeetCode](https://leetcode.com/problems/possible-bipartition/)

    ```C++
    class Solution {
    public:
        bool possibleBipartition(int N, vector<vector<int>>& dislikes) {
            vector<int> stat(N,0);
            vector<vector<int>> graph(N);

            for(auto dislike:dislikes)
            {
                graph[dislike[0]-1].push_back(dislike[1]-1);  //creating an undirected graph

                graph[dislike[1]-1].push_back(dislike[0]-1);

            }

            vector<int> group1;
            vector<int> group2;
            for(int i =0; i<N; i++)
            {
                if(stat[i] == 1) continue;


                vector<int> levels(N,-1);
                int start = i;
                levels[start] = 0;
                queue<int> bfs_q;
                bfs_q.push(start);
                stat[start] = -1;
                int curr_level = 0;
                while(!bfs_q.empty())
                {
                    int curr_node = bfs_q.front();
                    bfs_q.pop();

                    if(stat[curr_node] == 1) continue;

                    curr_level = levels[curr_node] + 1;
                    if(curr_level%2 == 0) //adding to corresponding group
                    {
                        group1.push_back(curr_node);
                    }
                    else
                    {
                        group2.push_back(curr_node);
                    }
                    for(int ngb: graph[curr_node])
                    {
                        if(stat[ngb] == 1) continue;
                        if(levels[ngb] == levels[curr_node]) return 0;
                        bfs_q.push(ngb);
                        stat[ngb] = -1;
                        levels[ngb] = curr_level;
                    }
                    stat[curr_node] = 1;
                }

            }
            for(auto a:group1){
                cout<<a+1<<" ";
            }
            return 1;
        }
    };
    ```

    ### **NOTES**
    * Maintain the expansion level during bfs.
    * All nodeat even levels go to one class and the odd ones go to the other
    * given the edges only convert it to a graph first.

6. ## **Number of Islands**</br>[Leetcode](https://leetcode.com/problems/number-of-islands/)

    Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

    ```C++
    class Solution {
    public:
        char query(int y, int x, vector<vector<char>>& grid) //helper funtion for reading the grid
        {
            int rows = grid.size();

            int cols = grid[0].size();
            if(y >= rows || y<0 || x>=cols || x<0){
                return '0';
            }

            else{
                return grid[y][x];
            }

        }

        int numIslands(vector<vector<char>>& grid) {
            int rows = grid.size();
            if(rows == 0) return 0;
            int cols = grid[0].size();
            vector<vector<int> > visited(rows,vector<int>(cols,0)); //maintaing the status of the nodes

            int n_dx[4] = {1,0,-1,0}; //4 connected grid neighbors
            int n_dy[4] = {0,1,0,-1};

            int num_islands = 0;

            for(int i=0; i<rows; i++){
                for(int j=0; j<cols; j++){
                    if(grid[i][j] == '0' || visited[i][j] == 1) continue;
                    num_islands++ ;

                    char start = grid[i][j];
                    queue<pair<int,int>> bfs_q; //using std pair for storing pair of coordinates
                    bfs_q.push(make_pair(i,j));
                    visited[i][j] = -1;

                    while(!bfs_q.empty()){
                        auto curr = bfs_q.front();
                        bfs_q.pop();
                        if(visited[curr.first][curr.second] == 1) continue;

                        for(int n=0;n<4; n++)
                        {
                            int n_y= curr.first + n_dy[n];
                            int n_x = curr.second +n_dx[n];
                            if(query(n_y,n_x,grid) == '1' && visited[n_y][n_x] == 0) //if neighbor is land and not visited keep going
                            {
                                bfs_q.push(make_pair(n_y,n_x));
                                visited[n_y][n_x] = -1;
                            }

                            visited[curr.first][curr.second] = 1;
                        }
                    }
                }
            }
            return num_islands;
        }
    };
    ```

    ### **NOTES**
    * Formulate Problem as finding connected components of a graph.
    * Each connected component is an island.
