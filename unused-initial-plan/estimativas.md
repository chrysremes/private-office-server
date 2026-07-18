# Estimativas atualizadas de tempo e custo

## Premissas adotadas

- Base da execução do plano conforme o checklist: 30 horas.
- Perfil do consultor: especialista em redes, servidores Linux/Windows, segurança básica e ambientes pequenos de escritório, conforme o papel descrito em [roles.md](roles.md).
- Margem para imprevistos e contingências: 20% sobre o tempo técnico estimado.
- Margem para deslocamento: 6 horas no total, considerando ida/volta e atendimento presencial em campo.
- Valor de referência do consultor: R$ 300/h.

## Estimativa de tempo de execução

| Item | Horas |
|---|---:|
| Execução técnica do plano | 30 |
| Deslocamento | 6 |
| Imprevistos e contingências | 6 |
| Total estimado | 42 |

## Estimativa de custo

### Custo de consultoria

- 42 horas x R$ 300/h = R$ 12.600

### Deslocamento

- Estimativa complementar para transporte e deslocamento em campo: R$ 800 a R$ 1.200

### Custo total estimado

- Faixa provável: R$ 13.400 a R$ 13.800

## Estimativa anual de manutenção

### Premissas para manutenção anual

- Manutenção preventiva simples: atualizações do sistema, checagem de funcionamento do Samba/Tailscale e revisão básica das permissões.
- Suporte esporádico: ajustes pontuais de rede, acesso remoto, Excel/macros e problemas de compartilhamento.
- Não inclui aquisição de hardware novo, licenças de software pagos ou implementação de soluções mais robustas.

### Estimativa de esforço anual

| Item | Estimativa |
|---|---:|
| Manutenção preventiva e revisões | 8 horas/ano |
| Suporte pontual e atendimento de incidentes | 8 horas/ano |
| Contingência para problemas imprevistos | 4 horas/ano |
| Total estimado | 20 horas/ano |

### Custo anual estimado de manutenção

- 20 horas x R$ 300/h = R$ 6.000/ano

### Observações

- Esta estimativa considera um cenário simples e operacional, com baixa complexidade e sem equipe interna dedicada.
- Em um cenário com maior uso remoto, mais usuários ou necessidade de suporte frequente, o custo anual pode subir para cerca de R$ 7.000 a R$ 8.000/ano.

## Observações

- Esta estimativa considera um cenário típico de execução com uso de Samba como solução principal e WebDAV apenas como fallback, se necessário.
- Se houver necessidade de maior complexidade de rede, falha de hardware, configuração adicional de Excel/macros ou implementação de fallback para WebDAV, o tempo pode aumentar para cerca de 45 a 50 horas.
- O valor acima já incorpora uma margem razoável para imprevistos, sem contar aquisições adicionais de hardware ou licenças.
