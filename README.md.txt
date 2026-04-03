#  SOC Lab - Detecção de Brute Force SSH com Splunk

##  Visão Geral

Este projeto simula um cenário real de um SOC (Security Operations Center), onde um ataque de força bruta via SSH é executado e detectado utilizando o Splunk.

O objetivo é demonstrar, na prática, habilidades essenciais de um Analista de SOC, como ingestão de logs, detecção de ameaças, criação de alertas e visualização de dados.

---

## 🧠 Arquitetura do Laboratório

* Atacante: Kali Linux
* Alvo: Ubuntu Server
* SIEM: Splunk
* Protocolo: SSH

Fluxo do ataque:

Kali (Hydra) → SSH → Ubuntu → Logs (/var/log/auth.log) → Splunk

---

## 🚨 Simulação de Ataque

Foi realizado um ataque de força bruta utilizando a ferramenta Hydra:

```bash id="6btv6s"
hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://192.168.15.103 -t 4
```

Esse ataque gerou milhares de tentativas de login mal sucedidas.

---

## 📥 Ingestão de Logs

Os logs foram coletados do arquivo:

```id="dnhkfl"
/var/log/auth.log
```

E indexados no Splunk com:

```id="vbkq8g"
index=soc_lab
```

---

## 🔍 Detecção do Ataque

A detecção foi feita com base em tentativas de login falhadas:

```spl id="7a1f6g"
index=soc_lab "Failed password"
```

### 🔥 Identificação do IP atacante

```spl id="9l7b2x"
index=soc_lab "Failed password"
| rex "from (?<src_ip>\d+\.\d+\.\d+\.\d+)"
| stats count by src_ip
| sort -count
```

---

## 🚨 Criação de Alerta

Foi configurado um alerta no Splunk com as seguintes características:

* Execução: a cada 1 minuto
* Condição: mais de 10 tentativas de login falhadas
* Severidade: Média

---

## 📊 Dashboard

Foi desenvolvido um dashboard para visualização do ataque, contendo:

* Total de tentativas de login falhadas
* Linha do tempo do ataque
* IP atacante
* Usuários alvo

---

## 📸 Evidências

### Ataque com Hydra

![Ataque](screenshots/hydra_attack.png)

### Logs no Splunk

![Logs](screenshots/logs.png)

### Query de Detecção

![Detecção](screenshots/detection_query.png)

### Dashboard Final

![Dashboard](screenshots/dashboard.png)

---

## 🎯 Habilidades Demonstradas

* SIEM (Splunk)
* Análise de logs
* Detecção de ataques
* Simulação de brute force
* SPL (Search Processing Language)
* Criação de alertas
* Criação de dashboards

---

## 🚀 Conclusão

Este laboratório demonstra na prática como um ataque de força bruta pode ser detectado e analisado em um ambiente controlado utilizando Splunk.

O projeto simula atividades reais de um Analista de SOC, incluindo monitoramento, detecção e investigação de incidentes.

---
