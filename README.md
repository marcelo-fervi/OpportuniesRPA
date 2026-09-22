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
- Enviará um Dictionary onde cada chave corresponde a uma oportunidade, porém cada valor será uma string JSON serializada será consumida pela **Automação 2**.

## Automação 2
Essa automação irá iniciar sozinha utilizando um **Queue Trigger** do Orchestrator que, assim que identificar a chegada da lista de oportunidades na fila (Queue), iniciará a automação, e buscará iterar por cada oportunidade, fazendo o seguinte:
- Fará a deserialização do JSON da oportunidade;
- Verificará a nacionalidade (Country) dela, permitindo somente "USA" ou "Germany";
- Abrirá o navegador padrão para acessar um formulário alvo do Google Forms para o cadastro da oportunidade;
- Preencherá os inputs do formulário conforme as regras impostas dentro dele;
- Tentará fazer submit do formulário;
- Aguardará por uma indicação de sucesso ou falha no envio.

Ao terminar a iteração de todas as oportunidades, um relatório será enviado via email indicando diversas estatísticas relacionadas a todo o procedimento, como total de oportunidades lidas, quantas foram sucedidas, quantas falharam, etc.
