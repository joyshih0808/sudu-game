## 最終程式碼
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <conio.h>
#include <windows.h>

#define SIZE 3
#define TIME_LIMIT 30

int mole_pos = 0;
int score = 0;
time_t start_time;

void hide_cursor() {
    CONSOLE_CURSOR_INFO ci;
    ci.dwSize = 1;
    ci.bVisible = FALSE;
    SetConsoleCursorInfo(GetStdHandle(STD_OUTPUT_HANDLE), &ci);
}

void set_cursor_position(int x, int y) {
    COORD pos = {x, y};
    SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE), pos);
}

// Cute cat face (small and compatible)
char* mole_art[3] = {
    "         ",
    " (n=^.w.^=n) ",
    "         "
};

#define CELL_WIDTH 11
#define CELL_HEIGHT 3

void draw() {
    set_cursor_position(0, 0);
    printf("== Whack-a-Cat ==\n");
    printf("Press number 1~9 to hit the cat!\n");
    printf("Time left: %ld sec   Score: %d\n\n", TIME_LIMIT - (time(NULL) - start_time), score);

    for (int row = 0; row < SIZE; row++) {
        for (int line = 0; line < CELL_HEIGHT; line++) {
            for (int col = 0; col < SIZE; col++) {
                int idx = row * SIZE + col;

                if (idx == mole_pos && line == 1) {
                    printf("%-*s", CELL_WIDTH, mole_art[line]);
                } else if (line == 1) {
                    printf("   [%d]     ", idx + 1);
                } else {
                    printf("           ");
                }
            }
            printf("\n");
        }
    }
}

void update_mole() {
    mole_pos = rand() % 9;
}

int main() {


    srand(time(NULL));
    hide_cursor();
    start_time = time(NULL);
    update_mole();

    while (1) {
        draw();

        time_t now = time(NULL);
        if (now - start_time >= TIME_LIMIT) {
            printf("\nTime's up! You hit %d cats!\n", score);
            break;
        }

        Sleep(800);

        if (_kbhit()) {
            char key = _getch();
            int pos = key - '1';

            if (pos == mole_pos) {
                score++;
            }
            update_mole();
        } else {
            update_mole();
        }
    }

    return 0;
}

```