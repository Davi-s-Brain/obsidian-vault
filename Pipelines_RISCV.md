---
cards-deck: OAC II
flashcards:
  q-idvz: { nid: 1789477105149, hash: wv7cme9c, sync: mkufpjsb }
  q-mzf7: { nid: 1789477105172, hash: wtijwgmk, sync: 4djwzua3 }
  q-2mta: { nid: 1789477105197, hash: c89kubn7, sync: wh6a3cqd }
  q-9mba: { nid: 1789477105223, hash: qn55g98b, sync: za3r3f9r }
  q-r3rh: { nid: 1789477105247, hash: 4zpt9tt4, sync: bnjk7ams }
  q-j4ub: { nid: 1789477105272, hash: f25gxxb7, sync: k3jcpeye }
  q-c8ty: { nid: 1789477105297, hash: zydnaw8h, sync: vu87g5vr }
---

# Pipelines RISC-V

Uma pipeline de processador é uma técnica de execução sobreposta onde múltiplas instruções estão em diferentes estágios de processamento simultaneamente.

## Estágios Básicos (5 estágios)

1. **Busca (IF)** - Instruction Fetch: busca a instrução na memória
2. **Decodificação (ID)** - Instruction Decode: decodifica a instrução e lê registradores
3. **Execução (EX)** - Execute: executa a operação na ULA
4. **Memória (MEM)** - Memory Access: acessa dados em memória (se necessário)
5. **Writeback (WB)** - Escreve o resultado de volta no registrador

## Hazards (Conflitos)

Problemas que impedem que a pipeline execute instruções em paralelo de forma ideal:

- **Hazard Estrutural** - Dois estágios competem pelo mesmo recurso hardware
- **Hazard de Dados** - Uma instrução depende do resultado de outra ainda não executada
- **Hazard de Controle** - Instruções seguintes dependem de um branch ainda não resolvido

## Soluções para Hazards de Dados

- **Forwarding (Bypassing)** - Redireciona o resultado diretamente para onde é necessário, sem esperar o Writeback
- **Stall (Bolha)** - Insere ciclos de espera até que o dado esteja disponível

## Flashcards

Qual a principal função de uma pipeline de processador?::Executar múltiplas instruções em sobreposição, com cada uma em diferentes estágios de processamento simultaneamente.
^q-idvz

Quais são os 5 estágios básicos de uma pipeline RISC-V?::Busca (IF), Decodificação (ID), Execução (EX), Memória (MEM) e Writeback (WB).
^q-mzf7

O que é um hazard de dados em uma pipeline?::Uma instrução depende do resultado de outra instrução que ainda não terminou sua execução.
^q-2mta

Qual a diferença entre forwarding e stall para resolver hazards de dados?::Forwarding redireciona o resultado diretamente para o estágio que precisa dele; stall insere ciclos de espera até o dado ficar disponível.
^q-9mba

O que causa um hazard estrutural?::Dois estágios da pipeline tentam acessar o mesmo recurso de hardware ao mesmo tempo.
^q-r3rh

Quais são os três tipos de hazards em uma pipeline?::Estrutural, de dados e de controle.
^q-j4ub

O que é um hazard de controle?::Instruções seguintes a um branch dependem de sua decisão ainda não resolvida.
^q-c8ty