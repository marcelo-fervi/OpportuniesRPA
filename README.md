## Solução usando RPA (uiPath Studio)

#### Separei o projeto em 2 automações, que irão trabalhar em conjunto utilizando Queues do Orchestrator para fazer o cadastro de oportunidades para um time de vendas.
#### O que exatamente será feito pelas automações e o que elas buscam fazer estará explicado mais abaixo no decorrer das etapas.

## Detalhes técnicos:
- Linguagem de programação: **C#**;
- Usa o **Orchestrator** e **Queues**.

## Automação 1
Deve ser iniciada manualmente, e ao ser acionada, ela cumprirá as seguintes etapas:
- Acessará o navegador padrão disponível (Chromium de preferência);
- Acessará uma URL com a tabela com dados de exemplo em: https://www.rpasamples.com/opportunities;
- Fará a extração dos dados da tabela Opportunities;
- Organizará cada oportunidade em um Dictionary separado;
- Enviará cada Dictionary separadamente para uma Queue que será consumida pela **Automação 2**.

## Automação 2
Essa automação irá iniciar sozinha utilizando um **Queue Trigger** do Orchestrator que, assim que identificar a chegada de um novo item na fila (Queue), iniciará a automação e fará o seguinte:
- Verificará a nacionalidade (Country) da oportunidade, permitindo somente "USA" ou "Germany";
- Abrirá o navegador padrão para acessar um formulário alvo do Google Forms para o cadastro das oportunidades;
- Preencherá os inputs do formulário conforme as regras impostas dentro dele;
- Tentará fazer submit do formulário;
- Aguardará por uma indicação de sucesso ou falha no envio;
- Enviará um relatório via email indicando o sucesso ou falha no cadastro dessa oportunidade.
