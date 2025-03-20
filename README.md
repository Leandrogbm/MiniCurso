# MiniCurso
#include <stdio.h>
#include <locale.h>

#define MAX_PROCESSOS 10
#define QUANTUM 30 // Definindo o quantum fixo em 30 segundos

// Estrutura de um processo
typedef struct {
    int id;
    int tempo_execucao;
    int tempo_restante;
} Processo;

// Função para o escalonamento Round Robin
void escalonamento_round_robin(Processo processos[], int num_processos) {
    int tempo_total = 0; // Tempo total de execução
    int concluidos = 0; // Número de processos concluídos
    int i; // Declarar a variável fora do loop para evitar erros em padrões mais antigos

    printf("Executando o escalonamento Round Robin:\n\n");

    while (concluidos < num_processos) {
        for (i = 0; i < num_processos; i++) { // Usando a variável já declarada
            // Verifica se o processo ainda precisa ser executado
            if (processos[i].tempo_restante > 0) {
                printf("Tempo atual: %d segundos | Executando processo P%d\n", tempo_total, processos[i].id);
                
                // Se o tempo restante do processo for menor ou igual ao quantum
                if (processos[i].tempo_restante <= QUANTUM) {
                    // Executa o processo até que ele seja concluído
                    tempo_total += processos[i].tempo_restante;
                    printf("Processo P%d concluído no tempo %d segundos\n", processos[i].id, tempo_total);
                    
                    // Marca o processo como concluído
                    processos[i].tempo_restante = 0;
                    concluidos++;
                } else {
                    // Executa o processo pelo tempo do quantum
                    tempo_total += QUANTUM;
                    processos[i].tempo_restante -= QUANTUM;
                    printf("Processo P%d pausado. Tempo restante: %d segundos\n", processos[i].id, processos[i].tempo_restante);
                }
            }
        }
    }

    printf("\nTodos os processos foram concluídos no tempo %d segundos.\n", tempo_total);
}

int main() {
    setlocale(LC_ALL, "Portuguese");

    int num_processos;
    int i; // Declarar a variável fora do loop para evitar erros em padrões mais antigos

    printf("Informe o número de processos (máximo %d): ", MAX_PROCESSOS);
    scanf("%d", &num_processos);

    if (num_processos > MAX_PROCESSOS) {
        printf("Erro: número máximo de processos é %d.\n", MAX_PROCESSOS);
        return 1;
    }

    Processo processos[MAX_PROCESSOS];

    // Lê os tempos de execução para cada processo
    for (i = 0; i < num_processos; i++) { // Usando a variável já declarada
        processos[i].id = i + 1;
        printf("Informe o tempo de execução do processo P%d em segundos: ", processos[i].id);
        scanf("%d", &processos[i].tempo_execucao);
        processos[i].tempo_restante = processos[i].tempo_execucao;
    }

    // Executa o algoritmo de escalonamento Round Robin com quantum fixo
    escalonamento_round_robin(processos, num_processos);

    return 0;
}



