KalahStandardStarter
The starter kit for the standard version of Kalah.

Note This repo is set up to run a continuous integration (CI) workflow on any push to the submission branch only (not to any other branch). It is highly recommended that you do your development on a different branch (e.g. main) and run the tests locally (using the Makefile or your IDE). When you want to "submit" and confirm that your tests still pass on the CI workflow, you merge your development branch with the submission branch.

README Contents
Repository Contents
Functional requirements
Test Infrastructure
Makefile
Functional Requirements
The details of the game (adapted from the Wikipedia article on Kalah) are as follows.

Kalah is a board game between two players. Each player has a set number of houses (6 for the assignment) and a store. Each house begins with a set number of seeds (4 for the assignment, but other numbers are frequently used). The houses and stores (sometimes collectively called "pits") are arranged on a rectangular board, with the stores at each end and the houses along the edge. A player controls the houses closest to her and the store on her right. The object of the game is to capture more seeds than the opponent.

A move by a player consists of the player choosing one of her houses with seeds in it, and "sowing" those seeds on the board. Sowing consists of moving around the board in an anti-clockwise direction starting with the house following the one chosen, and placing 1 seed in each house until all the seeds from the original house have been sown. A seed is also sown in the player's store, but not the opponent's store. Possible outcomes of a move are:

The last seed is sown in a house that is either not owned by the player, or the house already contains at least one seed. In which case it is the other player's turn.
The last seed is sown in the player's store. In which case the same player gets another move. There is no limit to how often this can happen.
The last seed is sown in one of the player's own houses, that house was empty before that seed was sown in, and the opposite house owned by the opponent has at least one seed in it. In which case the seed just sown and all the seeds in the opposite house are moved to the player's store. This is a "capture". If the opposite house is empty, then no capture takes place.
The game ends when the player whose turn it is has no possible moves (that is, all houses owned by the player are empty). The score for a player is determined by adding all the seeds in that player's houses and store.

Your implementation must assume keyboard input (stdin) and ASCII console output (stdout). The output is presenting using ASCII characters. The houses for each player are numbered 1—6, with the number of seeds in each house in square brackets ("[","]") beside the house number. The houses for P1 are along the bottom, running left to right, and the houses for P2 are along the top. running right to left. P1's store is on the right and P2's store is on the left. A store has the label for the player the store belongs to and the number of seeds in it.

A player indicates their move by choosing the number of the house, or 'q' to end the game.

The specifics (format, exactly what happens when the game ends, and so on) can be found in the various test specification files provided.

An example of how one move might look like is:

