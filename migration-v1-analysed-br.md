# Plano de Migração v1 – Plano de Ação Refinado da Fase 1
Com base em uma discussão, este documento resume as preocupações críticas refinadas para a Fase 1, incorporando seus comentários sobre orçamento, IP estático, itens adiados e o status atual do Ubuntu 26.04. Todos os itens não urgentes (monitoramento, treinamento, segurança avançada etc.) foram conscientemente transferidos para a Fase 2.

# Itens Explicitamente Adiados para a Fase 2 (Nenhuma Ação Necessária Agora)
Item adiado; Racional
Estratégia de backup; Não é uma preocupação no momento; manteremos cópias manuais básicas inicialmente.
Monitoramento / Alertas; Transferido para a Fase 2 para reduzir a complexidade inicial.
Treinamento de usuários e campeão local; Transferido para a Fase 2; instruções verbais básicas serão suficientes para o piloto.
Controle de acesso avançado (permissões de usuário); Transferido para a Fase 2 para evitar complicar demais a configuração inicial.
Fortalecimento de segurança (Fail2ban, SSH com chave); Transferido para a Fase 2; manteremos apenas o firewall UFW mínimo na Fase 1.
Documentação abrangente de manutenção; Transferido para a Fase 2; apenas um guia de referência rápida será criado para o piloto.

# Preocupações Críticas e Recomendações para a Fase 1
A seguir estão as três preocupações específicas que concordamos em explorar em profundidade, além de uma verificação sobre o Tailscale.

## Preocupação: Desempenho do Samba sobre WAN
Contexto: SMB/Samba é conhecido por ser “falante” e pode parecer lento em conexões de internet com alta latência (mesmo com boa largura de banda).

Recomendação para a Fase 1 (abordagem de teste primeiro):

Passo 1 (linha de base): Implantar o Samba sobre o Tailscale conforme o planejado originalmente. Medir o desempenho abrindo um arquivo típico (por exemplo, o Cadastro.xlsm).

Critério de aprovação: o arquivo abre em menos de 3 segundos e é salvo em menos de 2 segundos sobre a WAN.

Passo 2 (fallback – WebDAV): Se o Samba se mostrar muito lento, trocamos para o WebDAV sobre HTTPS.

Por quê: o WebDAV usa HTTP/2, lida melhor com a latência e ainda pode ser mapeado como uma unidade de rede do Windows (usando o assistente “Adicionar Local de Rede”).

Configuração: instalar Apache/nginx com mod_dav, protegê-lo com o Let's Encrypt (SSL).

Custo: gratuito (open-source).

Compatibilidade: o Windows 10/11 mapeia unidades WebDAV nativamente; Excel/Word abrem arquivos diretamente a partir dela.

Ação: testar primeiro o Samba. Se o desempenho falhar, fazemos a transição para o WebDAV (posso fornecer um script de configuração detalhado mediante solicitação).

## Preocupação: Estabilidade e Compatibilidade de Macro do Excel (.xlsm)
Contexto: o “Cadastro” depende de macros VBA. O Excel trata unidades de rede de forma diferente das unidades locais.

Recomendações para a Fase 1:

Locais Confiáveis (crítico): adicionar a unidade de rede mapeada (por exemplo, Z:\) ao Centro de Confiabilidade do Excel → Locais Confiáveis em cada máquina Windows. Sem isso, as macros serão bloqueadas ou gerarão avisos de segurança a cada vez.

Otimização do formato de arquivo: se possível, converter o .xlsm para .xlsb (Excel Binary Workbook).

Benefício: menor tamanho de arquivo e tempos de abertura/ salvamento mais rápidos sobre a rede.

Cuidado: testar uma cópia cuidadosamente para garantir que as macros funcionem de forma idêntica.

AutoRecover: garantir que a opção “Salvar informações de AutoRecovery a cada X minutos” esteja habilitada (defina para 5 minutos) para reduzir a perda de dados durante interrupções da rede.

Mitigação de quedas de rede: orientar os usuários a salvar e fechar explicitamente o arquivo antes de trocar de rede ou desligar as máquinas.

Ação: preparar um script em lote ou um guia simples para adicionar o Local Confiável em cada PC. Testar o .xlsm extensivamente antes de colocar o piloto em produção.

