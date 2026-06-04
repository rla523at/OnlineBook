# Monitoring with Prometheus and Grafana

## 한 줄 요약

Prometheus와 Grafana를 사용하는 monitoring 구조에서는 측정 대상이 metric을 노출하고, Prometheus가 metric을 주기적으로 수집하고, Grafana가 Prometheus를 조회해 dashboard로 표시한다.

이 문서에서 `monitoring`은 실행 중인 시스템의 상태를 metric으로 수집하고, 저장하고, 조회하고, 시각화하는 과정을 뜻한다.

## 먼저 알아야 할 역할

- Prometheus
  - metric을 수집하고 time series로 저장하며 PromQL로 조회하게 해 주는 monitoring system이다.
- Grafana
  - Prometheus 같은 datasource를 조회해 dashboard와 panel로 시각화하는 도구다.
- scrape
  - Prometheus가 측정 대상의 HTTP endpoint를 주기적으로 읽어 metric sample을 가져오는 동작이다.
- datasource
  - Grafana가 query를 보내 데이터를 읽어 오는 저장소나 API다.
- dashboard
  - 관련 panel을 모아 시스템 상태를 한 화면에서 볼 수 있게 만든 화면이다.

## 전체 구조

Prometheus와 Grafana를 함께 사용할 때의 기본 흐름은 다음과 같다.

```text
측정 대상
  -> metric 노출
  -> Prometheus scrape
  -> Prometheus time series 저장
  -> Grafana datasource query
  -> Grafana dashboard 표시
```

Linux 서버의 CPU, memory, filesystem 상태를 보는 경우에는 측정 대상과 metric 노출을 `node exporter`가 담당한다.

```text
Linux host
  -> node exporter
  -> http://host:9100/metrics
  -> Prometheus scrape job
  -> Prometheus time series
  -> Grafana Prometheus datasource
  -> Grafana dashboard panel
```

이 구조에서 Prometheus와 Grafana의 책임은 다르다. Prometheus는 metric을 수집하고 저장하는 시스템이다. Grafana는 Prometheus 같은 datasource를 조회해 그래프와 표로 보여주는 시각화 시스템이다.

## Metric

`metric`은 시스템의 어떤 상태나 사건을 숫자로 표현한 측정값이다.

예를 들어 다음은 metric으로 표현할 수 있는 값이다.

- CPU가 idle 상태로 보낸 누적 시간
- 사용 가능한 memory byte 수
- filesystem 전체 byte 수
- HTTP request 총 개수
- HTTP request 처리 시간

Prometheus는 데이터를 time series로 저장한다. `time series`는 같은 metric 이름과 같은 label 조합에 속한 timestamp/value 값들의 흐름이다.

예:

```text
node_memory_MemAvailable_bytes{job="automation-node", instance="127.0.0.1:9100"}
```

이 표현은 다음 요소로 나뉜다.

| 요소 | 의미 |
|------|------|
| `node_memory_MemAvailable_bytes` | metric 이름 |
| `job="automation-node"` | 이 값을 수집한 Prometheus job label |
| `instance="127.0.0.1:9100"` | scrape 대상 endpoint label |
| value | 특정 시각의 측정값 |
| timestamp | 측정값이 기록된 시각 |

label은 같은 metric 이름 안에서 대상을 구분하는 key/value 쌍이다. label 값이 달라지면 Prometheus에서는 다른 time series가 된다.

## Prometheus Metric 노출 형식

Prometheus가 metric을 가져가려면 측정 대상은 HTTP endpoint에서 Prometheus가 읽을 수 있는 형식으로 metric을 노출해야 한다.

Prometheus exporter와 Prometheus client library를 따르는 구성에서는 endpoint 경로로 `/metrics`를 쓰는 사례가 많다. endpoint 응답은 사람이 읽을 수 있는 text 형식이다.

예:

```text
# HELP http_requests_total The total number of HTTP requests.
# TYPE http_requests_total counter
http_requests_total{method="GET",code="200"} 1027
```

이 형식에서 `HELP`는 설명, `TYPE`은 metric type, 나머지 줄은 실제 sample이다.

Prometheus metric type에는 `counter`, `gauge`, `histogram`, `summary` 등이 있다. 이 문서에서는 exporter와 scrape 구조를 이해하는 것이 목적이므로 각 type의 세부 차이는 다루지 않는다.

## Exporter

`exporter`는 어떤 시스템의 상태를 Prometheus metric 형식으로 변환해 노출하는 프로그램이다.

모든 시스템이 처음부터 Prometheus metric을 직접 제공하지는 않는다. exporter는 운영체제, database, message queue, reverse proxy 같은 대상에서 값을 읽고 `/metrics` endpoint로 노출한다.

