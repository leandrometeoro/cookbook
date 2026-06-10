# Acesso ao GitHub via `gh` (HTTPS + keyring)

## Contexto

Acesso prático e seguro ao GitHub em qualquer máquina, sem gerenciar chaves
SSH: o GitHub CLI (`gh`) atua como *credential helper* do git via HTTPS. O
token OAuth fica no keyring do sistema (criptografado), é escopado e revogável
por máquina.

## Setup em uma máquina nova

```bash
sudo apt install gh          # ou o pacote equivalente da distro
gh auth login                # GitHub.com → HTTPS → Login with a web browser
                             # (abre github.com/login/device, digite o código)
gh auth setup-git            # registra o gh como credential helper do git
```

Pronto. A partir daí funcionam sem pedir credenciais:

```bash
git clone https://github.com/leandrometeoro/cookbook.git
git pull / git push
gh pr list / gh repo view / gh issue ...
```

!!! note "Repos já clonados via SSH"
    Trocar o remote: `git remote set-url origin https://github.com/<user>/<repo>.git`

    Se o `gh auth status` mostrar "Git operations protocol: ssh", forçar HTTPS:
    `gh config set -h github.com git_protocol https`

## Armadilhas

!!! warning "Máquina remota/headless: o navegador não abre e o login trava"
    Em servidor acessado por SSH, o `gh` tenta abrir um navegador local e
    trava (ex.: "Abrindo em uma sessão de navegador existente."). Solução:

    ```bash
    BROWSER=echo gh auth login
    ```

    Assim o `gh` apenas **imprime a URL** e fica aguardando. Abra
    `https://github.com/login/device` em **qualquer aparelho** (notebook,
    celular), digite o código mostrado no terminal e autorize — o terminal
    remoto conclui sozinho. O código expira em ~15 min; se passar, rode de novo.

!!! note "Colar token parece não funcionar"
    O prompt de token é **invisível** (não ecoa o que foi colado): cole com
    Ctrl+Shift+V e dê Enter mesmo sem ver nada. Token clássico precisa dos
    escopos `repo`, `read:org` e `workflow` — sem `read:org` o gh rejeita.

## Verificação

- `gh auth status` → conta logada, protocolo **https**
- `git fetch origin` num repo → autentica sem pedir senha nem chave

## Segurança

- Cada `gh auth login` gera um token **independente por máquina**. Para
  revogar uma máquina perdida/desativada: GitHub → Settings → Applications →
  Authorized OAuth Apps → GitHub CLI.
- Em servidores **sem keyring** (headless), o token é gravado em texto plano
  em `~/.config/gh/hosts.yml`. Aceitável em máquina pessoal; em máquina
  compartilhada, prefira um token fine-grained de escopo mínimo.

## Duas contas

Contas múltiplas podem coexistir logadas; alternar a ativa:

```bash
gh auth switch
```

O credential helper usa sempre a conta **ativa** para operações git.
