# Parallel pac-man solver
A course project for 15618 at Carnegie Mellon University
# Summary:
We are going to implement a game solver for Pac-Man with both sequential and parallel version. We would like to use OpenMP and OpenMPI to implement parallelism on the multi-core system. Finally, we will compare the performance of sequential and parallel versions.
# Background:
Pac-Man is a maze arcade game. [1] Basically, there are three roles in the game: Pac-Man, dots and ghosts. Pac-Man is controlled by the player and needs to eat all the dots without colliding with ghosts. The dots are statically distributed in the maze while ghosts will move randomly in the maze.
The algorithm we plan to use for implementing the solver is iterative deepening depth first search(IDDFS). It is a depth-first search with a depth limit. It can be run with increasing depth limit in order to achieve the search goal. Each round of search will search all nodes with the same level. Therefore, it looks like the breadth-first search but it consumes much less memory. 
# The challenge: 
The main idea to parallelize this solver is to divide the search space into different chunks. However, the search space is gradually generated through each iteration. Besides, according to the original algorithm, the iterations are separate and sequential. And even among each iteration, the search space is expanded gradually (because they are DFS tree searches). That is, it can not be divided at the very beginning and we have to decide it during the execution. This is the main reason why we think the parallelism for this solver is non-trivial. The ideal way to do that is to exploit the parallelism during both each iteration and between iterations. 
In addition, the workload is hard to predict and therefore static assignment won’t work well and we might have to design our own dynamic assignment method. This might involve designing a way to schedule tasks/designing a work queue. 

On the other hand, there are other constraints during the exploring of the search space. There are enemies (ghosts/monsters) in the grid and the behaviours of these enemies are randomized and should be determined by a single master worker/task. To make all the workers/tasks to observe the same behavior of all enemies, master worker/task should generate the behaviors and then deliver them to all other workers/tasks. This adds difficulty to parallelism. 

Another concern we cared about is power efficiency. The main design choice in this step is to choose an appropriate frame batch size. That is, instead of processing one frame at a time, we are going to process a batch of frames. This matters for both time efficiency and the master worker’s/task’s simulation. For example, generate a sequence of steps of enemies’ behaviors instead of generating one step at a time.
# Resources:
We would like to implement both sequential and parallel version of the game solver from scratch. We may search for the existing solvers to get an idea on how to implement it. We will also refer to the [following link](https://www.geeksforgeeks.org/iterative-deepening-searchids-iterative-deepening-depth-first-searchiddfs/) to implement the iterative deepening depth first search. 
We plan to use the multi-core system to implement our parallel game solver. Hence, the GHC machines and the latedays machines are sufficient to achieve our goals. If possible, we would like to analyze our program on Intel Xeon Phi.
Goals and deliverables:
Plan to achieve: implementing a parallel version of pac-man solver using OpenMP separately which has an approximate speedup of about 4x for workload-balance test case and 2x for workload-imbalance test case. The reason for these numbers is that each grid in the map has 4 neighbors and we will probably exploit parallelism among them. This part also includes writing a second parallel version of pac-man solver using OpenMPI. And in this case, we want to achieve a 4x speedup because of the same reason stated above. If we have even more extra time, we are going to strive for higher speedup. 

Hope to achieve: optimize it for power efficiency/time efficiency and other general constraints as if we wanted the parallel version to be executed on a mobile platform. 

About how to demo our project: we will probably want to implement a pac-man simulator which visualizes the game process and therefore illustrate the path the algorithm found. However, demoing the game is just a way to make the demo appealing. For the purpose of this course, the key point is to show how a sequential version of the algorithm and the parallel version we implemented differ in run time. Therefore, we will measure the runtime of both versions for different maps (can be randomly generated or intentionally designed for different workload-just like the corner cases and random cases in our QuadTree assignment). 
# Platform Choice 
We would like to use C++ as our target programming language because it has strong abstraction ability and high performance. Also, parallelization tools we would like to use in this project such as OpenMP and OpenMPI can only support C/C++.
In this algorithm, we have no idea on the scale of the parallelism before running the program. That means we need to exploit the parallelism during the execution of the program. Hence, the multi-core system is appropriate for this project while the data parallel system, for example, GPU is not a good candidate.
# Schedule
Oct. 30 - Nov. 4       Search for the existing implementations of the solver

Nov. 5 - Nov. 11       Implement the sequential solver from scratch

Nov. 12 - Nov. 18     Implement the parallel solver with OpenMP and achieve the highest speedup

Nov. 19 - Nov. 25     Implement the parallel solver with OpenMPI and achieve the highest speedup

Nov. 26 - Dec. 2      Compare the OpenMP and MPI implementation against sequential implementation
Combine both MPI and OpenMP implementation to achieve best performance

Dec. 3 - Dec. 9        Complete Final Report + Poster + Finalization of Code

# Reference:
https://en.wikipedia.org/wiki/Pac-Man
