# petro

Pipeline de geração automática de pares de perguntas e respostas (QA) para avaliação de sistemas RAG no domínio de geociências do petróleo.

## Visão Geral

O projeto gera quatro categorias de perguntas a partir de chunks de texto indexados em um banco vetorial (OpenSearch), utilizando modelos de embeddings e LLMs (GPT-4/4o). O objetivo é construir um benchmark para avaliar a capacidade de recuperação e geração de respostas em português.

## Estrutura

```
petro/
├── factuais.ipynb          # Perguntas factuais com respostas verbatim
├── compostas.ipynb         # Perguntas multi-hop (raciocínio em múltiplos passos)
├── parciais.ipynb          # Perguntas parcialmente respondíveis
├── nao-respondiveis.ipynb  # Perguntas sem resposta no corpus
└── prompts/
    ├── factual.txt         # Prompt para geração de QA extrativo
    ├── composeN.txt        # Prompt para geração de perguntas multi-hop
    └── prompt_regis.txt    # Prompt para resposta com citação de chunks
```

## Tipos de Perguntas

| Notebook | Tipo | Descrição |
|---|---|---|
| `factuais.ipynb` | Fatual | Pergunta com resposta literal retirada do chunk |
| `compostas.ipynb` | Composta (multi-hop) | Pergunta que exige encadeamento de 2+ chunks para ser respondida |
| `parciais.ipynb` | Parcial | Pergunta cujos documentos de suporte estão apenas parcialmente no corpus |
| `nao-respondiveis.ipynb` | Não respondível | Pergunta gerada a partir de documentos fora do corpus indexado |

## Tecnologias

- **Embeddings:** `intfloat/multilingual-e5-base`, `BAAI/bge-m3` (via `sentence-transformers`)
- **Busca vetorial:** OpenSearch (KNN)
- **LLM:** OpenAI GPT-4 / GPT-4o / GPT-4o-mini
- **Linguagem:** Python (Jupyter Notebooks)

## Configuração

Antes de executar os notebooks, configure as credenciais nos respectivos arquivos:

```python
# Conexão OpenSearch
client = OpenSearch(
    hosts=[{'host': 'SEU_HOST', 'port': PORTA}],
    http_auth=('USUARIO', 'SENHA'),
    ...
)

# Chave OpenAI
api_key = "SUA_CHAVE_OPENAI"
```

## Dependências

```bash
pip install openai opensearch-py torch sentence-transformers pandas tqdm scikit-learn
```

## Saídas

Cada notebook exporta um arquivo `.csv` com os pares QA gerados:

- `25_factuais.csv`
- `25_compostas_novo.csv`
- `25_parciais_gpt_simil.csv`
- `25_naorespondiveis_gpt.csv`
