# Análise de monitoramento dos processos Nifi - Prometheus e Grafana

##### Foram realizados estudos e análises visando realizar a implementação de metricas de monitoramento a serem recuperadas pelo Prometheus e disponibilizadas as informações em forma de gráficos de monitoramento pelo Grafana.

<h1 align="center" style="border-bottom: none">
    <a href="//prometheus.io" target="_blank"><img alt="NiFi" src="nifi/images/apache_nifi_logo.png"></a>
    <a href="//prometheus.io" target="_blank"><img alt="Prometheus" src="prometheus/images/prometheus-logo.svg"></a>
    <a href="//prometheus.io" target="_blank"><img alt="Grafana" src="grafana/images/Grafana_icon.png"></a>
</h1>

<p align="center">Visit <a href="//nifi.apache.org/" target="_blank">nifi.apache.org</a>, <a href="//prometheus.io" target="_blank">prometheus.io</a> ou <a href="//grafana.com/" target="_blank">grafana.com</a> for the full documentation,
examples and guides.</p>

<div align="center">

[![CI](https://github.com/prometheus/prometheus/actions/workflows/ci.yml/badge.svg)](https://github.com/prometheus/prometheus/actions/workflows/ci.yml)
![Docker Repository on Quay](https://quay.io/repository/prometheus/prometheus/status)
![Docker Pulls](https://img.shields.io/docker/pulls/prom/prometheus.svg?maxAge=604800)
[![Go Report Card](https://goreportcard.com/badge/github.com/prometheus/prometheus)](https://goreportcard.com/report/github.com/prometheus/prometheus)
[![CII Best Practices](https://bestpractices.coreinfrastructure.org/projects/486/badge)](https://bestpractices.coreinfrastructure.org/projects/486)
[![Gitpod ready-to-code](https://img.shields.io/badge/Gitpod-ready--to--code-blue?logo=gitpod)](https://gitpod.io/#https://github.com/prometheus/prometheus)
[![Fuzzing Status](https://oss-fuzz-build-logs.storage.googleapis.com/badges/prometheus.svg)](https://bugs.chromium.org/p/oss-fuzz/issues/list?sort=-opened&can=1&q=proj:prometheus)

</div>

## Monitoramento Nifi - PrometheusReportingTask

<p align="justify"> Verificamos a exsitência de um <b>reporting task chamado PrometheusReportingTask<b> que relata métricas no formato Prometheus criando um endpoint /metrics HTTP(S) que pode ser usado para monitoramento externo do aplicativo. A tarefa de relatório relata um conjunto de métricas sobre a JVM (opcional) e a instância NiFi. Observe que, se o servidor Jetty subjacente (ou seja, o endpoint Prometheus) não puder ser iniciado (por exemplo, se duas instâncias PrometheusReportingTask forem iniciadas na mesma porta), isso poderá causar um atraso no desligamento do NiFi enquanto ele aguarda a limpeza dos recursos do servidor acima. </p>

    Metricas disponibilizadas
        -NiFi Metrics
            FlowFilesReceivedLast5Minutes
            BytesReceivedLast5Minutes
            FlowFilesSentLast5Minutes
            BytesSentLast5Minutes
            FlowFilesQueued
            BytesQueued
            BytesReadLast5Minutes
            BytesWrittenLast5Minutes
            ActiveThreads
            TotalTaskDurationSeconds
            TotalTaskDurationNanoSeconds
        - JVM Metrics
            jvm.uptime
            jvm.heap_used
            jvm.heap_usage
            jvm.non_heap_usage
            jvm.thread_states.runnable
            jvm.thread_states.blocked
            jvm.thread_states.timed_waiting
            jvm.thread_states.terminated
            jvm.thread_count
            jvm.daemon_thread_count
            jvm.file_descriptor_usage
            jvm.gc.runs
            jvm.gc.time 

<p align="justify"> Para habilitar o <b>ReportingTask PrometheusReportingTask<b> basta entrar na UI do Nifi, menu hamburger no canto superior direito e selecionar Controller Settings </p>

![**Nifi**](nifi/images/image001.jpeg)
<p align="justify"> Clicamos em Create a new reporting task </p>

![**Nifi**](nifi/images/image002.jpeg)
<p align="justify"> selecionamos PrometheusReportingTask </p>

![**Nifi**](nifi/images/image003.jpeg)
<p align="justify"> e iniciamos o serviço - ícone de start </p>

![**Nifi**](nifi/images/image004.jpeg)
<p align="justify"> Com o serviço iniciado basta iniciarmos nosso fluxo de dados do Nifi e de forma automática, as metricas são enviadas para o prometheus, e disponníveis para visualição em gráficos do Grafana.</p>

![**Nifi**](nifi/images/image005.jpeg)
![**Nifi**](nifi/images/image006.jpeg)
![**Nifi**](nifi/images/image007.jpeg)
![**Nifi**](nifi/images/image008.jpeg)
![**Nifi**](nifi/images/image009.jpeg)

## Monitoramento Nifi - CustomReportingTask

<p align="justify">Podemos também criar <b>ReportingTasks</b> customizados, de acordo com nossas necessidades. para isto precisamos criar implementações em java, com classes sendo criadas extendendo AbstractReportingTask e sobrepondo dados de PropertyDescriptor. Criamos então um arquivo .nar (NiFi Archive) com a implementação Java supracitada e disponibilizamos este arquivo no containser NiFi em execução</p>

![**Nifi**](nifi/images/image010.jpeg)
![**Nifi**](nifi/images/image011.jpeg)
![**Nifi**](nifi/images/image012.jpeg)

<p align="justify"> Uma vez que o <b>ReportingTask customizado<b> é disponibilizado no container do NiFi,  basta entrar na UI do Nifi, menu hamburger no canto superior direito e selecionar Controller Settings </p>

![**Nifi**](nifi/images/image001.jpeg)
<p align="justify"> Clicamos em Create a new reporting task </p>

![**Nifi**](nifi/images/image002.jpeg)
<p align="justify"> selecionamos o ReportingTask criado (InfliuxBulletinReportingtask) e realizamos as configurações/apontamentos necessários e iniciamos o serviço clicando no ícone start</p>

![**Nifi**](nifi/images/image013.jpeg)
![**Nifi**](nifi/images/image014.jpeg)

<p align="justify"><b>Próximos passos é realizar uma implementação em ambiente de desenvolvimento do BB, recuperando dados real time de um fluxo em execução, e criando um reportingTask conforme nossas necessidades. </p>
