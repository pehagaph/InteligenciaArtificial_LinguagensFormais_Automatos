# Discussão Ética e Declaração de Uso de Inteligência Artificial

**Projeto:** Consultor Tático de MMA (Sistema Especialista + Machine Learning)  
**Disciplina:** Inteligência Artificial e Linguagens Formais e Autômatos (IA/LFA) — Centro Universitário UDF (Profa. Kadidja Valéria)  
**Autor:** Paulo Henrique Santos Lopes

---

## 1. Descrição Ética do Projeto

O sistema tem como finalidade auxiliar treinadores de MMA na definição da estratégia inicial (*gameplan*) contra um adversário, recomendando a abordagem tática adequada e justificando o raciocínio. É utilizado por treinadores e analistas de desempenho, e suas saídas afetam diretamente os lutadores durante a preparação e o combate, incluindo riscos à sua integridade física caso adotem uma tática inadequada. No projeto, um modelo de aprendizado supervisionado (`DecisionTreeClassifier`) analisa estatísticas médias do oponente (`golpes_significativos_min` e `quedas_por_luta`) para prever seu estilo predominante (`striker` ou `grappler`). Já as recomendações táticas finais decorrem exclusivamente das regras simbólicas (`R1` a `R4`) definidas pela equipe, que cruzam o estilo previsto com o fato contextual de vantagem de envergadura do atleta (`envergadura_maior`).

---

## 2. Análise dos Aspectos Éticos

| Aspecto | Situação Concreta e Quem Pode Ser Afetado | Medida Adotada pelo Projeto |
| :--- | :--- | :--- |
| **Privacidade e dados** | O protótipo utiliza apenas 16 registros numéricos simulados (`golpes_significativos_min`, `quedas_por_luta` e `envergadura_maior`), sem nomes ou dados biométricos. Em uso real, históricos médicos ou vulnerabilidades táticas poderiam expor atletas. | Aplicar minimização de dados (usar apenas as três métricas necessárias) e manter os registros anonimizados. |
| **Viés e equidade** | A árvore de decisão aprendeu um corte rígido apenas no volume de golpes em pé (`> 4,0` golpes/min = `striker`). Lutadores completos (*all-rounders*) ou contra-golpeadores de baixo volume podem ser classificados de forma distorcida, prejudicando atletas que enfrentam oponentes híbridos. | Ampliar a base futura com defesa de quedas e tentativas de finalização, testar casos de borda e sinalizar métricas próximas ao limiar de decisão. |
| **Transparência** | Se o treinador receber apenas a ordem tática sem justificativa, não saberá se a recomendação veio do padrão estatístico do modelo ou da vantagem física informada. | Exibir obrigatoriamente o estilo previsto, o fato de envergadura considerado, o código da regra disparada (`R1`–`R4`) e o aviso de caráter didático. |
| **Supervisão e responsabilidade** | Uma saída inadequada (como sugerir trocação na longa distância contra um nocauteador por erro de classificação) pode comprometer o desempenho e a integridade física do atleta. | Manter supervisão humana obrigatória: a decisão final cabe exclusivamente ao treinador principal (*head coach*), que pode corrigir os dados ou descartar a sugestão. |
| **Impacto social** | Beneficia equipes com menos recursos que não possuem analistas de dados dedicados, mas pode induzir erros táticos e risco de lesões se tratada como decisão definitiva em um esporte de contato. | Posicionar a ferramenta estritamente como apoio preliminar à análise humana, deixando explícito que não substitui a comissão técnica. |

---

## 3. Dilema Ético, Fundamentação e Reflexão

* **Dilema ético:** O sistema deve emitir uma recomendação tática quando recebe estatísticas de um adversário fora dos intervalos observados no treinamento (como um estreante ou atleta atípico) ou bloquear a resposta?
  * **Alternativa 1 (Bloquear a saída):** Elimina o risco de erro por extrapolação estatística, mas torna o sistema inútil para analisar casos novos fora do padrão.
  * **Alternativa 2 (Emitir com alerta explícito — adotada):** Preserva a utilidade de apoio ao treinador ao entregar a recomendação acompanhada de um aviso automático indicando que os dados estão fora da faixa de treino, exigindo revisão crítica antes do uso.
* **Fundamentação teórica:** Conforme discutido por Francisco Pires de Lacerda Júnior (2026) na abordagem *"da proibição à governança"*, o uso ético da IA exige transparência sobre limitações, declaração explícita do papel da ferramenta e manutenção da supervisão humana responsável sobre os resultados.
* **Reflexão sobre governança:** A manutenção da emissão com alerta de limitação e revisão humana obrigatória equilibra utilidade prática e segurança. Na implementação, a função `orientar_estrategia_mma()` verifica automaticamente se os atributos informados estão fora dos limites de `X_treino` e anexa o aviso correspondente ao retorno.

---

## 4. Declaração de Uso de Sistemas de IA

### IA incorporada ao projeto
O projeto utiliza **aprendizado de máquina supervisionado (`DecisionTreeClassifier` da biblioteca `scikit-learn`) integrado a um Sistema Especialista baseado em regras de produção (`SE... ENTÃO...`)** para **classificar o estilo predominante de um adversário no MMA (`striker` ou `grappler`) e recomendar a estratégia inicial (*gameplan*)**. O sistema recebe **a média de golpes significativos por minuto, a média de quedas por luta e o fato contextual de vantagem de envergadura (`envergadura_maior`)** e gera **o estilo previsto, a regra acionada (`R1` a `R4`), a recomendação tática, a explicação dos fatos utilizados e um aviso de verificação de faixa**. O funcionamento foi verificado por meio de **divisão estratificada entre treino (75%) e teste (25%), cálculo de acurácia, matriz de confusão, inspeção visual da árvore de decisão e simulação de três novos casos inéditos**. As limitações identificadas incluem **o uso de um dataset simulado reduzido (16 registros fictícios), a separação linear simples que desconsidera atletas completos (*all-rounders*) e a ausência de variáveis como defesa de quedas e resistência física, exigindo revisão sempre que os dados fugirem da faixa de treino ou envolverem lutadores híbridos**. A revisão cabe a **Paulo Henrique Santos Lopes (autor do projeto) e, em contexto aplicado, ao treinador principal responsável pelo atleta**.

### IA utilizada na elaboração do trabalho
Utilizei **o modelo de linguagem Gemini (Google)** para **estruturar a arquitetura do código em Python, gerar o dataset simulado, organizar a documentação do repositório no GitHub e apoiar a sistematização da análise ética e dos tópicos de estudo**. Aproveitei **a estruturação do notebook no Google Colab, o motor de inferência com módulo de explicação e o mapeamento dos conceitos teóricos** após **executar e validar todas as células no Colab, corrigir manualmente o endereço e o nome do repositório no GitHub, testar os casos de borda e conferir a aderência ao roteiro da disciplina**. Assumo a responsabilidade pelas escolhas, verificações e conteúdo da entrega.
