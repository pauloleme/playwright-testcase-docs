# Test Cases – Saucedemo Login

---

| **ID**   | **Cenário**                              | **Usuário**              | **Senha**      | **Resultado Esperado**                                                        |
|:--------:|:------------------------------------------|:-------------------------|:--------------|:-------------------------------------------------------------------------------|
| TC-001   | Login válido – Standard User              | `standard_user`          | `secret_sauce`| Login realizado com sucesso, redireciona para `/inventory`                     |
| TC-002   | Login válido – Problem User               | `problem_user`           | `secret_sauce`| Login realizado com sucesso, redireciona para `/inventory`                     |
| TC-003   | Login válido – Performance Glitch User    | `performance_glitch_user`| `secret_sauce`| Login realizado com sucesso, redireciona para `/inventory`                     |
| TC-004   | Login válido – Error User                 | `error_user`             | `secret_sauce`| Login realizado com sucesso, redireciona para `/inventory`                     |
| TC-005   | Login válido – Visual User                | `visual_user`            | `secret_sauce`| Login realizado com sucesso, redireciona para `/inventory`                     |
| TC-006   | Login bloqueado – Locked Out User         | `locked_out_user`        | `secret_sauce`| Exibe mensagem: **Sorry, this user has been locked out.**                      |
| TC-007   | Login inválido – Usuário inexistente      | `usuario_inexistente`    | `secret_sauce`| Exibe mensagem: **Username and password do not match any user in this service** |
| TC-008   | Login inválido – Senha errada             | `standard_user`          | `senha_errada`| Exibe mensagem: **Username and password do not match any user in this service** |

---

## 📝 Observações

- Todos os testes acessam: [https://www.saucedemo.com/](https://www.saucedemo.com/)
- O campo de senha correto para todos os usuários válidos é sempre `secret_sauce`.
- O usuário `locked_out_user` deve exibir mensagem de bloqueio.
- Usuários inválidos ou senha incorreta devem exibir mensagem de erro padrão.

---

> Este arquivo cobre todos os cenários de login automatizados presentes no projeto.