+----+-------+-------+-------+-------+-------+-------+----+
| P2 | 6[ 5] | 5[ 1] | 4[ 2] | 3[ 3] | 2[ 4] | 1[ 0] | 14 |
|    |-------+-------+-------+-------+-------+-------|    |
|  4 | 1[ 5] | 2[ 5] | 3[ 2] | 4[ 0] | 5[ 0] | 6[ 3] | P1 |
+----+-------+-------+-------+-------+-------+-------+----+
prompt> Player 1's turn - Specify house number or 'q' to quit: 3
+----+-------+-------+-------+-------+-------+-------+----+
| P2 | 6[ 5] | 5[ 1] | 4[ 2] | 3[ 3] | 2[ 0] | 1[ 0] | 19 |
|    |-------+-------+-------+-------+-------+-------|    |
|  4 | 1[ 5] | 2[ 5] | 3[ 0] | 4[ 1] | 5[ 0] | 6[ 3] | P1 |
+----+-------+-------+-------+-------+-------+-------+----+
Player P1 chooses house 3, which has two seeds in it. One of those seeds is placed in house 4 (meaning there is now 1 seed in it) and the last seed in house 5. Since house 5 was empty before that seed was placed in it and the opposing house (p2's house 2) is empty, this is a "capture", and that seed, plus the 4 seeds in P2's house 2 are placed in P1's store.

Here is a longer example, with some moves starting from the beginning of the game.

+----+-------+-------+-------+-------+-------+-------+----+
| P2 | 6[ 4] | 5[ 4] | 4[ 4] | 3[ 4] | 2[ 4] | 1[ 4] |  0 |
|    |-------+-------+-------+-------+-------+-------|    |
|  0 | 1[ 4] | 2[ 4] | 3[ 4] | 4[ 4] | 5[ 4] | 6[ 4] | P1 |
+----+-------+-------+-------+-------+-------+-------+----+
Player P1's turn - Specify house number or 'q' to quit: 2
+----+-------+-------+-------+-------+-------+-------+----+
| P2 | 6[ 4] | 5[ 4] | 4[ 4] | 3[ 4] | 2[ 4] | 1[ 4] |  0 |
|    |-------+-------+-------+-------+-------+-------|    |
|  0 | 1[ 4] | 2[ 0] | 3[ 5] | 4[ 5] | 5[ 5] | 6[ 5] | P1 |
+----+-------+-------+-------+-------+-------+-------+----+
Player P2's turn - Specify house number or 'q' to quit: 3
+----+-------+-------+-------+-------+-------+-------+----+
| P2 | 6[ 5] | 5[ 5] | 4[ 5] | 3[ 0] | 2[ 4] | 1[ 4] |  0 |
|    |-------+-------+-------+-------+-------+-------|    |
|  1 | 1[ 4] | 2[ 0] | 3[ 5] | 4[ 5] | 5[ 5] | 6[ 5] | P1 |
+----+-------+-------+-------+-------+-------+-------+----+
Player P2's turn - Specify house number or 'q' to quit: 5
+----+-------+-------+-------+-------+-------+-------+----+
| P2 | 6[ 6] | 5[ 0] | 4[ 5] | 3[ 0] | 2[ 4] | 1[ 4] |  0 |
|    |-------+-------+-------+-------+-------+-------|    |
|  2 | 1[ 5] | 2[ 1] | 3[ 6] | 4[ 5] | 5[ 5] | 6[ 5] | P1 |
+----+-------+-------+-------+-------+-------+-------+----+
Player P1's turn - Specify house number or 'q' to quit: q
Game over
+----+-------+-------+-------+-------+-------+-------+----+
| P2 | 6[ 6] | 5[ 0] | 4[ 5] | 3[ 0] | 2[ 4] | 1[ 4] |  0 |
|    |-------+-------+-------+-------+-------+-------|    |
|  2 | 1[ 5] | 2[ 1] | 3[ 6] | 4[ 5] | 5[ 5] | 6[ 5] | P1 |
+----+-------+-------+-------+-------+-------+-------+----+
The example above consists of the following:

The example begins at the start of a game. All houses have 4 seeds in them, and the stores are empty.
The player with the first move is P1, who chooses house 2. This house has 4 seeds in it. These are sown one at a time starting from house 3. The last seeds is sown in house 6, which is non-empty, so P1's turn ends.
P2 chooses house 3. In this case P2's move ends in the store, meaning P2 gets another move.
P2 moves again, this time choosing house 5. The 5 seeds this time wrap on to P1's side (including 1 in P2's store), adding seeds to P1's houses 1, 2 and 3.
Repository Contents
Makefile - build script for make. Supports building implementation and running tests (see below)
README.md - this file (markdown)
resources - directory containing:
IO.html - documentation for IO interface in test infrastructure
kalah20200717.jar - jar file containing infrastructure, including test class and test specifications. Has to be on the classpath for compiling and testing
test_specifications - directory containing the specifications for the tests. This is what is in the jar file, but has been unpacked for easy access. See below for description of contents
junit-3.8.2.jar - jar file for 3.8 JUnit. Has to be on classpath for testing.
src/kalah - directory containing:
Kalah.java - Stub class for Kalah set up to use test infrastructure. The CI will perform all of the testing by calling the void play(IO) method so this is what you need to implement.
Test Infrastructure
While there probably is no need to create more tests for the assignment, it is useful to understand how it works in order to write code that has to pass the tests using the supplied test infrastructure, and in particular to interpret the information from failed tests.

The test infrastructure is designed to test I/O for applications that only use stdin and stdout (typically keyboard and console). To use it, one specifies the complete sequence (until program termination) of input and output actions in a file, and the test infrastructure will run the program and confirm that, for the given inputs, the specified outputs occur.

The file that specifies the expected I/O behaviour consists of a sequence of lines of text, where the first character specifies what the rest of the line means. There are three possibilities:

< — the rest of the line is to be supplied as input
> — the rest of the line is output
# — the rest of the line is a comment and is ignored for the testing process.
Below is the contents of the test specification file simple_start.txt. The first line is a comment. The next 6 lines are output. The 8th line (<1) indicates that "1" will be supplied as input.

