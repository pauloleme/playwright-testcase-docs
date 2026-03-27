# Playwright Saucedemo Test Automation

This project contains automated tests using [Playwright](https://playwright.dev/) for the [saucedemo.com](https://www.saucedemo.com/) website.

## Prerequisites

- [Node.js](https://nodejs.org/) (recommended v16 or higher)
- npm (usually included with Node.js)

## Installation

1. Clone this repository or download the files to your machine.
2. In the terminal, navigate to the project folder.
3. Install dependencies:

```bash
npm install
```

## Running the tests

To run all tests in headless mode:

```bash
npx playwright test
```

To run tests with UI (for debugging):

```bash
npx playwright test --headed
```

## Reports

After running the tests, an HTML report will be generated in the `playwright-report` folder. To view it:

```bash
npx playwright show-report
```

## Test Structure

Tests are located in the `tests/` folder and cover:
- Login with all valid users
- Login with wrong password
- Login with non-existent user
- Login with locked out user

## Documented Test Cases

See the [TEST_CASES.md](./TEST_CASES.md) file for detailed documentation of the implemented test scenarios.

## Customization

You can add more test scenarios by editing or creating files in the `tests/` folder.

---

Questions or suggestions? Open an issue or get in touch!

---

# Playwright Saucedemo Testes

Este projeto contém testes automatizados usando [Playwright](https://playwright.dev/) para o site [saucedemo.com](https://www.saucedemo.com/).

## Pré-requisitos

- [Node.js](https://nodejs.org/) (recomendado v16 ou superior)
- npm (geralmente já incluso com o Node.js)

## Instalação

1. Clone este repositório ou baixe os arquivos para sua máquina.
2. No terminal, navegue até a pasta do projeto.
3. Instale as dependências:

```bash
npm install
```

## Executando os testes

Para rodar todos os testes em modo headless (sem interface gráfica):

```bash
npx playwright test
```

Para rodar os testes com interface gráfica (útil para depuração):

```bash
npx playwright test --headed
```

## Relatórios

Após a execução dos testes, um relatório HTML será gerado na pasta `playwright-report`. Para visualizar:

```bash
npx playwright show-report
```

## Estrutura dos testes

Os testes estão localizados na pasta `tests/` e cobrem:
- Login com todos os usuários válidos
- Login com senha errada
- Login com usuário inexistente
- Login com usuário bloqueado

## Casos de Teste Documentados

Consulte o arquivo [TEST_CASES.md](./TEST_CASES.md) para ver a documentação detalhada dos cenários de teste implementados.

## Personalização

Você pode adicionar mais cenários de teste editando ou criando arquivos na pasta `tests/`.

---

Dúvidas ou sugestões? Abra uma issue ou entre em contato!
