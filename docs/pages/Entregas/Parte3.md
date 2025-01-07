# Compreendendo os Conceitos de CSP (Problemas de Satisfação de Restrições)

## Introdução

Nesta resenha, apresentarei uma análise crítica de todo o material explicado pelo professor Fabiano Araujo Soares, material esse escrito por ele, do semestre 2024.2. Este material trata dos assuntos de:

1. Representação Atômica vs. Fatorada;
2. Definindo Problemas de Satisfação de Condições;
3. Tipos de condições;
4. Consistência:
    - Nó;
    - Arco;
    - Trajeto
    - K;
    - Globais.
5.  Algoritmos
6. Estrutura de problemas 

O objetivo desta resenha é examinar os principais pontos e oferecer uma avaliação crítica sobre o tema na minha visão, uma visão de uma aluna no penúltimo período de Engenharia de Software.

## Representação Atômica vs. Fatorada
A distinção entre **representação atômica** e **representação fatorada** é fundamental no contexto dos **Problemas de Satisfação de Restrições** (CSPs, do inglês *Constraint Satisfaction Problems*), um dos principais campos de estudo da Inteligência Artificial. Essa diferença impacta diretamente a forma como os problemas são modelados e resolvidos. A representação fatorada é amplamente preferida em IA devido à sua escalabilidade e clareza, como argumentado por Russell e Norvig (2021).

### Representação Atômica
Na **representação atômica**, cada possibilidade ou configuração do problema é tratada como uma **entidade indivisível**. Ou seja, todas as possíveis soluções do problema são combinadas em uma única estrutura ou conjunto, sem considerar a decomposição em partes menores ou variáveis. 

#### Exemplo:
- No caso de um problema como o **Sudoku**, a representação atômica envolveria listar todas as **combinações possíveis de preenchimento** de uma grade 9x9, o que pode ser extremamente ineficiente em termos de armazenamento e processamento, especialmente para problemas de grande escala.

### Representação Fatorada
Por outro lado, a **representação fatorada** decompõe o problema em **variáveis** e **restrições**. Essa abordagem permite um **maior grau de flexibilidade** na modelagem do problema, promovendo soluções mais escaláveis e compreensíveis. Na representação fatorada, cada variável é associada a um **domínio de valores possíveis**, e as restrições que regem a relação entre essas variáveis são explicitamente definidas.

#### Exemplo:
- No **Sudoku**, a representação fatorada envolve tratar **cada célula da grade como uma variável** (por exemplo, as células na linha 1, coluna 1, 2, etc.), e as restrições que governam essas variáveis, como as **linhas, colunas e blocos** que não podem ter números repetidos. Dessa forma, o problema é dividido em variáveis com restrições, o que permite uma abordagem de solução mais eficiente, por meio de algoritmos de busca, inferência ou outras técnicas de resolução de CSPs.

### Vantagens da Representação Fatorada
- **Escalabilidade**: A representação fatorada é **muito mais escalável** do que a atômica. Com a decomposição em variáveis e restrições, a solução de problemas de maior escala, como o Sudoku 16x16 ou outros problemas complexos de CSPs, torna-se mais viável.
- **Clareza e Flexibilidade**: Permite uma modelagem clara e flexível, onde é possível facilmente adicionar ou modificar variáveis e restrições, sem precisar reconfigurar toda a representação.
- **Eficiência**: Facilita a aplicação de técnicas de **propagação de restrições** e **algoritmos de busca** eficientes, como o Backtracking, que são comuns em problemas de CSPs.

---
## Definindo Problemas de Satisfação de Condições (CSP)
**Problemas de Satisfação de Condições (CSPs, do inglês *Constraint Satisfaction Problems*)** envolvem a tarefa de determinar valores para um conjunto de variáveis, de forma que todas as restrições definidas sobre essas variáveis sejam satisfeitas. Este tipo de problema é fundamental em Inteligência Artificial e é amplamente utilizado para modelar e resolver uma grande variedade de desafios.

