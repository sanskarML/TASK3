#include <iostream>
#include <vector>

// Function to initialize the game board
void InitializeBoard(std::vector<std::vector<char>>& board) {
    for (int i = 0; i < 3; i++) {
        std::vector<char> row(3, ' ');
        board.push_back(row);
    }
}

// Function to display the game board
void DisplayBoard(const std::vector<std::vector<char>>& board) {
    std::cout << "  1 2 3\n";
    for (int i = 0; i < 3; i++) {
        std::cout << i + 1 << " ";
        for (int j = 0; j < 3; j++) {
            std::cout << board[i][j];
            if (j < 2) std::cout << "|";
        }
        std::cout << "\n";
        if (i < 2) std::cout << "  -+-+-\n";
    }
}

// Function to check if the game is a draw
bool IsDraw(const std::vector<std::vector<char>>& board) {
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            if (board[i][j] == ' ')
                return false;
        }
    }
    return true;
}

// Function to check if a player has won
bool CheckWin(const std::vector<std::vector<char>>& board, char player) {
    for (int i = 0; i < 3; i++) {
        if (board[i][0] == player && board[i][1] == player && board[i][2] == player) return true;
        if (board[0][i] == player && board[1][i] == player && board[2][i] == player) return true;
    }
    if (board[0][0] == player && board[1][1] == player && board[2][2] == player) return true;
    if (board[0][2] == player && board[1][1] == player && board[2][0] == player) return true;
    return false;
}

int main() {
    std::vector<std::vector<char>> board;
    InitializeBoard(board);

    char currentPlayer = 'X';
    bool gameOver = false;

    while (!gameOver) {
        DisplayBoard(board);

        int row, col;
        std::cout << "Player " << currentPlayer << ", enter your move (row and column): ";
        std::cin >> row >> col;

        if (row < 1 || row > 3 || col < 1 || col > 3 || board[row - 1][col - 1] != ' ') {
            std::cout << "Invalid move. Try again." << std::endl;
            continue;
        }

        board[row - 1][col - 1] = currentPlayer;

        if (CheckWin(board, currentPlayer)) {
            DisplayBoard(board);
            std::cout << "Player " << currentPlayer << " wins!" << std::endl;
            gameOver = true;
        } else if (IsDraw(board)) {
            DisplayBoard(board);
            std::cout << "It's a draw!" << std::endl;
            gameOver = true;
        }

        // Switch players
        currentPlayer = (currentPlayer == 'X') ? 'O' : 'X';
    }

    char playAgain;
    std::cout << "Play again? (Y/N): ";
    std::cin >> playAgain;

    if (playAgain == 'Y' || playAgain == 'y') {
        InitializeBoard(board);
        gameOver = false;
    }

    return 0;
}
