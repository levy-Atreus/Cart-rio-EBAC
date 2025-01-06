# Cart-rio-EBAC
Projeto do curso de TI da EBAC


#include <stdio.h> // biblioteca de comunicação com usuário
#include <stdlib.h> // biblioteca de alocação de espaço de memória
#include <locale.h> // biblioteca de alocações de texto por região
#include <string.h> // biblioteca responsável por cuidar das strings
#include <dirent.h> // biblioteca para manipulação de diretórios

int registro() 
{
    char arquivo[40];
    char cpf[40];
    char nome[40];
    char sobrenome[40];
    char cargo[40];
    
    printf("Digite o CPF a ser cadastrado: ");
    scanf("%s", cpf);
    
    strcpy(arquivo, cpf); // responsável por copiar os valores das strings
    
    FILE *file; // crio o arquivo
    file = fopen(arquivo, "w"); // cria o arquivo 
    fprintf(file, "%s", cpf); // salva o valor da variável
    fclose(file); // fecha o arquivo
    
    file = fopen(arquivo, "a");
    fprintf(file, ",");
    fclose(file);
    
    printf("Digite o nome a ser cadastrado: ");
    scanf("%s", nome);
    
    file = fopen(arquivo, "a");
    fprintf(file, "%s", nome);
    fclose(file);
    
    file = fopen(arquivo, "a");
    fprintf(file, ",");
    fclose(file);
    
    printf("Digite o sobrenome a ser cadastrado: ");
    scanf("%s", sobrenome);
    
    file = fopen(arquivo, "a");
    fprintf(file, "%s", sobrenome);
    fclose(file);
    
    file = fopen(arquivo, "a");
    fprintf(file, ",");
    fclose(file);
    
    printf("Digite o cargo a ser cadastrado: ");
    scanf("%s", cargo);
    
    file = fopen(arquivo, "a");
    fprintf(file, "%s", cargo);
    fclose(file);
    getchar();
    getchar();
    return 0;
}

//início da consultant consulta()
int consulta()
{
    char cpf[40]; //Adicione o ponto e vírgula
    char conteudo[200]; //Adicione o ponto e vírgula

    printf("Digite o CPF a ser consultado: ");
    scanf("%s", cpf);

    FILE *file = fopen(cpf, "r"); //Abra o arquivo para leitura

    if (file == NULL) //Verifique se o arquivo foi aberto corretamente
    {
        printf("Não foi possível abrir o arquivo, não localizado");
    }

    while (fgets(conteudo, 200,file) != NULL) //Corrija o loop
    {
        printf("Essas são as informações do usuário: ");
        printf("%s", conteudo);
        printf("\n\n");
    }

    fclose(file); //Não esqueça de fechar o arquivo
    getchar(); //Aguarde o usuário pressionar Enter
    getchar();
    return 0;
}
//fim da consulta

int deletar() 
{
    char cpf[40];
    printf("Digite o CPF a ser deletado: ");
    scanf("%s", cpf);

    remove(cpf);

    FILE *file;
    file = fopen(cpf, "r");

    if (file == NULL) 
    {
        printf("Usuário deletado com sucesso!\n");
    }
     else 
    {
        printf("Usuário não se encontra no sistema!\n");
        getchar();
        getchar();
    }
    getchar();
    return 0;
}

int sair() 
{
    printf("Saindo do menu de usuário!\n\n");
    printf("Pressione Enter para continuar...");
    getchar();
    getchar();
}


//mudança 

void listarUsuarios() 
{
    struct dirent *entrada;
    DIR *diretorio = opendir("."); // Abre o diretório atual

    if (diretorio == NULL) 
    {
        printf("Erro ao abrir o diretório!");
        return;
    }
    printf("Lista de usuários:");
    while ((entrada = readdir(diretorio)) != NULL) 
    {
        // Ignora as entradas '.' e '..'
        if (entrada->d_name[0] != '.') 
        {
            FILE *file = fopen(entrada->d_name, "r");
            if (file != NULL) 
            {
                char conteudo[200];
                printf("Informações do usuário %s:", entrada->d_name);
                while (fgets(conteudo, sizeof(conteudo), file) != NULL) 
                {
                    printf("%s", conteudo);
                }
                fclose(file);
                printf("");
            }
        }
    }
    closedir(diretorio); // Fecha o diretório
}
    
//mudança 


int main() 
{
    int opcao = 0; // variáveis
    int laco = 1;

    for (laco = 1; laco == 1;) 
    {
        system("clear"); // Limpa a tela no Linux
        setlocale(LC_ALL, "portuguese"); // Definindo a linguagem

        printf("\t###  Cartório da EBAC  ###\n\n"); // menu do usuário
        printf("\tMenu de usuários\n\n");
        printf("Escolha uma opção:\n\n");
        printf("\t1 - Registrar nomes\n");
        printf("\t2 - Consultar nomes\n");
        printf("\t3 - Deletar nomes\n\n");
        printf("\t4 - Sair do sistema\n\n");
        printf("\t5 - listar todos os usuários\n\n");
        printf("Opção: "); // fim do menu

        scanf("%d", &opcao); // armazenando a escolha do usuário

        system("clear");
        
        switch(opcao) 
        {
            case 1:
                registro();
                break;
            case 2:
                consulta();
                break;
            case 3:
                deletar();
                break;
            case 4:
                printf("obrigado por usar o sistema volte sempre\n");
                sair();
                return 0;
                break;
            case 5:
                printf("lista de usuários\n");
                listarUsuarios();
            default:
                printf("Essa opção não está disponível!\n\n");
                printf("Pressione Enter para continuar...");
                getchar();
                getchar(); // fim da seleção
                break;
        }

    }
}   