## Preocupação: Bloqueio de Arquivos vs. Gerenciamento Manual de Conflitos
Contexto: você mencionou que o gerenciamento manual de conflitos funciona hoje, mas com acesso remoto simultâneo surge um novo risco de conflitos de versão.

Verificação e recomendação para a Fase 1:

Os padrões do Samba (especificamente oplocks = yes e kernel oplocks = yes) já fornecem um mecanismo básico de bloqueio de arquivos. Isso deve impedir gravações simultâneas na maioria dos casos.

No entanto, o gerenciamento manual de conflitos é atualmente um processo, não uma função técnica.

Recomendação: manter o processo manual, mas acrescentar uma regra informal de comunicação (por exemplo, enviar uma mensagem no WhatsApp para a equipe antes de abrir o Cadastro.xlsm remotamente). Essa é uma solução barata e sem tecnologia que se alinha com a cultura do negócio.

Monitorar durante o piloto se ocorrerem conflitos. Se eles se tornarem frequentes, poderemos revisitar a ativação de bloqueios mais rigorosos ou a implementação de um simples arquivo de texto de “check-out” na Fase 2.

## Verificação: Tailscale + IP Estático
Contexto: você perguntou se o Tailscale ainda funciona com um IP estático.

Confirmação:
Sim, o Tailscale funciona perfeitamente com um IP estático.

O Tailscale é uma VPN em malha de camada 3; ele não se importa se o seu IP público é estático ou dinâmico.

Um IP estático oferece o benefício adicional de conectar diretamente via SSH (ou WebDAV) sem depender dos servidores de relay do Tailscale, caso você precise de um método alternativo de acesso.

Ainda assim, usaremos o Tailscale para o acesso aos arquivos (Samba/WebDAV) para manter o tráfego criptografado e evitar expor diretamente as portas do Samba para a internet.

# Plano de Ação Revisado da Fase 1 (Passo a Passo)
Passo; Tarefa; Detalhes / Ferramentas
1; Configuração do sistema do servidor; Instalar o Ubuntu 26.04 LTS. Habilitar atualizações automáticas de segurança. Configurar o UFW para permitir apenas SSH (porta 22) e as portas do Tailscale (padrão).
2; Implantação do Tailscale; Instalar o Tailscale no servidor e em todos os PCs Windows. Garantir que todos os dispositivos apareçam no painel de administração do Tailscale. Testar o ping entre Borrazópolis e Marilândia usando os IPs do Tailscale.
3; Configuração do Samba (principal); Configurar o compartilhamento do Samba para a pasta central. Mapear como Z:\ nos PCs Windows locais (Borrazópolis) usando o IP local para melhor desempenho.
4; Teste de acesso remoto (Samba); A partir de Marilândia, mapear Z:\ usando o IP do Tailscale do servidor. Testar a abertura e o salvamento do Cadastro.xlsm. Medir o tempo de resposta.
5; Fallback condicional do WebDAV; Se o Passo 4 for lento, configurar o Apache/WebDAV com SSL. Mapear a mesma pasta remotamente usando a URL do WebDAV. Testar novamente. Escolher o método mais rápido para os usuários remotos.
6; Configuração do Excel; Adicionar a unidade Z:\ como Local Confiável em todos os PCs. Opcionalmente, converter uma cópia de teste do banco de dados para .xlsb para comparação.
7; Execução do piloto; Selecionar 1 funcionário em Borrazópolis e 1 em Marilândia para usar o novo sistema por 2 semanas. Manter os compartilhamentos antigos do Windows locais como fallback para rollback.
8; Guia de referência rápida; Criar um PDF de 1 página com screenshots sobre como mapear a unidade e adicionar o Local Confiável (a documentação foi adiada, mas isso é essencial para o piloto).

# Resumo Final das Decisões da Fase 1
Critérios de aceitação para o Samba: usaremos o Samba, a menos que ele pareça incrivelmente lento durante o piloto. Se estiver lento, implantamos o WebDAV.

Macros: o principal gargalo são os Locais Confiáveis — garantimos que isso esteja configurado corretamente.

Bloqueio: confiar nos padrões do Samba + uma regra verbal de comunicação para o arquivo Cadastro.

Infraestrutura: Ubuntu 26.04 LTS + Tailscale + IP estático (o IP estático é opcional, mas pode servir como um fallback confiável para SSH/WebDAV).
