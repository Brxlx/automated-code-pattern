# Setup de projeto para automatização de padrões de código com _Husky_ e _lint-staged_

> Criando app _React_ com template typescript

- Instalar localmente:

    `npx create-react-app [name] --template=typescript` ou `yarn create react-app [nome] --template=typescript`

- _Cleanup_ de arquivos:
    - Deletar `App.css`, `index.css`, `App.spec.tsx`, `logo.svg`, `serviceWorker.ts`, `setupTest.ts`
    - Deletar importações dos respectivos arquivos no `App.tsx` e `index.tsx`

> Padronziando _end of lines_:

- Criar diretório `.vscode` na raiz do projeto
    - Dentro dele, criar arquivo `settings.json`:

    Para LF: 
    ```json
    {
    "files.eol": "\n"
    }
    ```

    Para CRLF:
    ```json
    {
    "files.eol": "\r\n",
    }
    ```
    *Observação: Não corrige arquivos já criados, fazê-la manualmente.

- Para desabilitar warnings do git:

    `git config [--global] core.safecrlf false`

> Husky

 `yarn add husky -D`