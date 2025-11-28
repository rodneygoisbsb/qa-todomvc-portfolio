<<<<<<< HEAD
# 🧪 QA Portfolio: TodoMVC (React)

![Status](https://img.shields.io/badge/Status-Concluído-success)
![Context](https://img.shields.io/badge/Contexto-Transição%20de%20Carreira-blue)
![Tools](https://img.shields.io/badge/Tools-Notion%20%7C%20Gherkin%20%7C%20Edge-orange)

Este repositório documenta o planejamento e execução de testes funcionais para o sistema **[TodoMVC (React Version)](https://todomvc.com/examples/react/dist/)**.

O projeto simula um ambiente real de QA, cobrindo desde a análise de requisitos até a execução e reporte de bugs, utilizando o **Notion** como plataforma central de gestão de qualidade.

---

## 🎯 Objetivo
Demonstrar a aplicação prática de conceitos de Qualidade de Software, incluindo:
- **Análise de Requisitos:** Identificação de fluxos críticos e regras de negócio.
- **BDD (Behavior Driven Development):** Escrita de cenários em Gherkin.
- **Test Management:** Uso do Notion como ferramenta de gestão de ciclo de vida dos testes (substituindo planilhas tradicionais).
- **Execução Manual:** Validação funcional no navegador Edge.

---

## 📚 Documentação do Projeto (Live Dashboard)

Toda a estratégia, planejamento e execução foram documentados em um **Dashboard Interativo no Notion**.
Isso permite uma visualização em tempo real do progresso, métricas e gestão de defeitos.

### 👉 [ACESSE O PLANO DE TESTES NO NOTION AQUI](https://cord-fin-e67.notion.site/Plano-de-Testes-TodoMVC-React-2b879cd581f18071bf8febf89df1b1f7?source=copy_link)

> **O que você encontrará no Dashboard:**
> * 🗺️ **Estratégia:** Definição de escopo (In/Out), ambiente e critérios de aceite.
> * 🗃️ **Repositório de Testes:** Casos de teste detalhados com BDD.
> * 🏃‍♂️ **Kanban de Execução:** Visualização ágil do status dos testes.
> * 🐛 **Gestão de Bugs:** Relatórios de defeitos com evidências e severidade.

---

## 🛠️ Escopo Funcional Validado

O projeto garantiu a qualidade das seguintes funcionalidades críticas (CRUD):

| Funcionalidade | Descrição |
| :--- | :--- |
| **Tela Inicial** | Validação de *Empty States*, foco automático e elementos de UI. |
| **Adicionar Itens** | Inserção de tarefas via teclado, validação de *trim* e *null values*. |
| **Filtros** | Lógica de visualização das abas *All*, *Active* e *Completed*. |
| **Gestão de Estado** | Conclusão unitária, reversão de status e conclusão em lote (*Toggle All*). |
| **Limpeza** | Exclusão de itens concluídos e persistência de dados pendentes. |

---

## 📝 Especificações Técnicas (Gherkin)

Os cenários foram escritos utilizando a sintaxe **Gherkin** para garantir clareza e facilitar a comunicação entre QA e Desenvolvedores.
Você pode consultar o arquivo fonte dos cenários aqui:

📄 **[Ver Especificações em Gherkin](./specs/cenarios-bdd.md)**

---

## 🚀 Próximos Passos (Roadmap)

A evolução deste projeto visa a automação dos cenários já mapeados:
- [ ] Automação Web E2E com **Cypress**.
- [ ] Integração com **GitHub Actions** (CI/CD).
- [ ] Testes de Regressão Visual.

---

## 👨‍💻 Sobre o Analista

**Rodney Góis**
*QA em transição de carreira, focado em organização, processos ágeis e Testes Funcionais.*
[LinkedIn](https://www.linkedin.com/in/rodney-gois/)
