# Collections | Mensalidade Escolar | Segmentação & Teste A/B

Análise de inadimplência e performance de recuperação (Case isaac/Arco Educação).

**Python version** - 3.12
**Status** - Em Refinamento

## Reflexões importantes

* A legislação 9.870/99 estabelece pontos importantes para garantir a formação infantil. Do ponto de vista do negócio, a análise de crédito se torna importante com esta regulação. Neste case temos uma carteira corrompida, com dívidas longa, e cujos RFs já possuíam dívidas de anos anteriores e/ou um Score que não foi suficiente para prever o comportamento observado.
* Foram realizados Teste A/B, junto de um grupo de controle. Os resultados são concretos, porém não se deve perder de vista o impacto na relação com o RF. Um método que se utiliza de alavancas de recuperação mais ostensivas, as vezes gera um retorno um pouco maior, mas com um atrito que pode ser consideravelmente impactante.

## Visão Geral

O isaac é uma fintech educacional e atua no ecossistema da Arco Educação. Seu principal produto é a Garantia de Mensalidades: a empresa compra os recebíveis das escolas parceiras, garante repasses financeiros em datas pré-definidas e assumi o risco de inadimplência. Os responsáveis financeiros (RFs) pagam as mensalidades diretamente para o isaac.

A inadimplência afeta o fluxo de caixa, com consequências ao planejamento e novos investimentos, assim consome esforços administrativos e desvia o negócio escolar de sua fonte de especialidade: gestão pedagógica.

A empresa atua de acordo com a regulamentação do setor (Lei 9.870/99), que não permite a interrupção de estudos durante o ciclo letivo anual, porém os RF's são impedidos de rematrícula enquanto não houver regularização. Isaac apoia no planejamento financeiro, possibilita controle de fluxo de caixa e fornece produtos adicionais como o Seguro Familiar, em conjunto com a Porto, para garantir a tranquilidade financeira em caso de imprevistos (óbito, incapacidade temporária ou perda de vínculo CLT).

A área de Collections é responsável por aumentar a recuperação de valores em atraso com o menor custo possível, por exemplo, utilizando-se de análise de dados e gerenciamento da comunicação.

## Objetivos

Propor uma solução para o case baseado em:

- Bloco 1: Definir e calcular indicadores de inadimplência e risco da carteira, explicitando as escolhas e sua aplicação;
- Bloco 2: Segmentação de RFs, com critérios expostos, seguidos de análise de dados e insight de atuação;
- Bloco 3: Avaliar o resultado e a rentabilidade do teste de cobrança.

## Dataset

O arquivo xlsx fornecido contém duas abas com dados a serem trabalhados:

> aba `bd_blocos_1_e_2`: reflete a situação da carteira ao final de agosto, o oitavo mês de um ciclo letivo de 12 meses.

| *Atributo*               | *Descrição*                                                                                                                                                                            |
| :------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Aluno`                  | Identificador do aluno (ex.: Aluno 0001)                                                                                                                                                   |
| `Responsavel_Financeiro` | Identificador do RF pagador (ex.: RF0001). Um mesmo RF pode ter mais de um aluno matriculado                                                                                               |
| `Escola`                 | Escola vinculada (ex: Escola01)                                                                                                                                                            |
| `Valor_Mensalidade`      | Valor da mensalidade da escola (R$)                                                                                                                                                        |
| `Mensalidades_Totais`    | Total de mensalidades no contrato anual                                                                                                                                                    |
| `Mensalidades_Vencidas`  | Mensalidades com data de vencimento já passada                                                                                                                                            |
| `Mensalidades_em_Atraso` | Mensalidades vencidas e não pagas                                                                                                                                                         |
| `Dias_de_Atraso`         | Dias em atraso                                                                                                                                                                             |
| `Score_RF`               | Pontuação de risco do RF, de 0 a 1000. Quanto maior, maior a probabilidade de pagamento                                                                                                  |
| `Status_Matricula_RF`    | Situação do RF em relação ao ano anterior: Rematriculado s/ Pendências de anos anteriores, Rematriculado c/ Pendências de anos anteriores ou Matrícula Nova. Consistente para um RF |

> aba `bd_bloco_3`: dados referentes ao teste executado em setembro, referente à recuperação do valor atrasado em agosto.

| *Atributo*                      | *Descrição*                                                                              |
| :-------------------------------- | :------------------------------------------------------------------------------------------- |
| `RF`                            | Identificador do RF pagador (ex.: RF0001). Um mesmo RF pode ter mais de um aluno matriculado |
| `Mensalidades_em_Atraso_inicio` | Mensalidades em atraso no início do teste (agosto)                                          |
| `Mensalidades_Pagas`            | Mensalidades pagas durante o mês de agosto                                                  |
| `Grupo`                         | Grupo do RF no teste (Controle, Teste A ou Teste B)                                          |

## Estrutura do Repositório

├── Dataset/
│   └── bd_isaac.xlsx                    # Dataset principal
├── Docs/                                # Documentação e contexto do case
├── Notebooks/
│   └── 01_analise_exploratoria.ipynb    # Análise exploratória (código fonte limpo)
├── Reports/
│   ├── 01_analise_exploratoria_files/   # Gráficos e imagens geradas na conversão
│   └── 01_analise_exploratoria.md       # Relatório final renderizado com resultados
├── environment.yml                      # Arquivo de configuração do ambiente Conda
├── .gitignore                           # Arquivos ignorados
└── README.md                            # Este arquivo

##### Versionamento de Notebook

⚠️ Este projeto ainda está em desenvolvimento ativo. Os notebooks estão versionados sem outputs (uso de nbstripout). 

- Para visualizar a análise completa, com todos os gráficos, tabelas e testes de hipótese renderizados (sem precisar configurar o ambiente local), acesse o documento final na pasta **[Reports/01_analise_exploratoria.md](./Reports/01_analise_exploratoria.md)**.

- Para executar localmente, ver "Como Reproduzir".

## Como Reproduzir

1. **Clone o repositório:**

```bash
   git clone https://github.com/feliperodrigues09/Collections_MensalidadeEscolar.git
   cd Collections_MensalidadeEscolar
```

2. **Crie o ambiente a partir do arquivo YAML:**

```bash
   conda env create -f environment.yml
```

3. **Ative o ambiente:**

```bash
   conda activate Collections_MensalidadeEscolar
```

4. **Dados:** O dataset está na pasta correspondente `Dataset/`.
5. **Execute o pipeline:**
   Abra e execute o `Notebooks/01_analise_exploratoria.ipynb` na ordem.

## Fases

- [X] Fase 1: Business Understanding
- [X] Fase 2: Data Understanding
- [X] Fase 3: Data Preparation
- [X] Fase 4: Bloco 1 - Indicadores de Inadimplência e Performance
- [X] Fase 5: Bloco 2 - Segmentação da Base
- [X] Fase 6: Bloco 3 - Avaliação de Teste A/B de Cobrança
- [X] Fase 7: Conclusões

## Tecnologias

- Python, Pandas, NumPy, Matplotlib, Seaborn, Scipy, VS Code, Jupyter, nbstripout

## Autor

- Felipe Rodrigues
- LinkedIn: [[link](https://www.linkedin.com/in/felipe-rodrigues-a551864b/)]
- GitHub: [[link](https://github.com/feliperodrigues09)]

## Licença

- MIT License
- Como citar este projeto
