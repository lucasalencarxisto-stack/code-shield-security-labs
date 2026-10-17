# Roteiro de testes — SMB Access Control

> Executar somente em máquina e rede sob sua autorização. Substituir `<IP_DO_LAB>` pelo IP da máquina de testes. Digite senhas apenas no prompt interativo.

## Preparação e verificação no Windows (PowerShell elevado)

```powershell
Get-SmbShare | Select-Object Name, Path, Special
Get-SmbShareAccess -Name "CS_LAB"
Get-SmbServerConfiguration |
  Select-Object EnableSMB1Protocol, EnableSMB2Protocol, RequireSecuritySignature
Get-LocalGroupMember -SID "S-1-5-32-544"
net accounts
```

**Não registrar** informações pessoais, senhas ou tokens nas capturas.

## Reconhecimento de serviço

```bash
nmap -sV -p 445 <IP_DO_LAB>
```

A porta aberta indica serviço TCP disponível **do ponto de origem do teste**, não falha de segurança.

## Enumeração dos compartilhamentos (WSL2)

```bash
smbclient -L //<IP_DO_LAB> -U lab_smb -m SMB3
```

Usar a senha exclusiva da conta de laboratório. A listagem dos nomes não é prova de acesso ao conteúdo.

## Autorização negativa: C$

```bash
smbclient //<IP_DO_LAB>/C$ -U lab_smb -m SMB3 -c 'ls'
```

**Observado:** `NT_STATUS_ACCESS_DENIED`.

## Autorização positiva: CS_LAB

```bash
smbclient //<IP_DO_LAB>/CS_LAB -U lab_smb -m SMB3 -c 'ls'
```

**Observado:** listagem permitida, incluindo o arquivo fictício de evidência.

## Teste de gravação somente com arquivo fictício

```bash
printf '%s\n' "Code Shield: teste de escrita" > /tmp/upload-probe.txt
smbclient //<IP_DO_LAB>/CS_LAB -U lab_smb -m SMB3 \
  -c 'put /tmp/upload-probe.txt upload-probe.txt'
```

**Resultado observado e informado pelo operador:** `NT_STATUS_ACCESS_DENIED opening remote file \upload-probe.txt`. O bloqueio foi confirmado para a tentativa de envio do arquivo de teste; consulte [evidencias/09-escrita-negada.md](../evidencias/09-escrita-negada.md).

Se a gravação funcionar, interrompa os testes, não altere arquivos reais e investigue as ACLs efetivas.

**Observação:** os testes feitos a partir do WSL2 do próprio computador **não equivalem** a um teste a partir de outro dispositivo físico na LAN.

## Encerramento

Ao terminar, remover a conta, o compartilhamento e o diretório **somente após revisar se contêm algo necessário**. Não remover compartilhamentos administrativos padrão.
