# Otimizando Custos no Azure

Este repositório foi criado como parte do desafio da **DIO (Digital Innovation One)** sobre **otimização e gerenciamento de custos no Microsoft Azure**.

O objetivo é documentar como estimar, acompanhar e controlar os gastos de soluções em nuvem, equilibrando **desempenho, disponibilidade, segurança e orçamento**.

> Este material é um resumo de estudos. Os exemplos apresentados são ilustrativos e não representam necessariamente recursos provisionados durante o laboratório.

## Objetivos do desafio

- Compreender os fatores que influenciam o preço dos serviços Azure.
- Conhecer ferramentas para estimar custos antes de provisionar recursos.
- Monitorar o consumo e identificar desperdícios.
- Utilizar orçamentos, alertas e boas práticas de governança financeira.
- Comparar estratégias de economia para diferentes cargas de trabalho.

## 1. Como funcionam os custos na nuvem?

Na Azure, a cobrança depende do serviço e das condições de uso. Alguns fatores comuns são:

- **Computação:** tipo, tamanho e tempo de utilização de máquinas e serviços.
- **Armazenamento:** capacidade, operações, redundância e camada de acesso.
- **Rede:** principalmente transferências de saída e serviços de conectividade aplicáveis.
- **Região:** preços e disponibilidade podem variar geograficamente.
- **Licenciamento:** alguns sistemas e softwares exigem licenças adicionais.
- **Serviços gerenciados:** modelos de cobrança variam conforme a configuração.

Não existe um preço único para todos os cenários. Antes de criar recursos, é importante estimar a carga esperada e compreender os componentes da fatura.

## 2. Azure Pricing Calculator

A **Calculadora de Preços da Azure** permite montar uma estimativa de custo para uma arquitetura antes da implantação.

Exemplo conceitual:

```text
Aplicação Web
├── Máquina Virtual
├── Disco Gerenciado
├── Banco de Dados
└── Tráfego de Saída
       ↓
Estimativa de custo mensal
```

O resultado é uma **estimativa**, não uma cobrança garantida. É necessário conferir região, moeda, opções de licenciamento, horas de utilização e outros serviços necessários.

[Acessar Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator/)

## 3. Total Cost of Ownership (TCO)

O **TCO (Custo Total de Propriedade)** representa o custo completo de manter uma solução ao longo do tempo.

Uma comparação entre infraestrutura local e nuvem pode considerar:

| Infraestrutura local | Nuvem Azure |
| --- | --- |
| Compra de servidores | Consumo de recursos e serviços |
| Energia e refrigeração | Custos incorporados aos serviços contratados |
| Manutenção de hardware | Infraestrutura física gerenciada pelo provedor |
| Capacidade adquirida antecipadamente | Possibilidade de ajustar capacidade |
| Operação e equipe | Operação e equipe continuam necessárias |

Migrar para a nuvem **não garante economia automaticamente**: depende do desenho da solução, consumo e modelo operacional.

## 4. Microsoft Cost Management

O **Microsoft Cost Management** ajuda a analisar e gerenciar despesas de nuvem.

Entre as funcionalidades estão:

- análise de custos por período;
- visualização por assinatura, grupo de recursos e outros agrupamentos disponíveis;
- filtros por serviço e outros atributos de cobrança;
- acompanhamento de tendências;
- exportação de dados para análises financeiras;
- orçamentos e alertas.

A visibilidade depende do escopo e das permissões de acesso à cobrança.

## 5. Orçamentos e alertas

**Budgets** permitem definir limites de referência para acompanhar gastos e disparar notificações ao atingir determinados valores ou percentuais.

Exemplo:

```text
Orçamento mensal: R$ 200 (exemplo)
           ↓
80% → alerta de acompanhamento
100% → alerta de orçamento atingido
           ↓
Investigar consumo e tomar providências
```

**Importante:** alertas de orçamento não interrompem automaticamente o consumo nem impedem cobranças. Ações automatizadas exigem configuração específica e validação de riscos.

## 6. Dimensionamento correto (Rightsizing)

Uma máquina superdimensionada consome mais recursos financeiros do que o necessário.

Exemplo conceitual:

```text
VM com 8 vCPUs
Uso recorrente de CPU baixo
         ↓
Analisar CPU, memória, disco e rede
         ↓
Considerar VM menor
         ↓
Validar desempenho após ajuste
```

A CPU isoladamente não é suficiente para tomar a decisão. Também devem ser considerados picos, memória, latência, capacidade de disco e necessidades futuras.

O **Azure Advisor** fornece recomendações que podem apoiar a análise de custos, desempenho e outras áreas.

## 7. Desligamento e desalocação de recursos

Ambientes de laboratório e desenvolvimento nem sempre precisam funcionar 24 horas por dia.