### Definição de CSP
Um **problema de satisfação de condições** consiste em determinar os **valores** adequados para as **variáveis** dentro de um dado **conjunto de restrições**. O objetivo é garantir que, ao final, todas as restrições impostas sobre as variáveis sejam **satisfeitas simultaneamente**. Este processo pode ser realizado por meio de algoritmos que buscam explorar o espaço das soluções possíveis e verificar se as condições são atendidas.

#### Exemplo Cotidiano: Cronograma Escolar
Um exemplo simples e cotidiano de um problema de satisfação de condições é a **organização de um cronograma escolar**. Nesse caso:
- **Variáveis**: São as disciplinas que precisam ser alocadas aos horários disponíveis (como Matemática, História, Química, etc.).
- **Domínios**: São os horários possíveis para alocar cada disciplina.
- **Restrições**: As restrições podem incluir: não alocar duas disciplinas ao mesmo tempo (evitar conflitos de horário), garantir que um professor não tenha sobrecarga de aulas em um único dia, e garantir que disciplinas relacionadas não sejam agendadas em horários próximos, entre outras.

Portanto, o problema consiste em atribuir horários às disciplinas (variáveis), respeitando as restrições, como a não sobreposição de horários (restrições de horário).

### Componentes de um CSP
Segundo **Dechter (2003)**, um **problema de satisfação de condições** (CSP) envolve três componentes essenciais:
1. **Conjunto de Variáveis**: 
   - As variáveis são os elementos sobre os quais se deseja encontrar valores. No exemplo do cronograma escolar, as variáveis seriam as disciplinas a serem alocadas.
2. **Domínios**: 
   - Cada variável tem um conjunto de valores possíveis, conhecido como **domínio**. No caso das disciplinas, o domínio seria os horários possíveis em que uma disciplina pode ser agendada.
3. **Restrições**: 
   - As restrições definem as condições que devem ser atendidas pelas variáveis. No exemplo do cronograma, as restrições podem incluir que duas disciplinas não podem ser alocadas para o mesmo horário e que um professor não pode lecionar em dois lugares ao mesmo tempo.

---
## Tipos de Restrições em Problemas de Satisfação de Condições (CSP)
As **restrições** desempenham um papel crucial na modelagem e resolução de **Problemas de Satisfação de Condições** (CSPs), determinando as condições que devem ser atendidas pelas variáveis do problema. Essas restrições podem ser classificadas em três tipos principais: **unárias**, **binárias** e **n-árias**. A seguir, explicamos cada um desses tipos e sua importância na resolução de CSPs.

### Restrições Unárias
As **restrições unárias** envolvem apenas **uma única variável** e são usadas para definir **condições** sobre essa variável específica. Elas impõem limites sobre os valores que uma variável pode assumir, baseando-se em uma característica dessa variável.

#### Exemplo:
- "Um aluno deve ter pelo menos 18 anos para se inscrever."
  - Neste caso, a restrição unária aplica-se à variável **idade do aluno**, limitando os valores possíveis para essa variável (só permitindo idades iguais ou superiores a 18).

### Restrições Binárias
As **restrições binárias** envolvem **dois pares de variáveis** e são aplicadas quando existe uma condição que relaciona diretamente duas variáveis. Esse tipo de restrição é frequentemente utilizado para definir relações entre dois elementos do problema.

#### Exemplo:
- "João não pode ter aula no mesmo horário que Maria."
  - Aqui, a restrição binária aplica-se às variáveis **horário de aula de João** e **horário de aula de Maria**, assegurando que essas duas variáveis não tenham o mesmo valor (ou seja, que João e Maria não tenham aula no mesmo horário).

### Restrições N-árias
As **restrições n-árias** envolvem **três ou mais variáveis**, aumentando significativamente a complexidade do problema. Essas restrições impõem condições sobre a interação entre múltiplas variáveis e são essenciais quando há dependências mais complexas entre diferentes partes do sistema.

#### Exemplo:
- "Três amigos, Ana, Beatriz e Carlos, não podem ir ao cinema ao mesmo tempo."
  - Essa restrição n-ária aplica-se às variáveis **horário de cinema de Ana**, **horário de cinema de Beatriz** e **horário de cinema de Carlos**, exigindo que pelo menos uma dessas variáveis tenha um valor diferente, ou seja, ao menos uma pessoa deve ir ao cinema em horário diferente das outras.

