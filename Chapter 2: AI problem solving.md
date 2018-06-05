# Contents:
1. Search and problem solving
2. Solving problems with AI
3. Search and games

# Search and problem solving
Many problems can be phrased as search problems. This requires that we start by formulating the alternative choices and their consequences.

## Search in practice: getting from A to B
Imagine you're in a foreign city, at some address and want to use public transport to get to another address. What do you do? You pull out your smartphone, type in the destination and start following the instructions.

This question belongs to the class of search and planning problems. Similar problems need to be solved by self-driving cars, and AI for playing games. In the game of chess, for example, the difficulty is not so much in getting a piece from A to B as keeping your pieces safe from the opponent.

Often there are many different ways to solve the problem, some of which may be more preferable in terms of time, effort, cost and other criteria. Different search techniques may lead to different solutions, and developing advanced search algorithms is an established research area.

We will discuss two kinds of problems:
* Search and planning in static environment with only one "agent"
* Games with two-players ("agents") competing against each other

## Toy problem: chicken crossing
We'll start from a simple puzzle to illustrate the ideas. A robot on a rowboat needs to move three pieces of cargo across a river: a fox, a chicken, and a sack of chicken-feed. The fox will eat the chicken if it has the chance, and the chicken will eat the chicken-feed if it has chance, and neither is a desirable outcome. The robot is capable of keeping the animals from doing harm when it is near them, but only the robot can operate the rowboat and only two of the pieces of cargo can fit on the rowboat together with the robot. How can the robot move all of its cargo to the opposite bank of the river?

We will model the puzzle by noting that five movable things have been identified: the robot, the rowboat, the fox, the chicken, and the chicken-feed. In principle, each of the five can be on either side of the river, but since only the robot can operate the rowboat, the two will always be on the same side. Thus there are four things with two possible positions for each, which makes for sixteen combinations, which we will call states:

### States of the chicken crossing puzzle
State	Robot	Fox	Chicken	Chicken-feed
NNNN	Near side	Near side	Near side	Near side
NNNF	Near side	Near side	Near side	Far side
NNFN	Near side	Near side	Far side	Near side
NNFF	Near side	Near side	Far side	Far side
NFNN	Near side	Far side	Near side	Near side
NFNF	Near side	Far side	Near side	Far side
NFFN	Near side	Far side	Far side	Near side
NFFF	Near side	Far side	Far side	Far side
FNNN	Far side	Near side	Near side	Near side
FNNF	Far side	Near side	Near side	Far side
FNFN	Far side	Near side	Far side	Near side
FNFF	Far side	Near side	Far side	Far side
FFNN	Far side	Far side	Near side	Near side
FFNF	Far side	Far side	Near side	Far side
FFFN	Far side	Far side	Far side	Near side
FFFF	Far side	Far side	Far side	Far side

We have given short names to the states, because otherwise it would be cumbersome to talk about them. Now we can say that the starting state is NNNN and the goal state is FFFF, instead of something like "in the starting state, the robot is on the near side, the fox is on the near side, the chicken is on the near side, and also the chicken-feed is on the near side, and in the goal state the robot is on the far side", and so on.

Some of these states are forbidden by the puzzle conditions For example, in state NFFN, the fox will eat the chicken, which we cannot have. Thus we can rule out states NFFN, NFFF, FNNF, FNNN, NNFF, and FFNN. We are left with the following ten states:

State	Robot	Fox	Chicken	Chicken-feed
NNNN	Near side	Near side	Near side	Near side
NNNF	Near side	Near side	Near side	Far side
NNFN	Near side	Near side	Far side	Near side
NFNN	Near side	Far side	Near side	Near side
NFNF	Near side	Far side	Near side	Far side
FNFN	Far side	Near side	Far side	Near side
FNFF	Far side	Near side	Far side	Far side
FFNF	Far side	Far side	Near side	Far side
FFFN	Far side	Far side	Far side	Near side
FFFF	Far side	Far side	Far side	Far side

Next we will figure out which state transitions are possible, meaning simply that as the robot rows the boat with some of the items as cargo, what resulting state is in each case. It's best to draw a diagram of the transitions.

Now let's draw the transitions. We could draw arrows that have a direction so that they point from one node to another, but in this puzzle the transitions are symmetric: if the robot can row from state NNNN to state FNFF, it can equally well row the other way from FNFF to NNNN. Thus it is simpler to draw the transitions simply with lines that don't have a direction. Starting from NNNN, we can go to FNFN, FNFF, FNFN, FFNF, and FFFN:

