# O-Jogo-da-Velha---O-Jogo-do-Galo
Jogo da velha ou o Jogo do Galo foi criado durante o curso de linguagem de programação em C

#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <locale.h>
#include <conio.h>
#include <windows.h>

Subprogramas e variáveis globais
char tabuleiro[3][3];
int vitoriasJogador = 0;
int vitoriasMaquina = 0;
void splashScreen();
void mostrarMenu();
void inicializarTabuleiro();
void mostrarTabuleiro();
int verificarVitoria(char simbolo); // para verificar se o X (player 1) ou se o O (máquina) ganhou
int tabuleiroCheio();
void jogarContraMaquina();
void jogadaJogador();
void jogadaMaquina();
void limparBuffer(); 

int main() {
	
	setlocale(LC_ALL, "");
    char opcao;	
    const char *Inst[1] = {
	"\nO jogo da velha é jogado 1x por rodada do qual o jogador(a) é o X e a máquina é a O. Após iniciada a partida, deverá buscar formar uma linha de X na horizontal, vertical ou diagonal para vencer o jogo. Lembrando que deverá pôr as cordenadas da sua jogada, pois o tabuleiro do jogo é feito numa matriz 3x3, ou seja, com 3 linhas e 3 colunas. Pense bem onde irá fazer a sua jogada e ponha as cordenadas separadas por vírgulas e boa sorte!"
	};

    srand(time(NULL)); // Inicializa o gerador de números aleatórios
    splashScreen();

    do {
        
		mostrarMenu();
        printf("Escolha uma opção: ");
        opcao = getche(); 
        system("cls");
        
		        
        switch (opcao) {
        	        	
            case '1':
                jogarContraMaquina();
                break;
            case '2':
                printf("Instruções:\n  %s\n", Inst[0]); // imprimir a string na posição 0 (única)
                printf("\nPressione qualquer tecla para voltar ao menu");
                getch();
                system("cls");
                break;
			case '3':
                printf("\nVitórias:\nVocê: %d\nMáquina: %d\n\n", vitoriasJogador, vitoriasMaquina);
                break;
            case '0':
                printf("\nObrigado(a) por jogar!\n");
                Sleep(400);
                exit(0);
                
            default:
                printf("\n\aOpção inválida! Tente novamente.\n");
                system("color 47");
                Sleep(400);
                system("color 07");
                printf("Pressione qualquer tecla para voltar ao menu\n");
                getch();
                system("cls");
                break;
        }

    } while (opcao != 0);

    return 0;
}

// Menu
void splashScreen() {
    printf("======================================\n");
    printf("      BEM-VINDO(A) AO JOGO DA VELHA     \n");
    printf("            CONTRA A MÁQUINA             \n");
    printf("======================================\n\n");
}

// Seleção para o menu principal
void mostrarMenu() {
    printf("========= MENU =========\n");
    printf("1. Jogar contra a máquina\n");
    printf("2. Instruções\n");
    printf("3. Ver vitórias\n");
    printf("0. Sair\n");
    printf("========================\n");
}

// Inicializa o tabuleiro com espaços
// Função para zerar o tabuleiro [3][3] antes de cada nova partida através do ' ' em cada posição da matriz	garantindo que 
// que não tem nenhum X ou O nas matrizes
void inicializarTabuleiro() {
    for (int i = 0; i < 3; i++) // Linha
        for (int j = 0; j < 3; j++) //Coluna
            tabuleiro[i][j] = ' '; // Linha e coluna vazias
}

// Montar o tabuleiro
void mostrarTabuleiro() {
	printf("\n\n\n"); // Centralização vertical (3 linhas acima)
    for (int i = 0; i < 3; i++) { //percorrer as 3 linhas do tabuleiro
    printf("            "); // Centralização horizontal
    printf(" %c | %c | %c \n", tabuleiro[i][0], tabuleiro[i][1], tabuleiro[i][2]); // as 3 colunas da linha 1
    if (i < 2) { // para não imprimir mais uma linha
            printf("            "); // Centralização horizontal para o separador
            printf("---|---|---\n");
        }
    }

    printf("\n"); // Um espaço abaixo do tabuleiro
}


// Verificar se um jogador venceu, o simbolo aqui é declardo somente dentro desta função que pode assumir valores X ou O
int verificarVitoria(char simbolo) {
    for (int i = 0; i < 3; i++) {
        if (tabuleiro[i][0] == simbolo && tabuleiro[i][1] == simbolo && tabuleiro[i][2] == simbolo)
            return 1;  // Verifica se todos os elementos da linha i são iguais ao símbolo, se encontrar uma linha ou coluna completa com o mesmo símbolo, retorna 1 (vitória).
        if (tabuleiro[0][i] == simbolo && tabuleiro[1][i] == simbolo && tabuleiro[2][i] == simbolo)
            return 1; // Verifica se todos os elementos da coluna i são iguais ao símbolo, se encontrar uma linha ou coluna completa com o mesmo símbolo, retorna 1 (vitória).
    }

    if (tabuleiro[0][0] == simbolo && tabuleiro[1][1] == simbolo && tabuleiro[2][2] == simbolo)
        return 1; // Verifica vitória na diagona principal

    if (tabuleiro[0][2] == simbolo && tabuleiro[1][1] == simbolo && tabuleiro[2][0] == simbolo)
        return 1; // Verifica vitória na diagona secundária

    return 0;
}