### Importância e Complexidade

A **complexidade** das restrições aumenta à medida que o número de variáveis envolvidas cresce. As **restrições unárias** são as mais simples, pois lidam com uma única variável. As **restrições binárias** introduzem uma interação entre duas variáveis, aumentando a complexidade do problema. Já as **restrições n-árias** são as mais complexas, pois lidam com múltiplas variáveis simultaneamente e podem tornar o processo de resolução mais desafiador.

---
## Consistência em Problemas de Satisfação de Condições (CSP)
O conceito de **consistência** em **Problemas de Satisfação de Condições (CSP)** refere-se à ideia de **reduzir o espaço de busca** para tornar a resolução de um problema mais eficiente. A consistência busca eliminar valores inviáveis para as variáveis e simplificar a verificação das restrições. Existem diferentes formas de consistência, cada uma com seu nível de complexidade e benefício computacional. A seguir, detalhamos os principais tipos de consistência utilizados em CSPs.

### Tipos de Consistência
1. **Consistência de Nó**: A **consistência de nó** é o tipo mais simples de consistência e trata-se da verificação de **restrições unárias** para cada variável individualmente. Para cada variável, verifica-se se os valores do seu **domínio** são consistentes com as restrições unárias associadas a ela. Se algum valor não for consistente, ele é removido do domínio da variável.
    - Exemplo: Se uma variável tem a restrição "deve ser maior que 5", e ela tem o valor 3 no seu domínio, o valor 3 será removido do domínio da variável.
2. **Consistência de Arco**: A **consistência de arco** é um conceito que envolve a análise de **pares de variáveis**. A ideia é garantir que para cada valor em uma variável, exista pelo menos um valor correspondente na outra variável, de forma que a restrição binária entre elas seja satisfeita. Este processo é exemplificado pelo algoritmo **AC-3**, que ajusta os domínios das variáveis até que todos os arcos sejam consistentes.
    - Exemplo: Se tivermos duas variáveis, X e Y, com a restrição "X < Y", a consistência de arco garante que para cada valor em X, existirá pelo menos um valor possível em Y que satisfaça essa restrição. Se X tem o valor 3, então Y deve ter valores maiores que 3.
3. **Consistência de Trajeto**: A **consistência de trajeto** é uma extensão da consistência de arco para **três variáveis** interligadas por restrições. Ela verifica se para qualquer subconjunto de três variáveis, existe uma forma de associar valores às variáveis de modo que todas as restrições binárias entre elas sejam satisfeitas. Isso reduz ainda mais o espaço de busca.
    - Exemplo: Se tivermos três variáveis, X, Y e Z, com restrições binárias entre cada par (X < Y, Y < Z), a consistência de trajeto verifica se é possível associar valores às variáveis de modo que todas as restrições sejam satisfeitas simultaneamente.
4. **Consistência K**: A **consistência K** generaliza o conceito de consistência de trajeto para **K variáveis**. Ela garante que, para qualquer subconjunto de até K variáveis, seja possível associar valores a essas variáveis de maneira que todas as restrições sejam satisfeitas. Este tipo de consistência é útil para problemas com maior complexidade e mais variáveis interligadas.
    - Exemplo: Em um problema com 4 variáveis, a consistência 3 verifica todos os subconjuntos possíveis de 3 variáveis, garantindo que as restrições entre qualquer grupo de 3 variáveis sejam atendidas.
5. **Consistência Global**: A **consistência global** verifica todas as restrições do problema, garantindo que o conjunto completo de restrições seja satisfeito. Isso envolve analisar todas as variáveis e restrições simultaneamente, considerando todas as interações possíveis entre elas. A consistência global é uma verificação de última instância, garantindo que o problema seja solucionável sem violar nenhuma das restrições.
    - Exemplo: Para um problema com várias variáveis e restrições complexas, a consistência global garante que, ao final, todas as restrições entre todas as variáveis sejam atendidas, verificando a solução de forma integral.

