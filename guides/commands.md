# Comandos e instruções git, para um bom funcionamento do projeto

Este guia serve para que todos os integrantes do time, mesmo com pouca ou nenhuma experiência com Git e GitHub, consigam trabalhar no projeto sem travar ou causar problemas no repositório.

---

## 1. Configuração inicial (fazer apenas uma vez por computador)

Antes de tudo, configure seu nome e e-mail no Git. Isso identifica quem fez cada alteração.

```bash
git config --global user.name "Seu Nome"
git config --global user.email "seu-email@exemplo.com"
```

Verifique se está tudo certo:

```bash
git config --list
```

---

## 2. Clonando o projeto pela primeira vez

Se você ainda não tem o projeto no seu computador:

1. No GitHub, entre no repositório do projeto.
2. Clique no botão verde **Code** e copie o link (HTTPS).
3. Abra o terminal na pasta onde você quer salvar o projeto e rode:

```bash
git clone <link-copiado>
```

4. Entre na pasta do projeto:

```bash
cd nome-da-pasta-do-projeto
```

5. Abra a pasta no VSCode:

```bash
code .
```

---

## 3. O que fazer assim que abrir o VSCode (todo dia, antes de começar a trabalhar)

**Sempre** faça isso antes de escrever qualquer código, para garantir que você está com a versão mais atual do projeto:

1. Abra o terminal integrado do VSCode (`Ctrl + '` ou menu **Terminal > New Terminal**).
2. Confira em qual branch você está:

```bash
git branch
```

3. Vá para a branch principal (main ou develop, conforme combinado no time):

```bash
git checkout main
```

4. Atualize com as últimas mudanças do repositório remoto:

```bash
git pull origin main
```

> ⚠️ Nunca comece a trabalhar sem dar `git pull` primeiro. Isso evita conflitos desnecessários depois.

---

## 4. Criando uma branch para trabalhar em uma tarefa

Nunca trabalhe diretamente na branch `main`. Sempre crie uma branch específica para a sua tarefa.

```bash
git checkout -b nome-da-branch
```

Sugestão de nomenclatura:

- `feature/nome-da-funcionalidade` — para novas funcionalidades
- `fix/nome-do-bug` — para correções
- `docs/nome-da-alteracao` — para documentação

Exemplo:

```bash
git checkout -b feature/tela-de-login
```

---

## 5. Salvando suas alterações (commit)

Depois de fazer alterações no código, siga estes passos:

1. Veja quais arquivos foram alterados:

```bash
git status
```

2. Adicione os arquivos que você quer salvar:

```bash
git add .
```

> Se quiser adicionar só um arquivo específico: `git add caminho/do/arquivo.js`

3. Faça o commit com uma mensagem clara, explicando o que foi feito:

```bash
git commit -m "feat: adiciona tela de login"
```

Padrão de mensagens recomendado:

- `feat:` para novas funcionalidades
- `fix:` para correções de bugs
- `docs:` para alterações em documentação
- `style:` para ajustes visuais/formatação
- `refactor:` para refatoração de código sem mudar comportamento

---

## 6. Enviando suas alterações para o GitHub (push)

```bash
git push origin nome-da-branch
```

Na primeira vez que você faz push de uma branch nova, pode ser necessário:

```bash
git push --set-upstream origin nome-da-branch
```

---

## 7. Abrindo um Pull Request (PR)

1. Acesse o repositório no GitHub.
2. Vai aparecer um aviso sugerindo criar um **Pull Request** para a branch que você acabou de enviar. Clique em **Compare & pull request**.
3. Escreva um título e uma descrição explicando o que foi feito.
4. Marque algum integrante do time para revisar (se combinado assim).
5. Clique em **Create pull request**.
6. Aguarde a aprovação antes de fazer o merge (não faça merge sozinho na main sem revisão, salvo combinado diferente pelo time).

---

## 8. Atualizando sua branch com as mudanças da main (evitar conflitos)

Se a branch `main` foi atualizada por outra pessoa enquanto você trabalhava na sua branch, atualize a sua antes de continuar ou antes de abrir o PR:

```bash
git checkout main
git pull origin main
git checkout nome-da-branch
git merge main
```

Se aparecerem conflitos, vá para a seção 9.

---

## 9. Resolvendo conflitos

Quando o Git avisa `CONFLICT`, siga estes passos:

1. Abra os arquivos indicados no VSCode. Você verá marcações assim:

```
<<<<<<< HEAD
código da sua branch
=======
código da outra branch
>>>>>>> main
```

2. Decida qual código deve ficar (ou combine os dois), removendo as marcações `<<<<<<<`, `=======` e `>>>>>>>`.
3. Salve o arquivo.
4. Marque o conflito como resolvido:

```bash
git add nome-do-arquivo
```

5. Finalize o merge:

```bash
git commit
```

6. Envie novamente para o GitHub:

```bash
git push origin nome-da-branch
```

> 💡 Dica: se travar em um conflito difícil, chame outro integrante do time antes de tentar "forçar" qualquer comando. Evite `git push --force` a menos que o time combine isso explicitamente.

---

## 10. Comandos úteis do dia a dia

| Comando                    | O que faz                                               |
| -------------------------- | ------------------------------------------------------- |
| `git status`               | Mostra o que foi alterado e o estado atual              |
| `git branch`               | Lista as branches e mostra em qual você está            |
| `git log --oneline`        | Mostra o histórico de commits de forma resumida         |
| `git diff`                 | Mostra as diferenças exatas nos arquivos alterados      |
| `git checkout nome-branch` | Troca para outra branch                                 |
| `git stash`                | Guarda temporariamente alterações não commitadas        |
| `git stash pop`            | Recupera as alterações guardadas com `git stash`        |
| `git pull origin main`     | Atualiza sua branch atual com o que está na main remota |

---

## 11. Erros comuns e o que fazer

**"fatal: not a git repository"**
Você não está dentro da pasta do projeto. Use `cd` para entrar na pasta correta.

**"Your branch is behind ... "**
Sua branch está desatualizada. Rode `git pull origin main` (ou a branch correspondente).

**"error: failed to push some refs"**
Alguém enviou alterações antes de você. Rode `git pull origin nome-da-branch` primeiro, resolva conflitos se aparecerem, e tente o push novamente.

**Commitei algo errado e quero desfazer o último commit (sem perder o código)**

```bash
git reset --soft HEAD~1
```

**Quero descartar todas as alterações não commitadas de um arquivo**

```bash
git checkout -- nome-do-arquivo
```

> ⚠️ Use com cuidado: isso apaga as alterações não salvas naquele arquivo.

---

## 12. Boas práticas do time

- Sempre dar `git pull` na main antes de criar uma branch nova.
- Nunca commitar diretamente na `main`.
- Fazer commits pequenos e frequentes, com mensagens claras.
- Sempre abrir Pull Request para revisão antes do merge.
- Em caso de dúvida ou conflito complicado, perguntar ao time antes de forçar qualquer comando.
