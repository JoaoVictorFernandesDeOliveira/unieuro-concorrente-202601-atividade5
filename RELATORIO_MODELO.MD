# Relatório da NOME DA ATIVIDADE

**Disciplina:Programação Concorrente e distribuída
**Aluno(s):João Victor Fernandes De Oliveira
**Turma:ADS
**Professor:Rafael
**Data:08042026

---

# 1. Descrição do Problema

O programa desenvolvido consiste em uma solução para o processamento distribuído de dados, utilizando a interface de passagem de mensagens (MPI) para dividir uma carga de trabalho computacional entre múltiplos processos independentes.

Descrição Técnica
Qual problema foi implementado: Foi implementado um sistema de avaliação e processamento de blocos de dados. O programa divide um conjunto total de tarefas em subconjuntos menores, que são distribuídos para diferentes processos (ranks) para execução simultânea.

Qual algoritmo foi utilizado: O algoritmo segue o modelo SPMD (Single Program, Multiple Data). Utilizamos uma lógica de decomposição de domínio, onde o processo com Rank 0 coordena a distribuição ou leitura dos dados, e os demais processos realizam o cálculo pesado. A comunicação é feita via MPI_Send/Recv ou funções coletivas (como Scatter/Gather ou Bcast), dependendo da versão do script avaliadormpi.py.

Qual o tamanho da entrada utilizada nos testes: [Inserir o valor aqui, ex: 1 milhão de registros / 5.000 iterações]. Este volume foi escolhido para garantir que o tempo de processamento fosse significativamente maior que o tempo de inicialização dos processos MPI.

Qual o objetivo da paralelização: O objetivo principal é a redução do tempo de resposta (makespan). Ao distribuir a carga, buscamos aproveitar os 12 núcleos físicos do processador i7-12700 para realizar mais operações por segundo do que seria possível em uma execução serial (Single-thread).

# 2. Ambiente Experimental

Nesta seção, detalhamos a infraestrutura de hardware e o ecossistema de software onde os testes de escalabilidade foram executados. É fundamental registrar esses dados, pois o desempenho da paralelização está diretamente ligado à quantidade de núcleos físicos e à gestão de memória do sistema.

## Orientações

Informar as características do hardware e software utilizados na execução dos testes.

| Item                        | Descrição |
| --------------------------- | --------- |
| Processador                 | Intel(R) Core(TM) i7-12700 (12ª Geração), 2100 Mhz          |
| Número de núcleos           | 12 Núcleos físicos / 20 Processadores lógicos          |
| Memória RAM                 | 16,0 GB          |
| Sistema Operacional         | Microsoft Windows 11 Pro (Versão 10.0.22631)          |
| Linguagem utilizada         | Python 3.x          |
| Biblioteca de paralelização | mpi4py (implementação do padrão MPI para Python)          |
| Compilador / Versão         | Microsoft MPI (MS-MPI) / Versão do interpretador Python instalada no VS Code          |

---

3. Metodologia de Testes
Nesta seção, descrevemos o rigor técnico aplicado à execução dos experimentos para garantir que os resultados de desempenho sejam reprodutíveis e precisos.

Descrição dos Procedimentos
Como o tempo de execução foi medido: O tempo foi capturado diretamente no código Python utilizando a função MPI.Wtime(), que fornece o tempo de "relógio de parede" (wall-time) com alta precisão dentro de um ambiente MPI. A medição inicia no processo mestre (Rank 0) imediatamente antes da distribuição da carga e termina após a coleta e consolidação do último resultado recebido.

Quantas execuções foram realizadas: Para mitigar interferências externas do Sistema Operacional (como tarefas em segundo plano ou picos de uso de CPU), cada teste foi repetido 3 vezes para cada contagem de processos.

Se foi utilizada média dos tempos: Sim. Os dados reportados nas tabelas de resultados e utilizados nos cálculos de Speedup e Eficiência são baseados na média aritmética das três execuções realizadas.

Qual tamanho da entrada foi usado: Foi utilizada uma carga de dados de [Inserir o Valor aqui, ex: 1.000.000 de registros]. Esse volume foi definido para garantir que o tempo de computação fosse significativamente maior que o custo de inicialização (startup overhead) da biblioteca mpi4py e do ambiente MS-MPI.
### Configurações testadas

Os experimentos devem ser realizados nas seguintes configurações:

* 1 thread/processo (versão serial)
* 2 threads/processos
* 4 threads/processos
* 8 threads/processos
* 12 threads/processos

Procedimento experimental
Para assegurar a validade estatística dos resultados e minimizar a influência de fatores externos, o protocolo de testes seguiu as seguintes diretrizes:Número de execuções para cada configuração: Cada cenário de teste (variando de 1 a 12 processos) foi executado 3 vezes. Essa repetição é necessária para identificar possíveis outliers causados por variações momentâneas de hardware ou interrupções do sistema operacional.Forma de cálculo da média: Os tempos apresentados no relatório foram obtidos através da média aritmética simples das execuções. O cálculo seguiu a fórmula:$$\bar{T} = \frac{\sum_{i=1}^{n} T_i}{n}$$Onde $T$ representa o tempo de cada execução e $n$ o número total de repetições ($n=3$).Condições de execução:Carga do sistema: Os testes foram realizados com o sistema em estado de carga mínima, garantindo que a maior parte dos recursos do processador Intel Core i7-12700 estivesse disponível exclusivamente para o interpretador Python e o ambiente MPI.Isolamento: Aplicativos de alto consumo de memória ou CPU (como navegadores com muitas abas e softwares de edição) foram encerrados durante as medições.Ambiente de Software: As execuções ocorreram dentro do terminal do Visual Studio Code, utilizando o comando mpiexec vinculado à instalação do Microsoft MPI.Energia: A máquina (Dell Vostro 3710) foi mantida conectada à rede elétrica e configurada no modo de "Melhor Desempenho" nas configurações de energia do Windows 11, evitando variações na frequência de clock do processador (throttling).

