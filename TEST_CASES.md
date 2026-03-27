# Test Cases – Saucedemo Login

---

| **ID**   | **Scenario**                          | **Username**              | **Password**      | **Expected Result**                                                        |
|:--------:|:--------------------------------------|:-------------------------|:-----------------|:----------------------------------------------------------------------------|
| TC-001   | Valid login – Standard User           | `standard_user`          | `secret_sauce`   | Login successful, redirects to `/inventory`                                 |
| TC-002   | Valid login – Problem User            | `problem_user`           | `secret_sauce`   | Login successful, redirects to `/inventory`                                 |
| TC-003   | Valid login – Performance Glitch User | `performance_glitch_user`| `secret_sauce`   | Login successful, redirects to `/inventory`                                 |
| TC-004   | Valid login – Error User              | `error_user`             | `secret_sauce`   | Login successful, redirects to `/inventory`                                 |
| TC-005   | Valid login – Visual User             | `visual_user`            | `secret_sauce`   | Login successful, redirects to `/inventory`                                 |
| TC-006   | Blocked login – Locked Out User       | `locked_out_user`        | `secret_sauce`   | Shows message: **Sorry, this user has been locked out.**                    |
| TC-007   | Invalid login – Nonexistent User      | `usuario_inexistente`    | `secret_sauce`   | Shows message: **Username and password do not match any user in this service** |
| TC-008   | Invalid login – Wrong Password        | `standard_user`          | `senha_errada`   | Shows message: **Username and password do not match any user in this service** |

---

## 📝 Notes

- All tests access: [https://www.saucedemo.com/](https://www.saucedemo.com/)
- The correct password for all valid users is always `secret_sauce`.
- The user `locked_out_user` should display a blocked message.
- Invalid users or wrong passwords should display the standard error message.

---

> This file covers all automated login scenarios present in the project.
