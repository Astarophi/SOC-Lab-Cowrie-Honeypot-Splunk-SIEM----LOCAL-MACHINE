# Consultas SPL — SOC Lab

Este documento reúne consultas SPL utilizadas para analisar os eventos registrados pelo Cowrie Honeypot no Splunk Enterprise.

As consultas foram elaboradas para praticar pesquisa de logs, identificação de eventos de autenticação e análise de interações registradas no honeypot.

## 1. Consultar os eventos recebidos

```spl
index=main sourcetype=_json
```

**Objetivo:** consultar os eventos JSON indexados no Splunk.

**Explicação:**
- `index=main`: pesquisa os eventos armazenados no índice `main`.
- `sourcetype=_json`: filtra eventos identificados com esse tipo de fonte.

**Uso na análise:** verificar se os registros do Cowrie estão chegando ao Splunk.

## 2. Contabilizar eventos por tipo

```spl
index=main sourcetype=_json
| stats count by eventid
| sort - count
```

**Objetivo:** visualizar a quantidade de eventos de cada categoria.

**Explicação:**
- `stats count by eventid`: conta os registros agrupados pelo campo `eventid`.
- `sort - count`: ordena a contagem do maior para o menor.

**Uso na análise:** entender quais tipos de eventos foram registrados durante os testes.

## 3. Investigar tentativas de login malsucedidas

```spl
index=main sourcetype=_json eventid="cowrie.login.failed"
| table _time src_ip username protocol dst_port
```

**Objetivo:** consultar tentativas de autenticação malsucedidas registradas pelo Cowrie.

**Explicação:**
- `eventid="cowrie.login.failed"`: filtra os eventos de falha de autenticação.
- `table`: apresenta os campos relevantes em formato tabular.
- `_time`: horário do evento.
- `src_ip`: endereço IP de origem registrado.
- `username`: usuário informado na tentativa.
- `protocol`: protocolo registrado.
- `dst_port`: porta de destino.

**Uso na análise:** observar tentativas de acesso e os usuários informados.

## 4. Analisar comandos registrados

```spl
index=main sourcetype=_json eventid="cowrie.command.input"
| table _time src_ip username input
| sort - _time
```

**Objetivo:** visualizar os comandos registrados durante as sessões simuladas.

**Explicação:**
- `eventid="cowrie.command.input"`: seleciona eventos de entrada de comandos.
- `table`: exibe horário, IP, usuário e comando.
- `sort - _time`: organiza os resultados do mais recente para o mais antigo.

**Uso na análise:** observar o comportamento de uma sessão simulada e documentar os comandos registrados.

## 5. Identificar IPs por quantidade de conexões

```spl
index=main sourcetype=_json eventid="cowrie.session.connect"
| stats count by src_ip
| sort - count
```

**Objetivo:** contabilizar os eventos de conexão por IP de origem.

**Explicação:**
- `eventid="cowrie.session.connect"`: filtra os eventos de conexão.
- `stats count by src_ip`: agrupa e conta os registros por IP.
- `sort - count`: exibe primeiro os IPs com maior contagem.

**Uso na análise:** identificar as origens mais frequentes dentro dos dados coletados.

## Observações

- Os resultados dependem dos eventos existentes no ambiente e do período selecionado no Splunk.
- Um evento de login aceito pelo Cowrie representa uma sessão simulada pelo honeypot, não um acesso confirmado ao sistema operacional real.
- Os testes deste projeto foram realizados em ambiente local. Portanto, os resultados não devem ser descritos como ataques reais originados da internet.
- Antes de compartilhar capturas ou dados exportados, remova senhas, credenciais e outras informações sensíveis.

---

Projeto pessoal de estudos em cibersegurança.
