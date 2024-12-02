# -SD-ControleConcorrenciaJava

# Controle de Concorrência em Java

## Descrição
Este projeto apresenta diferentes mecanismos de controle de concorrência em Java, como `Monitor`, `Lock`, `ReadWriteLock` e `Synchronized`.

## Análise
### Concorrente
- Problemas observados: **Race conditions**, inconsistências no saldo.
- Logs demonstram que múltiplas threads acessaram os mesmos dados ao mesmo tempo.

### Lock
- Controle explícito, mas com risco de **deadlock** se não usado corretamente.

### ReadWriteLock
- Eficiência em cenários de leitura intensiva, mas com impacto em desempenho para escrita.

### Synchronized
- Simples de implementar, mas menos flexível.

## Conclusão
- Para acessos simultâneos com alta leitura: **ReadWriteLock**.
- Para exclusão mútua: **Lock** com bom uso.
- Em sistemas simples: **Synchronized**.

#### 1. Pasta Concorrente
Descrição: Implementação básica, sem controle de concorrência.
Comportamento Observado:
A conta inicializa com um saldo de R$100,00.
Não há sincronização entre os acessos dos clientes. Como resultado:
Clientes conseguem realizar saques simultâneos, levando o saldo da conta a valores inconsistentes.
Tentativas de saque ocorrem mesmo quando o saldo já foi zerado.
Conflitos evidenciam a falta de um controle para evitar condições de corrida.
**Este modelo é altamente suscetível a problemas de consistência de dados devido à ausência de sincronização.** 
##### 2. Pasta Lock
Descrição: Implementação com uso de ReentrantLock.
Comportamento Observado:
A conta inicializa com diferentes valores de saldo (ex.: R$100, R$200).
Os saques são sincronizados por meio de locks:
O saldo é atualizado corretamente para cada saque válido.
Saques simultâneos são impedidos, garantindo consistência no saldo.
Clientes com tentativas de saque inválidas (saldo insuficiente) recebem mensagens apropriadas.
**O uso de ReentrantLock melhora a consistência e previne condições de corrida. Embora eficaz, esta abordagem pode apresentar overhead devido ao gerenciamento manual do lock.**
###### 3. Pasta RwLock
Descrição: Implementação com ReentrantReadWriteLock.
Comportamento Observado:
A conta inicializa com saldo de R$100.
Leitura e escrita são tratadas separadamente:
Escritores (saques) bloqueiam temporariamente os leitores.
O saldo é atualizado de forma consistente para cada saque válido.
Saques simultâneos não ocorrem, evitando inconsistências.
**A separação entre leitura e escrita melhora o desempenho em cenários de alta concorrência com muitas operações de leitura. Adequado para situações onde há mais leituras do que escritas.**
###### 4. Pasta Synk
Descrição: Implementação utilizando synchronized.
Comportamento Observado:
A conta inicializa com saldo de R$100.
Os métodos sincronizados garantem exclusão mútua:
Apenas um cliente acessa a conta por vez, prevenindo condições de corrida.
Saques são realizados de forma consistente, e mensagens de saldo insuficiente são exibidas adequadamente.
O desempenho pode ser limitado em cenários de alta concorrência devido ao bloqueio em nível de método.
**O uso de synchronized oferece simplicidade e garante consistência. No entanto, a granularidade do bloqueio pode levar a contenção em aplicações com alta concorrência.**
###### Conclusão Geral
Cada abordagem apresenta vantagens e desvantagens:

**Concorrente**: Simples, mas inadequado para cenários concorrentes devido à ausência de sincronização.
**Lock**: Oferece controle explícito e consistência, mas com maior complexidade de implementação.
**RwLock**: Melhora o desempenho em leituras concorrentes, sendo ideal para sistemas com predominância de leituras.
**Synk**: Simples e eficaz, mas com potencial para contenção em cenários de alta concorrência.