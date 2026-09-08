---
date: "2026-03-20"
time: "19:31"
tags: [trabalho, devops, plataforma, pagbank, kubernetes, carreira]
status: rascunho
source: 
aliases: []
---

# Trabalho PAGBANK

## Resumo
Notas de trabalho sobre Engenharia de Plataforma e DevOps na PagBank/PagSeguro: conceitos de plataforma, DevOps, Kubernetes, fluxos de segurança (SAST/DAST), gestão de acesso no GitHub, backup em S3 e o pipeline padrão (Mazagon).

## Conteúdo

## Engenharia de Plataforma

- Suporta o time de desenvolvimento e faz a sustentação da aplicação, para garantir que ela rode da melhor forma.
- Ajuda os devs a codificar melhor e acelera a velocidade das entregas.
- Vão limitar alguns tipos de configuração para deploy no Kubernetes.

### Termos e componentes chave

- **Ingress** — objeto no Kubernetes que expõe o endpoint da aplicação. Precisa de certificado; há externo e interno.
- **Certificados** — expiravam; criaram uma aplicação para monitorar. O Darvin fez uma solução que emite apontando para a CA do PagSeguro; quando expira, o Kubernetes renova o certificado automático.
- **PagCloud** — repo a ser clonado.
- **JFrog** — armazena as imagens do Docker.
- **Groovy** — a linguagem do Jenkins (por isso é usada).
- **AD (Active Directory)** — solução Microsoft de gestão de acesso; gerencia os acessos.
- **SonarQube** — ferramenta de qualidade de código (identar, "deixar pimposo"). Usado via action no GitHub com credenciais específicas.
- **Bootstrap** — API da Pag para fazer integrações; "mexe com tudo!".
- **Jira/Teams** — maneira como os clientes abrem chamado (work in progress, etc.).

## Terraform

- Funciona de forma **declarativa**: você descreve no arquivo o estado desejado das máquinas e ele configura tudo.
- Abstrai a forma como a infraestrutura é configurada.
- **Comandos:**
  - `refresh` — query para pegar o estado atual da infra.
  - `plan` — prevê como será a aplicação das mudanças.
  - `apply` — aplica as mudanças planejadas.
  - `destroy` — remove os recursos configurados.

## Conceitos de DevOps

DevOps é uma cultura de colaboração entre desenvolvimento e operações para agilizar processos e garantir estabilidade.

- Nasceu para integrar melhor as equipes de desenvolvimento e operação.
- Agiliza entrega, integração e manutenção de software.
- Comunicação é essencial entre devs e operação.
- **Infraestrutura como código (IaC)** é um dos pilares para criar um ambiente de entregas estável, escalável e confiável.

### Destaques
1. O conceito começou oficialmente em 2009, em uma conferência.
2. A cultura visa superar as lacunas entre devs e operações.
3. Automação, monitoramento e compartilhamento são pilares fundamentais.
4. Infraestrutura ágil e cultura de empatia são cruciais para o sucesso.
5. Traz benefícios para profissionais e empresas, facilitando o trabalho e promovendo inovação.

### Passos para a implantação
1. **Compreensão da Cultura** — colaboração, comunicação aberta e confiança entre devs e operações.
2. **Automatização de Processos** — automatize o máximo: CI/CD e provisionamento de infraestrutura.
3. **Monitoramento e Feedback** — sistemas robustos de monitoramento em tempo real; itere continuamente.
4. **Padronização e Documentação** — padrões claros e procedimentos documentados.
5. **Infraestrutura Ágil** — práticas como IaC para gerenciar ambientes de dev, teste e produção.

## Kubernetes

- **Pod** é a menor unidade do Kubernetes (não é o container) — pode ter 1 ou mais contêineres.
- **Container** — isola vários processos numa máquina usando um só kernel, compartilhando recursos, sem que um processo enxergue/pise no outro.

## Fluxo de segurança (SAST / DAST)

