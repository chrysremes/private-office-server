# Guia detalhado de execução da migração do compartilhamento de arquivos

## 1. Objetivo do guia

Este documento transforma o plano de migração em uma execução prática, passo a passo, mantendo a simplicidade, o baixo custo e o fluxo atual de trabalho do escritório. Ele incorpora as decisões refinadas do plano v1 analisado, especialmente no que diz respeito a:

- uso inicial de Samba como opção principal;
- fallback para WebDAV se o desempenho remoto não for suficiente;
- configuração de Excel para trabalhar com unidades mapeadas;
- manutenção da gestão manual de conflitos para o arquivo "Cadastro";
- implantação piloto antes da expansão.

---

## 2. Premissas e princípios que devem ser seguidos

Antes de iniciar, confirmar que o projeto seguirá estes princípios:

- evitar custos com nuvem paga, Microsoft 365 e licenças de Windows Server;
- preservar o uso atual do Excel e do Word sem exigir mudança profunda no processo;
- manter a solução simples de instalar, operar e manter;
- usar o computador servidor já previsto em Borrazópolis como ponto central;
- manter a regra manual de comunicação para evitar conflitos no arquivo "Cadastro";
- fazer um piloto antes de expandir para todos os usuários.

---

## 3. Checklist prévio antes da instalação

### 3.1 Confirmar o hardware do servidor

Verificar se o computador que funcionará como servidor possui:

- processador compatível com o Ubuntu Server;
- pelo menos 8 GB de RAM, preferencialmente 16 GB;
- armazenamento suficiente para os arquivos do escritório e espaço de reserva;
- conexão estável com a rede da empresa;
- fonte de alimentação confiável ou UPS, se possível.

### 3.2 Confirmar a rede

- definir se o servidor terá IP fixo ou reserva de IP no roteador;
- confirmar que o servidor consegue acessar a internet;
- confirmar que os computadores de Borrazópolis e Marilândia conseguem alcançar o servidor por rede local ou por VPN.

### 3.3 Preparar os ativos e a equipe

- identificar a pasta principal de documentos e o arquivo "Cadastro";
- definir quais usuários participarão do piloto;
- preparar uma cópia inicial dos arquivos importantes em mídia externa ou cópia manual.

### 3.4 Definir o que não será implementado ainda

Os seguintes itens foram explicitamente deixados para a Fase 2, portanto não devem consumir esforço agora:

- estratégia completa de backup;
- monitoramento e alertas;
- treinamento formal e indicação de campeão local;
- controle avançado de permissões por usuário;
- hardening avançado como Fail2ban e SSH com chave;
- documentação extensa de manutenção.

---

## 4. Fase 1 - Preparação do servidor

### Passo 1: Instalar o Ubuntu Server LTS

1. Carregar o Ubuntu Server LTS em um pendrive bootável.
2. Instalar o sistema no servidor físico em Borrazópolis.
3. Definir nome do host, por exemplo: `srv-office`.
4. Criar um usuário administrativo e definir senha forte.
5. Concluir a instalação com rede ativa.

### Passo 2: Configurar rede e IP

1. Definir o servidor com IP fixo ou reservar o IP no roteador.
2. Confirmar que o nome do host e o IP estão corretamente registrados na rede.
3. Testar conectividade com os computadores da empresa e com a internet.

> A recomendação do plano refinado é manter a opção de IP estático como boa prática, embora Tailscale também funcione sem depender disso.

### Passo 3: Habilitar atualizações automáticas de segurança

1. Ativar atualizações automáticas do sistema.
2. Garantir que o servidor fique protegido com patches regulares.
3. Manter o sistema atualizado para reduzir riscos operacionais.

### Passo 4: Configurar o firewall básico

1. Habilitar o firewall UFW.
2. Permitir apenas o necessário para a operação inicial:
   - SSH na porta 22;
   - portas do Tailscale;
   - portas de WebDAV apenas se houver necessidade futura.
3. Manter a abertura de portas desnecessárias inativa.

### Passo 5: Criar a estrutura inicial de pastas

Criar uma estrutura simples e previsível, por exemplo:

- /srv/empresa/documentos
- /srv/empresa/planilhas
- /srv/empresa/kml
- /srv/empresa/operacao

Essa estrutura deve refletir o uso real do escritório e facilitar a gestão das permissões.

### Passo 6: Preparar o primeiro backup simples

Antes de publicar o compartilhamento, criar uma cópia inicial dos arquivos importantes em um disco externo ou em outra pasta local segura.

