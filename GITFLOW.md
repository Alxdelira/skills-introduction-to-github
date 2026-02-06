# 📘 Documentação Completa do GitFlow

## 📑 Índice

1. [Introdução](#introdução)
2. [O que é GitFlow?](#o-que-é-gitflow)
3. [Estrutura de Branches](#estrutura-de-branches)
4. [Instalação](#instalação)
5. [Inicializando o GitFlow](#inicializando-o-gitflow)
6. [Workflows Passo a Passo](#workflows-passo-a-passo)
7. [Comandos Essenciais](#comandos-essenciais)
8. [Boas Práticas](#boas-práticas)
9. [Cenários Comuns](#cenários-comuns)
10. [Solução de Problemas](#solução-de-problemas)

---

## Introdução

Esta documentação fornece um guia completo e passo a passo sobre GitFlow, um modelo de branching para Git que facilita o gerenciamento de versões e o desenvolvimento colaborativo de software.

## O que é GitFlow?

GitFlow é um modelo de workflow Git criado por **Vincent Driessen** em 2010. Ele define uma estrutura rigorosa de branches projetada em torno do lançamento de versões do projeto.

### 🎯 Principais Benefícios

- ✅ Organização clara do código
- ✅ Desenvolvimento paralelo eficiente
- ✅ Gestão simplificada de releases
- ✅ Correções rápidas de bugs em produção
- ✅ Histórico de versões bem documentado

---

## Estrutura de Branches

O GitFlow trabalha com cinco tipos principais de branches:

### 1. 🌳 Branch Main (ou Master)

- **Propósito**: Contém o código em produção
- **Ciclo de vida**: Permanente
- **Origem**: Criada na inicialização do repositório
- **Merge de**: Release branches e Hotfix branches

```
main
  |
  └── Código pronto para produção
```

### 2. 🌱 Branch Develop

- **Propósito**: Branch de integração para desenvolvimento
- **Ciclo de vida**: Permanente
- **Origem**: Criada a partir da main
- **Merge de**: Feature branches, Release branches finalizadas, Hotfix branches

```
develop
  |
  └── Última versão de desenvolvimento com features completas
```

### 3. 🚀 Feature Branches

- **Propósito**: Desenvolvimento de novas funcionalidades
- **Ciclo de vida**: Temporária
- **Origem**: Criada a partir da develop
- **Merge para**: develop
- **Nomenclatura**: `feature/<nome-da-feature>`

```
feature/login-system
feature/user-profile
feature/payment-integration
```

### 4. 📦 Release Branches

- **Propósito**: Preparação para um novo release em produção
- **Ciclo de vida**: Temporária
- **Origem**: Criada a partir da develop
- **Merge para**: main e develop
- **Nomenclatura**: `release/<versão>`

```
release/1.0.0
release/1.1.0
release/2.0.0
```

### 5. 🔥 Hotfix Branches

- **Propósito**: Correções urgentes em produção
- **Ciclo de vida**: Temporária
- **Origem**: Criada a partir da main
- **Merge para**: main e develop
- **Nomenclatura**: `hotfix/<descrição>`

```
hotfix/critical-security-fix
hotfix/payment-bug
hotfix/1.0.1
```

---

## Instalação

### Opção 1: Instalação do Git Flow AVH Edition (Recomendado)

#### No Linux (Ubuntu/Debian)

```bash
sudo apt-get update
sudo apt-get install git-flow
```

#### No macOS

```bash
brew install git-flow-avh
```

#### No Windows

1. Baixe o instalador do Git Flow para Windows
2. Ou use o Git Bash e instale via:

```bash
wget -q -O - --no-check-certificate https://raw.github.com/petervanderdoes/gitflow-avh/develop/contrib/gitflow-installer.sh install stable | bash
```

### Opção 2: Uso Manual (Sem instalar extensão)

Você pode seguir o GitFlow manualmente usando comandos Git padrão, conforme mostrado nesta documentação.

---

## Inicializando o GitFlow

### Passo 1: Criar um novo repositório

```bash
# Criar diretório do projeto
mkdir meu-projeto
cd meu-projeto

# Inicializar repositório Git
git init
```

### Passo 2: Criar commit inicial

```bash
# Criar arquivo README
echo "# Meu Projeto" > README.md

# Fazer primeiro commit na main
git add .
git commit -m "Initial commit"
```

### Passo 3: Inicializar GitFlow

#### Usando a extensão Git Flow:

```bash
git flow init
```

Você será questionado sobre os nomes das branches. Use as configurações padrão:

```
Branch name for production releases: [main]
Branch name for "next release" development: [develop]
Feature branches? [feature/]
Bugfix branches? [bugfix/]
Release branches? [release/]
Hotfix branches? [hotfix/]
Support branches? [support/]
Version tag prefix? []
```

#### Manualmente (sem extensão):

```bash
# Criar branch develop a partir da main
git branch develop
git push -u origin develop
```

---

## Workflows Passo a Passo

### 🚀 Workflow 1: Desenvolvendo uma Nova Feature

#### Passo 1: Criar a feature branch

**Com Git Flow:**
```bash
git flow feature start login-system
```

**Manualmente:**
```bash
git checkout develop
git pull origin develop
git checkout -b feature/login-system
```

#### Passo 2: Desenvolver a feature

```bash
# Fazer alterações no código
vim src/login.js

# Adicionar mudanças
git add src/login.js

# Fazer commits
git commit -m "feat: add login form component"
git commit -m "feat: implement authentication logic"
git commit -m "test: add login tests"
```

#### Passo 3: Finalizar a feature

**Com Git Flow:**
```bash
git flow feature finish login-system
```

**Manualmente:**
```bash
# Voltar para develop
git checkout develop

# Fazer merge da feature
git merge --no-ff feature/login-system

# Deletar a branch local
git branch -d feature/login-system

# Enviar para o repositório remoto
git push origin develop
```

#### Passo 4: Publicar feature para colaboração (opcional)

**Com Git Flow:**
```bash
git flow feature publish login-system
```

**Manualmente:**
```bash
git push origin feature/login-system
```

---

### 📦 Workflow 2: Criando um Release

#### Passo 1: Criar a release branch

**Com Git Flow:**
```bash
git flow release start 1.0.0
```

**Manualmente:**
```bash
git checkout develop
git pull origin develop
git checkout -b release/1.0.0
```

#### Passo 2: Preparar o release

```bash
# Atualizar versão no package.json, pom.xml, etc.
vim package.json

# Atualizar CHANGELOG
vim CHANGELOG.md

# Fazer testes finais e correções de bugs
git add .
git commit -m "chore: bump version to 1.0.0"
git commit -m "docs: update CHANGELOG for v1.0.0"
```

#### Passo 3: Finalizar o release

**Com Git Flow:**
```bash
git flow release finish 1.0.0
```

Isso irá:
1. Fazer merge da release na main
2. Criar uma tag com a versão
3. Fazer merge da release de volta na develop
4. Deletar a branch de release

**Manualmente:**
```bash
# Fazer merge na main
git checkout main
git pull origin main
git merge --no-ff release/1.0.0
git tag -a 1.0.0 -m "Version 1.0.0"

# Fazer merge na develop
git checkout develop
git pull origin develop
git merge --no-ff release/1.0.0

# Deletar branch de release
git branch -d release/1.0.0

# Enviar tudo para o repositório remoto
git push origin main develop --tags
```

---

### 🔥 Workflow 3: Aplicando um Hotfix

#### Passo 1: Criar a hotfix branch

**Com Git Flow:**
```bash
git flow hotfix start critical-security-fix
```

**Manualmente:**
```bash
git checkout main
git pull origin main
git checkout -b hotfix/critical-security-fix
```

#### Passo 2: Corrigir o bug

```bash
# Fazer a correção
vim src/security.js

# Commit da correção
git add src/security.js
git commit -m "fix: resolve critical security vulnerability"
```

#### Passo 3: Finalizar o hotfix

**Com Git Flow:**
```bash
git flow hotfix finish critical-security-fix
```

**Manualmente:**
```bash
# Fazer merge na main
git checkout main
git merge --no-ff hotfix/critical-security-fix
git tag -a 1.0.1 -m "Hotfix 1.0.1"

# Fazer merge na develop
git checkout develop
git merge --no-ff hotfix/critical-security-fix

# Deletar branch de hotfix
git branch -d hotfix/critical-security-fix

# Enviar tudo
git push origin main develop --tags
```

---

## Comandos Essenciais

### Comandos Git Flow

#### Features

```bash
# Iniciar uma feature
git flow feature start <nome>

# Publicar uma feature
git flow feature publish <nome>

# Obter uma feature publicada
git flow feature pull origin <nome>

# Finalizar uma feature
git flow feature finish <nome>

# Listar features
git flow feature list
```

#### Releases

```bash
# Iniciar um release
git flow release start <versão>

# Publicar um release
git flow release publish <versão>

# Finalizar um release
git flow release finish <versão>

# Listar releases
git flow release list
```

#### Hotfixes

```bash
# Iniciar um hotfix
git flow hotfix start <versão>

# Finalizar um hotfix
git flow hotfix finish <versão>

# Listar hotfixes
git flow hotfix list
```

### Comandos Git Padrão Equivalentes

| Git Flow | Git Padrão |
|----------|------------|
| `git flow feature start <nome>` | `git checkout develop && git checkout -b feature/<nome>` |
| `git flow feature finish <nome>` | `git checkout develop && git merge --no-ff feature/<nome> && git branch -d feature/<nome>` |
| `git flow release start <v>` | `git checkout develop && git checkout -b release/<v>` |
| `git flow release finish <v>` | Vários comandos de merge e tag |
| `git flow hotfix start <v>` | `git checkout main && git checkout -b hotfix/<v>` |
| `git flow hotfix finish <v>` | Vários comandos de merge e tag |

---

## Boas Práticas

### 1. 📝 Commits

- Use mensagens de commit claras e descritivas
- Siga o padrão [Conventional Commits](https://www.conventionalcommits.org/)
  ```
  feat: adiciona nova funcionalidade
  fix: corrige bug
  docs: atualiza documentação
  style: formatação, ponto e vírgula, etc
  refactor: refatoração de código
  test: adiciona testes
  chore: tarefas de manutenção
  ```

### 2. 🌿 Branches

- Mantenha branches feature curtas e focadas
- Delete branches após o merge
- Use nomes descritivos: `feature/user-authentication` não `feature/fix`
- Sempre comece features a partir da develop atualizada

### 3. 🔄 Merges

- Use `--no-ff` (no fast-forward) para preservar o histórico
- Resolva conflitos localmente antes de fazer push
- Faça code review antes de finalizar features

### 4. 🏷️ Tags

- Use versionamento semântico (MAJOR.MINOR.PATCH)
- Documente mudanças em CHANGELOG.md
- Crie tags anotadas: `git tag -a v1.0.0 -m "Release version 1.0.0"`

### 5. 🔍 Code Review

- Use Pull Requests para features importantes
- Peça revisão de código antes de fazer merge
- Mantenha discussões construtivas

### 6. 📚 Documentação

- Mantenha README.md atualizado
- Documente breaking changes
- Atualize documentação API quando necessário

---

## Cenários Comuns

### Cenário 1: Trabalhar em múltiplas features simultaneamente

```bash
# Feature 1
git flow feature start login
# ... trabalhar na feature login ...
git stash  # Salvar trabalho temporariamente

# Feature 2
git flow feature start payment
# ... trabalhar na feature payment ...
git flow feature finish payment

# Voltar para Feature 1
git checkout feature/login
git stash pop  # Recuperar trabalho
# ... continuar trabalho ...
git flow feature finish login
```

### Cenário 2: Colaboração em uma feature

**Desenvolvedor A:**
```bash
git flow feature start shared-feature
# ... fazer mudanças ...
git flow feature publish shared-feature
```

**Desenvolvedor B:**
```bash
git flow feature pull origin shared-feature
# ... fazer mudanças ...
git push origin feature/shared-feature
```

**Desenvolvedor A:**
```bash
git pull origin feature/shared-feature
# ... fazer mais mudanças ...
git push origin feature/shared-feature
```

**Qualquer desenvolvedor pode finalizar:**
```bash
git flow feature finish shared-feature
```

### Cenário 3: Release com bug crítico encontrado

```bash
# Durante o release
git flow release start 2.0.0

# Bug encontrado!
# Corrigir na própria branch de release
git add .
git commit -m "fix: resolve critical bug found in release testing"

# Continuar com o release
git flow release finish 2.0.0
```

### Cenário 4: Hotfix enquanto há release em andamento

```bash
# Se há release/2.0.0 aberta e você precisa fazer hotfix:

# 1. Criar e finalizar hotfix normalmente
git flow hotfix start 1.5.1
# ... correção ...
git flow hotfix finish 1.5.1

# 2. Fazer merge do hotfix na release também
git checkout release/2.0.0
git merge hotfix/1.5.1

# 3. Continuar com o release
```

### Cenário 5: Desfazer uma feature não finalizada

```bash
# Se você iniciou uma feature mas quer descartá-la
git checkout develop
git branch -D feature/feature-indesejada

# Se já foi publicada
git push origin --delete feature/feature-indesejada
```

---

## Solução de Problemas

### Problema 1: Conflitos ao finalizar feature

**Sintoma:**
```
Auto-merging file.js
CONFLICT (content): Merge conflict in file.js
```

**Solução:**
```bash
# 1. Resolver conflitos manualmente
vim file.js  # Editar e resolver conflitos

# 2. Adicionar arquivos resolvidos
git add file.js

# 3. Continuar o merge
git commit

# 4. Se estava usando git flow
git checkout develop
git merge --no-ff feature/<nome>
```

### Problema 2: Esqueceu de criar branch develop

**Solução:**
```bash
# Criar develop a partir da main
git checkout -b develop main
git push -u origin develop
```

### Problema 3: Commit acidental na branch errada

**Solução:**
```bash
# Copiar o hash do commit
git log  # Ex: abc123

# Voltar o commit
git reset --hard HEAD~1

# Ir para a branch correta
git checkout <branch-correta>

# Aplicar o commit
git cherry-pick abc123
```

### Problema 4: Precisa atualizar feature com mudanças da develop

**Solução (Merge):**
```bash
git checkout feature/<nome>
git merge develop
# Resolver conflitos se houver
```

**Solução (Rebase - mais limpo):**
```bash
git checkout feature/<nome>
git rebase develop
# Resolver conflitos se houver
```

### Problema 5: Release finalizado mas tem bug

**Se ainda não fez deploy:**
```bash
# Reabrir a tag
git tag -d <versão>
git push origin :refs/tags/<versão>

# Criar novo hotfix
git flow hotfix start <nova-versão>
```

**Se já fez deploy:**
```bash
# Criar hotfix normalmente
git flow hotfix start <versão-patch>
# Corrigir e finalizar
```

---

## Fluxo Visual Completo

```
main        ────●────────────────●─────────────●──────>
             1.0.0           1.1.0         1.1.1
                │              │             │
                │              │             │ (hotfix)
                │              │             │
develop     ────●──┬──┬──┬──●──┴──┬──┬──●──┴──┬───>
                │  │  │  │  │     │  │  │     │
                │  │  │  │  │     │  │  │     │
feature/a   ────┘  │  │  │  │     │  │  │     │
                   │  │  │  │     │  │  │     │
feature/b   ───────┘  │  │  │     │  │  │     │
                      │  │  │     │  │  │     │
feature/c   ──────────┘  │  │     │  │  │     │
                         │  │     │  │  │     │
release/1.1.0  ──────────┘  │     │  │  │     │
                            │     │  │  │     │
feature/d   ────────────────┘     │  │  │     │
                                  │  │  │     │
feature/e   ───────────────────────┘  │  │     │
                                      │  │     │
release/1.2.0  ────────────────────────┘  │     │
                                          │     │
hotfix/1.1.1  ─────────────────────────────┘     │
                                                 │
feature/f   ──────────────────────────────────────┘
```

---

## Referências e Recursos Adicionais

### 📚 Documentação Oficial

- [Git Flow Original](https://nvie.com/posts/a-successful-git-branching-model/)
- [Git Flow AVH Edition](https://github.com/petervanderdoes/gitflow-avh)
- [Atlassian Git Flow Tutorial](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)

### 🛠️ Ferramentas

- [GitKraken](https://www.gitkraken.com/) - Cliente Git visual com suporte a GitFlow
- [SourceTree](https://www.sourcetreeapp.com/) - Cliente Git gratuito com GitFlow
- [Git Flow Cheatsheet](https://danielkummer.github.io/git-flow-cheatsheet/)

### 📖 Leitura Recomendada

- [Semantic Versioning](https://semver.org/)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [Git Best Practices](https://git-scm.com/book/en/v2)

---

## Conclusão

O GitFlow é uma metodologia poderosa para gerenciar o desenvolvimento de software de forma organizada e profissional. Embora possa parecer complexo inicialmente, seguir este guia passo a passo tornará o processo natural e eficiente.

### Resumo dos Principais Pontos

✅ **Main**: Código em produção  
✅ **Develop**: Integração de desenvolvimento  
✅ **Feature**: Novas funcionalidades  
✅ **Release**: Preparação para produção  
✅ **Hotfix**: Correções urgentes  

### Quando Usar GitFlow

✔️ Projetos com releases programados  
✔️ Equipes grandes e distribuídas  
✔️ Produtos com múltiplas versões em produção  
✔️ Necessidade de manutenção de versões antigas  

### Quando NÃO Usar GitFlow

❌ Projetos muito simples ou pequenos  
❌ Deployment contínuo (CI/CD extremo)  
❌ Equipes muito pequenas (1-2 desenvolvedores)  
❌ Projetos que não têm releases formais  

Para esses casos, considere workflows mais simples como:
- **GitHub Flow**: main + feature branches
- **GitLab Flow**: Mais focado em ambientes
- **Trunk-Based Development**: Commits diretos com feature flags

---

## Contribuindo

Encontrou algum erro ou tem sugestões? Contribuições são bem-vindas! Abra uma issue ou pull request.

---

**Última atualização**: 2026  
**Versão**: 1.0.0  
**Autor**: Documentação GitFlow Completa  
**Licença**: MIT

---

**🎉 Parabéns! Você agora tem todo o conhecimento necessário para implementar GitFlow em seus projetos!**
