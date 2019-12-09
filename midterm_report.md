1. We have updated our project main page up to date with the checkpoint. Here is the link: https://yiran-shen.github.io/cmu15618-project-pacman-solver/

2. We completed the implementation of the sequential solver using iterative deepening search and A* algorithm based on a simulator infrastructure found online. We also tried to parallelize iterations of the search with different depth limit in iterative deepening search and A* search because they are independent. After that we can hardly find any speedup. Then, we analyzed the algorithm and found it the inherent property of two algorithms, which limits the overall speedup. Then we did some research on the improvement of those two algorithms. We found an algorithm called parallel window search[1], which resolves the issue. We now plan to implement this algorithm and attempt to receive the target speedup. Those changes are reflected in the main page of our project.

3. After some attempts, we found that it is impossible to achieve the target speedup by directly applying existing parallelization tools such as OpenMP and MPI because the algorithm has inherently limited speedup. Hence, we decided to change the implementation of our solver. However, I believe we can still provide a demo of the simulation of pan-man game by the end of the project.

4. In the poster session, we first would like to show the graph of speedup of our new algorithm with different cores. We will also provide a demo on the simulator with our algorithm of pac-man game.

5. Some preliminary results for our baseline sequential version



Above are our timing for two test cases: one is a tiny maze and the other is a small maze. One thing need to notice is that ids runs slowly on large test cases and we probably want to use only these two test cases as our benchmark. 
For example, this is how a small maze looks like

Another thing need to mention is that there is no enemy (ghost) in the above test cases. However, if you look through the code structure, you will find that the location of ghost (if it exists) is also included in problem.getSuccessor method and since the “problem” is a shared variable for all processes, our method will work whether or not there is a ghost in the map. If we have enough time, we will construct our own maze wil random ghosts in it to verify this. 

Currently, the second test case (small maze) took 72.8s to complete. We still believe if we can implement the parallel window search mentioned above, we should get a satisfactory speedup. 

6. We found the vanilla iterative deepening search and A* search have inherently limited parallelism. That cannot give us the target speedup. Now, we found the new algorithm which we think will work. However, we have not evaluated the difficulty of implementation.