- O dev faz o código e sobe para prod ← **problema**: não há scan de segurança/vulnerabilidades/chaves privadas.
- **SAST** — analisa o código em busca de vulnerabilidades (análise estática do código em si; não verifica o comportamento em execução).
- **DAST** — olha a aplicação em andamento, pega as nuances e aferes vulnerabilidades em tempo de execução.
- **Ideal: os dois trabalharem juntos.**
- **SonarQube** — scan de código, mais voltado para qualidade de código.
- **GitHub Advanced Security** — uma série de verificações:
  - Verifica a cada commit se uma credencial foi exposta.
  - Dá para definir expressões regulares para detectar coisas.
  - **Code scanning** (CodeQL): scan estático do código para achar vulnerabilidades; dá para bloquear vulnerabilidades altas e médias.
  - O time Arlington faz isso; Mazagon ajuda a integrar com o GitHub.
  - **Problema:** como bloquear? Os times conseguem modificar as configurações dos repos (bypass em coisas obrigatórias). Definido ao nível de organização, mas dá para bypassar.
- Sempre pensar em **caminhos alternativos** que o dev pode usar para desviar do fluxo de segurança, garantindo que ele seja respeitado.

### Proteção do fluxo
- Só olha para `master` e `main` → problema: alguém pode fazer deploy de outra branch para fugir do scan.
- **Soluções:** scan inicial; se um novo commit trouxer nova vulnerabilidade, tratar antes do commit.
- **Regra:** não é permitido bypass em ruleset da organização.
- **Custom Properties** — ao nível de organização; os admins do repo não podem mudar.
- **3 formas de scan:**
  1. Scan em máquina do GitHub — muito custoso (50 mil minutos de actions grátis; excedeu, paga); exige VPN e IPs liberados; qualquer um pode clonar com credenciais. Porém: mais suporte, mais controle e mais simples de configurar.
  2. **Solução (webhook):** a aplicação está no Kubernetes; passam a URL da aplicação e o scan é disparado por uma ação específica. Config que vale para tudo ("custom properties é delícia").
  3. **Gate** — o bloqueio das coisas; automação para definir a custom property.

## Fluxo de acesso no GitHub

- A maioria das orgs é organizada por **top domain → domain → subdomain**. Ideal: 1 top domain por coisa (mas orgs legadas não seguem isso).
- Na aba de configuração dá para ver as entidades que participam.
- Para acessar um repo: precisa ser parte de um **time** com acesso.
- Precisa de acesso à **org** + acesso ao **time** do repo.

### Verificação de acesso (IDM/LDAP)
- Um container docker bate no serviço do **IDM** para ver as pessoas nos grupos:
  `docker run --rm --network=host -it repo.intranet.pags/corpplatform-docker-dev-local/mazagon/ldap-manager:stable -g gucamara`
- **Problemas comuns:**
  - Solicitou acesso no IDM, foi aprovado, mas não recebeu invite nem foi adicionado ao grupo → pedir aprovação.
  - Se não está na org, só vê repos públicos (mas dá para ver via o comando docker acima).
- Um job no Jenkins envia o invite por e-mail; só dá acesso a quem tem os grupos de acesso específicos.

### Contas e vínculos
- Perder o MFA/token = não recupera a conta; problema com conta é com o GitHub.
- Ao remover o vínculo, ainda fica salvo no SSO (vínculo entre Azure e GitHub).
- O login do GitHub é sincronizado com o login do e-mail; é preciso fazer **revoke no linked SSO identity**.
- Adm pode adicionar via grupo do AD; dá para direcionar para o corporativo.

## Backup GitHub (S3)

- [x] Criar o token.
- Tentar puxar as infos sem ser owner.
- Testar numa org para puxar repos, times, permissionamentos e grupos.
- **Arquivos:** um para repos/times com permissão; outro para todos os times e seus membros/lista atrelada.
- **Fluxo:** pegar todos os times → bater no `teams-sync` → se não tiver nada, bater no `/members`.
  - `/members` traz só o login; `teams-sync` traz só o nome. Armazenar o group name também.
  - Times → lista com membros; Repo → `list repositories times` (`repos/{org}/{repo}`).
- Para cada org: pegar todos os times e listar membros; para todos os repos: puxar times e permissões.
- **Checklist:**
  - [x] Pegar todos os times de uma org
    - [x] Listar os membros
  - [x] Pegar os repos de uma org
    - [x] Puxar os times
    - [x] Pegar as permissões de um repo
  - [x] Pega um repo, seus times e permissões
    - [x] Pegar os times de um repo
    - [x] Pegar as permissões dos times
  - [x] Implementar paginação na bootstrap
- **S3** — armazenamento de upload/download de arquivos. **Storage class** = velocidade de acesso (dados quentes ou frios). APIs: AWS API e AWS-CLI.
- **Setup:** repo com Terraform para criar o bucket → cronjob no Kubernetes → credenciais no **PagVault** → pod temporário clona o repo, executa o script, joga os arquivos no S3, e o pod morre.
- [x] Criar arquivos de backup para todas as orgs e metrificar o tempo. Se demorar muito, cria um pod por org; se for rápido, um pod só.

