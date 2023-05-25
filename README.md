# Trabalho-EDD
Trabalho do capeta



#include <stdio.h>
#include <stdlib.h>

typedef struct no {
    int valor;
    struct no* prox;
} NO;

void inserirValorNaLista(NO** inicio, NO** fim, int valor) {
    NO* novoNo = (NO*)malloc(sizeof(NO));
    novoNo->valor = valor;
    novoNo->prox = NULL;
    if (*inicio == NULL) {
        *inicio = novoNo;
        *fim = novoNo;
    }else{
        (*fim)->prox = novoNo;
        *fim = novoNo;
    }
}

void inserirValorNaPosicaoLista(NO** inicio, NO** fim, int valor, int posicao) {
    if (posicao < 0) {
        printf("POSIÇÃO ESCOLHIDA NÃO EXISTE\n");
        return;
    }
    
    NO* novoNo = (NO*)malloc(sizeof(NO));
    novoNo->valor = valor;

    if (posicao == 0) {
        novoNo->prox = *inicio;
        *inicio = novoNo;
        if (*fim == NULL) {
            *fim = novoNo;
        }
        return;
    }

    NO* atual = *inicio;
    int contador = 0;
    while (atual != NULL && contador < posicao - 1) {
        atual = atual->prox;
        contador++;
    }

    if (atual == NULL || atual->prox == NULL) {
        printf("POSIÇÃO ESCOLHIDA NÃO EXISTE\n");
        return;
    }

    novoNo->prox = atual->prox;
    atual->prox = novoNo;
    if (atual == *fim) {
        *fim = novoNo;
    }
}

void removerUltimoValorLista(NO** inicio, NO** fim) {
    if (*inicio == NULL) {
        printf("LISTA VAZIA\n");
        return;
    }

    if (*inicio == *fim) {
        free(*inicio);
        *inicio = NULL;
        *fim = NULL;
        return;
    }

    NO* atual = *inicio;
    while (atual->prox != *fim) {
        atual = atual->prox;
    }

    free(*fim);
    *fim = atual;
    atual->prox = NULL;
}

void removerValorLista(NO** inicio, NO** fim, int valor) {
    if (*inicio == NULL) {
        printf("LISTA VAZIA\n");
        return;
    }

    NO* atual = *inicio;
    NO* anterior = NULL;

    while (atual != NULL) {
        if (atual->valor == valor) {
            if (anterior == NULL) {
                *inicio = atual->prox;
                if (*fim == atual) {
                    *fim = NULL;
                }
            } else {
                anterior->prox = atual->prox;
                if (*fim == atual) {
                    *fim = anterior;
                }
            }

            free(atual);
            printf("VALOR REMOVIDO\n");
            return;
        }

        anterior = atual;
        atual = atual->prox;
    }

    printf("VALOR NÃO ENCONTRADO NA LISTA\n");
}

void removerPosicaoLista(NO** inicio, NO** fim, int posicao) {
    if (*inicio == NULL) {
        printf("LISTA VAZIA\n");
        return;
    }

    if (posicao < 0) {
        printf("POSIÇÃO ESCOLHIDA NÃO EXISTE\n");
        return;
    }

    if (posicao == 0) {
        NO* noRemovido = *inicio;
        *inicio = (*inicio)->prox;
        if (*fim == noRemovido) {
            *fim = NULL;
        }
        free(noRemovido);
        return;
    }

    NO* atual = *inicio;
    int contador = 0;
    while (atual != NULL && contador < posicao - 1) {
        atual = atual->prox;
        contador++;
    }

    if (atual == NULL || atual->prox == NULL) {
        printf("POSIÇÃO ESCOLHIDA NÃO EXISTE\n");
        return;
    }

    NO* noRemovido = atual->prox;
    atual->prox = noRemovido->prox;
    if (noRemovido == *fim) {
        *fim = atual;
    }
    free(noRemovido);
}

void pesquisarValorLista(NO* inicio, int valor) {
    NO* atual = inicio;
    int posicao = 0;

    while (atual != NULL) {
        if (atual->valor == valor) {
            printf("O VALOR EXISTE NA LISTA NA POSIÇÃO %d\n", posicao);
            return;
        }

        atual = atual->prox;
        posicao++;
    }

    printf("VALOR NÃO ENCONTRADO NA LISTA\n");
}

void imprimirLista(NO* inicio) {
    NO* atual = inicio;

    while (atual != NULL) {
        printf("%d ", atual->valor);
        atual = atual->prox;
    }

    printf("\n");
}

void liberarLista(NO** inicio) {
    NO* atual = *inicio;
    NO* proximo;

    while (atual != NULL) {
        proximo = atual->prox;
        free(atual);
        atual = proximo;
    }

    *inicio = NULL;
}

int main() {
    NO* inicio = NULL;
    NO* fim = NULL;
    int opcao, valor, posicao;

    do {
        printf("\nMENU\n");
        printf("1. Inserir Valor na Lista\n");
        printf("2. Inserir Valor na Posição da Lista\n");
        printf("3. Remover Último Valor da Lista\n");
        printf("4. Remover Valor da Lista\n");
        printf("5. Remover Posição da Lista\n");
        printf("6. Pesquisar Valor na Lista\n");
        printf("7. Imprimir Lista\n");
        printf("0. Finalizar Programa\n");
        printf("Escolha uma opção: ");
        scanf("%d", &opcao);

        switch (opcao) {
            case 1:
                printf("Digite o valor a ser inserido: ");
                scanf("%d", &valor);
                inserirValorNaLista(&inicio, &fim, valor);
                break;
            case 2:
                printf("Digite o valor a ser inserido: ");
                scanf("%d", &valor);
                printf("Digite a posição: ");
                scanf("%d", &posicao);
                inserirValorNaPosicaoLista(&inicio, &fim, valor, posicao);               
                break;
            case 3:
                removerUltimoValorLista(&inicio, &fim);
                break;
            case 4:
                printf("Digite o valor a ser removido: ");
                scanf("%d", &valor);
                removerValorLista(&inicio, &fim, valor);
                break;
            case 5:
                printf("Digite a posição a ser removida: ");
                scanf("%d", &posicao);
                removerPosicaoLista(&inicio, &fim, posicao);           
                break;
            case 6:
                printf("Digite o valor a ser pesquisado: ");
                scanf("%d", &valor);
                pesquisarValorLista(inicio, valor);
                break;
            case 7:
                imprimirLista(inicio);
                break;
            case 0:
                liberarLista(&inicio);
                printf("Programa finalizado.\n");
                break;
            default:
                printf("Opção inválida.\n");
                break;
        }
    } while (opcao != 0);

    return 0;
}
