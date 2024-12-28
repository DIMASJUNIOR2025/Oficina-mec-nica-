#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_PRODUTOS 100
#define MAX_VENDAS 100

typedef struct {
    int id;
    char nome[50];
    int quantidade;
    float preco;
} Produto;

typedef struct {
    int idProduto;
    int quantidadeVendida;
    float valorTotal;
} Venda;

Produto estoque[MAX_PRODUTOS];
Venda vendas[MAX_VENDAS];
int totalProdutos = 0;
int totalVendas = 0;

void adicionarProduto() {
    if (totalProdutos >= MAX_PRODUTOS) {
        printf("Estoque cheio!\n");
        return;
    }
    
    Produto p;
    p.id = totalProdutos + 1;
    printf("Digite o nome do produto: ");
    scanf("%s", p.nome);
    printf("Digite a quantidade: ");
    scanf("%d", &p.quantidade);
    printf("Digite o preço: ");
    scanf("%f", &p.preco);
    
    estoque[totalProdutos] = p;
    totalProdutos++;
    printf("Produto adicionado com sucesso!\n");
}

void listarProdutos() {
    if (totalProdutos == 0) {
        printf("Estoque vazio!\n");
        return;
    }
    
    for (int i = 0; i < totalProdutos; i++) {
        printf("ID: %d, Nome: %s, Quantidade: %d, Preço: %.2f\n", 
            estoque[i].id, estoque[i].nome, estoque[i].quantidade, estoque[i].preco);
    }
}

void registrarVenda() {
    if (totalProdutos == 0) {
        printf("Estoque vazio! Não é possível registrar vendas.\n");
        return;
    }
    
    int idProduto, quantidade;
    printf("Digite o ID do produto: ");
    scanf("%d", &idProduto);
    
    if (idProduto <= 0 || idProduto > totalProdutos) {
        printf("Produto não encontrado!\n");
        return;
    }
    
    printf("Digite a quantidade vendida: ");
    scanf("%d", &quantidade);
    
    if (quantidade > estoque[idProduto - 1].quantidade) {
        printf("Quantidade insuficiente em estoque!\n");
        return;
    }
    
    estoque[idProduto - 1].quantidade -= quantidade;
    
    Venda v;
    v.idProduto = idProduto;
    v.quantidadeVendida = quantidade;
    v.valorTotal = quantidade * estoque[idProduto - 1].preco;
    vendas[totalVendas] = v;
    totalVendas++;
    
    printf("Venda registrada com sucesso! Total: %.2f\n", v.valorTotal);
}

void atualizarProduto() {
    if (totalProdutos == 0) {
        printf("Estoque vazio! Não é possível atualizar produtos.\n");
        return;
    }

    int idProduto;
    printf("Digite o ID do produto que deseja atualizar: ");
    scanf("%d", &idProduto);

    if (idProduto <= 0 || idProduto > totalProdutos) {
        printf("Produto não encontrado!\n");
        return;
    }

    Produto *p = &estoque[idProduto - 1];
    
    printf("Digite o novo nome do produto (atual: %s): ", p->nome);
    scanf("%s", p->nome);
    printf("Digite a nova quantidade (atual: %d): ", p->quantidade);
    scanf("%d", &p->quantidade);
    printf("Digite o novo preço (atual: %.2f): ", p->preco);
    scanf("%f", &p->preco);

    printf("Produto atualizado com sucesso!\n");
}

int main() {
    int opcao;
    
    do {
        printf("\n--- Oficina Mecânica ---\n");
        printf("1. Adicionar Produto\n");
        printf("2. Listar Produtos\n");
        printf("3. Registrar Venda\n");
        printf("4. Atualizar Produto\n");
        printf("5. Sair\n");
        printf("Escolha uma opção: ");
        scanf("%d", &opcao);
        
        switch (opcao) {
            case 1:
                adicionarProduto();
                break;
            case 2:
                listarProdutos();
                break;
            case 3:
                registrarVenda();
                break;
            case 4:
                atualizarProduto();
                break;
            case 5:
                printf("Saindo...\n");
                break;
            default:
                printf("Opção inválida! Tente novamente.\n");
        }
    } while (opcao != 5);
    
    return 0;
}
