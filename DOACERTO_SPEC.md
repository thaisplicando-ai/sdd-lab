# DoaCerto — especificação de referência

Conteúdo para `openspec/changes/gerenciar-doacoes/specs/doacoes/spec.md` no projeto Python. Esta é uma **nova capacidade**, portanto usa `ADDED Requirements`.

## ADDED Requirements

### Requirement: Cadastrar produtos
O sistema SHALL cadastrar um produto com nome não vazio após remover espaços nas extremidades, quantidade em estoque inteira não negativa e meta inteira não negativa. O sistema SHALL preservar a grafia normalizada do primeiro cadastro para exibição. O sistema SHALL identificar nomes por `casefold()` e rejeitar um segundo cadastro com a mesma identidade. Entradas inválidas ou duplicadas SHALL gerar `ValueError` sem alterar o catálogo. Valores booleanos SHALL ser tratados como inválidos, mesmo que Python os considere subclasse de `int`.

#### Scenario: Cadastro válido
- **GIVEN** o catálogo vazio
- **WHEN** a pessoa cadastra `Arroz` com estoque 8 e meta 20
- **THEN** o catálogo contém `Arroz` com estoque 8 e meta 20

#### Scenario: Estoque e meta zero
- **GIVEN** o catálogo vazio
- **WHEN** a pessoa cadastra `Sabonete` com estoque 0 e meta 0
- **THEN** o produto é cadastrado e sua meta está atingida

#### Scenario: Nome repetido com outra capitalização
- **GIVEN** `Arroz` cadastrado com estoque 8 e meta 20
- **WHEN** a pessoa tenta cadastrar ` arroz ` com estoque 3 e meta 10
- **THEN** ocorre `ValueError` e o cadastro original continua com estoque 8 e meta 20

#### Scenario: Dados inválidos
- **GIVEN** o catálogo vazio
- **WHEN** a pessoa tenta cadastrar nome composto apenas de espaços, estoque -1, meta -1, fração, texto numérico ou booleano em qualquer quantidade
- **THEN** cada tentativa gera `ValueError` e o catálogo permanece vazio

### Requirement: Calcular quantidade faltante
O sistema SHALL calcular `faltam = max(0, meta - estoque)` para todo produto. O cálculo SHALL usar o estoque integral, mesmo quando superior à meta, e SHALL nunca produzir uma falta negativa.

#### Scenario: Meta ainda não atingida
- **GIVEN** `Arroz` com estoque 8 e meta 20
- **WHEN** a necessidade é calculada
- **THEN** faltam 12 unidades

#### Scenario: Meta exata
- **GIVEN** `Feijão` com estoque 15 e meta 15
- **WHEN** a necessidade é calculada
- **THEN** faltam 0 unidades

#### Scenario: Estoque excedente
- **GIVEN** `Arroz` com estoque 23 e meta 20
- **WHEN** a necessidade é calculada
- **THEN** faltam 0 unidades e o estoque continua 23

### Requirement: Registrar doações
O sistema SHALL adicionar ao estoque de um produto existente a quantidade inteira positiva recebida, sem limitar o estoque à meta. Uma quantidade zero, negativa, fracionária, textual ou booleana SHALL gerar `ValueError`. Um produto inexistente SHALL gerar `KeyError`. Erros SHALL manter o estoque e o catálogo inalterados. A busca pelo produto SHALL ignorar diferenças de maiúsculas/minúsculas e espaços nas extremidades.

#### Scenario: Doação parcial
- **GIVEN** `Arroz` com estoque 8 e meta 20
- **WHEN** a pessoa registra doação de 5 unidades para ` arroz `
- **THEN** o estoque de `Arroz` passa a 13 e faltam 7 unidades

#### Scenario: Doação acima da meta
- **GIVEN** `Arroz` com estoque 19 e meta 20
- **WHEN** a pessoa registra doação de 5 unidades
- **THEN** o estoque passa a 24, faltam 0 unidades e o excedente de 4 unidades permanece registrado

#### Scenario: Doação inválida
- **GIVEN** `Arroz` com estoque 8 e meta 20
- **WHEN** a pessoa tenta doar 0, -1, 1.5, `"2"` ou `True` em tentativas separadas
- **THEN** cada tentativa gera `ValueError` e o estoque continua 8

#### Scenario: Produto inexistente
- **GIVEN** nenhum `Sabonete` cadastrado
- **WHEN** a pessoa tenta doar 2 unidades para `Sabonete`
- **THEN** ocorre `KeyError` e o catálogo permanece inalterado

### Requirement: Gerar relatório de necessidades
O sistema SHALL apresentar todos os produtos em ordem alfabética sem diferenciar maiúsculas/minúsculas, com nome, estoque atual, meta e quantidade faltante. O sistema SHALL listar produtos com falta positiva em “PRODUTOS QUE PRECISAM DE DOAÇÃO” e produtos com falta zero em “PRODUTOS COM META ATINGIDA”. Produtos com estoque acima da meta SHALL continuar no relatório. Uma seção sem produtos SHALL exibir uma mensagem explícita de lista vazia. O relatório SHALL poder ser gerado sem modificar os dados.

#### Scenario: Mistura de necessidades e metas atingidas
- **GIVEN** `Arroz` 8/20, `Feijão` 15/15 e `Sabonete` 3/10
- **WHEN** o relatório é gerado
- **THEN** a tabela mostra faltas 12, 0 e 7, nessa ordem alfabética; Arroz e Sabonete aparecem entre os que precisam de doação; Feijão aparece em meta atingida

#### Scenario: Meta superada continua visível
- **GIVEN** `Arroz` com estoque 24 e meta 20
- **WHEN** o relatório é gerado
- **THEN** Arroz aparece com falta 0 e na seção de meta atingida

#### Scenario: Catálogo vazio
- **GIVEN** nenhum produto cadastrado
- **WHEN** o relatório é gerado
- **THEN** a tabela não tem linhas de produtos e as duas seções mostram uma mensagem explícita de lista vazia

#### Scenario: Relatório não modifica estoque
- **GIVEN** `Arroz` com estoque 8 e meta 20
- **WHEN** o relatório é gerado duas vezes
- **THEN** as duas saídas são iguais e o estoque continua 8
