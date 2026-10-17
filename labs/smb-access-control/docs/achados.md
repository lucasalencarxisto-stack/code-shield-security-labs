# Achados, interpretação e limites

## AC-01 — Serviço SMB identificado (informativo)

**Evidência:** porta TCP 445 aberta no escaneamento a partir do próprio host, com atendimento do serviço SMB.

**Interpretação:** serviço disponível; não há exploração confirmada nem prova de acesso à internet.

**Classificação:** informativo.

## AC-02 — Acesso administrativo negado (controle validado)

**Evidência:** ao usar `lab_smb` e tentar listar `C$`, o `smbclient` retornou `NT_STATUS_ACCESS_DENIED`.

**Interpretação:** a conta do laboratório não pôde listar arquivos do compartilhamento administrativo na tentativa observada.

**Classificação:** controle de autorização validado nesse teste.

## AC-03 — Compartilhamento de leitura permitido (controle validado)

**Evidência:** `CS_LAB` com `Read` para `lab_smb` e listagem bem-sucedida de arquivo de teste.

**Interpretação:** autorização para leitura demonstrada. Não se pode inferir autorização para escrita.

**Classificação:** controle validado nesse teste.

## AC-04 — ACLs NTFS herdadas (revisão recomendada)

**Evidência:** o diretório do laboratório possui permissões herdadas amplas para alguns grupos autenticados.

**Interpretação:** verificar permissões efetivas; a ACL NTFS não deve ser confundida com a permissão SMB do compartilhamento.

**Classificação:** ponto de revisão, não vulnerabilidade confirmada.

## AC-05 — Escrita bloqueada para conta de leitura (controle validado)

**Hipótese:** a conta `lab_smb`, com permissão SMB `Read`, não deve conseguir enviar um arquivo de teste ao compartilhamento `CS_LAB`.

**Comando observado:** `smbclient` com opção `-c 'put ... upload-probe.txt'` apontando para `CS_LAB`, usando SMB3 e a conta `lab_smb`.

**Resultado informado pelo operador:** `NT_STATUS_ACCESS_DENIED opening remote file \upload-probe.txt`.

**Interpretação:** a operação de criação/gravação no compartilhamento foi negada. Isso valida o controle neste teste; não substitui uma auditoria geral de ACLs ou de outros usuários.

**Evidência textual:** [registro sanitizado](../evidencias/09-escrita-negada.md).

**Classificação:** controle de escrita validado no cenário testado.

## Limitações gerais

- Testes originados no mesmo notebook, incluindo Ubuntu WSL2; sem prova de alcance a partir de outro computador.
- Não foi realizada auditoria completa de todas as portas ou versões do Windows.
- Não houve exploração de vulnerabilidades nem brute force.
- Não é possível inferir ausência absoluta de falhas com base nesta amostra.
