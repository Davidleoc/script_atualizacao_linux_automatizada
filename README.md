# Automação de Atualização Segura Linux (Ansible + Nagios XI)

Este repositório contém a solução completa para a automação de patches e atualizações de pacotes em ambientes Linux (Debian/Ubuntu/CentOS), focada em **segurança, preservação de configurações e monitoramento em tempo real**.

A solução resolve o problema de janelas de manutenção manuais que podem levar meses, transformando-as em processos ágeis, auditáveis e livres de erro humano.

## 🚀 Funcionalidades

- **Patching Seguro**: Utiliza `apt-get upgrade` com flags de preservação configuracional (`--force-confold`/`--force-confdef`).
- **Proteção de Versões**: Garante a aplicação de patches de segurança sem realizar o upgrade de versões major (ex: Java 8 permanece Java 8).
- **Snapshot de Serviços**: Identifica quais serviços críticos estão ativos antes da atualização e garante que retornem ao mesmo estado após o processo ou reboot.
- **Cura Automática**: Reinicia automaticamente serviços que falharam durante a janela de manutenção.
- **Monitoramento via NRPE**: Integração nativa com Nagios XI, reportando o status exato (Atualizando, Reiniciando, OK ou Falha) durante cada etapa.
- **Relatório Detalhado**: Log visual completo no Ansible com o status de cada serviço (`[OK]`, `[WARN]`, `[FAIL]`).

## 🛠️ Estrutura do Projeto

- `atualiza_server.yml`: Playbook principal de atualização e orquestração.
- `instalar_check_nrpe.yml`: Playbook para implantar o plugin de monitoramento nos hosts.
- `check_atualizacao_automatizada.sh`: Script/Plugin executado pelo NRPE para reportar o status ao Nagios.
- `.env.example`: Modelo de variáveis de ambiente para credenciais de acesso.

## 📋 Pré-requisitos

1. **Ansible** instalado no servidor Controller (ex: VERSAILLES).
2. **NRPE** configurado nos servidores de destino.
3. Acesso SSH via chave privada para o usuário de gestão (ex: `srvlinuxmgmt`).

## 🔧 Configuração e Uso

### 1. Preparar o Ambiente
Renomeie o arquivo `.env.example` para `.env` e preencha com as suas informações:
```bash
cp .env.example .env
```

### 2. Instalar o Monitoramento
Execute o playbook para preparar os servidores para o monitoramento do Nagios:
```bash
ansible-playbook -i seu_inventario instalar_check_nrpe.yml
```

### 3. Executar Atualizações
Para rodar a atualização em um pool de servidores:
```bash
ansible-playbook -i seu_inventario atualiza_server.yml --limit nome_do_grupo
```

## 📊 Visualização no Nagios XI

A solução cria um serviço no Nagios que pode assumir os seguintes estados:
- **🟢 OK**: Nenhuma atualização pendente ou concluída com sucesso.
- **🟡 WARNING**: Atualização em andamento ou servidor reiniciando.
- **🔴 CRITICAL**: Falha na atualização ou serviço crítico não subiu após o processo.

---
**Desenvolvido para garantir estabilidade e velocidade na gestão de infraestrutura Linux.**