# Do one move, which doesn't affect the player's store, then quit.
>+----+-------+-------+-------+-------+-------+-------+----+
>| P2 | 6[ 4] | 5[ 4] | 4[ 4] | 3[ 4] | 2[ 4] | 1[ 4] |  0 |
>|    |-------+-------+-------+-------+-------+-------|    |
>|  0 | 1[ 4] | 2[ 4] | 3[ 4] | 4[ 4] | 5[ 4] | 6[ 4] | P1 |
>+----+-------+-------+-------+-------+-------+-------+----+
>Player 1's turn - Specify house number or 'q' to quit:
<1
>+----+-------+-------+-------+-------+-------+-------+----+
>| P2 | 6[ 4] | 5[ 4] | 4[ 4] | 3[ 4] | 2[ 4] | 1[ 4] |  0 |
>|    |-------+-------+-------+-------+-------+-------|    |
>|  0 | 1[ 0] | 2[ 5] | 3[ 5] | 4[ 5] | 5[ 5] | 6[ 4] | P1 |
>+----+-------+-------+-------+-------+-------+-------+----+
>Player 2's turn - Specify house number or 'q' to quit:
<q
>Game over
>+----+-------+-------+-------+-------+-------+-------+----+
>| P2 | 6[ 4] | 5[ 4] | 4[ 4] | 3[ 4] | 2[ 4] | 1[ 4] |  0 |
>|    |-------+-------+-------+-------+-------+-------|    |
>|  0 | 1[ 0] | 2[ 5] | 3[ 5] | 4[ 5] | 5[ 5] | 6[ 4] | P1 |
>+----+-------+-------+-------+-------+-------+-------+----+
In order for the test infrastructure to work, the program you want to test using it must use a different stdin and stdout than that usually used in Java programs (i.e. System.in and System.out). These are supplied by the MockIO class. Input is supplied by the readFromKeyboard() and readInteger() methods and output is provided by print() and println(). MockIO provides a more restricted (but in some cases more useful) set of I/O operations than those available on System.in and System.out, however they are sufficient for our purposes.

One point to note. In order to allow the test infrastructure to properly monitor what your implementation is doing, the print() and println() methods will not allow inputs with newlines (\n) embedded in them. You will get an error when you try to do this.

The checks that MockIO does to support the testing process are somewhat primitive. Roughly speaking they are:

Does a complete line of actual output match what has been specified? If it does not, then it may be manifested in several ways:
The actual output finishes the line when more output is expected on the line
The expected output ends a line but the actual output produces more for that line
A character in the actual output does not match the corresponding character in the expected output.
Is input provided when it is expected?
Is input expected when it is provided?
Once the program has terminated execution, does the test specification file have more content (indicating the termination was too early)?
When the test specification file has been exhausted, is the program still executing (indicating that it has not terminated when expected)?
When a test fails, a TextIOAssertionException is thrown with a report of what the problem is. The reporting of what went wrong is somewhat crude, so some interpretation is needed. The report will give an indication of where things went wrong (at the line number of the expected I/O and some indication of which case it is), but it may not always be straight-forward to determine what happened. The best place to look is the test specification files and compare what your program is going for the various inputs found in those files.

Makefile
The make utility provides a simple way to run non-trivial commands (it's actually a lot more than that but that's all we're using it for). The make utility reads the configuration file (called a "makefile") to determine what to do. The default name for a makefile is Makefile.

The supplied Makefile will be used to test your submission when you push it back to your Github repository (but only to the submission branch). Do not change this file unless you know what you are doing.

The supplied Makefile has 3 "targets", names that identify different operations that the makefile supports. Those targets are:

tests (default)
Compile the code (assumes everything is called from `Kalah.java`) and then run the tests.
play
Compile the code (assumes everything is called from `Kalah.java`) and then play the game.
compile
Compile the code (assumes everything is called from `Kalah.java`).
Targets are used by issuing the make command, followed by the target name. If no target name is given then the default is used. So

> make
will run the tests target.

> make tests
Will do the same thing. Whereas

> make compile
will just do the compilation.

Even if you don't want to use make, it may be worth looking at this file to see what the classpath should be when compiling and testing. Note that the syntax used here is typical for most forms of Unix. For Windows, at minimum "/" has to be replaced by "" and ":" (colon) has to be replaced by ";" (semicolon). Other changes may be necessary.

Note: The CI uses this makefile so don't change it. If you want to create a Windows version, give it a different name and use make -f filename to use it.