Exporter의 책임은 metric을 노출하는 것이다. Exporter가 Prometheus에 값을 직접 저장하거나 Grafana dashboard를 직접 만들지는 않는다.

## Node Exporter

`node exporter`는 Linux 같은 Unix 계열 host의 hardware와 OS metric을 노출하는 exporter다.

node exporter가 제공하는 metric 중 Linux host 상태를 나타내는 값은 `node_` prefix를 사용한다.

예:

| metric | 의미 |
|--------|------|
| `node_cpu_seconds_total` | CPU mode별 누적 시간 |
| `node_memory_MemAvailable_bytes` | 사용 가능한 memory byte 수 |
| `node_memory_MemTotal_bytes` | 전체 memory byte 수 |
| `node_filesystem_avail_bytes` | filesystem에서 사용 가능한 byte 수 |
| `node_filesystem_size_bytes` | filesystem 전체 byte 수 |

node exporter는 Linux host의 상태를 읽어 HTTP endpoint로 노출한다. Prometheus가 이 endpoint를 scrape하면 CPU, memory, filesystem 같은 서버 자원 지표가 Prometheus time series로 저장된다.

## Prometheus

Prometheus는 metric을 수집하고 저장하고 조회하는 monitoring system이다.

Prometheus의 핵심 동작은 다음과 같다.

1. 설정 파일에서 scrape할 target을 읽는다.
2. 정해진 주기마다 target의 metric endpoint에 HTTP request를 보낸다.
3. 응답으로 받은 metric sample에 label을 붙여 time series로 저장한다.
4. PromQL query로 저장된 time series를 조회한다.

Prometheus는 pull 방식으로 동작한다. 여기서 pull 방식은 Prometheus가 target에게 주기적으로 요청해서 metric을 가져오는 방식을 뜻한다.

## Scrape

`scrape`는 Prometheus가 target의 metric endpoint를 HTTP로 읽어 metric sample을 가져오는 동작이다.

예를 들어 target이 `127.0.0.1:9100`이고 metric path가 `/metrics`이면 Prometheus는 다음 주소를 읽는다.

```text
http://127.0.0.1:9100/metrics
```

scrape가 성공하면 Prometheus는 응답에 들어 있는 metric sample을 저장한다. scrape가 실패하면 Prometheus는 target 상태를 `up=0`으로 기록할 수 있다.

## Scrape Job과 Target

`scrape job`은 같은 목적의 scrape target들을 묶는 Prometheus 설정 단위다.

`target`은 Prometheus가 metric을 가져오기 위해 접근하는 endpoint다. Prometheus 문서에서는 scrape 가능한 endpoint를 `instance`라고 부르며, 같은 목적의 instance 묶음을 `job`이라고 설명한다.

Prometheus 설정 예:

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: node
    metrics_path: /metrics
    static_configs:
      - targets: ["127.0.0.1:9100"]
```

이 설정의 의미는 다음과 같다.

| 설정 | 의미 |
|------|------|
| `scrape_interval: 15s` | 기본 scrape 주기 |
| `job_name: node` | 이 target 묶음의 job 이름 |
| `metrics_path: /metrics` | target에서 metric을 읽을 HTTP path |
| `targets: ["127.0.0.1:9100"]` | scrape 대상 host와 port |

Prometheus는 scrape한 time series에 `job`과 `instance` label을 자동으로 붙인다.

위 예시에서는 다음 label이 붙는다.

```text
job="node"
instance="127.0.0.1:9100"
```

따라서 PromQL에서 특정 job의 metric만 조회하려면 label matcher를 사용한다.

```promql
node_memory_MemAvailable_bytes{job="node"}
```

## PromQL

`PromQL`은 Prometheus에 저장된 time series를 조회하고 계산하는 query language다.

단일 metric을 조회할 수 있고, label로 필터링할 수 있으며, 여러 time series를 집계하거나 rate를 계산할 수 있다.

예:

```promql
node_memory_MemAvailable_bytes{job="node"}
```

이 query는 `job="node"` label이 붙은 `node_memory_MemAvailable_bytes` time series를 조회한다.

CPU 사용률처럼 원본 metric을 그대로 표시하기 어렵다면 PromQL에서 계산한다.

```promql
100 * (1 - avg(rate(node_cpu_seconds_total{job="node", mode="idle"}[5m])))
```

이 query는 최근 5분 동안의 idle CPU 비율을 기준으로 CPU 사용률을 계산한다.

## Grafana

Grafana는 데이터를 직접 수집하는 시스템이 아니라 datasource를 조회해 시각화하는 시스템이다.

Grafana에서 Prometheus를 datasource로 등록하면 dashboard panel은 Prometheus에 PromQL query를 보내고, Prometheus가 반환한 time series를 그래프나 표로 표시한다.

Grafana dashboard의 주요 구성은 다음과 같다.

| 구성 | 의미 |
|------|------|
| datasource | Grafana가 query할 데이터 저장소 또는 API |
| dashboard | 관련 panel을 모아 놓은 화면 |
| panel | 하나의 그래프, 표, 숫자 표시 영역 |
| query | panel이 datasource에 보내는 조회식 |

Prometheus와 Grafana를 함께 사용할 때 Grafana는 Prometheus의 저장 데이터를 읽는다. Grafana가 node exporter를 직접 scrape하는 구조가 아니다.

## AutomationProject 예시

AutomationProject의 monitoring 설정에서는 node exporter, Prometheus, Grafana가 같은 Linux host에서 host network로 실행된다.

node exporter는 다음 옵션으로 metric endpoint를 연다.

```yaml
--web.listen-address=127.0.0.1:9100
```

이 설정은 node exporter가 현재 서버의 loopback address인 `127.0.0.1`의 `9100` port에서 HTTP endpoint를 제공한다는 뜻이다.

Prometheus는 다음 설정으로 node exporter를 scrape한다.

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: automation-node
    metrics_path: /metrics
    static_configs:
      - targets: ["127.0.0.1:9100"]
        labels:
          service: node
```

