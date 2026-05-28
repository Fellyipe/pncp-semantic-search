<h1 align="center">Sistema Inteligente de Análise de Licitações</h1>

<p align="center">
  Python | FastAPI | Ollama | PostgreSQL | Docker
</p>

<p align="center">
  <strong>Pipeline de dados e análise inteligente de licitações públicas com busca híbrida e IA.</strong>
</p>

> 🔒 **Nota de Portfólio:** Código privado devido a diretrizes de propriedade intelectual e viabilidade comercial. Este repositório documenta a arquitetura, decisões de engenharia e os desafios técnicos no desenvolvimento para fins de portfólio.

---

## 📝 Contexto

O Portal Nacional de Contratações Públicas (PNCP) centraliza dados de licitações públicas brasileiras, porém apresenta desafios importantes para a descoberta eficiente de oportunidades relevantes pelas empresas:

* 📦 Grande volume de registros
* 🎯 Baixa relevância em buscas textuais tradicionais
* 🌐 Múltiplos sistemas externos e endpoints de contratação
* ⚠️ Inconsistências operacionais e timeout na API pública
* 🤝 Dificuldade de matching entre empresas e licitações

Este projeto explora a construção de uma pipeline de ingestão, enriquecimento semântico e recuperação híbrida de informações para facilitar a descoberta de oportunidades relevantes em licitações públicas.

---

## 🏗️ Arquitetura atual do Sistema


```mermaid
graph TD
    A[API Pública do PNCP] -->|Collector ETL| B(Script Python)
    B -->|Ingestão Incremental| C[(PostgreSQL)]
    C -->|Full Text Search| F[tsvector]
    C -->|Embeddings| D[Sentence Transformers]
    C -->|Enriquecimento Semântico| I[LLM Local: Ollama + Qwen]
    I -->|Categorias, Tags e Descrição Semântica| J[(Tabela de Análises)]
    D -->|Busca Vetorial| E[(pgvector)]
    F -->|Busca Híbrida| G[FastAPI]
    E -->|Similaridade Semântica| G
    J -->|Contexto Inteligente| G
    G -->|Payload JSON| H[Cliente]
```

---

## 🛠️ Stack

- **Python**
- **FastAPI**
- **PostgreSQL**
- **SQLAlchemy**
- **Ollama**
- **Qwen 2.5**
- **pgvector** — busca vetorial
- **Sentence Transformers** — embeddings
- **Docker**

---

## ⚙️ Funcionalidades Atuais

- Coleta incremental de licitações do PNCP
- Enriquecimento de dados com LLM local
- Rotulação automática e classificação de licitações
- Pipeline de busca híbrida (PostgreSQL `tsvector` e embeddings)
- API REST para consulta, paginação e filtros
- Ordenação por similaridade semântica


---

## 🛡️ Desafios Técnicos

**1. Instabilidade da API do PNCP**
* **Problema:** Lentidão, timeouts e falhas em requisições consecutivas.
* **Solução:** Implementação de timeouts, retries controlados, delays entre chamadas e coleta incremental.

**2. Dados pouco estruturados**
* **Problema:** Muitas licitações possuem descrições vagas ou insuficientes, dificultando busca, categorização e matching semântico.
* **Solução:** Uso de LLM local para enriquecimento semântico, geração de descrições contextualizadas e classificação das contratações.

**3. Busca textual limitada**
* **Problema:** A busca por palavras-chave tem baixa precisão semântica. *Exemplo:* Buscas por `"alimentação"` retornando registros relacionados a transporte ou logística.
* **Solução:** Abordagem híbrida combinando PostgreSQL Full Text Search (`tsvector`) para termos exatos + busca vetorial com `pgvector` para contexto.

---

## 📸 Screenshots

### Análise da licitação

#### Requisição
<img src="https://github.com/user-attachments/assets/10d6b522-d91f-4df7-a595-368cd37685bc" alt="Swagger Request" width="1100">

#### Resposta
<img src="https://github.com/user-attachments/assets/9537bde4-b00f-4de2-befd-25308004225f" alt="Swagger Response" width="550">

### Busca Semântica

#### Requisição

<img src="https://github.com/user-attachments/assets/5c3583a1-defc-40ca-ab1d-c3cd2643767e" alt="Swagger Request" width="1100">

#### Resposta
<img src="https://github.com/user-attachments/assets/d3908c21-db18-4173-9377-8d8c55825a3b" alt="Swagger Response" width="550">

