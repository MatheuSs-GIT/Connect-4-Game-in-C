#include <stdio.h>
#include <stdbool.h>
#include <windows.h>
#include <math.h>

#define COLUMNS 7
#define ROWS 6
#define HUMAN_PLAYER 1
#define AI_PLAYER 2

int board[ROWS][COLUMNS], nodes = 0, engineEvaluation, depth;
float alpha = -INFINITY, beta = INFINITY;

void inicializeBoard();
void drawBoard();
void bestMove(int);
void uptadeBoard(int, int);
int minimax(int, bool);
int checkWinner();
int evaluation();


int main() {
  int player =  HUMAN_PLAYER, moveColumn, totalMoves = 0;

  SetConsoleOutputCP(65001);

  inicializeBoard();
  drawBoard();

  while(1) {
    if(player == HUMAN_PLAYER) {
        printf("\nJogador %d, faça sua escolha: ", player);
        printf("\nA avaliação atual é: %d\n", engineEvaluation);
        printf("Total de posições calculadas: %d\n", nodes);
        printf("Lances totais: %d\n", totalMoves);
        printf("Profundidade da avaliação: %d\n", depth);
        scanf("%d", &moveColumn);
        system("cls");
        if (moveColumn > 0 && moveColumn <= COLUMNS && board[0][moveColumn - 1] == 0) {
            moveColumn--;
            uptadeBoard(player, moveColumn);
            drawBoard();
            if (checkWinner() != 3 && checkWinner() != 0) {
                printf("\nJogador %d ganhou!", player);
                break;
            }
        } else {
        drawBoard();
        printf("\nEscolha inválida. Tente novamente.");
        continue;
        }
    } else {
        system("cls");
        bestMove(totalMoves);
        drawBoard();
        if (checkWinner() != 3 && checkWinner() != 0) {
            printf("\nJogador %d ganhou!", player);
            break;
        } else if(checkWinner() == 0) {
            printf("\nEmpatou!");
            break;
        }
    }
    player = (player == 1) ? 2 : 1;
    totalMoves++;
  }
  return 0;
}

void inicializeBoard(){ // Adjust every index to 0
  for (int r = 0; r < ROWS; r++) {
    for (int c = 0; c < COLUMNS; c++) {
      board[r][c] = 0; 
    }
  }
}

void uptadeBoard(int player, int moveColumn) { // Add gravity for the pieces, checking if there's a piece below
int r;
  for (r = 0; r < ROWS - 1; r++) {
    if (board[r + 1][moveColumn] != 0){
      break;
    }
  }
  board[r][moveColumn] = player;
}

void drawBoard() {
  for(int r = 0; r < ROWS; r++) { 
    printf("%d| ", r+1);
    //printf("[ ");
    for(int c = 0; c < COLUMNS; c++) {
        if(board[r][c] == 0) {
            printf("_ ");
        } else if(board[r][c] == 1) {
            printf("o ");
        } else {
            printf("x ");
        }
      //printf("%d. ", board[r][c]);
    }
    printf("|\n");
    //printf("]\n");
  }
  printf("   ");
  for(int c =0; c < COLUMNS; c++) {
    printf("%d ", c+1);
  }
  printf("\n");
}

int checkWinner() {
    int c, r, checkTie = 0;
    for(r = 0; r < ROWS; r++) { // Horizontal check
        for(c = 0; c < COLUMNS - 3; c++) {
            if(board[r][c] != 0) {
                if(board[r][c] == board[r][c+1] && board[r][c+1] == board[r][c+2] && board[r][c+2] == board[r][c+3]) {
                    return board[r][c];
                }
            }
        }
    }

    for(c = 0; c < COLUMNS; c++) { // Vertical check
        for(r = 0; r < ROWS - 3; r++) {
            if(board[r][c] != 0) {
                if(board[r][c] == board[r+1][c] && board[r+1][c] == board[r+2][c] && board[r+2][c] == board[r+3][c]) {
                    return board[r][c];
                }
            }
        }
    }
   for(r = 0; r < ROWS - 3; r++) { // Check from top left to bottom right diagonal
        for(c = 0; c < COLUMNS - 3; c++) {
            if(board[r][c] != 0) {
                if(board[r][c] == board[r+1][c+1] && board[r+1][c+1] == board[r+2][c+2] && board[r+2][c+2] == board[r+3][c+3]) {
                    return board[r][c];
                }
            }
        }
    } 
   for(r = 3; r < ROWS; r++) { // Check from bottom left to top right diagonal
        for(c = 0; c < COLUMNS - 3; c++) {
            if(board[r][c] != 0) {
                if(board[r][c] == board[r-1][c+1] && board[r-1][c+1] == board[r-2][c+2] && board[r-2][c+2] == board[r-3][c+3]) {
                    return board[r][c];
                }
            }
        }
    } 
    for(c = 0; c < COLUMNS; c++) { // Check if it's a draw
        if(board[0][c] != 0) {
            checkTie++;
        }  
        if(checkTie == COLUMNS) {
            return 0; 
        }
    }
    return 3; 
}

