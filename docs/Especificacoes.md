# 📚 Especificações Funcionais, Requisitos e Testes (TodoMVC)

Este documento centraliza todas as **Histórias de Usuário**, **Regras de Negócio** (RNs) e **Cenários BDD (Gherkin)** que guiaram a análise e execução de testes.

---

## 1. Funcionalidade: Tela Inicial

### 📜 História do Usuário
Como novo usuário, quero ver a interface limpa e o campo de entrada em destaque, para que eu entenda rapidamente onde devo digitar minha primeira tarefa.

### 🎯 Regras de Negócio (RNs)
* RN01 — O título da aplicação deve ser "todos" (centralizado no topo).
* RN02 — O campo de input deve exibir o placeholder: "What needs to be done?".
* RN03 — O campo de input deve vir com foco automático (cursor piscando) ao carregar a página.
* RN04 — Não deve haver lista de tarefas, rodapé ou filtros visíveis inicialmente (apenas o input).

### 🧪 Cenários BDD (Gherkin)
```gherkin
Cenário: Carregamento inicial da aplicação
  Dado que acesso a URL do projeto TodoMVC
  Então devo ver o título "todos" no topo da página
  E devo visualizar um campo de entrada de texto vazio
  E o campo de entrada deve conter o placeholder "What needs to be done?"

Cenário: Foco automático no input
  Dado que a página acabou de ser carregada
  Quando verifico onde está o cursor do mouse
  Então o campo de entrada de tarefas deve estar focado/ativo

Cenário: Ocultação de elementos desnecessários
  Dado que não possuo nenhuma tarefa cadastrada
  Então a lista de tarefas deve estar invisível
  E a barra de rodapé (com filtros e contador) não deve ser exibida
```

## 2. Funcionalidade: Adicionar Itens

### 📜 História do Usuário
Como usuário focado em produtividade, quero adicionar novas tarefas rapidamente pressionando Enter, para que eu possa registrar meus pendentes sem usar o mouse.

### 🎯 Regras de Negócio (RNs)
RN01 — A inclusão deve ocorrer ao pressionar a tecla "Enter" (não há botão "Salvar").
RN02 — O campo de input deve ser limpo automaticamente após a inclusão com sucesso.
RN03 — O sistema não deve permitir a criação de tarefas vazias.
RN04 — O sistema deve ignorar espaços em branco no início e no fim do texto (Trim).
RN05 — Ao adicionar a primeira tarefa, a lista e o rodapé devem se tornar visíveis (transição de estado).
RN06 — O item recém-adicionado deve ir para o final da lista.

### 🧪 Cenários BDD (Gherkin)
```Gherkin
Cenário: Adicionar tarefa com sucesso
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
  Dado que digito "    Fazer Café    " no input
  Quando pressiono Enter
  Então o item deve ser salvo apenas como "Fazer Café"

Cenário: Primeira inserção habilita elementos de controle
  Dado que não tenho tarefas registradas
  Quando adiciono a tarefa "Primeira Tarefa"
  Então a barra de rodapé (com contador e filtros) deve aparecer
  E a seta "Mark all as complete" (no input) deve ficar visível
```
## 3. Funcionalidade: Filtragem de Tarefas — Todos

### 📜 História do Usuário
Como usuário da aplicação, quero visualizar todas as tarefas (pendentes e concluídas), para que eu possa acompanhar meu progresso completo e gerenciar o que já foi feito.

### 🎯 Regras de Negócio (RNs)
RN01 — A aplicação deve iniciar com o filtro "Todos" selecionado por padrão.
RN02 — O filtro deve exibir tanto itens ativos quanto concluídos na lista.
RN03 — O contador "items left" deve ignorar tarefas concluídas, mostrando apenas as pendentes.
RN04 — O botão do filtro "Todos" deve estar visualmente destacado quando ativo (classe 'selected').

### 🧪 Cenários BDD (Gherkin)
```Gherkin
Cenário: Estado inicial da aplicação (Default)
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
```
## 4. Funcionalidade: Filtragem de Tarefas — Ativos

### 📜 História do Usuário
Como usuário da aplicação, quero visualizar apenas as tarefas pendentes (Active), para que eu não me distraia com o que já foi concluído.

### 🎯 Regras de Negócio (RNs)
RN01 — Ao selecionar o filtro "Active", tarefas marcadas como concluídas devem ser ocultadas.
RN02 — A URL da aplicação deve mudar para "/active" (ou "#/active").
RN03 — O botão "Active" deve estar visualmente selecionado.
RN04 — O contador deve continuar exibindo o número total de tarefas pendentes.

