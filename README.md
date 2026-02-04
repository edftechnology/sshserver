# Como configurar/instalar o `sshserver` no `Linux Ubuntu`

## Resumo

Neste documento estão contidos os principais comandos e configurações para configurar/instalar o `sshserver` no `Linux Ubuntu`.

## _Abstract_

_In this document are contained the main commands and settings to set up/install the `sshserver` on `Linux Ubuntu`._

## Descrição [2]

### `sshserver`

O `sshserver` no `Linux Ubuntu` é um servidor SSH (Secure Shell) que permite a conexão segura com o seu sistema Linux através do protocolo SSH. O SSH é um protocolo de rede que permite a comunicação segura e criptografada entre um cliente e um servidor. Ele é amplamente utilizado para acessar remotamente servidores e dispositivos Linux de forma segura, permitindo a transferência de dados e a execução de comandos de forma segura pela rede. O `sshserver` no `Linux Ubuntu` é responsável por aceitar conexões SSH de clientes remotos e autenticar os usuários, garantindo que apenas pessoas autorizadas tenham acesso ao sistema. Ele também gerencia a criptografia das comunicações para proteger os dados transmitidos.

## 1. Como configurar/instalar o `sshserver` no `Linux Ubuntu` [1][4]

Para instalar e configurar um servidor `SSH` no `Linux Ubuntu`, você pode seguir estas etapas:

1. Abra o `Terminal Emulator`. Você pode fazer isso pressionando:

    ```bash
    Ctrl + Alt + T
    ```


2. Certifique-se de que seu sistema esteja limpo e atualizado.

    2.1 Limpar o `cache` do gerenciador de pacotes `apt`. Especificamente, ele remove todos os arquivos de pacotes (`.deb`) baixados pelo `apt` e armazenados em `/var/cache/apt/archives/`. Digite o seguinte comando:
    
    ```bash
    sudo apt clean
    ```

    2.2 Remover pacotes `.deb` antigos ou duplicados do `cache` local. É útil para liberar espaço, pois remove apenas os pacotes que não podem mais ser baixados (ou seja, versões antigas de pacotes que foram atualizados). Digite o seguinte comando:
    
    ```bash
    sudo apt autoclean
    ```

    2.3 Remover pacotes que foram automaticamente instalados para satisfazer as dependências de outros pacotes e que não são mais necessários. Digite o seguinte comando:
    
    ```bash
    sudo apt autoremove -y
    ```

    2.4 Buscar as atualizações disponíveis para os pacotes que estão instalados em seu sistema. Digite o seguinte comando e pressione `Enter`:
    
    ```bash
    sudo apt update
    ```

    2.5 **Corrigir pacotes quebrados**: Isso atualizará a lista de pacotes disponíveis e tentará corrigir pacotes quebrados ou com dependências ausentes:
    
    ```bash
    sudo apt --fix-broken install
    ```

    2.6 Limpar o `cache` do gerenciador de pacotes `apt` novamente:
    
    ```bash
    sudo apt clean
    ```

    2.7 Para ver a lista de pacotes a serem atualizados, digite o seguinte comando e pressione `Enter`:
    
    ```bash
    sudo apt list --upgradable
    ```

    2.8 Realmente atualizar os pacotes instalados para as suas versões mais recentes, com base na última vez que você executou `sudo apt update`. Digite o seguinte comando e pressione `Enter`:
    
    ```bash
    sudo apt full-upgrade -y
    ```

Para começar a usar o `SSH` e poder monitorar as tentativas de _login_, você precisa primeiro iniciar o serviço e, se desejar, habilitá-lo para que ele inicie automaticamente em futuras inicializações do sistema. Aqui estão os comandos que você pode usar: [3]

1. **Instale o `openssh-server`**: Agora, instale o pacote `openssh-server`, que é o servidor `SSH` para o `Linux Ubuntu`:

    ```bash
    sudo apt install openssh-server -y
    ```

    Isso instalará o servidor `SSH` e todas as dependências necessárias.


2. **Verificar se o `SSH` está ativo:** Mesmo que você tenha o `sshserver`, ele pode não estar ativo. Verifique o _status_ usando:

    ```bash
    sudo systemctl status ssh
    ```

    O _status_ do serviço `SSH` (`ssh.service`) no seu sistema pode mostrar que está `"inactive (dead)"` e `"disabled"`, o que significa que o serviço não está atualmente em execução e também não está configurado para iniciar automaticamente na inicialização do sistema.

