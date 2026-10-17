# Evidências e imagens opcionais

Não é necessário ter screenshots para produzir um relatório profissional. Caso o Windows bloqueie capturas de terminais elevados, **não desabilite controles de segurança** para contornar isso. Prefira transcrições textuais reais revisadas e diagramas produzidos para explicar o processo.

Se optar por prints, cada captura deve mostrar **comando + resultado** sem senhas, IPs, nomes de usuário ou caminhos pessoais.

1. `01-porta-445.png` — Nmap identificando TCP/445 na máquina do lab.
2. `02-compartilhamentos.png` — `Get-SmbShare` com compartilhamentos padrão e `CS_LAB`.
3. `03-permissao-compartilhamento.png` — `Get-SmbShareAccess -Name CS_LAB` com `Read`.
4. `04-configuracao-smb.png` — SMB1 desabilitado, SMB2 habilitado, assinatura exigida.
5. `05-acesso-c-negado.png` — `smbclient` retorna `NT_STATUS_ACCESS_DENIED` para `C$`.
6. `06-leitura-cs-lab.png` — `smbclient` lista `evidencia.txt` no `CS_LAB`.
7. `07-escrita-negada.png` — tentativa de `put` + retorno real (**opcional**); já existe [evidência textual sanitizada](../evidencias/09-escrita-negada.md).

## Produção

- Windows: `Win + Shift + S` para capturar a região do terminal.
- Salvar os PNGs em `evidencias/`, usando os nomes acima.
- Cortar informações sem relação com o teste.
- Ocultar endereço IP real se preferir, hostname, nomes pessoais, caminhos sensíveis, endereços MAC e outras máquinas da rede.
- Não criar prints simulados; capturas devem corresponder a comandos que você realmente executou.
- Revisar todas as imagens antes de enviar o repositório ao GitHub.

## Uso no README

Após a revisão, as imagens podem ser inseridas com Markdown, por exemplo:

```markdown
![SMB C$ — acesso negado](evidencias/05-acesso-c-negado.png)
```