Desligar a VM dentro do sistema operacional pode não interromper a cobrança de computação. Para deixar de pagar pela computação da VM, normalmente é necessário que ela esteja **desalocada (Stopped/Deallocated)**.

Mesmo após desalocar, alguns recursos podem continuar gerando custos, como:

- discos gerenciados;
- determinados endereços IP públicos;
- armazenamento de backups e snapshots;
- serviços associados.

Ao terminar um laboratório, é recomendável verificar todos os recursos remanescentes.

## 8. Reservas e Azure Savings Plan

Algumas cargas previsíveis podem se beneficiar de compromissos de uso:

| Opção | Característica |
| --- | --- |
| **Azure Reservations** | Descontos para serviços elegíveis conforme compromisso e condições de reserva |
| **Azure Savings Plan for Compute** | Desconto em computação elegível mediante compromisso de gasto por hora por um período determinado |
| **Pay-as-you-go** | Maior flexibilidade, sem o mesmo tipo de compromisso de uso |

A melhor escolha depende da estabilidade da demanda, elegibilidade e risco de assumir compromissos não aproveitados. O compromisso não significa que qualquer serviço será coberto.

## 9. Governança e identificação de gastos

A organização dos recursos facilita o controle financeiro.

Exemplo:

```text
Assinatura Azure
├── Resource Group: projeto-dev
│   └── Tags: projeto=site, ambiente=dev
└── Resource Group: projeto-prod
    └── Tags: projeto=site, ambiente=prod
```

Boas práticas incluem:

- padronizar nomes;
- utilizar tags como `projeto`, `ambiente` e `centro-de-custo`;
- definir responsáveis pelos ambientes;
- acompanhar despesas por assinatura e grupo de recursos;
- aplicar Azure Policy quando for necessário reforçar regras de governança.

Tags podem facilitar relatórios, mas nem todos os custos são necessariamente atribuídos ou categorizados da mesma forma.

## 10. Otimização de armazenamento

Armazenamento também precisa ser dimensionado.

Possíveis estratégias:

- utilizar camadas **Hot, Cool, Cold e Archive** do Blob Storage quando compatíveis;
- aplicar políticas de ciclo de vida;
- eliminar arquivos duplicados e dados temporários desnecessários;
- revisar snapshots e backups antigos de acordo com as políticas de retenção;
- escolher o nível de redundância adequado.

Uma camada mais barata para manter dados pode cobrar mais por recuperação ou operações. Antes da mudança, compare o **custo total de uso**.

## Exemplo de avaliação de economia

Imagine um ambiente de desenvolvimento com uma VM utilizada apenas em horário comercial.

Uma avaliação técnica pode seguir este fluxo:

1. Identificar o horário real de uso do ambiente.
2. Medir consumo de CPU, memória, armazenamento e rede.
3. Calcular o custo atual com a Pricing Calculator e Cost Management.
4. Testar uma rotina de desalocação fora do expediente.
5. Validar se a rotina não interrompe testes, integrações ou processos agendados.
6. Comparar os custos efetivos ao longo de um período representativo.

A redução de custo deve ser validada com os dados de cobrança. Não basta presumir economia.

## Boas práticas de FinOps

**FinOps** é uma abordagem de colaboração entre engenharia, finanças e negócio para aumentar o valor obtido com os investimentos em tecnologia.

Práticas importantes:

1. **Visibilidade:** identificar quem consome recursos e por quê.
2. **Responsabilidade:** atribuir custos a equipes e projetos.
3. **Otimização:** eliminar desperdícios e ajustar a capacidade.
4. **Previsibilidade:** trabalhar com estimativas, tendências e orçamentos.
5. **Revisão contínua:** acompanhar os resultados das mudanças.

## Conclusão

Otimizar custos no Azure não significa escolher sempre o recurso mais barato. A decisão correta equilibra **custo, desempenho, segurança, resiliência e necessidades da aplicação**.

As principais lições do desafio são: **estimar antes de provisionar, monitorar depois de implantar e revisar continuamente o consumo**.

## Referências

- [Microsoft Cost Management](https://learn.microsoft.com/pt-br/azure/cost-management-billing/costs/overview-cost-management)
- [Calculadora de Preços Azure](https://azure.microsoft.com/pricing/calculator/)
- [Azure Advisor](https://learn.microsoft.com/pt-br/azure/advisor/advisor-overview)
- [Azure Savings Plan](https://learn.microsoft.com/pt-br/azure/cost-management-billing/savings-plan/savings-plan-compute-overview)
- [Reservations](https://learn.microsoft.com/pt-br/azure/cost-management-billing/reservations/save-compute-costs-reservations)
- [Otimização de custos — Azure Well-Architected](https://learn.microsoft.com/pt-br/azure/well-architected/cost-optimization/)
- [FinOps Foundation](https://www.finops.org/)

---

**Desafio de Projeto — DIO**
