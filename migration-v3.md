# Plano de Migração Adaptado para Windows Server

## Objetivo
Migrar o compartilhamento de arquivos de um modelo local restrito para uma solução simples, barata e funcional de acesso remoto, preservando o fluxo atual de trabalho do escritório, especialmente o uso de Word, Excel e arquivos KML.

## Princípios do plano
- Evitar custos com nuvem paga, Microsoft 365 e licenças de Windows Server, quando possível.
- Manter o processo atual de trabalho sem exigir mudanças profundas no uso do Excel e do Word.
- Priorizar simplicidade de instalação, operação e manutenção.
- Usar o servidor físico já previsto em Borrazópolis como ponto central do sistema.
- Preservar a gestão manual de conflitos para o arquivo “Cadastro”, como acontece hoje.
- Fazer uma implantação piloto antes de expandir para todos os usuários.

## Arquitetura recomendada
- Um computador desktop dedicado em Borrazópolis, funcionando como servidor.
- Windows Server como sistema operacional.
- Pasta central compartilhada para documentos, planilhas, arquivos KML e outros arquivos de operação.
- Acesso local via rede da empresa, com mapeamento de unidade no Windows.
- Acesso remoto via VPN simples e segura, preferencialmente com Tailscale, para não expor o compartilhamento diretamente à internet.
- Fallback para WebDAV caso o desempenho remoto do SMB não seja suficiente para o uso diário.

## Fase 1 - Preparação e implantação piloto

### 1. Preparação do servidor
- Confirmar o hardware disponível e o licenciamento do Windows Server.
- Instalar o Windows Server com IP estático ou reserva de IP no roteador.
- Habilitar atualizações automáticas e firewall básico.
- Criar uma estrutura simples de pastas para documentos, planilhas, KML e arquivos de operação.

### 2. Compartilhamento local de arquivos
- Configurar o compartilhamento de arquivos no servidor.
- Criar usuários locais simples, com permissões básicas e fáceis de administrar.
- Mapear a pasta compartilhada como unidade de rede nas máquinas Windows de Borrazópolis.
- Testar abertura e salvamento de documentos Word, planilhas Excel e arquivos KML.

### 3. Acesso remoto
- Configurar acesso remoto seguro entre o servidor e os computadores Windows de Marilândia.
- Usar a mesma pasta compartilhada para acesso remoto, com mapeamento de unidade de rede.
- Testar tempo de resposta e estabilidade do acesso remoto.
- Se o desempenho do SMB estiver ruim em WAN, migrar para WebDAV como alternativa de fallback.

### 4. Compatibilidade com Excel e macros
- Adicionar a unidade mapeada como Local de Confiança no Excel em cada máquina Windows.
- Ativar AutoRecover para reduzir risco de perda de dados em caso de falha de rede.
- Testar o arquivo “Cadastro” com atenção, especialmente se ele depender de macros .xlsm.
- Se necessário, avaliar uma cópia em formato .xlsb para teste comparativo.

### 5. Operação piloto
- Selecionar um usuário em Borrazópolis e outro em Marilândia para usar o novo acesso por duas semanas.
- Manter o acesso antigo como fallback durante o piloto.
- Registrar problemas de desempenho, travamentos, erros de permissão ou dificuldades de uso.
- Manter a regra de comunicação manual para evitar conflitos no arquivo “Cadastro”.

## O que fica mais simples com Windows Server
- O compartilhamento local de arquivos se torna mais natural para ambientes Windows.
- A integração com clientes Windows e com o mapeamento de unidades de rede fica mais direta.
- A compatibilidade com Excel, Word e macros tende a ser melhor em um ambiente Windows nativo.
- A administração pode ser mais intuitiva para uma equipe já acostumada com Windows.

## O que fica mais complexo com Windows Server
- O custo e o licenciamento aumentam em relação a uma solução baseada em Ubuntu.
- A instalação e a configuração inicial exigem mais atenção, principalmente se houver necessidade de integrar usuários e permissões de forma mais robusta.
- O acesso remoto “nativo” pode ficar mais trabalhoso do que usar Tailscale ou WireGuard.
- A manutenção e as atualizações tendem a exigir mais cuidados do que em um ambiente Linux mais simples.

## Recomendações práticas
- Para a fase 1, evitar complexidades como Active Directory, a menos que seja realmente necessário.
- Usar usuários locais simples e permissões básicas.
- Manter o uso de Tailscale para o acesso remoto, para preservar simplicidade e segurança.
- Fazer o piloto com poucos usuários antes de expandir para o restante da equipe.

## Resumo
Com Windows Server, o compartilhamento local fica mais simples e mais alinhado ao ambiente Windows, mas o custo, a licença e a configuração inicial passam a ser um ponto mais relevante. Se a prioridade for simplicidade e baixo custo, o Ubuntu continua sendo a opção mais leve; se a prioridade for integração nativa com o ambiente Windows, o Windows Server pode ser a melhor escolha.