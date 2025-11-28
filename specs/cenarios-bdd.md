## 🧪 Cenários de Teste — TodoMVC (Gherkin)

Abaixo estão todos os cenários escritos em Gherkin, organizados por funcionalidade.

---

### 📌 01 - Tela Inicial

```gherkin
Feature: Tela Inicial

Scenario: Carregamento inicial da aplicação
  Given que acesso a URL do projeto TodoMVC
  Then devo ver o título "todos" no topo da página
  And devo visualizar um campo de entrada de texto vazio
  And o campo de entrada deve conter o placeholder "What needs to be done?"

Scenario: Foco automático no input
  Given que a página acabou de ser carregada
  When verifico onde está o cursor do mouse
  Then o campo de entrada de tarefas deve estar focado

Scenario: Ocultação de elementos quando não existem tarefas
  Given que não possuo nenhuma tarefa cadastrada
  Then a lista de tarefas deve estar invisível
  And a barra de rodapé não deve ser exibida
```

### 📌 Adicionar Itens

```gherkin
Feature: Adicionar Itens

Scenario: Adicionar tarefa com sucesso
  Given que estou na tela inicial com foco no input
  When digito "Estudar Gherkin" e pressiono Enter
  Then o item "Estudar Gherkin" deve aparecer como último elemento da lista
  And o campo de input deve voltar a ficar vazio
  And um checkbox não marcado deve ser exibido ao lado do item

Scenario: Tentativa de adicionar tarefa vazia
  Given que o campo de input está vazio
  When pressiono Enter
  Then nenhuma tarefa deve ser adicionada
  And o rodapé deve continuar oculto

Scenario: Adicionar tarefa com espaços extras
  Given que digito "   Fazer Café   " no input
  When pressiono Enter
  Then o item deve ser salvo como "Fazer Café"

Scenario: Primeira inserção habilita controles
  Given que não tenho tarefas registradas
  When adiciono a tarefa "Primeira Tarefa"
  Then a barra de rodapé deve aparecer
  And a opção "Mark all as complete" deve ser exibida
```
# Filtragem: Todos

```gherkin
Feature: Filtragem de Tarefas - Todos

Scenario: Estado inicial deve exibir todas as tarefas
  Given que acessei a aplicação TodoMVC
  Then a lista deve exibir todos os itens pendentes e concluídos
  And o filtro "Todos" deve estar selecionado

Scenario: Alternar para o filtro Todos
  Given que estou visualizando apenas tarefas ativas
  And possuo tarefas concluídas
  When clico no filtro "Todos"
  Then devo ver tarefas pendentes e concluídas
  And o filtro "Todos" deve estar selecionado

Scenario: Contador deve ignorar tarefas concluídas
  Given que tenho 2 tarefas pendentes
  And tenho 1 tarefa concluída
  When visualizo o filtro Todos
  Then o contador deve exibir "2 items left"
```
# Filtragem: Ativos

```gherkin
Feature: Filtragem de Tarefas - Ativas

Scenario: Exibir apenas tarefas ativas
  Given que possuo a tarefa "Comprar Leite" pendente
  And possuo a tarefa "Pagar Conta" concluída
  When clico no filtro "Active"
  Then devo ver apenas "Comprar Leite"
  And a tarefa "Pagar Conta" não deve ser exibida
  And a URL deve conter "/active"

Scenario: Validação do contador no filtro Active
  Given que tenho 2 tarefas pendentes e 1 concluída
  When acesso o filtro "Active"
  Then devo ver apenas 2 itens
  And o contador deve exibir "2 items left"

```
# Filtragem: Concluídos

```gherkin
Feature: Filtragem de Tarefas - Concluídas

Scenario: Exibir apenas concluídas
  Given que possuo a tarefa pendente "Comprar Leite"
  And possuo a tarefa concluída "Pagar Conta"
  When clico no filtro "Completed"
  Then devo ver apenas "Pagar Conta"
  And "Comprar Leite" não deve ser exibida
  And a URL deve conter "/completed"

Scenario: Validação do contador no filtro Completed
  Given que tenho 1 tarefa pendente
  And tenho 2 tarefas concluídas
  When acesso o filtro "Completed"
  Then devo ver 2 tarefas concluídas
  And o contador deve exibir "1 item left"

```
# Marcar e Desmarcar Tarefas

```gherkin
Feature: Conclusão de Itens

Scenario: Marcar tarefa como concluída
  Given que possuo a tarefa ativa "Lavar a Louça"
  When clico no checkbox ao lado da tarefa
  Then o checkbox deve ficar marcado
  And o texto deve ficar riscado e cinza
  And a classe CSS "completed" deve ser aplicada

Scenario: Atualização do contador ao concluir tarefa
  Given que o contador exibe "3 items left"
  When marco uma tarefa como concluída
  Then o contador deve atualizar para "2 items left"

Scenario: Desmarcar tarefa concluída
  Given que possuo a tarefa "Lavar a Louça" concluída
  And o contador exibe "0 items left"
  When clico novamente no checkbox
  Then a tarefa deve voltar a ser ativa
  And o contador deve exibir "1 item left"

```
# Ações em Lote

```gherkin
Feature: Ações em Lote

Scenario: Concluir todas as tarefas
  Given que possuo "Tarefa A" pendente
  And possuo "Tarefa B" concluída
  When clico em "Mark all as complete"
  Then todas as tarefas devem ficar concluídas
  And o contador deve exibir "0 items left"

Scenario: Reabrir todas as tarefas
  Given que todas as tarefas estão concluídas
  And o contador exibe "0 items left"
  When clico em "Mark all as complete" novamente
  Then todas as tarefas devem voltar a ser ativas
  And o contador deve exibir "3 items left"

```
# Limpar Completadas

```gherkin
Feature: Limpar tarefas concluídas

Scenario: Remover apenas tarefas concluídas
  Given que possuo "Lavar Roupa" pendente
  And possuo "Pagar Boleto" concluída
  When clico em "Clear completed"
  Then apenas "Pagar Boleto" deve ser removida
  And "Lavar Roupa" deve permanecer
  And o contador deve exibir "1 item left"

Scenario: Clique sem tarefas concluídas
  Given que possuo apenas tarefas pendentes
  When clico em "Clear completed"
  Then nenhuma tarefa deve ser removida
  And o contador deve permanecer inalterado

```