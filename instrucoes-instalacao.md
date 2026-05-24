# Como instalar no Windows (via WinGet)
O WinGet já vem nativo no Windows 10 e 11. Ele configura o FFmpeg e o FFprobe automaticamente nas suas variáveis de ambiente ($PATH$).

   1. Abra o Terminal: Clique no menu Iniciar, digite PowerShell e abra-o.
   2. Execute o Comando: Copie e cole o comando abaixo:
   
   winget install --id=Gyan.FFmpeg -e
   
   3. Reinicie o Terminal: Feche o PowerShell e abra-o novamente para aplicar as mudanças.
   4. Verifique a Instalação: Use os comandos abaixo para confirmar que ambos estão funcionando:
   
   ffmpeg -version
   
   ffprobe -version
   
   
------------------------------
## Como instalar no Mac (via Homebrew)
No macOS, o gerenciador de pacotes padrão e mais seguro para essa instalação é o Homebrew. Ele instala o FFmpeg e o FFprobe de forma conjunta e automática.

   1. Abra o Terminal: Pressione Command + Barra de Espaço, digite Terminal e clique duas vezes para abrir.
   2. Instale o Homebrew (caso não tenha): Se você nunca usou o Homebrew, instale-lo colando este comando:
   
   /bin/bash -c "$(curl -fsSL https://githubusercontent.com)"
   
   3. Instale o FFmpeg: Com o Homebrew pronto, execute o comando de instalação:
   
   brew install ffmpeg
   
   4. Verifique a Instalação: Certifique-se de que o sistema reconhece as ferramentas rodando:
   
   ffmpeg -version
   
   ffprobe -version
   
   



