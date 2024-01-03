# snake-game
simple program for a snake game in c
#include <stdio.h>
#include <conio.h>
#include <stdlib.h>
#include <conio.h>

int length;
int gameover;
int score;
int x, y, fruitX, fruitY;
int tailX[100], tailY[100];
enum eDirecton { STOP = 0, LEFT, RIGHT, UP, DOWN };
enum eDirecton dir;

void setup() {
    gameover = 0;
    dir = STOP;
    x = 10;
    y = 10;
    fruitX = rand() % 20;
    fruitY = rand() % 20;
    score = 0;
}

void draw() {
    system("cls");
    for (int i = 0; i < 20; i++) {
        for (int j = 0; j < 20; j++) {
            if (i == 0 || i == 19 || j == 0 || j == 19) {
                printf("#");
            } else if (i == x && j == y) {
                printf("O");
            } else if (i == fruitX && j == fruitY) {
                printf("F");
            } else {
                int isPrinted = 0;
                for (int k = 0; k < length; k++) {
                    if (i == tailX[k] && j == tailY[k]) {
                        printf("o");
                        isPrinted = 1;
                    }
                }
                if (!isPrinted) {
                    printf(" ");
                }
            }
        }
        printf("\n");
    }
    printf("Score:%d", score);
    printf("\n");
    printf("Press 'x' to quit the game");
}

void input() {
    if (_kbhit())
    {
        switch (getch())
        {
        case 'a':
            dir = LEFT;
            break;
        case 'd':
            dir = RIGHT;
            break;
        case 'w':
            dir = UP;
            break;
        case 's':
            dir = DOWN;
            break;
        case 'x':
            gameover = 1;
            break;
        }
    }
}

void algorithm() {
    int prevX = tailX[0];
    int prevY = tailY[0];
    int prev2X, prev2Y;
    tailX[0] = x;
    tailY[0] = y;
    for (int i = 1; i < length; i++) {
        prev2X = tailX[i];
        prev2Y = tailY[i];
        tailX[i] = prevX;
        tailY[i] = prevY;
        prevX = prev2X;
        prevY = prev2Y;
    }
    switch (dir) {
    case LEFT:
        y--;
        break;
    case RIGHT:
        y++;
        break;
    case UP:
        x--;
        break;
    case DOWN:
        x++;
        break;
    default:
        break;
    }
    if (x < 0 || x > 19 || y < 0 || y > 19) {
        gameover = 1;
    }
    for (int i = 0; i < length; i++) {
        if (tailX[i] == x && tailY[i] == y) {
            gameover = 1;
        }
    }
    if (x == fruitX && y == fruitY) {
        score += 10;
        fruitX = rand() % 20;
        fruitY = rand() % 20;
        length++;
    }
}

int main() {
    setup();
    while (!gameover) {
        draw();
        input();
        algorithm();
    }
    return 0;
}
