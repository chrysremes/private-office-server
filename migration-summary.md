# Resumo da migração do compartilhamento de arquivos

## Contexto

O objetivo inicial era substituir o acesso local restrito por uma solução simples, remota e segura para compartilhamento de arquivos entre Borrazópolis e Marilândia.

## Opção inicial avaliada

A opção inicial considerava:

- um servidor físico em Borrazópolis;
- Ubuntu Server com compartilhamento via Samba;
- acesso remoto seguro com Tailscale ou VPN;
- mapeamento de unidade de rede para Windows;
- uso de Word, Excel e arquivos KML com fluxo operacional semelhante ao atual.

## Por que essa opção não foi adotada

Essa abordagem mostrou-se pouco pragmática por causa de:

- alto esforço de implantação e configuração;
- necessidade de manutenção operacional contínua;
- complexidade para ambientes com pouca estrutura de TI;
- custo e tempo elevados para manter uma solução própria;
- maior risco de problemas de compatibilidade com Excel e macros.

## Decisão final adotada

A solução escolhida foi utilizar Microsoft 365 Business com um número reduzido de licenças, alinhado ao número real de usuários que precisam de acesso.

## Benefícios da solução adotada

- implantação mais rápida e simples;
- baixo esforço operacional;
- acesso remoto nativo por browser e aplicativos desktop;
- melhor compatibilidade com Word e Excel;
- escalabilidade fácil caso o uso cresça no futuro;
- menor dependência de infraestrutura local.

## Direção de implementação

1. Definir os usuários pilotos e o escopo inicial das licenças.
2. Criar um ambiente de trabalho compartilhado com Microsoft 365.
3. Migrar os arquivos mais importantes e validar o fluxo com os usuários.
4. Testar compatibilidade com o arquivo "Cadastro" e demais arquivos críticos.
5. Expandir o uso conforme a operação demonstrar necessidade.

## Critério de sucesso

A solução será considerada adequada se:

- oferecer acesso remoto confiável;
- manter o trabalho diário sem grandes interrupções;
- reduzir a carga operacional em relação a um servidor próprio;
- permitir crescimento simples quando o volume de uso aumentar.
