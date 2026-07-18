# Checklist de execução da migração do compartilhamento de arquivos

## 1. Preparação inicial

- [ ] Confirmar que o computador em Borrazópolis será o servidor da solução.
- [ ] Verificar hardware do servidor: CPU, RAM, armazenamento e estabilidade de energia.
- [ ] Confirmar rede da empresa e disponibilidade de internet.
- [ ] Definir se o servidor usará IP fixo ou reserva de IP no roteador.
- [ ] Identificar a pasta principal de documentos, planilhas, KML e arquivos de operação.
- [ ] Identificar o arquivo "Cadastro" como ativo crítico para testes.
- [ ] Preparar uma cópia inicial dos arquivos importantes em disco externo ou outra pasta segura.
- [ ] Definir os dois usuários pilotos: um em Borrazópolis e um em Marilândia.

## 2. Instalação e configuração do servidor

- [ ] Instalar Ubuntu Server LTS no servidor físico.
- [ ] Definir nome do host do servidor.
  - Exemplo: `sudo hostnamectl set-hostname srv-office`
- [ ] Criar usuário administrativo e configurar senha forte.
- [ ] Configurar rede e IP fixo ou reserva de IP.
- [ ] Testar conectividade do servidor com a rede local e com a internet.
  - Exemplo: `ping -c 3 8.8.8.8`
- [ ] Habilitar atualizações automáticas de segurança.
  - Exemplo: `sudo apt update && sudo apt upgrade -y`
- [ ] Habilitar firewall básico com UFW.
  - Exemplo: `sudo ufw enable`
- [ ] Permitir SSH (porta 22) e as portas necessárias para o Tailscale.
  - Exemplo:
    - `sudo ufw allow 22/tcp`
    - `sudo ufw allow 41641/udp`
- [ ] Criar estrutura inicial de pastas para documentos, planilhas, KML e operação.
  - Exemplo: `sudo mkdir -p /srv/empresa/{documentos,planilhas,kml,operacao}`

## 3. Compartilhamento local com Samba

- [ ] Instalar o pacote Samba no servidor.
  - Comando: `sudo apt update && sudo apt install samba samba-common-bin -y`
- [ ] Confirmar que o serviço do Samba está ativo.
  - Comando: `sudo systemctl status smbd`
- [ ] Criar contas de usuário simples para os usuários autorizados.
  - Exemplo: `sudo useradd -m nomeusuario`
  - Exemplo: `sudo smbpasswd -a nomeusuario`
- [ ] Definir permissões básicas de acesso para cada pasta.
  - Exemplo: `sudo chown -R root:users /srv/empresa`
  - Exemplo: `sudo chmod -R 775 /srv/empresa`
- [ ] Configurar o compartilhamento principal da pasta central.
  - Arquivo de configuração: `/etc/samba/smb.conf`
- [ ] Testar acesso local na rede da empresa.
  - Comando: `sudo systemctl restart smbd`
- [ ] Mapear a pasta compartilhada como unidade de rede nas máquinas Windows de Borrazópolis.
- [ ] Testar abertura e salvamento de documentos Word e Excel.
- [ ] Testar abertura e salvamento de arquivos KML e outros arquivos operacionais.
- [ ] Validar o fluxo com o arquivo "Cadastro".

## 4. Acesso remoto seguro

- [ ] Instalar Tailscale no servidor.
  - Exemplo: `curl -fsSL https://tailscale.com/install.sh | sh`
- [ ] Autenticar o servidor no painel do Tailscale.
  - Exemplo: `sudo tailscale up`
- [ ] Instalar Tailscale nos computadores Windows que irão acessar remotamente.
- [ ] Autenticar os computadores Windows no Tailscale.
- [ ] Confirmar que todos os dispositivos aparecem na administração do Tailscale.
- [ ] Testar conectividade entre o servidor e os computadores remotos via Tailscale.
  - Exemplo: `ping -c 3 <ip-tailscale-do-servidor>`
- [ ] Mapear a unidade compartilhada a partir dos computadores remotos.
- [ ] Testar acesso remoto ao compartilhamento.

## 5. Validação de desempenho remoto

- [ ] Abrir um arquivo típico, como o "Cadastro", pelo acesso remoto.
- [ ] Medir tempo de abertura do arquivo.
- [ ] Salvar o mesmo arquivo e medir tempo de gravação.
- [ ] Registrar os resultados de desempenho.
- [ ] Verificar se o desempenho é suficiente para uso diário.
- [ ] Se o Samba estiver lento, instalar e configurar WebDAV como fallback.
  - Exemplo: `sudo apt install apache2 libapache2-mod-dav-svn libapache2-mod-dav -y`
- [ ] Testar novamente a abertura e o salvamento via WebDAV.
- [ ] Escolher o método mais adequado para o piloto: Samba ou WebDAV.

## 6. Compatibilidade com Excel e macros

- [ ] Adicionar a unidade mapeada como Local de Confiança no Excel em cada máquina Windows.
- [ ] Verificar se as macros do arquivo "Cadastro" continuam sendo aceitas.
- [ ] Habilitar AutoRecover no Excel.
- [ ] Definir intervalo de salvamento automático a cada 5 minutos.
- [ ] Testar o arquivo "Cadastro" com macros em rede.
- [ ] Se necessário, criar uma cópia de teste e avaliar conversão para .xlsb.
- [ ] Orientar os usuários a salvar e fechar o arquivo antes de trocar de rede ou desligar o computador.

## 7. Implantação piloto

- [ ] Iniciar o piloto com um usuário em Borrazópolis e um em Marilândia.
- [ ] Manter o acesso antigo como fallback durante o piloto.
- [ ] Usar o novo acesso de forma regular por duas semanas.
- [ ] Registrar problemas de desempenho, travamentos, permissões ou uso.
- [ ] Manter a regra manual de comunicação para evitar conflitos no arquivo "Cadastro".
- [ ] Revisar se a regra manual de conflitos é suficiente.
- [ ] Avaliar se o sistema atende às necessidades do uso diário.

## 8. Estabilização e expansão

- [ ] Ajustar permissões e estrutura de pastas conforme o uso real.
- [ ] Criar um guia simples com instruções para mapear a unidade e configurar o Excel.
- [ ] Definir rotina mínima de backup.
  - Exemplo: `mkdir -p /backup/empresa && cp -R /srv/empresa /backup/empresa/`
- [ ] Implementar apenas recursos adicionais realmente necessários.
- [ ] Expandir o uso para os demais usuários, se o piloto for aprovado.
