# Guia para Configurar Acesso SSH à VPS sem Necessidade de Especificar a Chave Privada

Este guia foca principalmente em comandos **bash**. No entanto, se você estiver utilizando o **PowerShell** no Windows, o fluxo será um pouco diferente:

- O caminho para a pasta **home** do usuário deve ser especificado usando **`$env:USERPROFILE\`** em vez de **`~/`**.

- Certifique-se de usar o **`\`** como separador de diretórios, conforme o padrão do Windows.

## Criar o par de chaves na máquina local (a partir da qual você quer acessar a VPS)

## Gerar Par de Chaves SSH

Para gerar o par de chaves SSH, utilize o seguinte comando:

```bash
ssh-keygen -t ed25519 -C "nome_da_key" -f ~/.ssh/minha_chave
# RSA: ssh-keygen -b 4096 -C "nome_da_key" -f ~/.ssh/minha_chave
```

### Parâmetros do comando `ssh-keygen`:

- **`ssh-keygen`**: Utilitário padrão para gerar pares de chaves SSH.

- **`-t ed25519`**: Define o algoritmo ED25519, mais seguro e eficiente. Alternativamente, o RSA pode ser usado para compatibilidade com sistemas legados.

- **`-C "nome_da_key"`**: Adiciona um comentário à chave pública para facilitar a identificação, como um e-mail ou descrição.

- **`-f ~/.ssh/minha_chave`**: Especifica o caminho e nome do arquivo para a chave privada. A chave pública será gerada no mesmo local.

- **`-b 4096`**: Define o tamanho da chave em bits (aplicável apenas ao RSA). Um valor de 4096 bits é recomendado para maior segurança.

### Dicas:

- É recomendado salvar suas chaves no diretório ~/.ssh do seu usuário, pois é o local padrão onde o SSH espera encontrar essas chaves.

- Durante a criação, você será solicitado a definir uma **_passphrase_** (senha), o que adiciona uma camada extra de segurança. Se você não quiser definir uma senha, basta pressionar Enter para pular essa etapa.

## Acesso à VPS usando **senha**

Se você tem acesso à VPS via senha, pode usar `ssh-copy-id` para copiar sua chave pública automaticamente para o servidor.

### Comando para usar `ssh-copy-id`:

```bash
ssh-copy-id -i ~/.ssh/nome_da_nova_chave.pub usuario_vps@ip_da_vps
```

- Isso copiará a chave pública para o arquivo `~/.ssh/authorized_keys` da VPS.

---

### No PowerShell (Windows)

O PowerShell não possui `ssh-copy-id`, então use o comando abaixo:

```powershell
type $env:USERPROFILE\.ssh\id_rsa.pub\nome_da_nova_chave.pub | ssh -i caminho_para_chave_privada_que_acessa_a_vps usuario_vps@ip_da_vps "cat >> ~/.ssh/authorized_keys"
```

- Esse comando envia a chave pública para a VPS e a adiciona ao arquivo `authorized_keys`.

---

## Acesso sem senha

Se você já acessa a VPS usando uma chave privada, adicione a nova chave pública com o seguinte comando:

```bash
cat ~/.ssh/nome_da_nova_chave.pub | ssh -i caminho_para_chave_privada_que_acessa_a_vps usuario_vps@ip_da_vps "cat >> ~/.ssh/authorized_keys"
```

- Se estiver usando PowerShell, siga as instruções da seção [No PowerShell](#no-powershell-windows).
- Esse comando anexará a nova chave pública ao arquivo `authorized_keys` na VPS.

## Caso os Métodos Anteriores Falhem: Adicionar Chave Pública no Painel da VPS

- Acesse o painel de controle do seu provedor de VPS, localize a seção de **_chaves SSH_** e adicione a chave pública copiada. Depois, repita os passos anteriores.

## Acessar a VPS usando a chave priava

Agora, para testar se a chave foi atrelada corretamente, utilize o seguinte comando:

```bash
ssh -i ~/.ssh/nome_da_nova_chave usuario_vps@ip_da_vps
```

## Configurar o arquivo `~/.ssh/config` para evitar passar a chave privada manualmente

No seu computador local, edite (ou crie) o arquivo `~/.ssh/config` com o editor **_vi_** e inclua o seguinte conteúdo:

```bash
Host aliasparavps
    HostName ipdavps
    User usuariovps
    IdentityFile ~/caminho_para_private_key
```

### Explicação dos campos:

- `Host`: Um alias que você escolhe para a VPS (ex: `meu-servidor`).
- `HostName`: O IP ou domínio da sua VPS.
- `User`: O usuário que você usa para acessar a VPS.
- `IdentityFile`: O caminho completo da chave privada usada para se conectar.

## Salvar e sair do arquivo

- Para salvar e sair, pressione ESC, depois digite :wq e pressione Enter.

## Acessar a VPS com o alias configurado

Agora, sempre que quiser acessar a VPS, você pode usar o comando:

```bash
ssh aliasparavps
```

Isso vai usar automaticamente as configurações definidas no arquivo `~/.ssh/config`, sem precisar passar o `-i` com a chave privada toda vez.
