# Opção 1: Script para Windows (PowerShell)
Para usar, abra o PowerShell, copie todo o código abaixo, cole e aperte Enter:

# 1. Escolha do modo de operação
$modo = Read-Host "Digite [1] para Processar Pasta em Lote ou [2] para Editar Áudio com Cortes/Fade"
if ($modo -eq "1") {
    $ext_original = Read-Host "Digite a extensão dos arquivos originais (ex: mp4, mp3)"
    $ext_final = Read-Host "Digite a extensão final desejada (ex: mkv, mp3)"
    
    Get-ChildItem "*.$ext_original" | ForEach-Object {
        ffmpeg -i $_.FullName -c copy "$($_.BaseName).$ext_final" -y
    }
    Write-Host "Processamento em lote concluído!" -ForegroundColor Green

} elseif ($modo -eq "2") {
    # 2. Entradas para edição de áudio
    $arquivo = Read-Host "Digite o nome do arquivo de áudio com a extensão (ex: musica.mp3)"
    $inicio = Read-Host "Tempo de INÍCIO do corte em segundos (ex: 10)"
    $fim = Read-Host "Tempo de FIM do corte em segundos (ex: 100)"
    $fade_in = Read-Host "Duração do Fade-In em segundos (ex: 3)"
    $fade_out = Read-Host "Duração do Fade-Out em segundos (ex: 3)"
    $bitrate = Read-Host "Qualidade/Compressão em kbps (ex: 128k, 192k)"

    # Cálculos automáticos dos pontos de fade
    $duracao = [double]$fim - [double]$inicio
    $st_out = [double]$fim - [double]$fade_out

    # Execução do comando FFmpeg
    ffmpeg -i "$arquivo" -ss $inicio -to $fim -af "afade=t=in:st=$inicio:d=$fade_in,afade=t=out:st=$st_out:d=$fade_out" -b:a $bitrate "resultado_editado.mp3" -y
    Write-Host "Áudio editado com sucesso! Arquivo salvo como: resultado_editado.mp3" -ForegroundColor Green
} else {
    Write-Host "Opção inválida!" -ForegroundColor Red
}

------------------------------
## Opção 2: Script para Mac (Terminal)
Para usar, abra o Terminal, copie todo o código abaixo, cole e aperte Enter:

echo "Digite [1] para Processar Pasta em Lote ou [2] para Editar Áudio com Cortes/Fade:"
read modo
if [ "$modo" = "1" ]; then
    echo "Digite a extensão dos arquivos originais (ex: mp4, mp3):"
    read ext_orig
    echo "Digite a extensão final desejada (ex: mkv, mp3):"
    read ext_final

    for f in *."$ext_orig"; do
        [ -e "$f" ] || continue
        ffmpeg -i "$f" -c copy "${f%.$ext_orig}.$ext_final" -y
    done
    echo "Processamento em lote concluído!"
elif [ "$modo" = "2" ]; then
    echo "Digite o nome do arquivo de áudio com a extensão (ex: musica.mp3):"
    read arquivo
    echo "Tempo de INÍCIO do corte em segundos (ex: 10):"
    read inicio
    echo "Tempo de FIM do corte em segundos (ex: 100):"
    read fim
    echo "Duração do Fade-In em segundos (ex: 3):"
    read fade_in
    echo "Duração do Fade-Out em segundos (ex: 3):"
    read fade_out
    echo "Qualidade/Compressão em kbps (ex: 128k, 192k):"
    read bitrate

    # Cálculos automáticos dos pontos de fade usando o utilitário bc
    st_out=$(echo "$fim - $fade_out" | bc)

    ffmpeg -i "$arquivo" -ss "$inicio" -to "$fim" -af "afade=t=in:st=$inicio:d=$fade_in,afade=t=out:st=$st_out:d=$fade_out" -b:a "$bitrate" "resultado_editado.mp3" -y
    echo "Áudio editado com sucesso! Arquivo salvo como: resultado_editado.mp3"else
    echo "Opção inválida!"fi



