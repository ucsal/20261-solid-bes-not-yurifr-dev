# Sistema das Olimpíadas - Refatoração com SOLID

Este projeto foi refatorado com foco na aplicação prática dos princípios SOLID em um sistema legado, preservando o comportamento funcional original do menu, dos cadastros, da aplicação da prova e do cálculo de nota.

## Principais mudanças

- `App` deixou de concentrar toda a lógica e passou a apenas montar as dependências da aplicação.
- A regra de negócio foi centralizada em `OlimpiadasService`.
- A persistência em memória foi abstraída pela interface `OlimpiadasRepository` e implementada em `InMemoryOlimpiadasRepository`.
- A entrada e saída do console foram desacopladas por meio de `TextInput`, `TextOutput` e `ConsoleIO`.
- A renderização do tabuleiro FEN foi extraída para `TabuleiroRenderer` e `FenTabuleiroRenderer`.
- A carga inicial da prova padrão foi isolada em `SeedDataLoader`.
- O cálculo da nota foi movido para `PontuacaoCalculator`.

## Aplicação dos princípios SOLID

### S - Single Responsibility Principle (SRP)

Cada classe passou a ter uma responsabilidade clara:

- `App`: inicialização da aplicação.
- `ConsoleApplication`: fluxo do menu e interação com o usuário.
- `OlimpiadasService`: orquestração da regra de negócio.
- `PontuacaoCalculator`: cálculo da nota.
- `SeedDataLoader`: carga de dados iniciais.
- `FenTabuleiroRenderer`: impressão do tabuleiro a partir de FEN.
- `InMemoryOlimpiadasRepository`: armazenamento em memória.

### O - Open/Closed Principle (OCP)

O sistema ficou aberto para extensão e fechado para modificação em pontos importantes:

- novas formas de persistência podem ser adicionadas implementando `OlimpiadasRepository`;
- novas formas de exibir o tabuleiro podem ser adicionadas implementando `TabuleiroRenderer`;
- novas entradas e saídas podem ser criadas implementando `TextInput` e `TextOutput`.

Assim, o comportamento pode evoluir sem precisar alterar a lógica principal do serviço ou do fluxo da aplicação.

### L - Liskov Substitution Principle (LSP)

As implementações concretas podem ser substituídas por outras que respeitem os mesmos contratos:

- `InMemoryOlimpiadasRepository` pode ser trocado por outra implementação de `OlimpiadasRepository`;
- `FenTabuleiroRenderer` pode ser trocado por outra implementação de `TabuleiroRenderer`;
- `ConsoleIO` pode ser substituído por outra implementação de `TextInput`/`TextOutput`.

Essas substituições preservam o comportamento esperado pela aplicação, sem exigir mudanças no código cliente.

### I - Interface Segregation Principle (ISP)

As interfaces foram separadas em contratos menores:

- `TextInput` contém apenas leitura;
- `TextOutput` contém apenas escrita;
- `TabuleiroRenderer` tem apenas a responsabilidade de renderização;
- `OlimpiadasRepository` expõe somente operações necessárias ao domínio.

Com isso, cada classe depende apenas do que realmente usa.

### D - Dependency Inversion Principle (DIP)

Os módulos de mais alto nível passaram a depender de abstrações:

- `ConsoleApplication` depende de `TextInput`, `TextOutput` e `TabuleiroRenderer`;
- `OlimpiadasService` depende de `OlimpiadasRepository`;
- `App` faz a composição das implementações concretas.

Isso reduz acoplamento e facilita testes, manutenção e evolução.

## Observações

- A lógica de negócio original foi preservada.
- Nenhuma funcionalidade existente foi removida.
- Nenhum framework externo foi adicionado.
