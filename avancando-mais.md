## 1. Processar uma pasta cheia de vídeos de uma vez (Lote)
Para aplicar qualquer comando a todos os arquivos de uma pasta, entre na pasta pelo terminal (cd "caminho/da/sua/pasta") e execute o script correspondente:
## No Windows (PowerShell):
Este script converte todos os arquivos .mp4 da pasta para .mkv de forma automática:

Get-ChildItem *.mp4 | ForEach-Object { ffmpeg -i $_.FullName -c copy "$($_.BaseName).mkv" }

## No Mac (Terminal):

for f in *.mp4; do ffmpeg -i "$f" -c copy "${f%.mp4}.mkv"; done

------------------------------
## 2. Cortar, Comprimir e Aplicar Fades em Áudio (Tudo em um só comando)
Para fazer tudo isso, usamos a biblioteca de filtros do FFmpeg (-af).
Explicação dos parâmetros do exemplo abaixo:

* -ss 00:00:10: Ponto de início do corte (começa em 10 segundos).
* -to 00:01:40: Ponto final do corte (termina em 1 minuto e 40 segundos. Duração total: 90 segundos).
* afade=t=in:st=10:d=3: Fade-in começando no segundo 10 (início do áudio), com duração de 3 segundos.
* afade=t=out:st=97:d=3: Fade-out com duração de 3 segundos. Atenção: O st (tempo de início) do fade-out é calculado com base no tempo global do arquivo original (10s de início + 90s de duração - 3s de fade = 97).
* -b:a 128k: Compressão. Define a qualidade para 128 kbps (padrão excelente e leve para MP3).

## Comando para Windows (PowerShell) e Mac (Terminal):

ffmpeg -i "musica_original.mp3" -ss 00:00:10 -to 00:01:40 -af "afade=t=in:st=10:d=3,afade=t=out:st=97:d=3" -b:a 128k "audio_final.mp3"