### 🧪 Cenários BDD (Gherkin)
```Gherkin
Cenário: Filtrar tarefas ativas em uma lista mista
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
```

## 5. Funcionalidade: Filtragem de Tarefas — Concluídos

### 📜 História do Usuário
Como usuário da aplicação, quero visualizar apenas as tarefas que já finalizei, para sentir satisfação com o dever cumprido.

### 🎯 Regras de Negócio (RNs)
RN01 — O filtro deve exibir apenas tarefas com status "completed" (riscadas).
RN02 — Tarefas ativas (pendentes) devem ser ocultadas da lista.
RN03 — O contador "items left" deve continuar mostrando o número de tarefas pendentes (e NÃO o número de tarefas concluídas visualizadas).
RN04 — A URL deve ser atualizada para "/completed".

### 🧪 Cenários BDD (Gherkin)
```Gherkin

Cenário: Visualizar tarefas concluídas
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
```

## 6. Funcionalidade: Conclusão de Itens - Unitária

### 📜 História do Usuário
Como usuário da aplicação, quero marcar tarefas como concluídas (e desmarcar se necessário), para que eu possa ver o que já foi feito do que ainda está pendente.

### 🎯 Regras de Negócio (RNs)
RN01 — Ao clicar no checkbox de um item ativo, ele deve ser marcado como concluído.
RN02 — Itens concluídos devem ter o texto riscado (line-through) e cor cinza (classe CSS 'completed').
RN03 — O contador "items left" deve ser decrementado ao concluir uma tarefa.
RN04 — Ao clicar no checkbox de um item já concluído, ele deve voltar a ser ativo (desmarcado).
RN05 — O contador "items left" deve ser incrementado ao reativar uma tarefa.

### 🧪 Cenários BDD (Gherkin)
```Gherkin

Cenário: Marcar uma tarefa como concluída (Sucesso)
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
```

## 7. Funcionalidade: Conclusão de Itens - Em Lote (Toggle All)

### 📜 História do Usuário
Como usuário da aplicação, quero alterar o status de todas as minhas tarefas de uma vez só, para agilizar a organização quando termino tudo ou quando preciso reiniciar meu dia.

### 🎯 Regras de Negócio (RNs)
RN01 — Se houver pelo menos uma tarefa pendente na lista, o clique na seta deve marcar todas como concluídas.
RN02 — Se todas as tarefas já estiverem concluídas, o clique na seta deve marcar todas como ativas (pendentes).
RN03 — O contador "items left" deve ser atualizado para "0" (quando tudo for concluído) ou para o total de tarefas (quando tudo for reaberto).
RN04 — A própria seta (toggle) deve mudar de cor (geralmente escurecer) para indicar que todas as tarefas estão concluídas.

### 🧪 Cenários BDD (Gherkin)
```Gherkin

Cenário: Concluir tudo (tendo itens mistos ou pendentes)
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
```

## 8. Funcionalidade: Limpar Itens Finalizados

### 📜 História do Usuário
Como usuário da aplicação, quero limpar os itens já concluídos, para me concentrar apenas nas tarefas a serem feitas.

### 🎯 Regras de Negócio (RNs)
RN01 — O botão "Clear completed" deve estar sempre visível no rodapé.
RN02 — Ao passar o mouse sobre o botão, ele deve ser sublinhado (feedback visual).
RN03 — Ao clicar no botão, os itens com status "checked" (concluídos) devem ser excluídos.
RN04 — O contador "items left" deve permanecer inalterado (pois conta apenas pendentes).
RN05 — Se não houver tarefas concluídas, clicar no botão não deve realizar nenhuma ação (o sistema não deve travar).

### 🧪 Cenários BDD (Gherkin)
```Gherkin

Cenário: Exclusão de itens finalizados (Lista Mista)
  Dado que possuo a tarefa "Lavar Roupa" (Pendente)
  E possuo a tarefa "Pagar Boleto" (Concluída)
  Quando clico em "Clear completed"
  Então a tarefa "Pagar Boleto" deve ser removida da lista
  Mas a tarefa "Lavar Roupa" deve continuar na lista
  E o contador deve continuar exibindo "1 item left"

Cenário: Clique sem itens concluídos (Teste de Robustez)
  Dado que possuo apenas tarefas pendentes
  E não possuo nenhuma tarefa concluída
  Quando clico em "Clear completed"
  Então nenhuma tarefa deve ser removida da lista
  E o contador deve permanecer inalterado
  ```