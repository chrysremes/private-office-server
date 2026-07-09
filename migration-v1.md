# Plano de Migração v1

## Objetivo
Migrar o compartilhamento de arquivos de um acesso local restrito para um modelo remoto, seguro e de baixo custo, mantendo a operação atual do escritório e evitando dependência de serviços em nuvem.

## Arquitetura proposta
- Servidor físico em Borrazópolis, rodando Ubuntu Server LTS.
- Compartilhamento de arquivos via Samba para acesso a partir de máquinas Windows.
- Acesso remoto por VPN, preferencialmente WireGuard, para manter o tráfego seguro e simples.
- Mapeamento de unidade de rede no Windows para acesso local e remoto como se fosse um drive externo.
- Armazenamento principal em disco local do servidor, com backup periódico para disco externo.

## Fases da migração
1. Preparação do servidor
   - Reaproveitar ou adquirir um desktop compatível para servir como servidor.
   - Instalar Ubuntu Server LTS com atualizações automáticas habilitadas.
   - Configurar firewall, SSH e monitoramento básico.
   - Definir uma pasta central para documentos, planilhas e arquivos KML.

2. Compartilhamento de arquivos
   - Criar um compartilhamento Samba para os arquivos do escritório.
   - Definir usuários e permissões simples, alinhadas ao modelo de trabalho atual.
   - Garantir que arquivos Excel e Word possam ser abertos e alterados pelo Windows sem exigir mudança no processo.
   - Manter a gestão de conflitos manual, conforme o processo atual.

3. Acesso local em Borrazópolis
   - Configurar o acesso via rede local com mapeamento de unidade no Windows.
   - Testar abertura de arquivos, performance e fluxo diário com os usuários.
   - Ajustar permissões e estrutura de pastas antes de expandir o uso.

4. Acesso remoto de Marilândia do Sul
   - Configurar VPN entre o servidor e os computadores Windows remotos.
   - Habilitar o acesso ao compartilhamento Samba apenas por meio da VPN.
   - Testar o acesso remoto com latência aceitável para uso diário.

5. Segurança e manutenção
   - Usar senhas fortes e contas separadas por usuário.
   - Restringir portas abertas ao necessário: SSH, VPN e Samba.
   - Habilitar atualizações automáticas de segurança.
   - Documentar procedimentos básicos de backup e recuperação.

## Critérios de sucesso
- Baixo custo operacional, sem depender de nuvem paga ou licenças Windows Server.
- Acesso simples para Windows, tanto local quanto remoto.
- Tempo de resposta suficiente para o trabalho diário, com prioridade em estabilidade e manutenção simples.
- Continuidade do processo atual sem grandes mudanças no uso de Excel e Word.

## Próximos passos imediatos
1. Definir o hardware do servidor e o local físico em Borrazópolis.
2. Instalar Ubuntu Server LTS e preparar o compartilhamento inicial.
3. Configurar VPN e testar o acesso remoto antes de disponibilizar para todos os usuários.
4. Migrar os arquivos mais usados e validar o fluxo com o time.
