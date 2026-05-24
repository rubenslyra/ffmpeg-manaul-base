## 1. Converter Vídeo (Ex: MP4 para MKV)
Muda o formato do arquivo sem perder qualidade.

* Windows (PowerShell):

ffmpeg -i "entrada.mp4" -c copy "saida.mkv"

* Mac (Terminal):

ffmpeg -i entrada.mp4 -c copy saida.mkv


------------------------------
## 2. Extrair o Áudio de um Vídeo (Ex: Salvar como MP3)
Isola o som do arquivo de vídeo e gera um arquivo de música.

* Windows (PowerShell):

ffmpeg -i "video.mp4" -vn -c:a libmp3lame -q:a 2 "audio.mp3"

* Mac (Terminal):

ffmpeg -i video.mp4 -vn -c:a libmp3lame -q:a 2 audio.mp3


------------------------------
## 3. Reduzir o Tamanho do Vídeo (Compressão)
Diminui o peso do arquivo usando o codec moderno H.265 para manter a imagem nítida.

* Windows (PowerShell):

ffmpeg -i "pesado.mp4" -vcodec libx265 -crf 28 "leve.mp4"

* Mac (Terminal):

ffmpeg -i pesado.mp4 -vcodec libx265 -crf 28 leve.mp4


------------------------------
## 💡 Bônus: Como usar o FFprobe para inspecionar arquivos
Se quiser ver os detalhes técnicos de um vídeo (resolução, taxa de quadros ou codecs), use:

* Comando Universal:

ffprobe -v error -show_format -show_streams "seu_arquivo.mp4"