- copiar os dados mais críticos;
- registrar onde está a cópia;
- manter esse backup como medida inicial, mesmo que simples.

---

## 5. Fase 2 - Compartilhamento local de arquivos

### Passo 7: Instalar o Samba

No servidor Ubuntu, instalar o Samba:

1. Atualizar os repositórios.
2. Instalar o pacote Samba.
3. Confirmar que o serviço está ativo.

### Passo 8: Criar usuários e permissões simples

1. Criar contas de usuário simples para os funcionários que acessarão o compartilhamento.
2. Definir senhas temporárias e orientações para troca posterior.
3. Garantir que cada usuário tenha acesso apenas às pastas necessárias.

### Passo 9: Configurar o compartilhamento

Configurar um compartilhamento principal para a pasta central, com:

- nome do compartilhamento claro;
- permissões adequadas para leitura e escrita;
- acesso restrito aos usuários autorizados;
- visibilidade simples para facilitar o uso.

A configuração deve ser pensada para não complicar a operação diária.

### Passo 10: Testar o compartilhamento localmente

Nas máquinas Windows de Borrazópolis:

1. mapear a pasta compartilhada como unidade de rede;
2. abrir documentos Word e Excel;
3. salvar arquivos;
4. testar arquivos KML e outros formatos usados no escritório;
5. verificar se o fluxo atual continua funcionando sem grandes mudanças.

### Passo 11: Validar o fluxo com o arquivo "Cadastro"

Este é um ponto crítico do projeto.

1. abrir o arquivo "Cadastro" a partir do compartilhamento;
2. testar abertura e salvamento;
3. verificar se o arquivo funciona corretamente em rede;
4. registrar qualquer problema de performance ou compatibilidade.

---

## 6. Fase 3 - Acesso remoto seguro

### Passo 12: Instalar o Tailscale no servidor

1. instalar o Tailscale no servidor Ubuntu;
2. autenticar a máquina na rede Tailscale;
3. confirmar que o servidor aparece na interface administrativa do Tailscale.

### Passo 13: Instalar o Tailscale nos computadores Windows

1. instalar o Tailscale em cada computador Windows que precisará acessar o compartilhamento;
2. autenticar os dispositivos;
3. verificar que todos aparecem na administração do Tailscale.

### Passo 14: Testar conectividade entre os locais

1. verificar se o servidor e os computadores de Marilândia conseguem fazer ping entre si via Tailscale;
2. testar troca de arquivos entre os dispositivos;
3. confirmar que a conexão segura está funcionando antes de começar a usar o compartilhamento de forma regular.

### Passo 15: Mapear a unidade remota

A partir dos computadores Windows em Marilândia:

1. mapear a pasta compartilhada usando o IP Tailscale do servidor ou o nome de dispositivo do Tailscale;
2. testar o acesso remoto;
3. verificar se a unidade responde corretamente.

---

## 7. Fase 4 - Teste de desempenho remoto e fallback

### Passo 16: Fazer o teste inicial com Samba

Usar como base o fluxo real de trabalho:

1. abrir um arquivo típico, como o arquivo "Cadastro";
2. medir o tempo de abertura;
3. salvar o arquivo e medir o tempo de gravação;
4. registrar os resultados.

### Passo 17: Definir o critério de aceitação

O Samba será aceito para o piloto se atender, de forma satisfatória, aos critérios:

- abertura do arquivo em menos de 3 segundos;
- salvamento em menos de 2 segundos;
- estabilidade razoável durante o uso remoto.

Se esses critérios não forem atingidos, a solução deve passar para o fallback.

### Passo 18: Implementar o fallback para WebDAV, se necessário

Se o Samba parecer lento ou pouco estável:

1. instalar Apache ou nginx com suporte a WebDAV;
2. configurar HTTPS com Let's Encrypt;
3. montar o compartilhamento via WebDAV;
4. testar novamente a abertura e o salvamento do arquivo.

A recomendação do plano refinado é usar o WebDAV como alternativa viável e gratuita, com boa compatibilidade com Windows.

---

## 8. Fase 5 - Compatibilidade com Excel e macros

### Passo 19: Configurar o Excel para confiar na unidade mapeada

Em cada máquina Windows:

1. abrir o Excel;
2. acessar a Central de Confiabilidade;
3. adicionar a unidade mapeada como Local de Confiança;
4. aplicar a configuração para todos os usuários ou em cada estação conforme o ambiente.

Esse passo é crítico, porque sem ele as macros podem ser bloqueadas ou gerar avisos frequentes.

