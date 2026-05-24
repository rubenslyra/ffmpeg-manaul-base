# Opção 1: Script para Windows (PowerShell)
Abra o PowerShell dentro da pasta onde estão seus arquivos, cole o código abaixo e aperte Enter:

# Cria a pasta de saída se ela não existir
$pasta_saida = "Resultados_FFmpeg"if (!(Test-Path $pasta_saida)) { New-Item -ItemType Directory -Path $pasta_saida | Out-Null }

Write-Host "--- SELECIONE A OPÇÃO ---" -ForegroundColor Cyan
Write-Host "1. Processar Pasta em Lote (Conversão)"
Write-Host "2. Editar Áudio (Corte, Fades e Compressão)"
$modo = Read-Host "Digite o número da opção desejada"
if ($modo -eq "1") {
    $ext_original = Read-Host "Digite a extensão dos arquivos originais (ex: mp4, mp3)"
    $ext_final = Read-Host "Digite a extensão final desejada (ex: mkv, mp3)"
    
    Get-ChildItem "*.$ext_original" | ForEach-Object {
        $nome_final = "$pasta_saida\$($_.BaseName)_convertido.$ext_final"
        ffmpeg -i $_.FullName -c copy "$nome_final" -y
    }
    Write-Host "Processamento concluído! Arquivos salvos na pasta '$pasta_saida'." -ForegroundColor Green

} elseif ($modo -eq "2") {
    $arquivo = Read-Host "Digite o nome do arquivo de áudio (ex: musica.mp3)"
    if (!(Test-Path $arquivo)) { Write-Host "Arquivo não encontrado!" -ForegroundColor Red; exit }

    $inicio = Read-Host "Tempo de INÍCIO do corte em segundos (ex: 10)"
    $fim = Read-Host "Tempo de FIM do corte em segundos (ex: 100)"
    $fade_in = Read-Host "Duração do Fade-In em segundos (ex: 3)"
    $fade_out = Read-Host "Duração do Fade-Out em segundos (ex: 3)"
    $bitrate = Read-Host "Qualidade/Compressão em kbps (ex: 128k, 192k)"

    # Cálculos automáticos
    $st_out = [double]$fim - [double]$fade_out
    $basename = [System.IO.Path]::GetFileNameWithoutExtension($arquivo)
    $ext = [System.IO.Path]::GetExtension($arquivo)
    $nome_final = "$pasta_saida\${basename}_editado${ext}"

    ffmpeg -i "$arquivo" -ss $inicio -to $fim -af "afade=t=in:st=$inicio:d=$fade_in,afade=t=out:st=$st_out:d=$fade_out" -b:a $bitrate "$nome_final" -y
    Write-Host "Áudio editado! Salvo em: $nome_final" -ForegroundColor Green
} else {
    Write-Host "Opção inválida!" -ForegroundColor Red
}

------------------------------
# Opção 2: Script para Mac (Terminal)
Abra o Terminal dentro da pasta dos seus arquivos, cole o código abaixo e aperte Enter:

# Cria a pasta de saída se ela não existir
pasta_saida="Resultados_FFmpeg"
mkdir -p "$pasta_saida"

echo "--- SELECIONE A OPÇÃO ---"
echo "1. Processar Pasta em Lote (Conversão)"
echo "2. Editar Áudio (Corte, Fades e Compressão)"
read modo
if [ "$modo" = "1" ]; then
    echo "Digite a extensão dos arquivos originais (ex: mp4, mp3):"
    read ext_orig
    echo "Digite a extensão final desejada (ex: mkv, mp3):"
    read ext_final

    for f in *."$ext_orig"; do
        [ -e "$f" ] || continue
        basename="${f%.$ext_orig}"
        ffmpeg -i "$f" -c copy "$pasta_saida/${basename}_convertido.$ext_final" -y
    done
    echo "Processamento concluído! Arquivos salvos na pasta '$pasta_saida'."
elif [ "$modo" = "2" ]; then
    echo "Digite o nome do arquivo de áudio (ex: musica.mp3):"
    read arquivo
    if [ ! -f "$arquivo" ]; then echo "Arquivo não encontrado!"; exit; fi

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

    # Cálculos automáticos dos pontos de fade
    st_out=$(echo "$fim - $fade_out" | bc)
    ext="${arquivo##*.}"
    basename="${arquivo%.*}"
    nome_final="$pasta_saida/${basename}_editado.$ext"

    ffmpeg -i "$arquivo" -ss "$inicio" -to "$fim" -af "afade=t=in:st=$inicio:d=$fade_in,afade=t=out:st=$st_out:d=$fade_out" -b:a "$bitrate" "$nome_final" -y
    echo "Áudio editado! Salvo em: $nome_final"else
    echo "Opção inválida!"fi


Qual dessas opções seria mais útil para o seu fluxo de trabalho?

