
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

7. ## **Diameter of a Binary Tree** </br> [Leetcode](https://leetcode.com/problems/diameter-of-binary-tree/)

    Given a binary tree, you need to compute the length of the diameter of the tree. The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

    ```C++
    /**
    * Definition for a binary tree node.
    * struct TreeNode {
    *     int val;
    *     TreeNode *left;
    *     TreeNode *right;
    *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
    * };
    */
    class Solution {
    public:

        int height_util(TreeNode* node, int& max_path){
            if(node == NULL){
                return 0;
            }


            int l_height = height_util(node->left,max_path); //height of left subtree
            int r_height = height_util(node->right,max_path); // height of right subtree
            int path_length = l_height + r_height + 1; //longest path will have the max sum of height of left and right subtree

            if(path_length > max_path){
                max_path = path_length;
            }

            return 1 + max(l_height,r_height);

        }

        int diameterOfBinaryTree(TreeNode* root) {

            if(root == NULL){
                return 0;
            }
            int max_path = -1;
            auto ht = height_util(root, max_path);
            return max_path - 1;
        }
    };
    ```

8. ## **Lowest Common Ancestor Binary Tree**</br> [Leetcode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

   Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

   ```C++
    /**
    * Definition for a binary tree node.
    * struct TreeNode {
    *     int val;
    *     TreeNode *left;
    *     TreeNode *right;
    *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
    * };
    */
    class Solution {
    public:
        TreeNode* lca_util(TreeNode* node, TreeNode* p, TreeNode* q){

            if(node == NULL){
                return NULL;
            }

            if(node->val == p->val || node->val == q->val){ 
                return node;
            }

            TreeNode* left_lca = lca_util(node->left,p,q);
            TreeNode* right_lca = lca_util(node->right, p,q);
            if(left_lca != NULL && right_lca != NULL){ // if found in both right and left subtrees then it is the lowest commmon ancestor
                return node;
            }


            return left_lca != NULL ? left_lca : right_lca; // if only found in one then return the one where it was found
        }

        TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {

            return lca_util(root,p,q);
        }
    };
    ```

    ### **NOTES**
    * Follow the bottom to up approach.

9. ## **Validating a BST** </br> [Leetcode](https://leetcode.com/problems/validate-binary-search-tree/submissions/)

    ```C++
        /**
    * Definition for a binary tree node.
    * struct TreeNode {
    *     int val;
    *     TreeNode *left;
    *     TreeNode *right;
    *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
    * };
    */
    class Solution {
    public:
        pair<long,long>* bst_util(TreeNode* node){

            if( node == NULL)
            {
            pair<long,long>* min_max = new pair<long,long> ;
                *min_max = make_pair(LONG_MAX, LONG_MIN); 
                return min_max;

            }


            pair<long,long>* left = bst_util(node->left); //finding min max for left subtree
            pair<long,long>* right = bst_util(node->right); //finding min max for right subtree

            if(left == NULL || right == NULL){
                return NULL;
            }


            //Max value in the left subtree should be smaller than the root value
            //Min value in the right subtree should be greater than the root value

            if(left->second >= node->val || right->first <= node->val){
                return NULL;
            }

            long min = node->left == NULL ? node->val : left->first;
            long max = node->right == NULL ? node->val : right->second;

            pair<long,long>* ret = new pair<long,long> ;
            *ret = make_pair(min, max); 
            return ret;


        }

        bool isValidBST(TreeNode* root) {

            if(root == NULL || (root->left == NULL && root->right == NULL)){
                return 1;
            }

            if(bst_util(root) != NULL){


                return 1;
            }
            return 0;
        }

    };
    ```

10. ## **Coin Change Problem DP**</br> [Leetcode](https://leetcode.com/problems/coin-change/)

    You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

    ```C++
    class Solution {
    public:

        int util_coins(vector<int>& coins, int amount ,vector<int> &count){

            if(amount == 0){
                return 0;
            }

            if(amount< 0){
                return -1;
            }

            if(count[amount-1] != 0) return count[amount-1];

            int min = INT_MAX;
            for(int coin:coins){
                int res = util_coins(coins,amount- coin,count) ;
                if(res>=0 && res <min){
                    min = 1 + res;
                }
            }
            count[amount-1] = min == INT_MAX ? -1 :min;

            return count[amount-1];
        }

        int coinChange(vector<int>& coins, int amount) {
        if(amount < 1){
            return 0;
        }
            vector<int> count(amount+1,0);
            return util_coins(coins,amount,count);
        }
    };
    ```

    ### **Intution**

    We can use top to bottom approach of Dynamic Programming. Optimal substructure of the problem is </br>$F(s) = F(s-c) +1$ .</br> But as we dont know the denomination of the closest last coin C to S. We compute $F(S-C_i)$ for each possible denomination and choose the minimum among them.  The following recuurence relation holds:</br>
    $F(S) = \min_{i=0..n-1} F(S-c_i)+1$ </br>
    subject to $S-c_i \geq 0$.

