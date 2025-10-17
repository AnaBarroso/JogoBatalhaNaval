# JogoBatalhaNaval
Desafio Jogo Batalha Naval
#include <stdio.h>
#include <stdlib.h>

#define TAM 10
#define TAM_H 5
#define AGUA 0
#define NAVIO 3
#define AREA 5

// ---------- Cria matrizes de habilidades ----------
void construir_cone(int m[TAM_H][TAM_H]) {
    int c = TAM_H / 2;
    for (int i = 0; i < TAM_H; i++) {
        for (int j = 0; j < TAM_H; j++) {
            m[i][j] = ((j - c <= i) && (c - j <= i)) ? 1 : 0;
        }
    }
}

void construir_cruz(int m[TAM_H][TAM_H]) {
    int c = TAM_H / 2;
    for (int i = 0; i < TAM_H; i++) {
        for (int j = 0; j < TAM_H; j++) {
            m[i][j] = (i == c || j == c);
        }
    }
}

void construir_octaedro(int m[TAM_H][TAM_H]) {
    int c = TAM_H / 2;
    for (int i = 0; i < TAM_H; i++) {
        for (int j = 0; j < TAM_H; j++) {
            m[i][j] = (abs(i - c) + abs(j - c) <= c);
        }
    }
}

// ---------- Aplica habilidade no tabuleiro ----------
void aplicar(int tab[TAM][TAM], int hab[TAM_H][TAM_H], int lin, int col) {
    int m = TAM_H / 2;
    for (int i = 0; i < TAM_H; i++) {
        for (int j = 0; j < TAM_H; j++) {
            if (hab[i][j]) {
                int r = lin + i - m;
                int c = col + j - m;
                if (r >= 0 && r < TAM && c >= 0 && c < TAM && tab[r][c] != NAVIO) {
                    tab[r][c] = AREA;
                }
            }
        }
    }
}

// ---------- Imprime o tabuleiro ----------
void imprimir(int t[TAM][TAM]) {
    for (int i = 0; i < TAM; i++) {
        for (int j = 0; j < TAM; j++) {
            printf("%d ", t[i][j]);
        }
        printf("\n");
    }
}

int main() {
    int tab[TAM][TAM] = {0};

    // Coloca navios
    for (int i = 5; i < 8; i++) {
        tab[i][5] = NAVIO;
    }
    for (int j = 6; j < 9; j++) {
        tab[4][j] = NAVIO;
    }
    for (int i = 0; i < 3; i++) {
        tab[1 + i][1 + i] = NAVIO;
        tab[1 + i][8 - i] = NAVIO;
    }

    printf("=== JOGO DE BATALHA NAVAL ===\n\n");
    printf("Tabuleiro base:\n");
    imprimir(tab);

    // Habilidades
    int cone[TAM_H][TAM_H];
    int cruz[TAM_H][TAM_H];
    int octa[TAM_H][TAM_H];

    construir_cone(cone);
    construir_cruz(cruz);
    construir_octaedro(octa);

    // Cópias do tabuleiro
    int t_cone[TAM][TAM];
    int t_cruz[TAM][TAM];
    int t_octa[TAM][TAM];

    for (int i = 0; i < TAM; i++) {
        for (int j = 0; j < TAM; j++) {
            t_cone[i][j] = t_cruz[i][j] = t_octa[i][j] = tab[i][j];
        }
    }

    // Aplicações
    aplicar(t_cone, cone, 2, 7);
    aplicar(t_cruz, cruz, 5, 3);
    aplicar(t_octa, octa, 6, 6);

    // Resultados
    printf("\nHabilidade CONE:\n");
    imprimir(t_cone);

    printf("\nHabilidade CRUZ:\n");
    imprimir(t_cruz);

    printf("\nHabilidade OCTAEDRO:\n");
    imprimir(t_octa);

    return 0;
}