이 설정은 다음처럼 해석한다.

| 설정 | AutomationProject에서의 의미 |
|------|------------------------------|
| `job_name: automation-node` | Linux host 자원 metric을 수집하는 Prometheus job 이름 |
| `metrics_path: /metrics` | node exporter metric endpoint path |
| `targets: ["127.0.0.1:9100"]` | node exporter가 metric을 노출하는 endpoint |
| `scrape_interval: 15s` | Prometheus가 target을 수집하는 기본 주기 |
| `service: node` | 이 target이 node exporter 계열임을 표시하는 추가 label |

따라서 AutomationProject에서 `job="automation-node"`는 Prometheus 설정에서 붙는 label이다. node exporter가 직접 정한 이름이 아니다.

서버 자원 지표를 조회하는 PromQL은 `job="automation-node"`를 사용한다.

예:

```promql
node_memory_MemAvailable_bytes{job="automation-node"}
```

Storage는 `/` filesystem 기준으로 조회한다.

```promql
node_filesystem_size_bytes{job="automation-node", mountpoint="/"}
```

## 확인 명령

node exporter가 metric을 노출하는지 확인한다.

```bash
curl http://127.0.0.1:9100/metrics
```

Prometheus가 target을 scrape하고 있는지 확인한다.

```bash
curl http://127.0.0.1:9090/api/v1/targets
```

Prometheus에 저장된 time series를 query한다.

```bash
curl 'http://127.0.0.1:9090/api/v1/query?query=up{job="automation-node"}'
```

RAM 사용 가능 용량 metric을 query한다.

```bash
curl 'http://127.0.0.1:9090/api/v1/query?query=node_memory_MemAvailable_bytes{job="automation-node"}'
```

Grafana에서는 Prometheus datasource를 등록한 뒤 dashboard panel에서 PromQL query를 사용한다.

## 자주 헷갈리는 구분

| 헷갈리는 표현 | 구분 |
|---------------|------|
| node exporter와 Prometheus | node exporter는 metric을 노출하고, Prometheus는 metric을 수집하고 저장한다. |
| Prometheus와 Grafana | Prometheus는 time series 저장과 query를 담당하고, Grafana는 datasource query 결과를 시각화한다. |
| scrape job과 systemd job | scrape job은 Prometheus 설정 단위이고, systemd job은 Linux service manager의 실행 단위다. |
| target과 metric | target은 Prometheus가 접근하는 endpoint이고, metric은 target 응답에 들어 있는 측정값이다. |
| job label과 service label | `job`은 Prometheus가 scrape job 이름으로 자동 부여하는 label이고, `service`는 설정에서 추가한 label이다. |

## 관련 문서

- [NetworkBasics.md](./NetworkBasics.md)
- [DockerNetwork.md](./DockerNetwork.md)
- [Cloud.md](./Cloud.md)
- [WebDeployment.md](./WebDeployment.md)
- [systemd](<../99 ETC/Linux/systemd.md>)
- [Prometheus data model](https://prometheus.io/docs/concepts/)
- [Prometheus jobs and instances](https://prometheus.io/docs/concepts/jobs_instances/)
- [Prometheus exposition formats](https://prometheus.io/docs/instrumenting/exposition_formats/)
- [Prometheus node exporter guide](https://prometheus.io/docs/guides/node-exporter/)
- [Grafana Prometheus data source](https://grafana.com/docs/grafana/latest/datasources/prometheus/)
- [Grafana dashboards](https://grafana.com/docs/grafana/latest/dashboards/)