### Benefícios e Custos Computacionais
Cada nível de consistência oferece **benefícios e custos computacionais**. Ao aplicar consistência de nó ou consistência de arco, por exemplo, a busca pode ser simplificada e o número de possibilidades a ser explorado pode ser reduzido drasticamente. No entanto, a aplicação de consistência mais avançada, como a consistência de trajeto, K ou global, pode exigir mais recursos computacionais.
- **Consistência de nó** é simples e de baixo custo, mas pode ser insuficiente em problemas complexos.
- **Consistência de arco** e **consistência de trajeto** oferecem um equilíbrio entre eficiência e redução do espaço de busca.
- **Consistência global** é a mais abrangente, mas também a mais custosa em termos computacionais.

---
## Algoritmos em Problemas de Satisfação de Condições (CSP)
Os **algoritmos** são a base da resolução de **Problemas de Satisfação de Condições (CSP)**, sendo responsáveis por explorar o espaço de soluções e garantir que as restrições sejam satisfeitas. Eles podem ser classificados em duas categorias principais: **algoritmos de busca cega** e **algoritmos avançados**. Cada abordagem tem suas vantagens, dependendo da complexidade e da estrutura do problema.

### Algoritmos de Busca Cega
1. **Backtracking**: O **backtracking** é um algoritmo clássico utilizado para resolver CSPs. Ele tenta atribuir valores às variáveis uma de cada vez e volta atrás (backtrack) sempre que encontra uma combinação de valores que viola uma restrição. Este processo é repetido até que uma solução válida seja encontrada ou o espaço de busca seja completamente explorado.
- **Vantagens**: Simples e intuitivo, adequado para problemas pequenos ou quando há poucas restrições.
- **Desvantagens**: Pode ser ineficiente, especialmente em problemas grandes ou com muitas variáveis, pois explora muitas possibilidades, incluindo as inviáveis.
    - Exemplo: Em um problema de Sudoku, o backtracking tentaria preencher cada célula uma por uma, e sempre que encontrasse uma solução inconsistente (como uma repetição de número em uma linha ou coluna), ele voltaria e tentaria outra opção.

### Algoritmos Avançados
2. **Forward-Checking**: O **forward-checking** é um algoritmo de poda utilizado durante a busca. Ele mantém o controle sobre as **variáveis futuras** e, sempre que uma variável recebe um valor, o algoritmo **elimina** os valores inconsistentes dos domínios das variáveis não atribuídas. Isso ajuda a evitar que o algoritmo explore combinações inviáveis, melhorando a eficiência da busca.
- **Vantagens**: Reduz o espaço de busca e pode melhorar significativamente a performance, especialmente em problemas com muitas variáveis e restrições.
- **Desvantagens**: Pode ser computacionalmente caro, pois requer a atualização constante dos domínios das variáveis.
    - Exemplo: Em um problema de alocação de horários, se uma variável (exemplo: "horário da aula de Matemática") for atribuída a um valor específico, o forward-checking verificará se o valor atribuído cria conflitos com as variáveis relacionadas (exemplo: "horário da aula de Física"), removendo valores incompatíveis do domínio das variáveis futuras.

3. **Propagação de Restrições (Constraint Propagation)**: A **propagação de restrições** é uma técnica avançada que usa o conhecimento das restrições para reduzir de maneira significativa os domínios das variáveis. Durante a busca, as restrições são usadas para **propagar** informações entre as variáveis, restringindo os valores possíveis para cada uma delas. Técnicas como **arc-consistency** (como o algoritmo AC-3) são frequentemente usadas para implementar a propagação de restrições.
- **Vantagens**: Pode reduzir drasticamente o espaço de busca, tornando a busca mais eficiente.
- **Desvantagens**: O custo computacional pode ser elevado em problemas com muitas variáveis e restrições.
    - Exemplo: Em um problema de colorir grafos, a propagação de restrições pode restringir rapidamente as opções de cores disponíveis para um nó, com base nas cores já atribuídas a seus vizinhos.

