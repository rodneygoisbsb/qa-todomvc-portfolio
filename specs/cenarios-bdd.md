Especificação de Cenários — TodoMVCEste documento contém os critérios de aceite mapeados em formato Gherkin, servindo de base para a execução manual e futura automação.Feature: Tela InicialComo novo usuário, quero uma tela clara e pronta para uso.Cenário: Carregamento inicial da aplicação
    Dado que acesso a URL do projeto TodoMVC
    Então devo ver o título "todos" no topo da página
    E devo visualizar um campo de entrada de texto vazio
    E o campo de entrada deve conter o placeholder "What needs to be done?"

Cenário: Foco automático no input
    Dado que a página acabou de ser carregada
    Quando verifico onde está o cursor do mouse
    Então o campo de entrada de tarefas deve estar focado/ativo

Cenário: Ocultação de elementos desnecessários (Empty State)
    Dado que não possuo nenhuma tarefa cadastrada
    Então a lista de tarefas deve estar invisível
    E a barra de rodapé (com filtros e contador) não deve ser exibida
Feature: Adicionar ItensComo usuário, quero adicionar novas tarefas e ver os elementos de controle serem habilitados.Cenário: Adicionar tarefa com sucesso
    Dado que estou na tela inicial (foco no input)
    Quando digito "Estudar Gherkin" e pressiono Enter
    Então o item "Estudar Gherkin" deve aparecer na última posição da lista
    E o campo de input deve voltar a ficar vazio
    E um checkbox não marcado deve ser exibido ao lado do item

Cenário: Tentativa de adicionar tarefa vazia (Negative Test)
    Dado que o campo de input está vazio
    Quando pressiono a tecla Enter
    Então nenhuma tarefa deve ser adicionada à lista
    E o rodapé deve continuar oculto (se não houver outros itens)

Cenário: Adicionar tarefa com espaços extras (Trim)
    Dado que digito "   Fazer Café   " no input
    Quando pressiono Enter
    Então o item deve ser salvo apenas como "Fazer Café"

Cenário: Primeira inserção habilita elementos de controle
    Dado que não tenho tarefas registradas
    Quando adiciono a tarefa "Primeira Tarefa"
    Então a barra de rodapé (com contador e filtros) deve aparecer
    E a seta "Mark all as complete" (no input) deve ficar visível
Feature: Filtragem de Tarefas - TodosComo usuário, quero visualizar todas as minhas tarefas, independentemente do status.Cenário: Estado inicial da aplicação (Default)
    Dado que acessei a aplicação TodoMVC
    Então a lista de tarefas deve exibir todos os itens pendentes e concluídos
    E o filtro "Todos" deve estar visualmente selecionado

Cenário: Alternar para o filtro "Todos"
    Dado que estou visualizando apenas as tarefas "Ativas"
    E possuo tarefas concluídas na minha lista
    Quando clico no filtro "Todos"
    Então devo ver as tarefas pendentes E as tarefas concluídas na lista
    E o link "Todos" deve ganhar o destaque de seleção

Cenário: Contador deve ignorar itens concluídos
    Dado que tenho 2 tarefas pendentes
    E tenho 1 tarefa concluída
    Quando visualizo a lista no filtro "Todos"
    Então o contador deve exibir o texto "2 items left"
Feature: Filtragem de Tarefas - AtivosComo usuário, quero focar apenas nas tarefas que ainda preciso fazer.Cenário: Filtrar tarefas ativas em uma lista mista
    Dado que possuo a tarefa "Comprar Leite" (Pendente)
    E possuo a tarefa "Pagar Conta" (Concluída)
    Quando clico no filtro "Active"
    Então a tarefa "Comprar Leite" deve ser exibida na lista
    Mas a tarefa "Pagar Conta" NÃO deve ser exibida
    E a URL deve conter "/active"

