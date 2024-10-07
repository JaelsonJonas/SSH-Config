# Guia para Configurar Acesso SSH à VPS sem Necessidade de Especificar a Chave Privada

## Criar o par de chaves na máquina local (a partir da qual você quer acessar a VPS)

## Gerar Par de Chaves SSH

Para gerar o par de chaves SSH, utilize o seguinte comando:

```bash
ssh-keygen -t ed25519 -C "nome_da_key" -f ~/.ssh/minha_chave
# RSA: ssh-keygen -b 4096 -C "nome_da_key" -f ~/.ssh/minha_chave
```

#### Explicação dos parâmetros utilizados:

- **`ssh-keygen`**: Esse é o utilitário padrão para gerar pares de chaves SSH.

- **`-t ed25519`**: Define o **tipo de algoritmo** a ser usado para gerar o par de chaves.

  - Aqui, o algoritmo **ED25519** é utilizado, que é altamente seguro e mais eficiente em termos de desempenho e tamanho de chave.

  - Alternativamente, você pode usar **RSA** com o comando comentado abaixo (não recomendado para novos projetos, mas amplamente compatível com sistemas legados).

- **`-C "nome_da_key"`**: Este parâmetro adiciona um **comentário** à chave pública gerada.

  - Esse comentário ajuda a identificar a chave posteriormente, como quando múltiplas chaves são usadas. Ele pode ser algo descritivo como seu e-mail ou uma descrição da chave, ex: `"minha_chave_trabalho"`.

- **`-f ~/.ssh/minha_chave`**: Especifica o **caminho e o nome do arquivo** para a chave privada gerada.

  - Nesse caso, a chave privada será salva em `~/.ssh/minha_chave` e a chave pública correspondente será gerada como `~/.ssh/minha_chave.pub`.

- **`-b 4096`**: Define o tamanho da chave em **bits**.

  - Este parâmetro só é usado com o algoritmo **RSA**. Um tamanho de chave de **4096 bits** é recomendado para uma maior segurança, pois oferece mais resistência a tentativas de quebra.
  - Um valor mais comum é **2048 bits**, mas 4096 bits oferece uma segurança mais robusta a longo prazo.

#### Dicas:

- É recomendado salvar suas chaves no diretório `~/.ssh` do seu usuário, pois é o local padrão onde o SSH espera encontrar essas chaves.
- Durante a criação, você será solicitado a definir uma **passphrase** (senha), o que adiciona uma camada extra de segurança. Se você não quiser definir uma senha, basta pressionar Enter para pular essa etapa.

Ao usar o comando **ED25519**, você estará optando por uma chave moderna, mais rápida e segura. Se preferir **RSA**, o segundo comando comentado oferece a alternativa, com uma chave de 4096 bits para maior segurança.

## Caso você tenha acesso à VPS usando **senha**

Se você pode acessar a VPS com senha, você pode usar o `ssh-copy-id` para copiar sua chave pública automaticamente para o servidor.

### Comando para usar `ssh-copy-id`:

```bash
ssh-copy-id -i ~/.ssh/nome_da_nova_chave.pub usuario_vps@ip_da_vps
```

- Você será solicitado a inserir a senha da VPS. O `ssh-copy-id` então adicionará a chave pública ao arquivo `authorized_keys` do servidor.

## Caso não haja senha para logar na VPS

Se você já tem uma chave privada que usa para se conectar à VPS, utilize o comando abaixo para adicionar a nova chave pública ao servidor:

```bash
cat ~/.ssh/nome_da_nova_chave.pub | ssh -i caminho_para_chave_privada_que_acessa_a_vps usuario_vps@ip_da_vps "cat >> ~/.ssh/authorized_keys"
```

Esse comando copia sua chave pública para o arquivo `authorized_keys` na VPS.

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
