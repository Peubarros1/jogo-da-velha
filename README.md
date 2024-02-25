# jogo-da-velha#include <stdio.h>

char tabuleiro[3][3];

void moverto11() {
    printf("mexeu para 11\n");
}

void moverto12() {
    printf("mexeu para 12\n");
}

void moverto13() {
    printf("mexeu para 13\n");
}

void moverto21() {
    printf("mexeu para 21\n");
}

void moverto22() {
    printf("mexeu para 22\n");
}

void moverto23() {
    printf("mexeu para 23\n");
}

void moverto31() {
    printf("mexeu para 31\n");
}

void moverto32() {
    printf("mexeu para 32\n");
}

void moverto33() {
    printf("mexeu para 33\n");
}

void moverto(int i, int j) {
    if (i == 0 && j == 0) {
        moverto11();
    } else if (i == 0 && j == 1) {
        moverto12();
    } else if (i == 0 && j == 2) {
        moverto13();
    } else if (i == 1 && j == 0) {
        moverto21();
    } else if (i == 1 && j == 1) {
        moverto22();
    } else if (i == 1 && j == 2) {
        moverto23();
    } else if (i == 2 && j == 0) {
        moverto31();
    } else if (i == 2 && j == 1) {
        moverto32();
    } else if (i == 2 && j == 2) {
        moverto33();
    }
}

void inicializar_tabuleiro() {
    int i, j;
    for (i = 0; i < 3; i++) {
        for (j = 0; j < 3; j++) {
            tabuleiro[i][j] = ' ';
        }
    }
}

void imprimir_tabuleiro() {
    int i, j;
    printf("-------------\n");
    for (i = 0; i < 3; i++) {
        printf("| ");
        for (j = 0; j < 3; j++) {
            printf("%c | ", tabuleiro[i][j]);
        }
        printf("\n-------------\n");
    }
}

char verificar_vencedor(char jogador) {
    int i;
    // Verificar linhas e colunas
    for (i = 0; i < 3; i++) {
        if ((tabuleiro[i][0] == jogador && tabuleiro[i][1] == jogador && tabuleiro[i][2] == jogador) ||
            (tabuleiro[0][i] == jogador && tabuleiro[1][i] == jogador && tabuleiro[2][i] == jogador)) {
            return jogador;
        }
    }
    // Verificar diagonais
    if ((tabuleiro[0][0] == jogador && tabuleiro[1][1] == jogador && tabuleiro[2][2] == jogador) ||
        (tabuleiro[0][2] == jogador && tabuleiro[1][1] == jogador && tabuleiro[2][0] == jogador)) {
        return jogador;
    }
    return ' ';
}

int verificar_empate() {
    int i, j;
    for (i = 0; i < 3; i++) {
        for (j = 0; j < 3; j++) {
            if (tabuleiro[i][j] == ' ') {
                return 0;
            }
        }
    }
    return 1;
}

int validar_jogada(int linha, int coluna) {
    if (linha >= 0 && linha < 3 && coluna >= 0 && coluna < 3 && tabuleiro[linha][coluna] == ' ') {
        return 1;
    }
    return 0;
}

void fazer_jogada(int linha, int coluna, char jogador) {
    if (validar_jogada(linha, coluna)) {
        tabuleiro[linha][coluna] = jogador;
    }
}

void jogada_jogador() {
    int linha, coluna;
    printf("Sua vez de jogar. Informe a linha e a coluna (0-2): ");
    scanf("%d %d", &linha, &coluna);
    if (validar_jogada(linha, coluna)) {
        fazer_jogada(linha, coluna, 'X');
        printf("Voce jogou em %d %d.\n", linha, coluna);
    } else {
        printf("Jogada invalida. Tente novamente.\n");
        jogada_jogador();
    }
}

void jogada_computador() {
    int i, j;

    // Verifica se pode ganhar na próxima jogada
    for (i = 0; i < 3; i++) {
        for (j = 0; j < 3; j++) {
            if (validar_jogada(i, j)) {
                tabuleiro[i][j] = 'O';
                if (verificar_vencedor('O') == 'O') {
                    moverto(i, j);
                    return;
                }
                tabuleiro[i][j] = ' ';
            }
        }
    }

    // Verifica se o jogador pode ganhar na próxima jogada e bloqueia
    for (i = 0; i < 3; i++) {
        for (j = 0; j < 3; j++) {
            if (validar_jogada(i, j)) {
                tabuleiro[i][j] = 'X';
                if (verificar_vencedor('X') == 'X') {
                    tabuleiro[i][j] = 'O';
                    moverto(i, j);
                    return;
                }
                tabuleiro[i][j] = ' ';
            }
        }
    }

    // Faz uma jogada em uma posição estratégica
    if (validar_jogada(1, 1)) {
        tabuleiro[1][1] = 'O';
        moverto22();
    } else {
        if (validar_jogada(0, 0)) {
            tabuleiro[0][0] = 'O';
            moverto11();
        } else if (validar_jogada(0, 2)) {
            tabuleiro[0][2] = 'O';
            moverto13();
        } else if (validar_jogada(2, 0)) {
            tabuleiro[2][0] = 'O';
            moverto31();
        } else if (validar_jogada(2, 2)) {
            tabuleiro[2][2] = 'O';
            moverto33();
        }
    }
}

int main() {
    inicializar_tabuleiro();
    printf("Bem-vindo ao Jogo da Velha!\n");
    printf("Voce eh X, e o computador eh O.\n");
    printf("Aqui esta o tabuleiro:\n");
    imprimir_tabuleiro();

    while (1) {
        jogada_jogador();
        imprimir_tabuleiro();
        if (verificar_vencedor('X') == 'X') {
            printf("Parabens! Voce venceu!\n");
            break;
        }
        if (verificar_empate()) {
            printf("O jogo terminou em empate.\n");
            break;
        }

        jogada_computador();
        printf("O computador jogou:\n");
        imprimir_tabuleiro();
        if (verificar_vencedor('O') == 'O') {
            printf("O computador venceu!\n");
            break;
        }
        if (verificar_empate()) {
            printf("O jogo terminou em empate.\n");
            break;
        }
    }

    return 0;
}
