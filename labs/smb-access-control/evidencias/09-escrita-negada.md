# Evidência 09 — tentativa de gravação bloqueada

**Origem:** resultado fornecido pelo operador a partir do terminal Ubuntu/WSL2.
**Modalidade:** transcrição textual sanitizada; **não é um screenshot**.
**Escopo:** SMB3 do notebook de laboratório; conta não administrativa `lab_smb`.

## Comando executado (com endereço e caminho substituídos por marcadores)

```bash
smbclient //<IP_DO_LAB>/CS_LAB -U lab_smb -m SMB3 \
  -c 'put <ARQUIVO_LOCAL_DE_TESTE> upload-probe.txt'
```

## Retorno original relevante (preservado)

```text
Password for [WORKGROUP\lab_smb]:
NT_STATUS_ACCESS_DENIED opening remote file \upload-probe.txt
```

## Interpretação

O servidor negou a abertura/criação remota de `upload-probe.txt` no compartilhamento `CS_LAB`, quando a conexão utilizava a conta `lab_smb`. Essa conta estava autorizada a ler, mas tinha somente `Read` nas permissões do compartilhamento.

**Limite da conclusão:** o bloqueio da escrita foi observado nessa operação específica e a partir do próprio host por WSL2. Não demonstra que todos os caminhos, identidades e origens são seguros. Não foi verificada exposição a partir de outro computador.

**Privacidade:** endereço IP local e caminho absoluto do arquivo foram substituídos por marcadores. O texto de erro foi preservado. Não foram armazenadas senhas.