## Copilot Jira Automation

- [x] Criar um time no GitHub para o Copilot.
- [x] Vincular os users no time.
- [x] Fazer um Jira automation para inserir a pessoa no time.
- **Campos:** [x] nome do cara no GitHub.
- **Opção:** rechamar o `add member`.
- No def do Copilot, passar o time como "copilot". O retorno da função é o que fica no Jira.
- **Pontos de atenção:** verificar aprovação; ver se dá para setar aprovadores.
- Ver se dá para pegar o usuário do GitHub a partir do usuário do AD.
- Criar um novo form no Jira. Usar o repo [engprod-release-jiraps-bff](https://github.com/ps-corp-platform/engprod-release-jiraps-bff): novo field para adicionar user no Copilot; reusar o `add member` existente ou chamar a Bootstrap para adicionar no time do Copilot.
- Documentar o novo fluxo na docs do PagCloud.
- **Resumo:** dar acesso ao Copilot e adicionar num time.
- **Próximos passos:** dar get nos membros do grupo do Copilot no GitHub; remover o `g_uolps_github_ps-copilot` do time; adicionar novamente; lógica de só colocar no Copilot se for o Jira chamando; adicionar apenas como membro se o time for o Copilot.

## Dar acesso de download às engenharias de software

- [x] Criar uma função para verificar quando quem chamou o job está na lista de SRE.

## Palestra — Como funciona o Mazagon

### Competências / responsabilidades
- Gerenciar a esteira de CI/CD: criação e padronização de pipelines e integração entre ferramentas (governança, segurança, qualidade).
- Suporte às ferramentas de CI/CD (segurança, automatização, atualização, testes nas ferramentas internas).
- Padronizar pipelines com steps obrigatórios para entrega de infra nos ambientes AWS e PagCloud.
- **Ferramentas da esteira:** Atlantis, JFrog, GitHub, Sonar.

### Pipeline padrão
- **Motivação:** cada um fazia deploy de um jeito → tempo de suporte alto, falta de padronização; pipeline facilita inclusão de novas funcionalidades.
- **O que é:** uma grande biblioteca do Jenkins; ao ser chamada, executa uma porrada de coisa.
- **Estrutura:**
  - **Multi-repo**: cada aplicação tem seu próprio repositório e job.
  - **Mono repo**: um repo com vários sistemas separados por pastas.
- Os parâmetros não podem ser modificados (quebra a pipe).
- **Permissionamento por sistema:** consulta feita via **Pandora**; o time dono do sistema é o responsável. Time dono tem acesso + times parceiros.
- **Erros comuns:** time de dev já tem acesso ao job, mas se for especialista/gerente, precisa ver o acesso no IDM.

### Workflow do job
- **Pr Builder:** executa ao abrir PR ou commit no PR. Faz build, teste, SonarQube e docker build. Objetivo: testar e antecipar erros.
- **Dev:** tudo que o Pr Builder faz + push da imagem e deploy para DEV.
- **QA:** faz release e deploy de fato para QA.
- **Prod:** branch tem que ser main/master (por causa da tag de versão). Não testa, mas vê se já teve teste antes; passa por gate de segurança; faz promote da imagem de QA para PROD e faz deploy.
- **Hotfix:** tem que ter incidente no Jira; branch deve ser `hotfix/incidente`; tem que ser o último PR.

### Exceções e casos de erro
- **Exceção de steps:** para pular steps, ir nas custom properties e ver o que faz sentido pular.
- **Problemas comuns:**
  - Pipeline não acha o script do Docker → slug da aplicação e nome de pasta diferentes.
  - Existe propriedade para colocar o caminho do arquivo docker.
  - Deploy type tem que estar numa linha só.
  - YAMLs em pastas diferentes de `deploy/ambiente`.
  - Repositório classificado incorretamente (geralmente por não achar o DockerFile).
  - Referenciar direto as pastas dentro do Dockerfile.
  - Excluir pasta só quando a parada está certa no Pandora — tem que excluir no Pandora também.
- **Raftel** cuida do PagStage.

## Ação
- Revisar e aplicar os próximos passos do Copilot Jira automation
- Documentar os conceitos de plataforma/DevOps para estudo próprio

---


