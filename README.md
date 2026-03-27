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
>>>>>>> 3208020 (Initial project)
