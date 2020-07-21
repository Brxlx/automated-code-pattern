# Setup de projeto para automatização de padrões de código com _Husky_ e _lint-staged_

[![Commitizen friendly](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg)](http://commitizen.github.io/cz-cli/)

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


> Rules

- Em caso de erro do módulo React:

    `yarn add -D @types/react`

- Em caso de erro de extensão de arquivos `.ts` ou `.tsx`, no arquivo `.eslintrc.json`:

    ```json
    "rules": {
        "react/jsx-filename-extension": [1, { "extensions":[".js", ".jsx", ".ts", ".tsx"] }],
        "import/extensions": [
            "error",
            "ignorePackages",
            {
              "js": "never",
              "jsx": "never",
              "ts": "never",
              "tsx": "never"
            }
         ]
    }
    ```

> Cross ENV
- Adicionar dependência `cross-env` para funcionamento de testes em sistemas UNIX e Windows:

    `yarn add cross-env -D`

- No arquivo `package.json`, adicionar:

    ```json
    "lint-staged": {
    "*.ts?(x)": [
      "eslint --fix",
      "cross-env CI=true yarn test --bail --findRelatedTests",
      "git add"
    ]
  }
    ```

> Commit lint

- Adicionar biblioteca:

    `yarn add @commitlint/cli @commitlint/config-conventional -D`

- Criar arquivo `commitlint.config.js` ou `commitlintrc.js` na raiz do projeto.

    - Dentro do arquivo, colocar:

        ```javascript
        module.exports = {extends: ['@commitlint/config-conventional']}
        ```

- Adicionar o _commitlint_ aos _hooks_ do husky:

    ```json
        "husky": {
            "hooks": {
            "pre-commit": "lint-staged",
            "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
        }
    }
    ```

- Seguir o padrão de _commit_: 

    ```
    type(scope?): subject
    body?
    footer?

    ```

- Editando e personalizando padrões:

    - Modificar arquivo `commitlint.config.js` ou `commitlintrc.js` com chaves e valores configuráveis:

        - Documentação das regras: `https://github.com/conventional-changelog/commitlint/blob/master/docs/reference-rules.md`

        - Arquivo de  configuração do _conventional_: `https://github.com/conventional-changelog/commitlint/blob/master/@commitlint/config-conventional/index.js`

> Commitizen

- Adicionar biblioteca:

    `yarn add commitizen -D`

- Inicializar _commitizen_ no projeto:

    ` `npm` commitizen init cz-conventional-changelog --save-dev --save-exact`

    ou

    ` `yarn` commitizen init cz-conventional-changelog --yarn --dev --exact`

- Adicionar _hook_ para execução ao chamar `commit` no `git`:

    ```json
        "husky": {
            "hooks": {
                "prepare-commit-msg": "exec < /dev/tty && git cz --hook || true",
            }
        }
    ```

    - Executar comando `git commit` e `ENTER` para execução do _cli_.

> _Custom_ configs para _commits_

- Criar arquivo `commitlint.config.js` na raiz para personalizar o _commitlint_:

    ```javascript
        module.exports = {
        parserPreset: 'conventional-changelog-conventionalcommits',
        rules: {
            // ...configurações
        }
    };
    ```

- Criar arquivo `changelog.config.js` para personalizar o _commitizen_:

    ```javascript
    module.exports = {
        // ...configurações
    };

    ```