NNNN -> FNFN
        FNFF
        FFNF
        FFFN
        
Then we fill in the rest:
NNNN -> FNFN
        FNFF
        FFNF
        FFFN
        
NNNF -> FNFF
        FFNF
        FFFF
        
NNFN -> FNFN
        FNFF
        FFFN
        FFFF

NFNN -> FFNF
        FFFN
        FFFF
        
NFNF -> FFNF
        FFFF

We have now done quite a bit of work on the puzzle without seeming any closer to the solution, and there is doubt that you could have solved the whole puzzle already by using your "natural intelligence". But for more complex problems, where the number of possible solutions grows in the thousands and in the millions, our systematic or mechanical approach will shine since the hard part will be suitable for a simple computer to do. Now that we have formulated the alternative states and transitions between them, the rest becomes a mechanical task: find a path from the initial state NNNN to the final state FFFF.

One such path:

NNNN -> FFFN -> NFNN -> FFFF

State space, transitions, and costs
To formalize a planning problem, we use concepts as the state space, transitions and costs.

## Key terminology
The state space: means the set of possible situations. In the chicken-crossing puzzle, the state space consisted of ten allowed states NNNN through FFFF. If the task is to navigate from place A to place B, the state space could be the set of locations defined by their (x, y) coordinates that can be reached from the starting point A. Or we could use a constrained set of locations, for example, different street addresses so that the number of possible states is limited.

Transitions: are possible moves between one state and another, such as NNNN to FNFN. It is important to note that we only count direct transitions between neighboring states as transitions. A sequence of multiple transitions, for example, from A to C, from C to D, and from D to B (the goal), is a path rather than a single transition.

Costs: refer to the fact that, oftentimes the different transitions aren´t all alike. They can differ in ways that make some transitions more preferable or cheaper (in a not necessarily monetary sense of the word), and others most costly. We can express this by associating with each transition a certain cost. If the goal is to minimize the total distance traveled, then a natural cost is the geographical distance between states. On the other hand, the goal could actually be to minimize the time instead of the distance, in which case the natural cost would obviously be the time. If all the transitions are equal, then we can ignore the costs.

## Exercise 5: A smaller rowboat
In the traditional version of this puzzle the robot can only fit one thing on the boat with it. The state space is still the same, but fewer transitions are possible. Use the state transition diagram below to find a path from the initial state to the final one (we suggest drawing this out to really try).
Please type your answer as the **number of transitions in the shortest path.**

## Exercise 6: The towers of Hanoi
Let's do another puzzle: the well-known Towers of Hanoi. In our version, the puzzle involves three pegs, and two discs: one large, and one small. In the initial state, both discs are stacked in the first (leftmost) peg. The goal is to move the discs to the third peg. You can move one disc at a time, from any peg to another, as long as there is no other disc on top of it. It is not allowed to put a larger disc on top of a smaller disc.
**Your task**: Draw the state diagram. The diagram should include all the nine possible states in the game, connected by lines that show the possible transitions. The picture below shows the overall structure of the state diagram and the positions of the first three states. It shows that from the starting state (at the top corner), you can move to two other states by moving the small disc. Complete the state diagram by placing the remaining states in the correct places. Note that the transitions are again symmetric and you can also move sideways (left or right) or up in the diagram.

What state should be in box 1?
State E

What state should be in box 2?
State B

What state should be in box 3?
State F

What state should be in box 4?
State D

What state should be in box 5?
State C

What state should be in box 6?
State A

# Solving problems with AI
## Note
John McCarthy’s key statement about AI:

“The study is to proceed on the basis of the conjecture that every aspect of learning or any other feature of intelligence can in principle be so precisely described that a machine can be made to simulate it.”

# Search and games
Example: playing tic tac toe
Maxine and Minnie are true game enthusiasts. They just love games. Especially two-person, perfect information games such as tic-tac-toe or chess. One day they were playing tic-tac-toe. Maxine, or Max as her friends call her, was playing with X. Minnie, or Min as her friends call her, had the Os.

## Game trees
To solve games using AI, we will introduce the concept of a game tree. The different states of the game are represented by nodes in the game tree, very similar to the above planning problems. The idea is just slightly different. In the game tree, the nodes are arranged in levels that correspond to each player's turns in the game so that the “root” node of the tree (usually depicted at the top of the diagram) is the beginning position in the game. In tic-tac-toe, this would be the empty grid with no Xs or Os played yet. Under root, on the second level, there are the possible states that can result from the first player’s moves, be it X or O. We call these nodes the “children” of the root node.

