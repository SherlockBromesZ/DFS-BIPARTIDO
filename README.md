# Algoritmo de Verificação de Grafos Bipartidos

## Descrição

Este programa verifica se um grafo não direcionado é bipartido. Um grafo é bipartido se seus vértices podem ser divididos em dois conjuntos disjuntos, de tal forma que não existem arestas que conectem dois vértices do mesmo conjunto.

Para verificar se um grafo é bipartido, o programa utiliza uma busca em profundidade (DFS, *Depth-First Search*), tentando colorir o grafo com duas cores. Se dois vértices adjacentes são coloridos com a mesma cor, então o grafo não é bipartido.

## Como o Algoritmo Funciona?

1. **Coloração com DFS:** O algoritmo tenta colorir o grafo usando duas cores (representadas por 0 e 1). Começando de um vértice não colorido, ele colore o vértice com uma cor e tenta colorir todos os seus vizinhos com a cor oposta, recursivamente.

2. **Verificação de Conflitos:** Se um vértice adjacente já foi colorido com a mesma cor, então o algoritmo determina que o grafo não é bipartido.

3. **Componentes Conexas:** O algoritmo verifica cada componente conexa do grafo. Se qualquer componente conexa não pode ser bipartida, então o grafo inteiro não é bipartido.

## Estrutura do Programa

### Cabeçalhos e Namespace

```cpp
#include<bits/stdc++.h>
using namespace std;
```

- Inclui todos os headers padrões da biblioteca do C++.
- Utiliza o namespace `std` para simplificar a escrita do código.

### Função `dfsBipartido`

```cpp
bool dfsBipartido(int inicio, vector<vector<int>>& grafo, vector<int> &cor, int corAtual) {
    cor[inicio] = corAtual;
    
    for (int vizinho : grafo[inicio]) {
        if (cor[vizinho] == -1) {
            if (!dfsBipartido(vizinho, grafo, cor, 1 - corAtual)) return false;
        } else if (cor[vizinho] == cor[inicio]) {
            return false;
        }
    }
    return true;
}
```

- **Parâmetros:**
  - `inicio`: O vértice atual no DFS.
  - `grafo`: A lista de adjacências representando o grafo.
  - `cor`: Vetor onde `cor[i]` é a cor do vértice `i`.
  - `corAtual`: A cor atual para tentar colorir o vértice `inicio`.

- **Funcionamento:**
  - Colorir o vértice `inicio` com `corAtual`.
  - Para cada vizinho do vértice `inicio`, tenta colorir recursivamente com a cor oposta (`1 - corAtual`).
  - Se encontrar um conflito de cores, retorna `false`.

### Função `main`

```cpp
int main() {
    int num_nos, num_arestas;
    cin >> num_nos >> num_arestas;
    
    vector<vector<int>> grafo(num_nos);
    for (int i = 0; i < num_arestas; i++) {
        int u, v;
        cin >> u >> v;
        grafo[u].push_back(v);
        grafo[v].push_back(u);
    }
    
    vector<int> cor(num_nos, -1);
    
    bool ehBipartido = true;
    for (int i = 0; i < num_nos; i++) {
        if (cor[i] == -1) {
            if (!dfsBipartido(i, grafo, cor, 0)) {
                ehBipartido = false;
                break;
            }
        }
    }
    
    if (ehBipartido) {
        cout << "O grafo é bipartido." << endl;
    } else {
        cout << "O grafo não é bipartido." << endl;
    }
    
    return 0;
}
```

- **Entrada:**
  - Lê o número de vértices (`num_nos`) e de arestas (`num_arestas`).
  - Constrói o grafo como uma lista de adjacências.

- **Processamento:**
  - Inicializa um vetor `cor` para manter a cor de cada vértice, começando com todos como `-1` (não coloridos).
  - Verifica cada componente conexa do grafo.
  - Utiliza a função `dfsBipartido` para tentar colorir cada componente.

- **Saída:**
  - Imprime se o grafo é bipartido ou não.

## Compilação e Execução

Para compilar e executar este programa, use o seguinte comando no terminal:

```bash
g++ -o programa programa.cpp
./programa
```

**Entrada de exemplo:**

```plaintext
4 4
0 1
1 2
2 3
3 0
```

**Saída esperada:**

```plaintext
O grafo não é bipartido.
```

## Conclusão

Este programa é uma implementação direta e eficiente para verificar se um grafo é bipartido usando DFS. É adequado para grafos representados como lista de adjacências e pode ser facilmente adaptado para outras representações de grafos.