3. **Iniciar o serviço `SSH`:** Para iniciar o serviço `SSH` agora, execute:

    ```bash
    sudo systemctl start ssh
    ```

4. **Habilitar o serviço `SSH` na inicialização:** Se você quiser que o `SSH` inicie automaticamente em cada reinicialização, use:
    
    ```bash
    sudo systemctl enable ssh
    ```

5. **Verificar o _status_ novamente:** Após iniciar o serviço, verifique o status novamente para confirmar que está ativo:

    ```bash
    sudo systemctl status ssh
    ```

6. **Monitorar tentativas de _login_:** Com o `SSH` ativo, as tentativas de _login_ serão registradas. Você pode monitorá-las através do `journalctl`. Digite o seguinte comando:

    ```bash
    sudo journalctl -u ssh
    ```

    Para tentativas de _login_ falhas, use:
    
    ```bash
    sudo journalctl -u ssh | grep 'Failed'
    ```

Lembre-se de que o `SSH` é uma ferramenta poderosa, mas também um alvo comum para tentativas de acesso não autorizado. Certifique-se de seguir as práticas recomendadas de segurança, como usar autenticação baseada em chave, alterar a porta padrão do `SSH` e considerar o uso de ferramentas como o `Fail2Ban` para proteção adicional.

O `preset: disabled` na saída do comando `systemctl status` refere-se à configuração padrão (`preset`) do serviço no `systemd`, que é o sistema de `init` usado pelo seu `Linux Ubuntu` para gerenciar serviços de sistema.

Vamos entender o que isso significa:

- **Loaded:** Mostra o caminho do arquivo da unidade de serviço e o seu estado atual de ativação (`enabled` ou `disabled`). No seu caso, `enabled` significa que o serviço SSH foi explicitamente habilitado para iniciar na inicialização do sistema.

- **Preset:** Refere-se à configuração padrão para o serviço, conforme definido pelos presets do `systemd`. No seu caso, `preset: disabled` significa que a configuração padrão para o SSH no seu sistema é não iniciar automaticamente na inicialização. No entanto, como o serviço está explicitamente `enabled`, ele irá iniciar na inicialização, independentemente do seu preset.

Basicamente, o `preset` é uma configuração de nível de sistema que define se serviços devem ser habilitados ou desabilitados por padrão. Porém, essa configuração pode ser sobrescrita por ações explícitas do administrador do sistema, como quando você usa `systemctl enable` para ativar um serviço.

Então, mesmo que o `preset` seja `disabled`, o seu serviço SSH está ativo (`active (running)`) e habilitado `(enabled)`, portanto, iniciará automaticamente quando você reiniciar seu sistema.

## 2. Código completo para configurar/instalar/usar

Para configurar/instalar/usar o `sshserver` no `Linux Ubuntu` sem precisar digitar linha por linha, você pode seguir estas etapas:

1. Abra o `Terminal Emulator`. Você pode fazer isso pressionando:

    ```bash
    Ctrl + Alt + T
    ```

2. Digite o seguinte comando e pressione `Enter`:

    ```bash
    sudo apt clean
    sudo apt autoclean
    sudo apt autoremove -y
    sudo apt update
    sudo apt --fix-broken install
    sudo apt clean
    sudo apt list --upgradable
    sudo apt full-upgrade -y
    sudo systemctl status ssh
    sudo systemctl start ssh
    sudo systemctl enable ssh
    sudo systemctl status ssh
    ```

## Referências

[1] OPENAI. ***Instalar o `ssh` no `linux ubuntu` pelo `terminal emulator`.*** Disponível em: <https://chat.openai.com/c/0516a273-54dc-456c-9a4b-d5fa89bcb965> (texto adaptado). ChatGPT. Acessado em: 16/11/2023 15:56.

[2] OPENAI. ***Vs code: editor popular.*** Disponível em: <https://chat.openai.com/c/b640a25d-f8e3-4922-8a3b-ed74a2657e42> (texto adaptado). ChatGPT. Acessado em: 14/12/2023 08:51.

[3] OPENAI. ***Como eu consulto, pelo terminal, se alguém tentou acessar meu computador no linux?.*** Disponível em: <https://chat.openai.com/c/720d09db-238a-40cc-a97b-c41f71aa6dbf> (texto adaptado). ChatGPT. Acessado em: 14/12/2023 20:29.

[4] OPENAI. ***Comandos de manutenção do ubuntu.*** Disponível em: <https://chat.openai.com/c/4a8cfaa2-30d6-474d-821f-7047a6a39830> (texto adaptado). ChatGPT. Acessado em: 15/12/2023 08:25.

