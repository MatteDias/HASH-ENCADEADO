# HASH-ENCADEADO
import java.util.Random;

public class TabelaHash {

    // Nó da lista encadeada
    static class No {
        int valor;
        No proximo;

        No(int valor) {
            this.valor = valor;
            this.proximo = null;
        }
    }

    public static void main(String[] args) {
        int tamanho = 100;
        int quantidade = 1000;
        int limite = 100001; // nextInt(100001) gera de 0 a 100000

        Random random = new Random(1090);
        No[] tabela = new No[tamanho];

        // Os três últimos valores inseridos em listas encadeadas (colisões)
        int ultimo1 = -1; // mais recente
        int ultimo2 = -1;
        int ultimo3 = -1;

        // Inserção dos 1000 elementos
        for (int i = 0; i < quantidade; i++) {
            int valor = random.nextInt(limite);
            int posicao = valor % tamanho;
            No novo = new No(valor);

            if (tabela[posicao] == null) {
                // Posição livre: insere direto
                tabela[posicao] = novo;
            } else {
                // Colisão: percorre até o fim da lista e encadeia
                No atual = tabela[posicao];
                while (atual.proximo != null) {
                    atual = atual.proximo;
                }
                atual.proximo = novo;

                // Guarda os três últimos valores inseridos em lista encadeada
                ultimo3 = ultimo2;
                ultimo2 = ultimo1;
                ultimo1 = valor;
            }
        }

        // Análise da tabela
        int maiorTamanho = 0;
        int posicaoMaior = -1;
        int listasEncadeadas = 0; // posições com 2 ou mais elementos
        int posicoesVazias = 0;

        for (int i = 0; i < tamanho; i++) {
            if (tabela[i] == null) {
                posicoesVazias++;
            } else {
                int contador = 0;
                No atual = tabela[i];
                while (atual != null) {
                    contador++;
                    atual = atual.proximo;
                }

                if (contador > 1) {
                    listasEncadeadas++;
                }
                if (contador > maiorTamanho) {
                    maiorTamanho = contador;
                    posicaoMaior = i;
                }
            }
        }

        // Impressão da maior lista encadeada
        System.out.println("Maior lista encadeada (posicao " + posicaoMaior
                + ", " + maiorTamanho + " elementos):");
        No atual = tabela[posicaoMaior];
        while (atual != null) {
            System.out.print(atual.valor);
            if (atual.proximo != null) {
                System.out.print(" -> ");
            }
            atual = atual.proximo;
        }
        System.out.println();
        System.out.println();

        System.out.println("Quantidade de listas encadeadas geradas: " + listasEncadeadas);
        System.out.println("Quantidade de posicoes vazias: " + posicoesVazias);
        System.out.println();
        System.out.println("Tres ultimos valores inseridos em listas encadeadas:");
        System.out.println("Ultimo:        " + ultimo1);
        System.out.println("Penultimo:     " + ultimo2);
        System.out.println("Antepenultimo: " + ultimo3);
    }
}