// Verifica se o tabuleiro está cheio
int tabuleiroCheio() {
    for (int i = 0; i < 3; i++) // percorrer linhas
        for (int j = 0; j < 3; j++) // percorrer colunas
            if (tabuleiro[i][j] == ' ')
                return 0; // tabuleiro não está cheio ainda 
    return 1;
}

// Lógica principal do player contra a máquina - coração do jogo
void jogarContraMaquina() {
    inicializarTabuleiro(); //subprograma
    char jogador = 'X'; // Jogador é X, máquina é O
    int fimDeJogo = 0; // enquanto o fim do jogo for igual a zero, é pra continuar as jogadas, se fim de jogo for igual a 1, é falso, o jogo para

    while (!fimDeJogo) { // enquanto o jogo não terminar, é pra continuar a rodar, é tipo um while
    	system("cls"); // limpar a tela e não mostrar as jogadas anteriores 
		mostrarTabuleiro(); //subprograma pra mostrar o tabuleiro

        if (jogador == 'X') { // invocação das condições para a jogada do jogador
            jogadaJogador(); //subprograma pra fazer o playe 1 jogar
            if (verificarVitoria('X')) { // subprograma pra verificar a vitória do player 1
            	mostrarTabuleiro(); // subprograma pra mostrar a jogada
                printf("\n\033[0;32mParabéns! Você venceu!\033[0m\n"); //se o jogador vencer, vai aparece letra pintada de verde
                vitoriasJogador++; //contabilizar o nº de vitórias do jogador
                fimDeJogo = 1; // para acabar o jogo 
            }
        } else {
            jogadaMaquina();
            if (verificarVitoria('O')) { // verificar vitória da máquina atraves do subprograma
                mostrarTabuleiro(); // subprograma para mostrar o tabuleiro
                printf("\n\033[1;31mA máquina venceu!\033[0m\n"); // se a máquina vencer, vai aparecer letra pintada de verde
                vitoriasMaquina++; // contabilizar as vitórias da máquina
                fimDeJogo = 1;
            }
        }

        if (!fimDeJogo && tabuleiroCheio()) { //linha para verificar se o jogo não acabou, entretanto, o tabuleiro está todo cheio para então dar o empate
        	system("cls");
            mostrarTabuleiro();
            printf("\n\033[0;33mEmpate!\n033[0m\n"); // se for empate, as letras vão aparecer amarelas
            fimDeJogo = 1;
        }

        jogador = (jogador == 'X') ? 'O' : 'X'; // Alternância, redefinindo a variável jogador. if (jogador == 'X') {jogador = 'O'; } else {jogador = 'X'; }
    } // essa linha é para dar simultaneiadade das jogadas 
    
    // utilização de operador ternário para substituir o if/else, estrutura: (condição)? valor_se_for_verdadeiro : valor_se_for_falso
    // ? funciona como se fosse um if e : como se fosse um else

    printf("\nDeseja jogar novamente? (s/n): ");
    char resp;
    scanf(" %c", &resp);
    limparBuffer();
    if (resp == 's' || resp == 'S') {
        jogarContraMaquina(); } // Recomeça
        
    if (resp == 'n' || resp == 'N') { 
    
    system("cls");
    }
}


// Jogada do jogador humano
void jogadaJogador() {
    int linha, coluna;
    do {
        printf("Sua jogada (linha e coluna de 1 a 3): ");
        scanf("%d, %d", &linha, &coluna);
        limparBuffer();
        linha--; coluna--; // decremento porque nas matrizes os valores começam no 0, ou seja, se jogar 1, na matriz, o valor será 0
        if (linha < 0 || linha > 2 || coluna < 0 || coluna > 2 || tabuleiro[linha][coluna] != ' ') { // condição para as matrizes e verificação se a posição está vazia ou não, estando vazia, pode jogar
            printf("\aJogada inválida! Tente novamente.\n");
            system("color 47");
            Sleep(400);
            system("color 07");
            
        } else {
            break;
        }
    } while (1); // repetir a jogada se o valor for inválido, se for válido para no break e a posição assume o X

    tabuleiro[linha][coluna] = 'X'; // obedecendo as condições, no espaço vazio será adicionado o X
}

// Jogada aleatória da máquina
void jogadaMaquina() {
    int linha, coluna;
    do {
        linha = rand() % 3; // vai gerar números aleatórios que podem ser grandes e o objetivo é pegar o resto da divisão por 3 do qual o valor final será sempre 0, 1 ou 2
        coluna = rand() % 3;
    } while (tabuleiro[linha][coluna] != ' ');

    printf("Máquina jogou na posição: %d, %d\n", linha + 1, coluna + 1); 
    tabuleiro[linha][coluna] = 'O';
}

// Limpar o buffer do teclado (scanf)
void limparBuffer() {
    int c;
    while ((c = getchar()) != '\n' && c != EOF);
} // O getchar() lê um caractere por vez do teclado (da fila buffer) até encontrar o \n (Enter) ou o fim do arquivo (EOF) removendo o 'lixo'(caracteres) do teclado

