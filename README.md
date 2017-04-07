# Teoria dos grafos: ALGORITMO DE DIJKSTRA

Nome: Lucas Ribeiro Claudino RA: C74583-9
Nome: Daniel Ferreira Veloso RA: C740420 
Nome: Emmanuel Pereira Chaves RA: C7342A6


O algoritmo de Dijkstra é um dos mais conhecidos utilizado para calcular o caminho de custo mínimo entre vértices de um grafo.
Este algoritmo é muito útil para minimizar custos em várias áreas como por exemplo, na implementação de redes (por exemplos redes OSPF), ou no conhecido sistema GPS.
Num dado vértice (nó) no grafo, o algoritmo localiza o caminho com a menor custo (isto é, o caminho mais curto) entre esse vértice e todos os outros vértices, recorrendo ao peso/custo da aresta. Este sistema, pode também ser usado para encontrar custos de caminhos mais curtos a partir de um único vértice para um vértice de destino parando o algoritmo uma vez que o caminho mais curto para o vértice destino tiver sido determinado.
Exemplo em C++:

//Utilizando o algoritmo de Dijkstra para verificar a melhor rota entre uma parada e outra de um percurso de avião

#include <iostream>
#include <list>
#include <queue>
#define INFINITO 10000000

using namespace std;

class Grafo
{
private:
	int V; // número de vértices (paradas que o avião irar fazer).

	// ponteiro para um array contendo as listas de adjacências
	list<pair<int, int> > * adj;

public:

	// construtor
	Grafo(int V)
	{
		this->V = V; // atribui o número de vértices

		/*
		cria as listas onde cada lista é uma lista de pairs
		onde cada pair é formado pelo vértice destino e o custo
		*/
		adj = new list<pair<int, int> >[V];
	}

	// adiciona uma aresta ao grafo de v1(parada inicial) à v2 (parada final)
	void addAresta(int v1, int v2, int custo)
	{
		adj[v1].push_back(make_pair(v2, custo));
	}

	// algoritmo de Dijkstra
	int dijkstra(int orig, int dest)
	{
		// vetor de distâncias
		int dist[V];

		/*
		vetor de visitados serve para caso o vértice já tenha sido
		expandido (visitado), não expandir mais
		*/
		int visitados[V];

		// fila de prioridades de pair (distancia, vértice)
		priority_queue < pair<int, int>,
			vector<pair<int, int> >, greater<pair<int, int> > > pq;

		// inicia o vetor de distâncias e visitados
		for (int i = 0; i < V; i++)
		{
			dist[i] = INFINITO;
			visitados[i] = false;
		}

		// a distância de orig para orig é 0
		dist[orig] = 0;

		// insere na fila
		pq.push(make_pair(dist[orig], orig));

		// loop do algoritmo
		while (!pq.empty())
		{
			pair<int, int> p = pq.top(); // extrai o pair do topo
			int u = p.second; // obtém o vértice do pair
			pq.pop(); // remove da fila

			// verifica se o vértice não foi expandido
			if (visitados[u] == false)
			{
				// marca como visitado
				visitados[u] = true;

				list<pair<int, int> >::iterator it;

				// percorre os vértices "v" adjacentes de "u"
				for (it = adj[u].begin(); it != adj[u].end(); it++)
				{
					// obtém o vértice adjacente e o custo da aresta
					int v = it->first;
					int custo_aresta = it->second;

					// relaxamento (u, v)
					if (dist[v] > (dist[u] + custo_aresta))
					{
						// atualiza a distância de "v" e insere na fila
						dist[v] = dist[u] + custo_aresta;
						pq.push(make_pair(dist[v], v));
					}
				}
			}
		}

		// retorna a distância mínima até o destino
		return dist[dest];
	}
};

int main(int argc, char *argv[])
{
	Grafo g(5);

	g.addAresta(0, 1, 4);
	g.addAresta(0, 2, 2);
	g.addAresta(0, 3, 5);
	g.addAresta(1, 4, 1);
	g.addAresta(2, 1, 1);
	g.addAresta(2, 3, 2);
	g.addAresta(2, 4, 1);
	g.addAresta(3, 4, 1);

	cout << g.dijkstra(0, 4) << endl;

	return 0;
}















BIBLIOGRAFIAS:

http://www.inf.ufsc.br/grafos/temas/custo-minimo/dijkstra.html  acesso em 04/04/2017 as 18:10.

http://homepages.dcc.ufmg.br/~rainerpc/cursos/grafos/aulas/a05.pdf  acesso em 02/04/2017 as 15:41.

http://www.dainf.ct.utfpr.edu.br/petcoce/wp-content/uploads/2011/06/DiscreteMathFloydWarshall.pdf  acesso em 28/03/2017 as 07:19.

http://www.professeurs.polymtl.ca/michel.gagnon/Disciplinas/Bac/Grafos/CaminhoMin/caminho_min.html  acesso em 03/04/2017 as 17:34.

http://repositorio.uportu.pt:8080/bitstream/11328/568/2/TMMAT%20107.pdf  acesso em 01/04/2017 as 22:25.
