# Consultor Tático de MMA (Sistema Especialista + Machine Learning)

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/pehagaph/InteligenciaArtificial_LinguagensFormais_Automatos/blob/main/sistema-especialista-mma/Sistema_Especialista_MMA_IA_LFA.ipynb)

Projeto desenvolvido para a disciplina **Inteligência Artificial e Linguagens Formais e Autômatos (IA/LFA)** — Centro Universitário UDF (Profa. Kadidja Valéria).

## Objetivo e Arquitetura Híbrida

O sistema auxilia treinadores na definição da estratégia inicial (*gameplan*) contra um adversário no MMA, integrando duas abordagens:

1. **Aprendizado Supervisionado (`DecisionTreeClassifier`):** recebe duas estatísticas médias do oponente (`golpes_significativos_min` e `quedas_por_luta`) a partir de um dataset simulado de 16 lutadores e classifica o estilo predominante em **`striker`** ou **`grappler`**.
2. **Sistema Especialista (Base de Regras + Motor de Inferência):** recebe a previsão do modelo como um fato e a cruza com um fato contextual da luta (`envergadura_maior`: `True` ou `False`), aplicando encadeamento para frente (*Forward Chaining*) para recomendar a tática e explicar o motivo.

## Base de Conhecimento (Regras SE... ENTÃO...)

* **R1:** `SE estilo_oponente = striker E envergadura_maior = True, ENTÃO manter a longa distância com jabs, chutes baixos (low kicks) e controle do centro do octógono.`
* **R2:** `SE estilo_oponente = striker E envergadura_maior = False, ENTÃO encurtar a distância com pressão no clinch na grade e buscar quedas para neutralizar a trocação.`
* **R3:** `SE estilo_oponente = grappler E envergadura_maior = True, ENTÃO circular lateralmente usando golpes retos longos e defesa de quedas (sprawl) para manter a luta em pé.`
* **R4:** `SE estilo_oponente = grappler E envergadura_maior = False, ENTÃO trabalhar contragolpes curtos com base baixa, foco em esgrima (underhooks) e jiu-jítsu defensivo.`

## Casos Testados na Validação

| Caso | Golpes/min | Quedas/luta | Envergadura Maior? | Estilo Previsto (ML) | Regra Acionada (SE) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Caso 1** | 5.6 | 0.4 | `False` | `striker` | **R2** (Encurtar / Clinch e quedas) |
| **Caso 2** | 2.2 | 4.0 | `True` | `grappler` | **R3** (Circular / Golpes retos e *sprawl*) |
| **Caso 3** | 6.1 | 0.3 | `True` | `striker` | **R1** (Longa distância / Jabs e *low kicks*) |

## Como Executar

* **Via Google Colab (recomendado):** clique no botão **Open in Colab** no topo desta página e selecione `Ambiente de execução > Executar tudo`.
* **Localmente:** abra o arquivo `Sistema_Especialista_MMA_IA_LFA.ipynb` no Jupyter Notebook ou VS Code com `pandas`, `matplotlib` e `scikit-learn` instalados.
