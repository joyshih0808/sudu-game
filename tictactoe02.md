## 進階功能
'''c
#include <stdio.h>
#include <stdlib.h>
#include "rlutil.h"

char board[3][3];
int currentPlayer = 1; // 1 for X, 2 for O

void initBoard() {
    for (int i = 0; i < 3; ++i)
        for (int j = 0; j < 3; ++j)
            board[i][j] = ' ';
}

void drawBoard(int cursorX, int cursorY) {
    cls(); // 清除畫面
    gotoxy(0, 0);
    printf("Use arrow keys to move, press [Enter] to place your mark\n");

    for (int i = 0; i < 3; ++i) {
        for (int j = 0; j < 3; ++j) {
            int x = 10 + j * 4;
            int y = 3 + i * 2;
            gotoxy(x, y);
            
            if (i == cursorY && j == cursorX)
                setBackgroundColor(YELLOW);
            else
                setBackgroundColor(BLACK);

            if (board[i][j] == 'X') setColor(RED);
            else if (board[i][j] == 'O') setColor(BLUE);
            else setColor(WHITE);

            printf(" %c ", board[i][j]);
        }
        printf("\n");
    }

    resetColor();
    gotoxy(0, 10);
    if (currentPlayer == 1) {
        setColor(RED);
        printf("Player 1's Turn (X)\n");
    } else {
        setColor(BLUE);
        printf("Player 2's Turn (O)\n");
    }
    resetColor();
}

int checkWin() {
    // Row, column, diagonal
    for (int i = 0; i < 3; ++i) {
        if (board[i][0] != ' ' &&
            board[i][0] == board[i][1] &&
            board[i][1] == board[i][2])
            return board[i][0] == 'X' ? 1 : 2;
        if (board[0][i] != ' ' &&
            board[0][i] == board[1][i] &&
            board[1][i] == board[2][i])
            return board[0][i] == 'X' ? 1 : 2;
    }
    if (board[0][0] != ' ' &&
        board[0][0] == board[1][1] &&
        board[1][1] == board[2][2])
        return board[0][0] == 'X' ? 1 : 2;

    if (board[0][2] != ' ' &&
        board[0][2] == board[1][1] &&
        board[1][1] == board[2][0])
        return board[0][2] == 'X' ? 1 : 2;

    return 0;
}

int isDraw() {
    for (int i = 0; i < 3; ++i)
        for (int j = 0; j < 3; ++j)
            if (board[i][j] == ' ')
                return 0;
    return 1;
}

int main() {
    int cursorX = 0, cursorY = 0;
    int winner = 0;

    initBoard();
    hidecursor();

    while (1) {
        drawBoard(cursorX, cursorY);

        int key = getkey();

        if (key == KEY_UP && cursorY > 0) cursorY--;
        if (key == KEY_DOWN && cursorY < 2) cursorY++;
        if (key == KEY_LEFT && cursorX > 0) cursorX--;
        if (key == KEY_RIGHT && cursorX < 2) cursorX++;
        if (key == KEY_ENTER) {
            if (board[cursorY][cursorX] == ' ') {
                board[cursorY][cursorX] = currentPlayer == 1 ? 'X' : 'O';
                winner = checkWin();
                if (winner || isDraw()) break;
                currentPlayer = 3 - currentPlayer; // switch player
            }
        }
    }

    drawBoard(cursorX, cursorY);
    gotoxy(0, 12);
    if (winner) {
        if (winner == 1) {
            setColor(RED);
            printf("🎉 Player 1 (X) Wins!\n");
        } else {
            setColor(BLUE);
            printf("🎉 Player 2 (O) Wins!\n");
        }
    } else {
        setColor(GREY);
        printf("It's a Draw!\n");
    }

    resetColor();
    showcursor();
    getch(); // 等待按鍵後結束
    return 0;
}

'''