10. ## **Lowest Common Ancestor BST**</br>[Leetcode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/submissions/)

    ```C++
        /**
    * Definition for a binary tree node.
    * struct TreeNode {
    *     int val;
    *     TreeNode *left;
    *     TreeNode *right;
    *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
    * };
    */
    class Solution {
    public:
        TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
            if(root == NULL){
                return NULL;
            }

            //keep going left or right until both p and q are on different sides of the tree
            if(root->val < p->val && root->val <q->val){
                return(lowestCommonAncestor(root->right,p,q));
            }

            if(root->val >p->val && root->val > q->val){
                return (lowestCommonAncestor(root->left,p,q));
            }


            else{
                return root;
            }
        }
    };
    ```

11. ## **Merge Intervals**</br> [Leetcode](https://leetcode.com/problems/merge-intervals/)

    Given a collection of intervals, merge all overlapping intervals.

    ```C++
    struct Point{
    int val;
    bool start;


    Point(int val,bool start){
        this->val = val;
        this->start = start;
    }


    };

    struct less_than_key{ //custom comparator
        inline bool operator()(const Point &p1, const Point &p2){
            if(p1.val == p2.val){
                if(p1.start){
                    return 1;
                }
            }
            return (p1.val < p2.val);
        }

    };


    class Solution {
    public:
        vector<vector<int>> merge(vector<vector<int>>& intervals) {

            vector<Point> points;
            for(auto interval : intervals){
                Point start(interval[0],1);
                points.push_back(start);
                Point end(interval[1],0);
                points.push_back(end);
            }


            sort(points.begin(),points.end(),less_than_key());
            vector<vector<int>> merged_intervals;

            int start;
            int num_intervals = 0;
            for(int i=0; i<points.size();i++){
                if(points[i].start){
                    num_intervals++;
                    if(num_intervals == 1){
                        start = points[i].val;
                    }
                }

                else if(!points[i].start){
                    num_intervals--;
                    if(num_intervals == 0){
                        vector<int> m_int(2);
                        m_int[0] = start;
                        m_int[1] = points[i].val;
                        merged_intervals.push_back(m_int);
                    }
                }
            }


            return merged_intervals;
       }
    };
    ```

    ### **Notes**

    * Line Sweep Algorithm
    * Have a custom class point with start and end ids.
    * sort the vector of points according to time vals.
    * go through the vector and accordingly modify the count and keep track of start and end of overlapping intervals.

12. ## **Kth order Statistics (Selection Algorithm)**</br>[Leetcode](https://leetcode.com/problems/kth-largest-element-in-an-array/submissions/)

    Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

    ```C++
        class Solution {
        public:

        int get_random( int start, int end){
            if (start > end){
                cout<<"ERROR"<<"\n";
            }
            int randNum = rand()%(end-start + 1) + start;
            return randNum;
        }

        int partition(vector<int> &nums, int pivot_idx, int start, int end){
            int less = start;
            for(int i = start + 1; i<= end; i++){
                if(nums[i] <= nums[start]){
                    swap(nums[i],nums[++less]);
                }
            }

            swap(nums[start],nums[less]);
            return less;
        }

        int findkthUtil(vector<int>& nums, int tar_idx, int start,int end){

            int rand_idx = get_random(start,end);
            int pivot_idx = partition(nums,pivot_idx,start,end);

            if(pivot_idx == tar_idx){
                return nums[pivot_idx];
            }

            else if(pivot_idx > tar_idx){
                return findkthUtil(nums,tar_idx,start,pivot_idx - 1);
            }

            else{
                return findkthUtil(nums,tar_idx,pivot_idx+1,end);
            }
        }

        int findKthLargest(vector<int>& nums, int k) {
            int start =0;
            int end = nums.size()-1;
            srand(time(NULL));
            return findkthUtil(nums,nums.size() - k,start,end);
        }
    };
    ```
    ### **Notes**

    * Algorithm for finding kth smallest or largest element in array.
    * The Average expected time is $O(n)$ and he worst case time is $O(n^2)$.

13. ## **Merge Sort**

    ```C++
        class Solution {
        public:

            void merge(vector<int>& nums,int start, int mid,int end){
                int itr1= start;
                int itr2= mid+1;

                vector<int> merged;
                while(itr1 <= mid && itr2 <= end){

                    if(nums[itr1] <= nums[itr2]){
                        merged.push_back(nums[itr1]);
                        itr1++;
                    }
                    else{
                        merged.push_back(nums[itr2]);
                        itr2++;
                    }
                }

                while(itr1<=mid){
                    merged.push_back(nums[itr1]);
                    itr1++;
                }

                while(itr2<= end){
                    merged.push_back(nums[itr2]);
                    itr2++;
                }

                int itr =0 ;

                for(int i = start; i<= end; i++){
                    nums[i] = merged[itr];
                    itr++;
                }
            }

            void merge_sort(vector<int> &nums, int start,int end){
                if(start >= end){
                    return;
                }

                int mid = start + (end-start)/2;
                merge_sort(nums,start,mid);
                merge_sort(nums,mid+1,end);
                merge(nums,start,mid,end);
            }
            vector<int> sortArray(vector<int>& nums) {

                int start = 0;
                int end = nums.size()-1;
                merge_sort(nums,start,end);
                return nums;

            }
        };
    ```
