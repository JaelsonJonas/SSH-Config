
# Guia para Configurar Acesso SSH à VPS sem Necessidade de Especificar a Chave Privada

## 1. Criar o par de chaves na máquina local (a partir da qual você quer acessar a VPS)

### Comando para criar as chaves:

```bash
ssh-keygen -b 4096
```

- **Salve a chave** no local desejado. É recomendado salvar em `~/.ssh` do seu usuário.
- Isso criará dois arquivos: um com a chave privada e outro com a chave pública.

## 2. Copie a chave pública

- O conteúdo do arquivo com a extensão `.pub` é a sua **chave pública**. Copie o conteúdo desse arquivo.

## 3. Adicionar a chave pública ao seu provedor de VPS

- No painel de controle da sua VPS, procure a seção de **chaves SSH** e adicione a chave pública copiada anteriormente.

## 4. Caso você tenha acesso à VPS usando **senha**

Se você pode acessar a VPS com senha, você pode usar o `ssh-copy-id` para copiar sua chave pública automaticamente para o servidor.

### Comando para usar `ssh-copy-id`:

```bash
ssh-copy-id -i ~/.ssh/nome_da_nova_chave.pub usuario_vps@ip_da_vps
```

- Você será solicitado a inserir a senha da VPS. O `ssh-copy-id` então adicionará a chave pública ao arquivo `authorized_keys` do servidor.

## 5. Caso não haja senha para logar na VPS

Se você já tem uma chave privada que usa para se conectar à VPS, utilize o comando abaixo para adicionar a nova chave pública ao servidor:

```bash
cat ~/.ssh/nome_da_nova_chave.pub | ssh -i caminho_para_chave_privada_que_acessa_a_vps usuario_vps@ip_da_vps "cat >> ~/.ssh/authorized_keys"
```

Esse comando copia sua chave pública para o arquivo `authorized_keys` na VPS.

## 6. Configurar o arquivo `~/.ssh/config` para evitar passar a chave privada manualmente

No seu computador local, edite (ou crie) o arquivo `~/.ssh/config` com o seguinte conteúdo:

```bash
Host aliasparavps
    HostName ipdavps
    User usuariovps
    IdentityFile ~/caminhoparaprivatekey
```

### Explicação dos campos:

- `Host`: Um alias que você escolhe para a VPS (ex: `meu-servidor`).
- `HostName`: O IP ou domínio da sua VPS.
- `User`: O usuário que você usa para acessar a VPS.
- `IdentityFile`: O caminho completo da chave privada usada para se conectar.

## 7. Salvar e sair do arquivo

- Salve o arquivo (`Ctrl + O` e `Enter` no Nano) e saia (`Ctrl + X`).

## 8. Acessar a VPS com o alias configurado

Agora, sempre que quiser acessar a VPS, você pode usar o comando:

```bash
ssh aliasparavps
```

Isso vai usar automaticamente as configurações definidas no arquivo `~/.ssh/config`, sem precisar passar o `-i` com a chave privada toda vez.