Each node on the second level, would further have as its children nodes the states that can be reached from it by the opposing player's moves. This is continued, level by level, until reaching states where the game is over. In tic-tac-toe, this means that either one of the players get a line of three and wins, or the board is full and the game ends in a tie.

## Minimizing and maximizing value
In order to be able to create game AI that attempts to win the game, we attach a numerical value to each possible end result. To the board positions where X has a line of three so that Max wins, we attach the value +1, and likewise, to the positions where Min wins with three Os in a row we attach the value -1. For the positions where the board is full and neither player wins, we use the neutral value 0. (It doesn’t really matter what the values are as long as they in this order so that Max tries to maximize the value, and Min tries to minimize it.)

## A sample game tree
Consider, for example, the following game tree which begins not at the root but in the middle of the game (because otherwise, the tree would be way too big to display). Note that this is different from the game shown in the illustration in the beginning of this section. We have numbered the nodes with numbers 1, 2, ..., 14.

The tree is composed of alternating layers where it is either Min's turn to place an O or Max's turn to place an X at any of the vacant slots on the board. The player whose turn it is to play next is shown at the left.

## The Minimax algorithm
We can exploit the above concept of the value of the game to obtain an algorithm called the Minimax algorithm. It guarantees optimal game play in, theoretically speaking, any deterministic, two-person, perfect-information zero-sum game. Given a state of the game, the algorithm simply computes the values of the children of the given state and chooses the one that has the maximum value if it is Max’s turn, and the one that has the minimum value if it is Min’s turn.

The algorithm can be implemented using a few lines of code. However, we will be satisfied with having grasped the main idea.

## The problem of massive game trees
In many games, the game tree is simply way too big to traverse in full. For example, in chess the average branching factor, i.e., the average number of children (available moves) per node is about 35. That means that to explore all the possible scenarios up to only two moves ahead, we need to visit approximately 35 x 35 = 1225 nodes -- probably not your favorite pencil-and-paper homework exercise... A look-ahead of three moves requires visiting 42875 nodes; four moves 1500625; and ten moves 2758547353515625 (that’s about 2.7 quadrillion) nodes. In Go, the average branching factor is estimated to be about 250. Go means no-go for Minimax.

## More tricks: Managing massive game trees
A few more tricks are needed to manage massive game trees. Many of them were crucial elements in IBM’s Deep Blue computer defeating the chess world champion, Garry Kasparov, in 1997.

If we can afford to explore only a small part of the game tree, we need a way to stop the minimax recursion before reaching an end-node, i.e., a node where the game is over and the winner is known. This is achieved by using a so called heuristic evaluation function that takes as input a board position, including the information about which player’s turn is next, and returns a score that should be an estimate of the likely outcome of the game continuing from the given board position.

## Good heuristics
Good heuristics for chess, for example, typically count the amount of material (pieces) weighted by their type: the queen is usually considered worth about two times as much as a rook, three times a knight or a bishop, and nine times as much as a pawn. The king is of course worth more than all other things combined since losing it amounts to losing the game. Further, occupying the strategically important positions near the middle of the board is considered an advantage and the heuristic assign higher value to such positions.

## Exercise 7: Why so pessimistic, Max?

**Your task**:
Look at the game tree starting from the below board position. Using a pencil and paper, fill in the values of the bottom-level nodes where the game is over. Note that this time some of the games end in a draw, which means that the values of the nodes is 0 (instead of –1 or 1).

Answer: -1

## The limitations of plain search
It may look like we have a method to solve any problem by specifying the states and transitions between them, and finding a path from the current state to our goal. Alas, things get more complicated when we want to apply AI in real world problems. Basically, the number of states in even a moderately complex real-world scenario grows out of hand, and we can’t find a solution by exhaustive search (“brute force”) or even by using clever heuristics.

Moreover, the transitions which take us from one state to the next when we choose an action are not deterministic. This means that whatever we choose to do will not always completely determine the outcome because there are factors that are beyond our control, and that are often unknown to us.

The algorithms we have discussed above can be adapted to handle some randomness, for example randomness in choosing cards from a shuffled deck or throwing dice. This means that we will need to introduce the concept of uncertainty and probability. Only thus we can begin to approach real-world AI instead of simple puzzles and games.

# After completing Chapter 2 you should be able to:
* Formulate a real-world problem as a search problem
* Formulate a simple game (tic tac toe) as a game tree
* Use the minimax principle to find optimal moves in a limited-size game tree

