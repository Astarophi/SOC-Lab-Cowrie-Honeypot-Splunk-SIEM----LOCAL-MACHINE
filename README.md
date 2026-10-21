# SOC Lab — Cowrie Honeypot + Splunk Enterprise

Laboratório prático de cibersegurança desenvolvido em ambiente virtual local para estudar coleta, centralização e análise de eventos de segurança utilizando Cowrie Honeypot e Splunk Enterprise.

## Objetivo

Construir um ambiente controlado para simular sessões SSH, registrar eventos gerados pelo honeypot e analisar esses registros no Splunk, utilizando consultas SPL e um dashboard de monitoramento.

## Tecnologias utilizadas

- **Cowrie:** honeypot para simular serviços SSH e registrar interações.
- **Splunk Enterprise:** plataforma utilizada para pesquisar, analisar e visualizar logs.
- **Ubuntu Server:** sistema operacional do laboratório.
- **VirtualBox:** virtualização do ambiente.
- **SPL (Search Processing Language):** linguagem de pesquisa utilizada nas análises.

## Arquitetura


Ambiente virtual local
        |
        v
Cowrie Honeypot (SSH - porta 2222)
        |
        v
Logs em formato JSON
        |
        v
Splunk Enterprise
        |
        v
Consultas SPL e Dashboard


O Cowrie registra os eventos das sessões simuladas. O Splunk consulta o arquivo de log JSON monitorado e apresenta os dados em pesquisas e painéis.

## Implementações realizadas

- Instalação e inicialização do Cowrie.
- Configuração do serviço SSH do honeypot para escutar em `127.0.0.1:2222`.
- Geração de eventos por meio de testes locais controlados.
- Consulta dos registros JSON gerados pelo Cowrie.
- Pesquisa de eventos no Splunk.
- Criação de consultas SPL para analisar eventos e tentativas de autenticação.
- Construção de dashboard com indicadores e atualização automática.

## Consultas SPL

### Visão geral dos eventos

```spl
index=main sourcetype=_json
| stats count by eventid
| sort - count
```

(screenshots/Eventcommandsplunk.png)

Conta os eventos por tipo, ajudando a identificar quais categorias foram registradas com maior frequência.

### Tentativas de autenticação malsucedidas

```spl
index=main sourcetype=_json eventid="cowrie.login.failed"
| table _time src_ip username protocol dst_port
```

Exibe informações sobre tentativas de login malsucedidas registradas pelo Cowrie.

### Comandos registrados pelo honeypot

```spl
index=main sourcetype=_json eventid="cowrie.command.input"
| table _time src_ip username input
| sort - _time
```

Apresenta os comandos registrados nas sessões simuladas, permitindo observar as interações realizadas durante os testes.

## Evidências do laboratório

As capturas de tela do ambiente e dos resultados serão organizadas no diretório `screenshots/`.

- Eventos recebidos pelo Splunk.
- Dashboard de monitoramento.
- Consultas SPL e resultados.
- Atualização automática do dashboard.

## Segurança e limitações

- Os testes documentados foram realizados em ambiente virtual local.
- O endereço `127.0.0.1` representa a própria máquina; portanto, os testes locais não representam ataques reais originados da internet.

---

**Projeto pessoal de estudos em cibersegurança.**
