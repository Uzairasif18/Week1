# Week1
#include <iostream>
using namespace std;

class TicTacToe {
private:
    int board[3][3];
    int currentPlayer;

    bool validMove(int row, int col) {
        return row >= 0 && row < 3 && col >= 0 && col < 3 && board[row][col] == 0;
    }

    bool checkWin(int player) {
        // Check rows and columns
        for (int i = 0; i < 3; ++i) {
            if ((board[i][0] == player && board[i][1] == player && board[i][2] == player) ||
                (board[0][i] == player && board[1][i] == player && board[2][i] == player)) {
                return true;
            }
        }

        // Check diagonals
        if ((board[0][0] == player && board[1][1] == player && board[2][2] == player) ||
            (board[0][2] == player && board[1][1] == player && board[2][0] == player)) {
            return true;
        }

        return false;
    }

    bool isBoardFull() {
        for (int i = 0; i < 3; ++i) {
            for (int j = 0; j < 3; ++j) {
                if (board[i][j] == 0) {
                    return false;
                }
            }
        }
        return true;
    }

public:
    TicTacToe() : currentPlayer(1) {
        for (int i = 0; i < 3; ++i) {
            for (int j = 0; j < 3; ++j) {
                board[i][j] = 0;
            }
        }
    }

    void printBoard() {
        for (int i = 0; i < 3; ++i) {
            for (int j = 0; j < 3; ++j) {
                char symbol = board[i][j] == 1 ? 'X' : (board[i][j] == 2 ? 'O' : ' ');
                cout << symbol;
                if (j < 2) cout << " | ";
            }
            cout << endl;
            if (i < 2) cout << "--+---+--" << endl;
        }
    }

    bool makeMove(int row, int col) {
        if (!validMove(row, col)) {
            cout << "Invalid move. Try again." << endl;
            return false;
        }

        board[row][col] = currentPlayer;
        currentPlayer = (currentPlayer == 1) ? 2 : 1;
        return true;
    }

    int gameStatus() {
        if (checkWin(1)) return 1; // Player 1 wins
        if (checkWin(2)) return 2; // Player 2 wins
        if (isBoardFull()) return 0; // Draw
        return -1; // Game continues
    }

    int getCurrentPlayer() {
        return currentPlayer;
    }
};

int main() {
    TicTacToe game;
    int row, col;
    int status;

    cout << "Welcome to Tic-Tac-Toe!" << endl;
    game.printBoard();

    while ((status = game.gameStatus()) == -1) {
        cout << "Player " << game.getCurrentPlayer() << "'s turn. Enter row and column (0, 1, or 2): ";
        cin >> row >> col;
        while (!game.makeMove(row, col)) {
            cin >> row >> col;
        }
        game.printBoard();
    }

    if (status == 1 || status == 2) {
        cout << "Player " << status << " wins!" << endl;
    } else {
        cout << "It's a draw!" << endl;
    }

    return 0;
}