void bestMove(int totalMoves) { // AI's turn
  int score, move;
  float bestScore = -INFINITY;

  depth = 5+totalMoves*0.15;

  for(int c = 0; c < COLUMNS; c++) {
    for(int r = 0; r < ROWS; r++) {
      if(board[r][c] == 0 && board[r+1][c] != 0 || board[r][c] == 0 && r == ROWS - 1) { // Is the spot available
        board[r][c] = AI_PLAYER;
        score = minimax(depth, false);
        board[r][c] = 0;
        if(score > bestScore) {
            bestScore = score;
            engineEvaluation = bestScore;
            move = c;
        }
      }
    }
  }
  uptadeBoard(AI_PLAYER, move);
}

int minimax(int depth, bool isMaximazing) { // Minimax algorithm implementation
    int scores[3] = {0, -1000000, 1000000}, result, score;
    float bestScore;
    nodes++;
    result = checkWinner();
    if(result != 3) {
        return scores[result];
    }

    if(isMaximazing) {
        bestScore = -INFINITY;
        for(int c = 0; c < COLUMNS; c++) {
            for(int r = 0; r < ROWS; r++) {
                if(board[r][c] == 0 && board[r+1][c] != 0 || board[r][c] == 0 && r == ROWS - 1) { // Is the spot available
                    board[r][c] = AI_PLAYER;
                        if(depth > 0) {
                            score = minimax(depth-1, false);
                        } else {
                            score = evaluation();
                        }
                    board[r][c] = 0;
                    bestScore = max(score, bestScore);
                    alpha = bestScore;
                    if(alpha >= beta) {
                        break;
                    }
                }
            }
        }
        return bestScore;
    } else {
        bestScore = INFINITY;
        for(int c = 0; c < COLUMNS; c++) {
            for(int r = 0; r < ROWS; r++) {
                if(board[r][c] == 0 && board[r+1][c] != 0 || board[r][c] == 0 && r == ROWS - 1) { // Is the spot available
                    board[r][c] = HUMAN_PLAYER;
                        if(depth > 0) {
                            score = minimax(depth-1, true);
                        } else {
                            score = evaluation();
                        }
                    board[r][c] = 0;
                    bestScore = min(score, bestScore);
                    beta = bestScore;
                    if(alpha > beta) {
                        break;
                    }
                }
            }
        }
        return bestScore;
    }
}

int evaluation() {
    int aiScore = 0, humanScore = 0, score = 0, scoreTable[ROWS*COLUMNS] = {3, 4, 5, 7, 5, 4, 3, 4, 6, 8, 10, 8, 6, 4, 5, 7, 11, 13, 11, 7, 5, 5, 7, 11, 13, 11, 7, 5, 4, 6, 8, 10, 8, 6, 4, 3, 4, 5, 7, 5, 4, 3};
    for(int r = 0; r < ROWS ; r++) {
        for(int c = 0; c < COLUMNS; c++) {
            if(board[r][c] == AI_PLAYER) {
                aiScore += scoreTable[r*COLUMNS+c];
            } else if(board[r][c] == HUMAN_PLAYER) {
                humanScore += scoreTable[r*COLUMNS+c];
            }
        }
    }
    score = aiScore - humanScore;
    return score;
}