### Eficiência dos Algoritmos
A **eficiência** dos algoritmos depende de uma **interação bem projetada** entre o algoritmo escolhido e a **estrutura do problema**. Segundo Dechter (2003), uma boa escolha do algoritmo e das técnicas de otimização pode melhorar significativamente o desempenho na resolução de CSPs, especialmente em problemas grandes e complexos.

---
## Estrutura de Problemas em CSP
A **estrutura de um Problema de Satisfação de Condições (CSP)** pode ser visualizada como um **grafo**, onde os **nós** representam as **variáveis** do problema e as **arestas** representam as **restrições** entre essas variáveis. Essa estrutura gráfica é uma ferramenta poderosa para entender e resolver CSPs de forma eficiente.

### Representação de um CSP como Grafo
- **Nós (Variáveis)**: Cada variável em um CSP é representada como um nó no grafo. O valor que essa variável pode assumir vem do seu **domínio**, que é o conjunto de possíveis valores para essa variável.
- **Arestas (Restrições)**: As arestas conectam os nós e representam as restrições que envolvem essas variáveis. Cada aresta impõe uma condição que deve ser satisfeita para que uma solução seja considerada válida.
    - Exemplo: Em um problema de colorir grafos, onde o objetivo é atribuir cores a um conjunto de nós de forma que nós adjacentes não tenham a mesma cor:
        - Os **nós** são as variáveis, representando os vértices do grafo.
        - As **arestas** representam as restrições de que dois vértices adjacentes não podem ter a mesma cor.

### Grafos Esparsos vs. Grafos Densos
- **Grafos Esparsos**: São grafos com poucas arestas em relação ao número de nós. Em problemas de CSPs, grafos esparsos são mais fáceis de resolver, pois existem menos restrições a serem satisfeitas. Isso reduz o espaço de busca, tornando a resolução mais eficiente.
- **Grafos Densos**: São grafos com muitas arestas, significando que há muitas interações ou restrições entre as variáveis. Em CSPs com grafos densos, a solução pode ser mais difícil de encontrar, pois o espaço de busca é muito maior e mais interdependente.

### Estratégias de Decomposição de Árvore
Em problemas de CSP mais complexos, estratégias como a **decomposição em árvore (tree decomposition)** podem ser usadas para simplificar a resolução. A decomposição em árvore consiste em dividir o problema em **subproblemas menores** e mais gerenciáveis, cada um representado por uma **subárvore**. Essa abordagem permite resolver CSPs complexos de forma mais eficiente, especialmente em problemas com grafos densos.

---
## Opinião Pessoal sobre a Exploração de CSPs
A exploração de **Problemas de Satisfação de Condições (CSPs)** é fascinante porque revela como problemas aparentemente complexos podem ser abordados e resolvidos por meio de **estratégias sistemáticas e estruturadas**. A capacidade de decompor um problema em variáveis, restrições e domínios, e buscar soluções eficientes para satisfazer todas as condições impostas, é uma das maiores forças dessa área da inteligência artificial.

Um dos aspectos que mais me impressiona nos CSPs é a **elegância** dos **algoritmos de propagação de restrições**, como o **AC-3**. Esses algoritmos mostram como a **eficiência** da ciência da computação pode ser aplicada para simplificar e reduzir o espaço de busca de forma significativa. A propagação de restrições permite que o algoritmo ajuste dinamicamente o domínio das variáveis enquanto busca soluções, tornando o processo mais rápido e eficaz.

- **AC-3**: O algoritmo de **arc-consistency** (AC-3) é um exemplo clássico de propagação de restrições que simplifica problemas de CSP, removendo valores inconsistente dos domínios de variáveis ao longo do processo de busca.

Essa aplicação de **poda inteligente** evita explorar soluções inviáveis e torna o processo de resolução de CSPs mais eficiente e escalável.

Por outro lado, embora os algoritmos de propagação de restrições sejam extremamente poderosos, é fundamental ter uma **compreensão profunda da estrutura do problema**. A eficácia de qualquer abordagem depende de como a **estrutura das variáveis** e **restrições** é analisada e compreendida. Se a estrutura do problema não for devidamente entendida, pode ser difícil ou até impossível aplicar soluções otimizadas.

