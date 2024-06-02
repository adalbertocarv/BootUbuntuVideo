##Instale o VLC:
- O VLC é um reprodutor de mídia que possui suporte robusto para reprodução de vídeos em tela cheia e em loop. Se você ainda não o tem instalado, abra o terminal e execute:

'''sh
sudo apt update
sudo apt install vlc
'''

##Crie um script para iniciar o VLC: 
- Crie um script shell que inicia o VLC com o vídeo desejado em tela cheia e em loop. Abra um editor de texto (por exemplo, Mousepad) e crie um arquivo chamado start_video.sh com o seguinte conteúdo:

  '''sh
#!/bin/bash
vlc --fullscreen --loop /caminho/para/seu/video.mp4
  '''

  - Certifique-se de substituir /caminho/para/seu/video.mp4 pelo caminho real do vídeo que você deseja reproduzir. Salve o arquivo e torne-o executável:
 
  '''sh
chmod +x /caminho/para/start_video.sh
  '''

## Adicione o script aos aplicativos de inicialização: 
- Agora você precisa adicionar o script aos aplicativos de inicialização do Xubuntu. Para fazer isso:

- Abra as Configurações do sistema e vá para "Sessão e Inicialização" (Session and Startup).
- Selecione a aba "Aplicativos de Inicialização" (Application Autostart).
- Clique em "Adicionar" (Add).
- No campo "Nome" (Name), insira algo como "Iniciar Vídeo".
- No campo "Descrição" (Description), você pode colocar uma descrição como "Inicia um vídeo em tela cheia e em loop na inicialização".
- No campo "Comando" (Command), insira o caminho completo para o script que você criou, por exemplo: /caminho/para/start_video.sh.

  ##CONFIGURAR LOGIN SEM SENHA

  ##Abra o arquivo de configuração do LightDM:

- O LightDM é o gerenciador de login padrão no Xubuntu. Você precisará editar o arquivo de configuração dele. Abra um terminal e digite:

'''sh
sudo nano /etc/lightdm/lightdm.conf
''

##Adicione as configurações de login automático:

- No arquivo lightdm.conf, adicione (ou descomente e edite) as seguintes linhas:

'''sh
[Seat:*]
autologin-guest=false
autologin-user=seu_nome_de_usuario
autologin-user-timeout=0
user-session=xubuntu
'''

## Substitua seu_nome_de_usuario pelo nome de usuário da conta que você deseja que faça login automaticamente.

##Salve e feche o arquivo:

- Para salvar o arquivo no Nano, pressione Ctrl + O e depois Enter.
- Para sair do Nano, pressione Ctrl + X.
- Reinicie o computador:

##Após reiniciar, o sistema deve fazer login automaticamente no usuário especificado e iniciar o vídeo conforme configurado anteriormente.
