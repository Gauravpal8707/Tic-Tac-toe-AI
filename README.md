import numpy as np

class TicTacToe:
    def __init__(self):
        self.board = np.full((3, 3), ' ')
        self.current_player = 'X'  # X is human, O is AI

    def print_board(self):
        print("\n".join([" | ".join(row) for row in self.board]))
        print()

    def check_winner(self):
        for row in self.board:
            if row[0] == row[1] == row[2] != ' ':
                return row[0]
        for col in range(3):
            if self.board[0][col] == self.board[1][col] == self.board[2][col] != ' ':
                return self.board[0][col]
        if self.board[0][0] == self.board[1][1] == self.board[2][2] != ' ':
            return self.board[0][0]
        if self.board[0][2] == self.board[1][1] == self.board[2][0] != ' ':
            return self.board[0][2]
        return None

    def is_full(self):
        return ' ' not in self.board

    def minimax(self, depth, is_maximizing):
        scores = {'X': -1, 'O': 1, 'draw': 0}

        winner = self.check_winner()
        if winner is not None:
            return scores[winner]
        if self.is_full():
            return scores['draw']

        if is_maximizing:
            best_score = -np.inf
            for i in range(3):
                for j in range(3):
                    if self.board[i][j] == ' ':
                        self.board[i][j] = 'O'  # AI's turn
                        score = self.minimax(depth + 1, False)
                        self.board[i][j] = ' '  # Undo move
                        best_score = max(score, best_score)
            return best_score
        else:
            best_score = np.inf
            for i in range(3):
                for j in range(3):
                    if self.board[i][j] == ' ':
                        self.board[i][j] = 'X'  # Human's turn
                        score = self.minimax(depth + 1, True)
                        self.board[i][j] = ' '  # Undo move
                        best_score = min(score, best_score)
            return best_score

    def best_move(self):
        best_score = -np.inf
        move = (-1, -1)
        for i in range(3):
            for j in range(3):
                if self.board[i][j] == ' ':
                    self.board[i][j] = 'O'  # AI's turn
                    score = self.minimax(0, False)
                    self.board[i][j] = ' '  # Undo move
                    if score > best_score:
                        best_score = score
                        move = (i, j)
        return move

    def play_game(self):
        while True:
            self.print_board()
            if self.current_player == 'X':
                row, col = map(int, input("Enter row and column (0-2) for your move: ").split())
                if self.board[row][col] == ' ':
                    self.board[row][col] = 'X'
                    if self.check_winner():
                        self.print_board()
                        print("X wins!")
                        break
                    self.current_player = 'O'
            else:
                print("AI is making a move...")
                row, col = self.best_move()
                self.board[row][col] = 'O'
                if self.check_winner():
                    self.print_board()
                    print("O wins!")
                    break
                self.current_player = 'X'

            if self.is_full():
                self.print_board()
                print("It's a draw!")
                break

if __name__ == "__main__":
    game = TicTacToe()
    game.play_game()