- **Estrutura do Problema**: Ao abordar um CSP, é essencial considerar o grafo das variáveis, as interações entre elas e como essas interações afetam a resolução do problema. Com uma boa compreensão da estrutura do problema, podemos aplicar as estratégias mais apropriadas e alcançar soluções de forma mais eficiente.

---
## Discussão: Algoritmos de Satisfação de Restrições não discutidos em sala

Os algoritmos de satisfação de restrições desempenham um papel crucial na resolução de problemas que envolvem restrições lógicas ou matemáticas. Apesar de métodos como backtracking e AC-3 serem comumente abordados, outros algoritmos também oferecem soluções robustas e inovadoras para **Problemas de Satisfação de Restrições (CSPs)**. Nesta discussão, exploraremos algumas técnicas menos frequentes em sala de aula, mas altamente relevantes.

### 1. Algoritmos Baseados em Meta-Heurísticas
Algoritmos como o **Simulated Annealing (SA)** e **Algoritmos Genéticos (AG)** têm ganhado espaço no âmbito de CSPs. Esses métodos empregam estratégias inspiradas na natureza ou na física para encontrar soluções aproximadas para problemas complexos:
- **Simulated Annealing (SA)**: Baseado no processo de recozimento de metais, o SA é útil em problemas onde é necessário escapar de **mínimos locais**. Ele é amplamente utilizado em **aplicações como agendamento** e **otimização de redes**.
- **Algoritmos Genéticos (AG)**: Inspirados na evolução biológica, os AG operam por meio de **seleção, cruzamento e mutação** para evoluir soluções. Sua flexibilidade permite lidar com problemas onde a função objetivo não é trivial.

### 2. Algoritmos de Satisfação Booleana (SAT Solvers)
Problemas CSP frequentemente são reduzidos a instâncias de **SAT**, aproveitando a eficiência de solucionadores modernos como o **Z3** e o **MiniSAT**. Estes algoritmos exploram otimizações altamente especializadas para lidar com grandes espaços de soluções:
- **DPLL (Davis-Putnam-Logemann-Loveland)**: Um algoritmo recursivo que emprega técnicas de redução para resolver SAT.
- **CDCL (Conflict-Driven Clause Learning)**: Uma extensão do DPLL, que aprende novas restrições para evitar revisitar soluções inválidas.

### 3. Algoritmos Distribuídos
Problemas CSP também podem ser modelados e resolvidos em **ambientes distribuídos**, onde as variáveis e restrições estão espalhadas entre várias entidades. Um exemplo é o algoritmo **ABT (Asynchronous Backtracking)**, usado em **sistemas multiagentes**, onde agentes resolvem seus CSPs localmente e comunicam resultados a outros agentes.

### 4. Constraint Learning
Uma área emergente é a utilização de **aprendizado de máquina** para refinar ou gerar restrições. Algoritmos baseados em **aprendizagem por reforço** ou **redes neurais** ajudam a identificar padrões que podem ser transformados em **restrições ou heurísticas**, acelerando a resolução de CSPs dinâmicos.

### Opinião Pessoal
A integração de **meta-heurísticas** e **algoritmos clássicos de CSP** é um caminho promissor para resolver problemas cada vez mais complexos e dinâmicos. Particularmente, considero a combinação de **aprendizado de máquina** com **CSP** um campo empolgante e repleto de potencial. Embora a aplicação de soluções distribuídas seja desafiadora, seu impacto em **sistemas multiagentes** e **IoT (Internet das Coisas)** é inegável.

---
## Implementação de um Algoritmo CSP: Algoritmo Genético para Resolver o Sudoku
Este exemplo apresenta um algoritmo CSP baseado em Algoritmos Genéticos para resolver o Sudoku.

