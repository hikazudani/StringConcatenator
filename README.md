# StringConcatenator

## Sobre o Projeto
O **StringConcatenator** é um aplicativo Android implementado com Jetpack Compose para demonstrar o fluxo de navegação e a passagem de dados entre duas telas. O aplicativo foca em manter uma única string em memória, que cresce incrementalmente através de concatenações.

## Telas e Funcionalidades
O aplicativo foi estruturado com as seguintes telas e elementos:

*   **HomeScreen (Tela Inicial):**
    *   Possui um campo de texto não editável que exibe a string atual (que se inicia vazia).
    *   Contém um botão "Adicionar palavra" para navegar até a próxima tela.
    *   Contém um botão "Reiniciar" para limpar a string.
    *   É a única responsável por executar a regra de negócio da concatenação.

*   **AddWordScreen:**
    *   Exibe a string atual recebida da tela anterior em um campo não editável.
    *   Possui um campo de texto editável e vazio para o usuário digitar uma nova palavra.
    *   Contém um botão "Concatenar" para confirmar a ação.

## Fluxo de Navegação
1. O app inicia na `HomeScreen` com a string vazia.
2. O usuário clica em "Adicionar palavra" e o app navega para a `AddWordScreen`, levando a string atual como um argumento de rota.
3. O usuário digita a nova palavra no campo editável.
4. Ao clicar em "Concatenar", a tela é fechada (`popBackStack()`) e a palavra recém-digitada é devolvida para a `HomeScreen` através da pilha de navegação.
5. A `HomeScreen` recebe a nova palavra e a concatena ao valor existente (adicionando um espaço caso a string não estivesse vazia), atualizando a exibição.

---

## Justificativa das Tecnologias de Navegação

*   **Sealed Class para rotas:** abordagem ideal pois traz segurança de tipagem (type safety) para o grafo de navegação. Ao invés de usar strings soltas (hardcoded) sujeitas a erros de digitação na hora de navegar, as Sealed Classes garantem que as rotas e seus parâmetros sejam conhecidos em tempo de compilação, facilitando a manutenção e a clareza do código.
*   **Argumento de Rota:** mecanismo para a ida (enviar a string da `HomeScreen` para a `AddWordScreen`). Como a segunda tela precisa apenas receber a string para exibição (somente leitura), passá-la acoplada diretamente à rota de navegação é a forma mais simples, sem a necessidade de criar instâncias complexas ou viewmodels compartilhados desnecessariamente.
*   **SavedStateHandle:** recurso para a volta (devolver a nova palavra para a `HomeScreen`). Como o aplicativo não usa um `.navigate()` para voltar, mas sim tira a tela atual da pilha (`popBackStack()`), o `SavedStateHandle` permite injetar um resultado no back stack entry da tela anterior. Isso garante que a comunicação entre as telas seja reativa e isolada, deixando a responsabilidade da concatenação onde ela deve estar (na tela inicial).
