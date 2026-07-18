# Solução adotada - Microsoft 365 Business

## Decisão

A solução final adotada para o Private Office Server foi usar Microsoft 365 Business com licenças reduzidas para os usuários que realmente precisam de acesso.

## Motivo da escolha

Essa abordagem foi considerada a opção mais prática e sustentável porque:

- elimina a necessidade de montar e manter um servidor próprio;
- reduz o esforço operacional e o risco de falhas;
- permite acesso remoto de forma simples, com aplicativos desktop e navegador;
- oferece boa experiência com Word, Excel e compartilhamento de arquivos;
- permite escalar o número de licenças no futuro, se a demanda aumentar.

## O que foi descartado

A ideia de construir uma infraestrutura própria com servidor Linux/Windows, Samba, Tailscale e manutenção local foi descartada porque não era pragmática para o contexto do negócio.

## Como a solução funciona

A proposta é usar:

- Microsoft 365 Business para os usuários principais;
- SharePoint e OneDrive para armazenamento e compartilhamento de arquivos;
- aplicativos Office para edição local ou via navegador;
- permissões simples e organizadas por usuário ou grupo.

## Vantagens principais

- implementação mais rápida;
- menor necessidade de manutenção técnica;
- melhor experiência para trabalho remoto;
- redução de dependência de infraestrutura local;
- flexibilidade para crescer conforme o negócio precisar.

## Próximos passos

1. Definir o grupo inicial de usuários e o número de licenças.
2. Criar a estrutura de arquivos e permissões no Microsoft 365.
3. Migrar os documentos mais importantes.
4. Validar o uso com os usuários pilotos.
5. Expandir o uso quando a operação confirmar que a solução atende às necessidades.

## Resumo executivo

A decisão final foi seguir uma solução SaaS com Microsoft 365, em vez de investir em um servidor próprio. Essa opção foi considerada mais simples, mais rápida de colocar em operação e mais alinhada com o contexto real da empresa.