```python
import random
import numpy as np

# Parâmetros do Algoritmo Genético
POPULATION_SIZE = 500
MUTATION_RATE = 0.1
GENERATIONS = 1000

# Problema Sudoku
PUZZLE = [
    [5, 3, 0, 0, 7, 0, 0, 0, 0],
    [6, 0, 0, 1, 9, 5, 0, 0, 0],
    [0, 9, 8, 0, 0, 0, 0, 6, 0],
    [8, 0, 0, 0, 6, 0, 0, 0, 3],
    [4, 0, 0, 8, 0, 3, 0, 0, 1],
    [7, 0, 0, 0, 2, 0, 0, 0, 6],
    [0, 6, 0, 0, 0, 0, 2, 8, 0],
    [0, 0, 0, 4, 1, 9, 0, 0, 5],
    [0, 0, 0, 0, 8, 0, 0, 7, 9]
]

# Função para validar se um tabuleiro é válido

def is_valid(board):
    for row in board:
        if len(set(row)) != 9:
            return False
    for col in range(9):
        if len(set(board[row][col] for row in range(9))) != 9:
            return False
    for box_row in range(0, 9, 3):
        for box_col in range(0, 9, 3):
            box = [
                board[row][col]
                for row in range(box_row, box_row + 3)
                for col in range(box_col, box_col + 3)
            ]
            if len(set(box)) != 9:
                return False
    return True

# Função para criar uma solução inicial aleatória

def create_individual():
    individual = []
    for row in PUZZLE:
        new_row = row[:]
        for i in range(9):
            if new_row[i] == 0:
                new_row[i] = random.randint(1, 9)
        individual.append(new_row)
    return individual

# Função de avaliação

def fitness(individual):
    score = 0
    for row in individual:
        score += len(set(row))
    for col in range(9):
        score += len(set(individual[row][col] for row in range(9)))
    return score

# Função de cruzamento

def crossover(parent1, parent2):
    point = random.randint(0, 8)
    child = parent1[:point] + parent2[point:]
    return child

# Função de mutação

def mutate(individual):
    for row in individual:
        if random.random() < MUTATION_RATE:
            i, j = random.sample(range(9), 2)
            row[i], row[j] = row[j], row[i]

# Algoritmo Genético Principal

def genetic_algorithm():
    population = [create_individual() for _ in range(POPULATION_SIZE)]
    for generation in range(GENERATIONS):
        population = sorted(population, key=fitness, reverse=True)
        if fitness(population[0]) == 243:
            print(f"Solução encontrada na geração {generation}")
            return population[0]
        next_generation = population[:POPULATION_SIZE // 2]
        for i in range(POPULATION_SIZE // 2):
            parent1, parent2 = random.sample(next_generation, 2)
            child = crossover(parent1, parent2)
            mutate(child)
            next_generation.append(child)
        population = next_generation
    print("Nenhuma solução encontrada")
    return None

# Resolver Sudoku
solution = genetic_algorithm()
if solution:
    print(np.array(solution))
```
### Função `is_valid`
1. **Parâmetros**
   - **board**: Uma lista de listas que representa um tabuleiro de Sudoku (9x9).
2. **Passos da função**
   - A função verifica se um tabuleiro é válido em três dimensões:
     1. **Linhas**: Cada linha deve ter 9 números únicos (sem repetições).
     2. **Colunas**: Cada coluna deve ter 9 números únicos.
     3. **Caixas 3x3**: Cada uma das 9 subáreas 3x3 do Sudoku também deve ter 9 números únicos.
   - Se qualquer uma dessas verificações falhar, a função retorna `False`. Caso contrário, retorna `True`.

### Função `create_individual`
1. **Parâmetros**
   - Nenhum.
2. **Passos da função**
   - A função cria um "indivíduo" (uma solução possível) para o Sudoku, preenchendo as células vazias do tabuleiro (representadas por 0) com números aleatórios de 1 a 9.
   - O tabuleiro é gerado linha por linha, e para cada célula vazia, um número aleatório é atribuído.
3. **Retorno**
   - Retorna uma nova solução (indivíduo) gerada aleatoriamente para o Sudoku.

### Função `fitness`
1. **Parâmetros**
   - **individual**: Um tabuleiro de Sudoku (uma solução proposta para o problema).
