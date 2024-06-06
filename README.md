#
 __  __ _______ _______ _______ __  __ 
|  |/  |_     _|       |     __|  |/  |
|     < _|   |_|   -   |__     |     < 
|__|\__|_______|_______|_______|__|\__|
                                       
# Configurando um Vídeo Kiosk em Ubuntu Server

Este guia descreve como configurar um Ubuntu Server para iniciar automaticamente e reproduzir um vídeo em loop usando o MPV.

## Passos para Instalar o MPV e o Xorg

1. **Atualize os pacotes e instale o MPV e o Xorg:**
    ```bash
    sudo apt update
    sudo apt install mpv xorg
    ```

## Criando um Script de Inicialização para o MPV

1. **Crie um arquivo de script, por exemplo, `/home/usuario/startup-video.sh` (substitua `usuario` pelo seu nome de usuário):**
    ```bash
    sudo nano /home/usuario/startup-video.sh
    ```

2. **Adicione o seguinte conteúdo ao script:**
    ```bash
    #!/bin/bash
    xset s off
    xset -dpms
    xset s noblank
    mpv --fs --loop=inf /caminho/para/seu/video.mp4
    ```
    Substitua `/caminho/para/seu/video.mp4` pelo caminho do vídeo que você deseja reproduzir.

3. **Torne o script executável:**
    ```bash
    sudo chmod +x /home/usuario/startup-video.sh
    ```

## Configurando o Script para Ser Executado no Boot

1. **Crie um arquivo de serviço systemd para executar o script na inicialização. Crie um novo arquivo de serviço, por exemplo, `/etc/systemd/system/video-kiosk.service`:**
    ```bash
    sudo nano /etc/systemd/system/video-kiosk.service
    ```

2. **Adicione o seguinte conteúdo ao arquivo de serviço:**
    ```ini
    [Unit]
    Description=Video Kiosk
    After=network.target

    [Service]
    User=usuario
    Group=usuario
    ExecStart=/home/usuario/startup-video.sh
    Restart=always
    Environment=DISPLAY=:0
    Environment=XAUTHORITY=/home/usuario/.Xauthority

    [Install]
    WantedBy=multi-user.target
    ```
    Certifique-se de substituir `usuario` pelo seu nome de usuário.

3. **Habilite e inicie o serviço:**
    ```bash
    sudo systemctl enable video-kiosk.service
    sudo systemctl start video-kiosk.service
    ```

## Passos Adicionais (se necessário)

### Instalar um Gerenciador de Janelas Leve (opcional)

1. **Instale o openbox:**
    ```bash
    sudo apt install openbox
    ```

2. **Atualize o script de inicialização para iniciar o `openbox` antes do MPV:**
    ```bash
    sudo nano /home/usuario/startup-video.sh
    ```
    Adicione o seguinte conteúdo:
    ```bash
    #!/bin/bash
    xset s off
    xset -dpms
    xset s noblank
    openbox &
    mpv --fs --loop=inf /caminho/para/seu/video.mp4
    ```

## Configurando Login Automático

1. **Edite o arquivo de configuração do `getty` para habilitar o login automático:**
    ```bash
    sudo nano /etc/systemd/system/getty@tty1.service.d/override.conf
    ```

    Se o diretório `override.conf` não existir, você pode criá-lo:
    ```bash
    sudo mkdir -p /etc/systemd/system/getty@tty1.service.d
    sudo nano /etc/systemd/system/getty@tty1.service.d/override.conf
    ```

2. **Adicione o seguinte conteúdo ao arquivo `override.conf`:**
    ```ini
    [Service]
    ExecStart=
    ExecStart=-/sbin/agetty --autologin usuario --noclear %I $TERM
    ```
    Substitua `usuario` pelo nome de usuário que você deseja fazer o login automático.

3. **Recarregue o systemd para aplicar as mudanças:**
    ```bash
    sudo systemctl daemon-reload
    sudo systemctl restart getty@tty1.service
    ```

## Garantindo que o Ambiente Gráfico Inicie Automaticamente

1. **Edite o arquivo `.profile` do usuário:**
    ```bash
    nano /home/usuario/.profile
    ```

2. **Adicione o comando `startx` ao final do arquivo `.profile`:**
    ```bash
    if [ -z "$DISPLAY" ] && [ $(tty) = /dev/tty1 ]; then
        startx
    fi
    ```

Com essas configurações, sua máquina Ubuntu Server deve iniciar automaticamente, fazer login no usuário especificado e executar o ambiente gráfico e o MPV para reproduzir o vídeo em loop.

Se precisar de mais alguma coisa ou se encontrar problemas, por favor, me avise!
