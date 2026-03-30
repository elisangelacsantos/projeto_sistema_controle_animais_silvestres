//Projeto Animais Silvestres - versão 2


#include <stdio.h>
#include <stdlib.h>
#include <locale.h>
#include <string.h>

// Estrutura do Animal conforme especificações
typedef struct {
    int id;                // Até 5 dígitos
    char especie[20];      // Mamífero, ave, réptil, peixe
    char idade[20];        // 2 casas ou "Não sabe informar"
    char sexo[10];         // Macho ou Fêmea
    char data[12];         // DD/MM/AAAA
    char local[100];       // Bairro e Cidade
    char statusClinico[50]; // Filhote, gestante, ferido
    char formaEntrada[30];  // Resgate, apreendido, entrega voluntária, outros
} Animal;

// Função para limpar o buffer do teclado (evita pular campos)
void limparBuffer() {
    int c;
    while ((c = getchar()) != '\n' && c != EOF);
}

// Função para remover o \n que o fgets adiciona ao final da string
void removerQuebraLinha(char *str) {
    size_t len = strlen(str);
    if (len > 0 && str[len - 1] == '\n') {
        str[len - 1] = '\0';
    }
}

void cadastrarAnimal() {
    FILE *arquivo = fopen("banco_animais.txt", "a");
    if (arquivo == NULL) {
        printf("Erro ao abrir o arquivo!\n");
        return;
    }

    Animal a;
    printf("\n--- CADASTRO DE CONTROLE DE FAUNA ---\n");

    printf("ID do Animal (até 5 dígitos): ");
    scanf("%d", &a.id);
    limparBuffer();

    printf("Espécie (Mamífero, Ave, Réptil, Peixe): ");
    fgets(a.especie, 20, stdin);
    removerQuebraLinha(a.especie);

    printf("Idade (2 dígitos ou 'Não sabe informar'): ");
    fgets(a.idade, 20, stdin);
    removerQuebraLinha(a.idade);

    printf("Sexo (Macho ou Fêmea): ");
    fgets(a.sexo, 10, stdin);
    removerQuebraLinha(a.sexo);

    printf("Data do Resgate (DD/MM/AAAA): ");
    fgets(a.data, 12, stdin);
    removerQuebraLinha(a.data);

    printf("Local do Resgate (Bairro e Cidade): ");
    fgets(a.local, 100, stdin);
    removerQuebraLinha(a.local);

    printf("Status Clínico (Filhote, Gestante, Ferido): ");
    fgets(a.statusClinico, 50, stdin);
    removerQuebraLinha(a.statusClinico);

    printf("Forma de Entrada (Resgate, Apreendido, Entrega Voluntária, Outros): ");
    fgets(a.formaEntrada, 30, stdin);
    removerQuebraLinha(a.formaEntrada);

    // Grava no arquivo TXT separado por ;
    fprintf(arquivo, "%05d;%s;%s;%s;%s;%s;%s;%s\n", 
            a.id, a.especie, a.idade, a.sexo, a.data, a.local, a.statusClinico, a.formaEntrada);

    fclose(arquivo);
    printf("\n>>> Registro salvo com sucesso no arquivo 'banco_animais.txt'!\n");
}

void listarAnimais() {
    FILE *arquivo = fopen("banco_animais.txt", "r");
    if (arquivo == NULL) {
        printf("\nO banco de dados está vazio ou não foi criado ainda.\n");
        return;
    }

    Animal a;
    printf("\n%-6s | %-10s | %-5s | %-10s | %-10s | %-15s\n", "ID", "ESPÉCIE", "ID.","SEXO", "DATA", "STATUS");
    printf("--------------------------------------------------------------------------------\n");

    // Máscara de leitura para ler strings com espaços separadas por ;
    while (fscanf(arquivo, "%d;%[^;];%[^;];%[^;];%[^;];%[^;];%[^;];%[^\n]\n", 
           &a.id, a.especie, a.idade, a.sexo, a.data, a.local, a.statusClinico, a.formaEntrada) != EOF) {
        
        printf("%05d  | %-10s | %-5s | %-10s | %-10s | %-15s\n", 
               a.id, a.especie, a.idade, a.sexo, a.data, a.statusClinico);
    }

    fclose(arquivo);
}

int main() {
    setlocale(LC_ALL, "Portuguese");
    int opcao;

    do {
        printf("\n===== SISTEMA DE CONTROLE DE ANIMAIS SILVESTRES =====\n");
        printf("1. Cadastrar Novo Animal\n");
        printf("2. Listar Animais Registrados\n");
        printf("0. Sair\n");
        printf("Escolha uma opção: ");
        
        if (scanf("%d", &opcao) != 1) {
            limparBuffer();
            opcao = -1;
        } else {
            limparBuffer();
        }

        switch(opcao) {
            case 1: cadastrarAnimal(); break;
            case 2: listarAnimais(); break;
            case 0: printf("Encerrando sistema...\n"); break;
            default: printf("Opção inválida!\n");
        }
    } while (opcao != 0);

    return 0;
}