Cenário: Validação do contador no filtro Active
    Dado que tenho 2 tarefas pendentes e 1 concluída
    Quando acesso o filtro "Active"
    Então devo ver apenas 2 itens na lista
    E o contador deve exibir "2 items left"
Feature: Filtragem de Tarefas - ConcluídosComo usuário, quero revisar o que já foi feito.Cenário: Visualizar tarefas concluídas
    Dado que possuo a tarefa "Comprar Leite" (Pendente)
    E possuo a tarefa "Pagar Conta" (Concluída)
    Quando clico no filtro "Completed"
    Então a tarefa "Pagar Conta" deve ser exibida na lista
    Mas a tarefa "Comprar Leite" NÃO deve ser exibida (deve estar oculta)
    E a URL deve conter "/completed"

Cenário: Validação do contador no filtro Completed
    Dado que tenho 1 tarefa pendente
    E tenho 2 tarefas concluídas
    Quando acesso o filtro "Completed"
    Então devo ver 2 itens listados na tela
    Mas o contador deve exibir "1 item left"
Feature: Conclusão de Itens - UnitáriaComo usuário, quero marcar o progresso de forma individual.Cenário: Marcar uma tarefa como concluída (Sucesso)
    Dado que possuo a tarefa ativa "Lavar a Louça"
    Quando clico no checkbox ao lado da tarefa "Lavar a Louça"
    Então o checkbox deve ficar marcado (checked)
    E o texto da tarefa deve ficar riscado e cinza
    E o sistema deve aplicar a classe CSS "completed" ao item

Cenário: Atualização do contador ao concluir
    Dado que o contador exibe "3 items left"
    Quando marco uma tarefa como concluída
    Então o contador deve atualizar para "2 items left"

Cenário: Desmarcar uma tarefa (Reversão)
    Dado que possuo a tarefa "Lavar a Louça" já concluída (riscada)
    E o contador exibe "0 items left"
    Quando clico novamente no checkbox da tarefa
    Então a tarefa deve voltar a ser exibida como ativa (texto normal)
    E o contador deve atualizar para "1 item left"
Feature: Conclusão de Itens - Em LoteComo usuário, quero marcar todas as tarefas de uma vez.Cenário: Concluir tudo (tendo itens mistos ou pendentes)
    Dado que possuo a tarefa "Tarefa A" (Pendente)
    E possuo a tarefa "Tarefa B" (Concluída)
    Quando clico na seta "Mark all as complete" (ao lado do input)
    Então ambas as tarefas "Tarefa A" e "Tarefa B" devem ficar riscadas (concluídas)
    E o contador deve exibir "0 items left"

Cenário: Reabrir tudo (Desmarcar em lote)
    Dado que todas as minhas 3 tarefas estão marcadas como concluídas
    E o contador exibe "0 items left"
    Quando clico na seta "Mark all as complete" novamente
    Então todas as 3 tarefas devem voltar a ser ativas (texto normal)
    E o contador deve exibir "3 items left"
Feature: Limpar Itens FinalizadosComo usuário, quero remover tarefas concluídas para organizar a lista.Cenário: Exclusão de itens finalizados (Lista Mista)
    Dado que possuo a tarefa "Lavar Roupa" (Pendente)
    E possuo a tarefa "Pagar Boleto" (Concluída)
    Quando clico em "Clear completed"
    Então a tarefa "Pagar Boleto" deve ser removida da lista
    Mas a tarefa "Lavar Roupa" deve continuar na lista
    E o contador deve continuar exibindo "1 item left"

Cenário: Visibilidade do botão Clear
    Dado que possuo apenas tarefas pendentes
    E não possuo nenhuma tarefa concluída
    Quando o rodapé está visível
    Então o botão "Clear completed" não deve ser exibido

Cenário: Limpar botão deve reaparecer
    Dado que não há tarefas concluídas e o botão "Clear completed" está invisível
    Quando marco uma tarefa como concluída
    Então o botão "Clear completed" deve reaparecer na barra de rodapé
