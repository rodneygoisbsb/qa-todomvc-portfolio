## Cenários de Teste — TodoMVC (Gherkin)

Abaixo estão todos os cenários escritos em Gherkin, organizados por funcionalidade.

---

Feature: Tela Inicial

  @smoke @critical
  Scenario: Carregamento inicial da aplicação
    Given que acesso a URL do projeto TodoMVC
    Then devo ver o título "todos" no topo da página
    And devo visualizar um campo de entrada de texto vazio
    And o campo de entrada deve conter o placeholder "What needs to be done?"

  @regression
  Scenario: Foco automático no input
    Given que a página acabou de ser carregada
    When verifico onde está o cursor do mouse
    Then o campo de entrada de tarefas deve estar focado

  @negative @regression
  Scenario: Ocultação de elementos quando não existem tarefas
    Given que não possuo nenhuma tarefa cadastrada
    Then a lista de tarefas deve estar invisível
    And a barra de rodapé não deve ser exibida

Feature: Adicionar Itens

  @smoke @critical
  Scenario: Adicionar tarefa com sucesso
    Given que estou na tela inicial com o input focado
    When digito "Estudar Gherkin" e pressiono Enter
    Then o item "Estudar Gherkin" deve aparecer na lista
    And o campo de input deve voltar a ficar vazio
    And um checkbox não marcado deve estar exibido ao lado do item

  @negative
  Scenario: Não deve permitir tarefa vazia
    Given que o campo de input está vazio
    When pressiono Enter
    Then nenhuma tarefa deve ser adicionada
    And o rodapé deve continuar oculto

  @regression
  Scenario: Remover espaços na criação
    Given que digito "   Fazer Café   " no input
    When pressiono Enter
    Then o item deve ser salvo como "Fazer Café"

  @regression
  Scenario: Primeiro item deve exibir elementos de controle
    Given que não tenho tarefas registradas
    When adiciono a tarefa "Primeira Tarefa"
    Then a barra de rodapé deve ser exibida
    And a seta de marcar tudo deve ficar visível

Feature: Filtragem - Todos

  @smoke @critical
  Scenario: Filtro padrão ao acessar aplicação
    Given que acessei o TodoMVC
    Then todas as tarefas (pendentes e concluídas) devem ser exibidas
    And o filtro "All" deve estar selecionado

  @regression
  Scenario: Trocar para filtro "All" vindo de outra visualização
    Given que estou visualizando tarefas ativas
    And possuo tarefas concluídas
    When clico no filtro "All"
    Then todas as tarefas devem ser exibidas
    And o filtro "All" deve estar selecionado

  @regression
  Scenario: Contador deve exibir apenas tarefas ativas
    Given que tenho 2 tarefas pendentes
    And tenho 1 tarefa concluída
    When estou no filtro "All"
    Then o contador deve exibir "2 items left"

Feature: Filtragem - Ativos

  @smoke @regression @critical
  Scenario: Exibir apenas tarefas ativas
    Given que possuo a tarefa "Comprar Leite" pendente
    And possuo a tarefa "Pagar Conta" concluída
    When clico no filtro "Active"
    Then devo ver apenas "Comprar Leite"
    And tarefas concluídas devem estar ocultas

  @regression
  Scenario: Contador deve refletir somente tarefas ativas
    Given que tenho 2 tarefas pendentes
    And tenho 1 concluída
    When estou filtrando por tarefas ativas
    Then o contador deve exibir "2 items left"

  @negative
  Scenario: Lista vazia quando não houver tarefas ativas
    Given que todas as tarefas estão concluídas
    When clico no filtro "Active"
    Then nenhuma tarefa deve ser exibida

Feature: Filtragem - Concluídos

  @smoke @regression
  Scenario: Exibir apenas tarefas concluídas
    Given que possuo uma tarefa pendente
    And possuo duas tarefas concluídas
    When clico no filtro "Completed"
    Then apenas tarefas concluídas devem ser exibidas

  @regression
  Scenario: Contador deve ignorar tarefas concluídas
    Given que tenho 1 tarefa pendente
    And tenho 2 concluídas
    When estou no filtro "Completed"
    Then o contador deve exibir "1 item left"

Feature: Conclusão de Itens - Unitária

  @smoke @critical
  Scenario: Marcar tarefa como concluída
    Given que possuo a tarefa ativa "Lavar a Louça"
    When marco a tarefa como concluída
    Then a tarefa deve aparecer riscada
    And o checkbox deve estar selecionado

  @regression
  Scenario: Atualizar contador ao marcar tarefa
    Given que o contador exibe "3 items left"
    When marco uma tarefa como concluída
    Then o contador deve exibir "2 items left"

  @regression
  Scenario: Desmarcar tarefa concluída
    Given que a tarefa "Lavar a Louça" está marcada como concluída
    When desmarco a tarefa
    Then ela deve voltar ao estado ativo

Feature: Conclusão de Itens - Em Lote

  @critical @regression
  Scenario: Marcar todas as tarefas como concluídas
    Given que possuo tarefas pendentes e concluídas
    When marco todas como concluídas
    Then nenhuma tarefa deve permanecer ativa
    And o contador deve exibir "0 items left"

  @regression
  Scenario: Reabrir todas as tarefas
    Given que todas as tarefas estão concluídas
    When clico no botão "Marcar tudo"
    Then todas as tarefas devem voltar a ser ativas

Feature: Limpar tarefas concluídas

  @smoke @critical
  Scenario: Remover apenas tarefas concluídas
    Given que possuo tarefas concluídas e tarefas pendentes
    When clico em "Clear completed"
    Then apenas tarefas concluídas devem ser removidas

  @negative @regression
  Scenario: Nenhuma remoção quando não houver itens concluídos
    Given que não possuo tarefas concluídas
    When clico em "Clear completed"
    Then nenhuma alteração deve ocorrer