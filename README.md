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

 - No `package.json`:

    ```json
    "husky": {
    "hooks": {
      "pre-commit": "echo 'teste'"
    }
  
    ```

> lint-staged

 `yarn add lint-staged -D`

 - No `package.json`:

    ```json
    "lint-staged": {
    "src/**/*.ts": [ // Todos arquivos ts dentro de src
      "eslint --fix"
    ]
    }
    ```

- Instalar eslint -D para o funcionamento do linting.
    - Se faltar, instalar `yarn add -D @typescript-eslint/parser`
    - Adicionar regra pra arquivos `.jsx`, `.tsx`:

        `"react/jsx-filename-extension": [1, { "extensions":[".js", ".jsx", ".ts", ".tsx"] }]`

- Adicionar mais opções no arquivo, como verificações de testes, por exemplo:

    ```json
        "lint-staged": {
            "*.ts?(x)": [
            "eslint -c .eslintrc.json --fix",
            "yarn test -- findRelatedTests"
            ]
        }
    ```


