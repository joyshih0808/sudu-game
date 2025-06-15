## 基本功能
'''c
#include <stdio.h>

#define SIZE 3

char board[SIZE][SIZE];

// 初始化棋盤
void initBoard() {
    for (int i = 0; i < SIZE; i++)
        for (int j = 0; j < SIZE; j++)
            board[i][j] = ' ';
}

// 顯示棋盤
void printBoard() {
    printf("\n");
    for (int i = 0; i < SIZE; i++) {
        printf(" %c | %c | %c \n", board[i][0], board[i][1], board[i][2]);
        if (i < SIZE - 1)
            printf("---|---|---\n");
    }
    printf("\n");
}

// 判斷是否有人贏
char checkWin() {
    // 橫排或直排
    for (int i = 0; i < SIZE; i++) {
        if (board[i][0] == board[i][1] && board[i][1] == board[i][2] && board[i][0] != ' ')
            return board[i][0];
        if (board[0][i] == board[1][i] && board[1][i] == board[2][i] && board[0][i] != ' ')
            return board[0][i];
    }
    // 斜線
    if (board[0][0] == board[1][1] && board[1][1] == board[2][2] && board[0][0] != ' ')
        return board[0][0];
    if (board[0][2] == board[1][1] && board[1][1] == board[2][0] && board[0][2] != ' ')
        return board[0][2];

    return ' ';
}

// 判斷是否平手
int isDraw() {
    for (int i = 0; i < SIZE; i++)
        for (int j = 0; j < SIZE; j++)
            if (board[i][j] == ' ')
                return 0;
    return 1;
}

int main() {
    int row, col;
    char currentPlayer = 'X';
    initBoard();

    while (1) {
        printBoard();
        printf("Player %c, enter row and column (1-3): ", currentPlayer);
        scanf("%d %d", &row, &col);
        row--; col--;  // 調整為 0-based index

        if (row < 0 || row >= SIZE || col < 0 || col >= SIZE || board[row][col] != ' ') {
            printf("Invalid move, try again.\n");
            continue;
        }

        board[row][col] = currentPlayer;

        char winner = checkWin();
        if (winner != ' ') {
            printBoard();
            printf("Player %c wins!\n", winner);
            break;
        } else if (isDraw()) {
            printBoard();
            printf("It's a draw!\n");
            break;
        }

        currentPlayer = (currentPlayer == 'X') ? 'O' : 'X';
    }

    return 0;
}

'''