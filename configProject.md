# Configurando dependências do projeto

[Voltar](./README.md)

## Conventional commits

O padrão dos commits será seguindo o modelo convencional commits
[Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)

## Instalação de dependências

- Iniciar o projeto node

  -Y garante que seja criado o packet.json com valores padrão

```bash
npm init -y
```

- Dependência para garantir padrão de commits

```bash
npm i -D git-commit-msg-linter
```

- Dependência para o funcionamento do typescript

```bash
npm i -D typescript @types/node
```

- Arquivo tsconfig.json

```json
{
  "compilerOptions": {
    "outDir": "./dist",
    "module": "commonjs",
    "target": "es2019",
    "esModuleInterop": true,
    "allowJs": true
  }
}
```

- Instalar o eslint

Como o Javascript Standart está descontinuado iremos instalar o eslint padrão para o projeto.

```bash
npm init @eslint/config@latest
```

O vídeo abaixo serve de auxiliador para configurar o passo a passo do ESlint apesar de ser tranquilo de fazer por conta própria.
[Vídeo: configurar ESLint passo a passo](https://www.youtube.com/watch?v=eieTlMwCwWU)

Após a instalação do eslint, é necessário ignorar algumas pastas e arquivos que não serão corrigidos pelo eslint utilizando globalIgnores

o código fica como no exemplo abaixo:

```typescript
import js from "@eslint/js";
import globals from "globals";
import tseslint from "typescript-eslint";
import { defineConfig, globalIgnores } from "eslint/config";

export default defineConfig([
  globalIgnores(["node_modules/", "dist/", "build/", ".env", "*.local"]),
  {
    files: ["**/*.{js,mjs,cjs,ts,mts,cts}"],
    plugins: { js },
    extends: ["js/recommended"],
    rules: {
      "@typescript-eslint/no-explicit-any": "off", // exemplo: desliga a regra só para TS
      "no-var": "error", // reforça também para TS
    },
    languageOptions: { globals: globals.browser },
  },
  tseslint.configs.recommended,
]);
```

- Instalação e configuração do Husky e lint-staged.

```bash
npm i -D husky
```

```bash
npm i -D lint-staged
```

- Instale também o lint-staged para utilizar em conjunto com husky

Após a instalação é necessário utilizar o comando abaixo para gerar a pasta _.husky_

```bash
npx husky init
```

configure os comandos que serão efetuados antes do commit no caminho _.husky > pre-commit_

```bash
lint-staged
```

- Instale o jest para fazer os testes dos códigos

```bash
npm i -D jest @types/jest ts-jest
```

Em seguida inicie o jest no projeto com o comando abaixo

```bash
npx create-jest
```

Será criado o arquivo de configuração chamado jest.config.ts após a conclusão do passo a passo.

segue o arquivo de configuração abaixo:

```json
/**
* For a detailed explanation regarding each configuration property, visit:
* https://jestjs.io/docs/configuration
*/

import type { Config } from "jest";

const config: Config = {
 roots: ["<rootDir>/src/"],
 collectCoverageFrom: ["<rootDir>/src/**/*.ts"],
 coverageDirectory: "coverage",
 testEnvironment: "node",
 transform: {
   ".+\\.ts$": "ts-jest",
 },
};

export default config;

```

Crie ao menos 1 arquivo de teste dentro da pasta src/Controller.spec.ts

```typescript
describe("", () => {
  test("", () => {
    expect(1).toBe(1);
  });
});
```

Crie o script no package.json chamado test:staged

```json
  scripts: {
    "test:staged": "jest --passWithNoTests",
    "test": "jest",
  },
```

E configure o arquivo .lintstagedrc.json na raiz do projeto

```json
{
  "*.ts": ["eslint 'src/**' --fix", "npm run test:staged"]
}
```