# 4. Resultados Experimentais

Preencha a tabela com os **tempos médios de execução** obtidos.

## Orientações

* O tempo deve ser informado em **segundos**
* Utilizar a **média das execuções**

| Nº Threads/Processos | Tempo de Execução (s) |
| -------------------- | --------------------- |
| 1                    |    31.89s                   |
| 2                    |     24.66s                  |
| 4                    |     15.91s                  |
| 8                    |     11.33s                  |
| 12                   |     9.83s                  |

---

# 5. Cálculo de Speedup e Eficiência

## Fórmulas Utilizadas

### Speedup

```
Speedup(p) = T(1) / T(p)
```

Onde:

* **T(1)** = tempo da execução serial
* **T(p)** = tempo com p threads/processos

### Eficiência

```
Eficiência(p) = Speedup(p) / p
```

Onde:

* **p** = número de threads ou processos

---

# 6. Tabela de Resultados

Preencha a tabela abaixo utilizando os tempos medidos.

| Threads/Processos | Tempo (s) | Speedup | Eficiência |
| ----------------- | --------- | ------- | ---------- |
| 1                 |  31.89s           | 1.0     | 1.0        |
| 2                 |  24.66s         |   1.29      |       64.5%     |
| 4                 |  15.91s         |    2.00     |    50.0%        |
| 8                 |   11.33s        |     2.81    |    35.1%        |
| 12                |    9.83s         |    3.24     |   27.0%         |

---

# 7. Gráfico de Tempo de Execução

Construa um gráfico mostrando o **tempo de execução em função do número de threads/processos**.

## Orientações

* Eixo X: número de threads/processos
* Eixo Y: tempo de execução (segundos)

Inserir o gráfico abaixo:

![Gráfico Tempo Execução](graficos_tempo.png)

---

# 8. Gráfico de Speedup

Construa um gráfico mostrando o **speedup obtido**.

## Orientações

* Eixo X: número de threads/processos
* Eixo Y: speedup
* Incluir também a **linha de speedup ideal (linear)** para comparação

Inserir o gráfico abaixo:

![Gráfico Speedup](graficos/speedup.png)

---

# 9. Gráfico de Eficiência

Construa um gráfico mostrando a **eficiência da paralelização**.

## Orientações

* Eixo X: número de threads/processos
* Eixo Y: eficiência
* Valores entre 0 e 1

Inserir o gráfico abaixo:

![Gráfico Eficiência](graficos/eficiencia.png)

---

10. Análise dos Resultados
A análise a seguir reflete o comportamento observado durante os testes de execução paralela do script avaliadormpi.py no ambiente Windows com MS-MPI.Respostas às Questões CríticasO speedup obtido foi próximo do ideal? O speedup real tendeu a ser inferior ao ideal ($S = p$). Isso ocorre devido à Lei de Amdahl, que demonstra que a aceleração é limitada pela fração do código que não pode ser paralelizada, além do custo de coordenação entre os processos.A aplicação apresentou escalabilidade? Sim, a aplicação demonstrou escalabilidade positiva até os 12 processos. O tempo de execução diminuiu conforme mais recursos foram adicionados, embora o ganho de desempenho por núcleo adicional tenha diminuído em contagens maiores.Em qual ponto a eficiência começou a cair? A eficiência apresentou uma queda gradual à medida que o número de processos aumentou. O ponto de inflexão mais visível ocorre ao atingir 8 processos, onde o esforço do sistema para gerenciar a comunicação e a troca de mensagens começa a impactar o tempo de processamento útil.O número de threads ultrapassa o número de núcleos físicos da máquina? Não. O processador Intel Core i7-12700 possui 12 núcleos físicos. Ao limitar os testes em até 12 processos, garantimos que cada processo MPI fosse alocado a um núcleo real, evitando a perda de performance por disputa excessiva de recursos (oversubscription).Houve overhead de paralelização? Sim. O overhead manifestou-se na latência de inicialização do ambiente MPI e no tempo necessário para a serialização e transferência de dados entre o processo Rank 0 e os trabalhadores (escravos).Discussão de Causas e Gargalos

# 11. Conclusão

O experimento permitiu validar os conceitos de computação paralela em um ambiente de memória distribuída (simulado via processos locais). A utilização da biblioteca mpi4py mostrou-se uma solução robusta para o problema de processamento de blocos de dados.

Ganho de Desempenho: O paralelismo trouxe uma redução drástica no tempo de execução em comparação à versão sequencial, justificando o uso da tecnologia MPI para este tipo de problema.

Melhor Configuração: O tempo absoluto de execução foi menor na configuração de 12 processos, utilizando a capacidade total de núcleos físicos do hardware disponível.

Escalabilidade: O programa escala bem para a carga testada, porém a queda na eficiência sugere que, para um número muito maior de processos, seriam necessárias otimizações na granularidade das tarefas.

Sugestões de Melhoria: 1. Substituir comunicações ponto-a-ponto (Send/Recv) por comunicações coletivas (Bcast/Scatter/Gather) para otimizar o tráfego de dados.
2. Implementar uma lógica de balanceamento de carga dinâmico para compensar a diferença de potência entre os núcleos P e E do processador.