2. **Passos da função**
   - A função avalia a qualidade de uma solução para o Sudoku.
     1. Para cada linha no tabuleiro, conta o número de valores únicos (quanto maior o número de valores únicos em uma linha, melhor).
     2. Para cada coluna, realiza a mesma verificação, somando o número de valores únicos.
   - A função retorna um "score" (pontuação) que representa a qualidade da solução (quanto maior o número de valores únicos nas linhas e colunas, melhor).

### Função `crossover`
1. **Parâmetros**
   - **parent1**: O primeiro "indivíduo" (solução) para o Sudoku.
   - **parent2**: O segundo "indivíduo" (solução) para o Sudoku.
2. **Passos da função**
   - A função realiza o **crossover** entre dois indivíduos, criando um "filho" a partir deles.
     1. Um ponto de corte aleatório é escolhido entre 0 e 8.
     2. O filho é formado pela primeira parte do `parent1` até o ponto de corte, e pela segunda parte do `parent2` após o ponto de corte.
3. **Retorno**
   - Retorna o "filho" gerado a partir do crossover entre os dois pais.

### Função `mutate`
1. **Parâmetros**
   - **individual**: Um "indivíduo" (solução) para o Sudoku.

2. **Passos da função**
   - A função realiza a **mutação** do indivíduo, que envolve a troca de duas células aleatórias dentro de uma linha.
     1. Para cada linha no tabuleiro, há uma chance de ocorrer uma mutação (definida pela taxa de mutação `MUTATION_RATE`).
     2. Se ocorrer uma mutação, duas células aleatórias na linha são trocadas de posição.

### Função `genetic_algorithm`
1. **Parâmetros**
   - Nenhum.
2. **Passos da função**
   - A função implementa o **Algoritmo Genético** para resolver o Sudoku.
     1. A população inicial é gerada aleatoriamente, com `POPULATION_SIZE` indivíduos.
     2. A cada geração:
        - A população é ordenada de acordo com o **fitness**.
        - Se a melhor solução atingir um fitness de 243 (indicando que a solução é válida e completa), a função retorna essa solução.
        - A próxima geração é gerada através de **crossover** e **mutação**.
     3. O processo continua por um número de gerações definido (`GENERATIONS`). 
3. **Retorno**
   - Se uma solução válida for encontrada, a função retorna a solução. Caso contrário, retorna `None`.

### Função Principal
1. **Descrição**
   - A função **genetic_algorithm** é chamada para resolver o Sudoku utilizando o algoritmo genético.
   - Se uma solução for encontrada, ela é impressa como uma matriz NumPy.
2. **Exemplo de Execução**
   ```python
   solution = genetic_algorithm()
   if solution:
       print(np.array(solution))




# Referências

- DECHTER, Rina. Constraint Processing. San Francisco: Morgan Kaufmann, 2003. Disponível em: [https://www.sciencedirect.com/book/9781558608900/constraint-processing](https://www.sciencedirect.com/book/9781558608900/constraint-processing). Acesso em: 7 jan. 2025.

- RUSSELL, Stuart; NORVIG, Peter. Artificial Intelligence: A Modern Approach. 4. ed. New York: Pearson, 2021. Disponível em: [https://aima.cs.berkeley.edu/](https://aima.cs.berkeley.edu/). Acesso em: 7 jan. 2025.

- HOOS, Holger H.; STÜtZLE, Thomas. Stochastic Local Search: Foundations and Applications. San Francisco: Morgan Kaufmann, 2005. Disponível em: [https://www.sciencedirect.com/book/9781558608726/stochastic-local-search](https://www.sciencedirect.com/book/9781558608726/stochastic-local-search). Acesso em: 7 jan. 2025.

BIERE, Armin; HEULE, Marijn J. H.; VAN MAAREN, Hans; WALSH, Toby (Eds.). Handbook of Satisfiability. 2. ed. Amsterdam: IOS Press, 2021. Disponível em: [https://www.iospress.com/catalog/books/handbook-of-satisfiability](https://www.iospress.com/catalog/books/handbook-of-satisfiability). Acesso em: 7 jan. 2025.