Instale os pacotes necessários

Primeiro, certifique-se de que os pacotes necessários estão instalados. Você precisará do mpv, um reprodutor de mídia altamente configurável.

sh
Copiar código
sudo apt update
sudo apt install mpv
Crie o script de reprodução do vídeo

Crie um script que execute o mpv com o vídeo desejado em loop. Supondo que o vídeo está localizado em /home/usuario/videos/video.mp4, o script pode ser algo como:

sh
Copiar código
sudo nano /usr/local/bin/play-video.sh
E adicione o seguinte conteúdo:

sh
Copiar código
#!/bin/bash
mpv --loop=yes /home/usuario/videos/video.mp4
Salve e feche o arquivo, depois torne o script executável:

sh
Copiar código
sudo chmod +x /usr/local/bin/play-video.sh
Crie um serviço systemd

Agora, crie um arquivo de serviço systemd que irá iniciar o script no boot. Crie o arquivo de serviço com:

sh
Copiar código
sudo nano /etc/systemd/system/play-video.service
Adicione o seguinte conteúdo:

ini
Copiar código
[Unit]
Description=Play video in loop on startup
After=network.target

[Service]
ExecStart=/usr/local/bin/play-video.sh
Restart=always
User=usuario
Environment=DISPLAY=:0

[Install]
WantedBy=default.target
Salve e feche o arquivo.

Habilite e inicie o serviço

Habilite o serviço para que ele inicie automaticamente no boot:

sh
Copiar código
sudo systemctl enable play-video.service
E então, inicie o serviço:

sh
Copiar código
sudo systemctl start play-video.service
Reinicie o sistema

Finalmente, reinicie o sistema para testar se o vídeo inicia corretamente em loop:

sh
Copiar código
sudo reboot
Notas adicionais:
Usuário e caminho do vídeo: Certifique-se de substituir usuario e o caminho do vídeo pelos valores corretos no seu sistema.
Drivers de vídeo: Verifique se os drivers de vídeo estão corretamente instalados e configurados para garantir uma boa performance na reprodução do vídeo.
Depuração: Se o vídeo não iniciar corretamente, verifique o status do serviço com sudo systemctl status play-video.service para debugar possíveis problemas.
Seguindo esses passos, seu sistema Armbian no Orange Pi deverá reproduzir o vídeo desejado em looping toda vez que o sistema for iniciado.
