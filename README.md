import math

# Constants for the board
PLAYER_X = 'X'
PLAYER_O = 'O'
EMPTY = ' '

def print_board(board):
    for row in board:
        print('|'.join(row))
        print('-' * 5)

def is_winner(board, player):
    # Check rows, columns, and diagonals for a win
    for i in range(3):
        if all([board[i][j] == player for j in range(3)]):
            return True
        if all([board[j][i] == player for j in range(3)]):
            return True
    if all([board[i][i] == player for i in range(3)]):
        return True
    if all([board[i][2-i] == player for i in range(3)]):
        return True
    return False

def is_board_full(board):
    return all([board[i][j] != EMPTY for i in range(3) for j in range(3)])

def minimax(board, depth, is_maximizing, alpha=-math.inf, beta=math.inf, use_alpha_beta=True):
    if is_winner(board, PLAYER_X):
        return -1
    if is_winner(board, PLAYER_O):
        return 1
    if is_board_full(board):
        return 0

    if is_maximizing:
        best_score = -math.inf
        for i in range(3):
            for j in range(3):
                if board[i][j] == EMPTY:
                    board[i][j] = PLAYER_O
                    score = minimax(board, depth + 1, False, alpha, beta, use_alpha_beta)
                    board[i][j] = EMPTY
                    best_score = max(score, best_score)
                    if use_alpha_beta:
                        alpha = max(alpha, score)
                        if beta <= alpha:
                            break
        return best_score
    else:
        best_score = math.inf
        for i in range(3):
            for j in range(3):
                if board[i][j] == EMPTY:
                    board[i][j] = PLAYER_X
                    score = minimax(board, depth + 1, True, alpha, beta, use_alpha_beta)
                    board[i][j] = EMPTY
                    best_score = min(score, best_score)
                    if use_alpha_beta:
                        beta = min(beta, score)
                        if beta <= alpha:
                            break
        return best_score

def best_move(board):
    best_score = -math.inf
    move = None
    for i in range(3):
        for j in range(3):
            if board[i][j] == EMPTY:
                board[i][j] = PLAYER_O
                score = minimax(board, 0, False)
                board[i][j] = EMPTY
                if score > best_score:
                    best_score = score
                    move = (i, j)
    return move

def main():
    board = [
        [EMPTY, EMPTY, EMPTY],
        [EMPTY, EMPTY, EMPTY],
        [EMPTY, EMPTY, EMPTY]
    ]

    print("Welcome to Tic-Tac-Toe!")
    print_board(board)

    while True:
        # Player X's turn
        x_row = int(input("Enter the row (0-2) for X: "))
        x_col = int(input("Enter the column (0-2) for X: "))
        if board[x_row][x_col] == EMPTY:
            board[x_row][x_col] = PLAYER_X
        else:
            print("Cell already occupied! Try again.")
            continue

        print_board(board)

        if is_winner(board, PLAYER_X):
            print("Player X wins!")
            break
        if is_board_full(board):
            print("It's a tie!")
            break

        # AI's turn
        move = best_move(board)
        if move:
            board[move[0]][move[1]] = PLAYER_O
            print("AI placed O at:", move)
            print_board(board)

            if is_winner(board, PLAYER_O):
                print("Player O wins!")
                break
            if is_board_full(board):
                print("It's a tie!")
                break

if __name__ == "__main__":
    main()
