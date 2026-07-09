# Plano de Migração v2

## Objetivo
Migrar o compartilhamento de arquivos de um modelo local restrito para um modelo simples, barato e funcional de acesso remoto, preservando o fluxo atual de trabalho do escritório, especialmente o uso de Word, Excel e arquivos KML.

## Princípios do plano
- Evitar custos com nuvem paga, Microsoft 365 e licenças de Windows Server.
- Manter o processo atual de trabalho, sem exigir mudança profunda no uso do Excel e do Word.
- Priorizar simplicidade de instalação, operação e manutenção.
- Usar o servidor físico já previsto em Borrazópolis como ponto central do sistema.
- Preservar a gestão manual de conflitos para o arquivo "Cadastro", como acontece hoje.
- Fazer uma implantação piloto antes de expandir para todos os usuários.

## Arquitetura recomendada
- Um computador desktop dedicado em Borrazópolis, funcionando como servidor.
- Ubuntu Server LTS com atualizações automáticas de segurança habilitadas.
- Pasta central compartilhada para documentos, planilhas, arquivos KML e outros arquivos de operação.
- Acesso local via rede da empresa, com mapeamento de unidade no Windows.
- Acesso remoto via VPN simples e segura, preferencialmente com Tailscale ou WireGuard, para não expor o compartilhamento diretamente à internet.
- Fallback para WebDAV caso o desempenho remoto do Samba não seja suficiente para o uso diário.

## Fase 1 - Preparação e implantação piloto

### 1. Preparação do servidor
- Confirmar o hardware disponível ou adquirir um desktop compatível para servir como servidor.
- Instalar Ubuntu Server LTS e configurar nome do host, IP estático ou reserva de IP no roteador.
- Habilitar atualizações automáticas e firewall básico.
- Criar uma estrutura simples de pastas para documentos, planilhas, KML e arquivos de operação.
- Preparar um backup simples para disco externo ou cópia manual inicial.

### 2. Compartilhamento local de arquivos
- Configurar um compartilhamento de arquivos no servidor.
- Criar contas de usuário simples, com permissões básicas e fáceis de administrar.
- Mapear a pasta compartilhada como unidade de rede nas máquinas Windows de Borrazópolis.
- Testar abertura e salvamento de documentos Word, planilhas Excel e arquivos KML.

### 3. Acesso remoto
- Configurar acesso remoto seguro entre o servidor e os computadores Windows de Marilândia do Sul.
- Usar a mesma pasta compartilhada para acesso remoto, com mapeamento de unidade de rede.
- Testar tempo de resposta e estabilidade do acesso remoto.
- Se o desempenho do Samba estiver ruim em WAN, migrar para WebDAV como alternativa de fallback.

### 4. Compatibilidade com Excel e macros
- Adicionar a unidade mapeada como Local de Confiança no Excel em cada máquina Windows.
- Ativar AutoRecover para reduzir risco de perda de dados em caso de falha de rede.
- Testar o arquivo "Cadastro" com atenção, especialmente se ele depender de macros .xlsm.
- Se necessário, avaliar uma cópia em formato .xlsb para teste comparativo.

### 5. Operação piloto
- Selecionar um usuário em Borrazópolis e um em Marilândia do Sul para usar o novo acesso por duas semanas.
- Manter o acesso antigo como fallback durante o piloto.
- Registrar problemas de desempenho, travamentos, erros de permissão ou dificuldades de uso.
- Manter a regra de comunicação manual para evitar conflitos no "Cadastro".

## Fase 2 - Estabilização
- Ajustar permissões e estrutura de pastas conforme o uso real.
- Criar um guia simples com screenshots para mapear a unidade e configurar o Excel.
- Definir uma rotina mínima de backup e recuperação.
- Implementar apenas o que for realmente necessário para manter a operação estável.

## Fase 3 - Expansão para o restante da equipe
- Abrir o acesso para os demais usuários conforme os testes forem aprovados.
- Continuar usando o mesmo processo manual de gestão de conflitos.
- Evitar introduzir mecanismos mais complexos de bloqueio ou controle de versões antes de existir necessidade real.

## Critérios de sucesso
- O sistema funciona com baixo custo, sem depender de serviços em nuvem pagos.
- Usuários Windows conseguem acessar os arquivos local e remotamente com mapeamento de rede.
- Arquivos Word e Excel abrem e salvam de forma compatível com o processo atual.
- O desempenho remoto é suficiente para uso diário, ainda que seja inferior ao local.
- A manutenção continua simples o bastante para ser feita sem equipe de TI dedicada.

## Próximos passos imediatos
1. Confirmar o hardware do servidor e o local físico em Borrazópolis.
2. Instalar Ubuntu Server LTS e criar a pasta compartilhada inicial.
3. Configurar o acesso local e validar o fluxo com o arquivo "Cadastro".
4. Ativar o acesso remoto e executar o piloto antes da expansão.
