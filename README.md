<h1>ExpNo 7 : Implement Alpha-beta pruning of Minimax Search Algorithm for a Simple TIC-TAC-TOE game</h1> 
<h3>NAME:       </h3>
<h3>REGISTER NUMBER:           </h3>
<H3>AIM:</H3>
<p>
Implement Alpha-beta pruning of Minimax Search Algorithm for a Simple TIC-TAC-TOE game
</p>
<h3>GOALS of Alpha-Beta Pruning in MiniMax Search Algorithm</h3>

<h3>Improve the decision-making efficiency of the computer player by reducing the number of evaluated nodes in the game tree.</h3>
<h3>Tic-Tac-Toe game implementation incorporating the Alpha-Beta pruning and the Minimax algorithm with Python Code.</h3>
<h3>IMPLEMENTATION</h3>

The project involves developing a Tic-Tac-Toe game implementation incorporating the Alpha-Beta pruning with the Minimax algorithm. Using this algorithm, the computer player analyzes the game state, evaluates possible moves, and selects the optimal action based on the anticipated outcomes.

<h3>The Minimax algorithm</h3>

recursively evaluates all possible moves and their potential outcomes, creating a game tree.

<h3>Alpha-Beta pruning</h3>

Alpha–Beta (𝛼−𝛽) algorithm is actually an improved minimax using a heuristic. It stops evaluating a move when it makes sure that it’s worse than a previously examined move. Such moves need not to be evaluated further.

When added to a simple minimax algorithm, it gives the same output but cuts off certain branches that can’t possibly affect the final decision — dramatically improving the performance
<hr>
<h3>SAMPLE INPUT AND OUPUT:</h3>

![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/8d5e329a-9aff-41a6-bcf0-46efa10e1b92)
![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/438b242d-54ba-443e-b040-a936e6ae3b55)
![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/99a33390-fa11-4ade-a19f-e93bcd7aaec9)
![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/440797bd-53cb-49c1-b18d-89776864c3e7)
![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/81575a16-26b2-46f1-a8ac-27c9ed0a0fe5)

<h3>PROGRAM:</h3>

```
import time

# Initialize the game
current_state = [
    ['.', '.', '.'],
    ['.', '.', '.'],
    ['.', '.', '.']
]

def initialize_game():
    global current_state
    current_state = [
        ['.', '.', '.'],
        ['.', '.', '.'],
        ['.', '.', '.']
    ]

# Display board

def draw_board():
    for i in range(3):
        for j in range(3):
            print('{}|'.format(current_state[i][j]), end=" ")
        print()
    print()

# Check valid move

def is_valid(px, py):
    if px < 0 or px > 2 or py < 0 or py > 2:
        return False
    if current_state[px][py] != '.':
        return False
    return True

# Check game status

def is_end():
    # Vertical win
    for i in range(3):
        if (current_state[0][i] != '.' and
            current_state[0][i] == current_state[1][i] and
            current_state[1][i] == current_state[2][i]):
            return current_state[0][i]

    # Horizontal win
    for i in range(3):
        if current_state[i] == ['X', 'X', 'X']:
            return 'X'
        elif current_state[i] == ['O', 'O', 'O']:
            return 'O'

    # Main diagonal
    if (current_state[0][0] != '.' and
        current_state[0][0] == current_state[1][1] and
        current_state[1][1] == current_state[2][2]):
        return current_state[0][0]

    # Second diagonal
    if (current_state[0][2] != '.' and
        current_state[0][2] == current_state[1][1] and
        current_state[1][1] == current_state[2][0]):
        return current_state[0][2]

    # Check if board is full
    for i in range(3):
        for j in range(3):
            if current_state[i][j] == '.':
                return None

    # Tie
    return '.'

# Alpha-Beta for X (MIN)
def min_alpha_beta(alpha, beta):
    result = is_end()

    if result == 'X':
        return -1, -1, -1

    if result == 'O':
        return 1, -1, -1

    if result == '.':
        return 0, -1, -1

    best_score = 2
    best_x = -1
    best_y = -1

    for i in range(3):
        for j in range(3):
            if current_state[i][j] == '.':
                current_state[i][j] = 'X'
                score, _, _ = max_alpha_beta(alpha, beta)
                current_state[i][j] = '.'

                if score < best_score:
                    best_score = score
                    best_x = i
                    best_y = j

                beta = min(beta, best_score)

                # Alpha-Beta pruning
                if beta <= alpha:
                    break
        if beta <= alpha:
            break
    return best_score, best_x, best_y

# Alpha-Beta for O (MAX)

def max_alpha_beta(alpha, beta):
    result = is_end()

    if result == 'X':
        return -1, -1, -1

    if result == 'O':
        return 1, -1, -1

    if result == '.':
        return 0, -1, -1

    best_score = -2
    best_x = -1
    best_y = -1

    for i in range(3):
        for j in range(3):
            if current_state[i][j] == '.':
                current_state[i][j] = 'O'
                score, _, _ = min_alpha_beta(alpha, beta)
                current_state[i][j] = '.'
                if score > best_score:
                    best_score = score
                    best_x = i
                    best_y = j
                alpha = max(alpha, best_score)

                # Alpha-Beta pruning
                if beta <= alpha:
                    break
        if beta <= alpha:
            break
    return best_score, best_x, best_y

# Play the game

def play_alpha_beta():
    player_turn = 'X'
    while True:
        draw_board()
        result = is_end()

        # Game over
        if result is not None:
            if result == 'X':
                print('The winner is X!')
            elif result == 'O':
                print('The winner is O!')
            elif result == '.':
                print("It's a tie!")
            return

        # Player X
        if player_turn == 'X':
            while True:
                start = time.time()
                m, qx, qy = min_alpha_beta(-2, 2)
                end = time.time()
                print(
                    'Recommended move: X = {}, Y = {}'.format(
                        qx, qy
                    )
                )
                px = int(input("Enter X coordinate: "))
                py = int(input("Enter Y coordinate: "))
                if is_valid(px, py):
                    current_state[px][py] = 'X'
                    player_turn = 'O'
                    break

                else:
                    print('The move is not valid! Try again.')

        # AI O
        else:
            m, px, py = max_alpha_beta(-2, 2)
            current_state[px][py] = 'O'
            player_turn = 'X'


# Start game
initialize_game()
play_alpha_beta()
```

<h3>OUPUT:</h3>
<img width="309" height="707" alt="image" src="https://github.com/user-attachments/assets/5d65a52f-b61a-4120-841e-f8508abc2fdb" />
<img width="306" height="506" alt="image" src="https://github.com/user-attachments/assets/5c0299a0-1b66-42a3-8882-e90e7bf71711" />


<h3>RESULT:</h3>
Thus, the program is executed successfully.