### Passo 20: Ativar o AutoRecover

Em cada computador Windows:

1. habilitar o AutoRecover;
2. definir o intervalo para salvar automaticamente a cada 5 minutos;
3. garantir que o recurso esteja ativo antes do uso remoto.

Isso reduz o risco de perda de dados em eventuais quedas de conexão.

### Passo 21: Testar o arquivo "Cadastro" com atenção

1. abrir o arquivo com macros;
2. validar os principais fluxos de uso;
3. verificar se as macros continuam funcionando corretamente;
4. registrar qualquer comportamento inesperado.

### Passo 22: Avaliar a conversão para .xlsb, se necessário

Se o arquivo for grande ou apresentar lentidão:

1. criar uma cópia de teste;
2. converter para .xlsb;
3. validar se as macros continuam funcionando;
4. comparar desempenho com o formato atual.

Essa etapa é opcional e deve ser tratada como teste, não como mudança obrigatória.

### Passo 23: Orientar os usuários sobre uso em rede instável

Antes do piloto, orientar os usuários a:

- salvar e fechar o arquivo antes de trocar de rede;
- evitar desligar o computador com o arquivo aberto;
- informar imediatamente qualquer problema de conexão.

---

## 9. Fase 6 - Implantação piloto

### Passo 24: Selecionar os usuários pilotos

Escolher:

- um usuário em Borrazópolis;
- um usuário em Marilândia.

### Passo 25: Executar o piloto por duas semanas

Durante o piloto:

1. usar o novo acesso de forma regular;
2. manter o acesso antigo como fallback;
3. registrar problemas de:
   - desempenho;
   - travamentos;
   - erros de permissão;
   - problemas de uso com o Excel.

### Passo 26: Manter a regra manual de conflitos

Para o arquivo "Cadastro", manter o processo atual de gestão manual.

Recomendação prática:

- antes de abrir o arquivo remotamente, avisar o time por WhatsApp ou outro canal de comunicação;
- evitar abertura simultânea do arquivo por mais de uma pessoa;
- registrar se essa regra é suficiente ou se o problema cresce.

### Passo 27: Avaliar os resultados do piloto

Ao fim do piloto, revisar:

- se o uso remoto foi aceitável;
- se a performance foi suficiente;
- se o Excel e as macros funcionaram como esperado;
- se os usuários conseguiram operar sem grande dificuldade.

---

## 10. Fase 2 e estabilização

Depois do piloto aprovado, entrar na fase de estabilização com foco em simplicidade.

### Passo 28: Ajustar permissões e estrutura de pastas

- ajustar as permissões de pasta conforme o uso real;
- reorganizar o conteúdo se necessário;
- evitar criar uma estrutura complexa demais.

### Passo 29: Criar um guia rápido para os usuários

Criar uma folha simples com:

- passo a passo para mapear a unidade;
- como adicionar a unidade como Local de Confiança no Excel;
- instruções básicas de uso remoto;
- contatos para suporte inicial.

### Passo 30: Definir rotina mínima de backup

Mesmo que simples:

- manter cópias periódicas em disco externo;
- registrar onde estão as cópias;
- definir quem é responsável por preservar os dados.

### Passo 31: Implementar apenas o que for realmente necessário

Não introduzir recursos adicionais antes da necessidade aparecer. O objetivo é manter a operação estável e simples.

---

## 11. Fase 3 - Expansão para os demais usuários

Quando o piloto tiver sido bem sucedido:

1. abrir o acesso para os demais usuários;
2. manter o mesmo processo manual de conflitos;
3. evitar mecanismos mais complexos de bloqueio ou controle de versões no início.

---

## 12. Critérios de sucesso do projeto

A migração pode ser considerada bem sucedida se:

- o sistema funcionar com baixo custo;
- os usuários Windows conseguirem acessar os arquivos local e remotamente;
- os arquivos Word e Excel abrirem e salvarem corretamente;
- o desempenho remoto for suficiente para o uso diário;
- a manutenção continuar simples o bastante para ser feita sem equipe de TI dedicada.

---

## 13. Resumo prático das decisões prioritárias

- usar Samba como opção principal;
- usar Tailscale como camada de acesso remoto seguro;
- usar WebDAV apenas como fallback se o Samba for muito lento;
- garantir que o Excel reconheça a unidade mapeada como Local de Confiança;
- habilitar AutoRecover para reduzir risco de perda de dados;
- manter a gestão manual de conflitos para o arquivo "Cadastro";
- dar prioridade ao piloto antes da expansão.
