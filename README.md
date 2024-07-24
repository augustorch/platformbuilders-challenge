# platformbuilders-challenge

## Challenge 
Desenho da arquitetura feito usando componentes de algumas das clouds listadas, e atendendo aos requisitos do cliente

## Flags 
- [x] Desenhar a arquitetura no draw.io 
- [x] Criar um repositório no GitHub incluindo o desenho e uma explicação do mesmo 
- [x] Usar o BigQuery como Data Warehouse 
- [x] Usar o Airflow como orquestrador 
- [x] Extrair dados de uma API 
- [x] Extrair arquivo JSON de um SFTP 
- [x] Disponibilizar os dados para os analistas de dados


## Differentials
Breve texto explicando as decisões tomadas ao analisar os componentes da arquitetura e os relacionamentos entre eles.

## Explanation
Toda a arquitetura foi pensada nas soluções disponíveis no GCP. O motivo foi por questão de familiaridade e maior prática com a plataforma, mas são conceitos fáceis de se adaptar em outras plataformas de cloud. 

Para a extração do JSON do SFTP usei um operador do próprio Airflow que faz essa extração e envia para o GCS. 

Para a API optei por pensar no uso de um operador customizado para ter maior controle no acesso e informações que serão tragas, salvando um PARQUET em uma pasta temporária local e enviado para o GCS. 

Após a ingestão dos dados no GCS, usei o operador do Airflow que trás esses dados do GCS para o BigQuery. Ele consegue lidar com formatos diferentes de arquivos como JSON, CSV e PARQUET. Esse operador será o responsável por inserir os dados nas tabelas designidas. 

Feito todo o processo de extração dos dados, um último operador é executado afim de excluir os arquivos salvos no GCS e evitar custos com armazenamento. 

Todo esse fluxo será orquestrado pelo Airflow a cada 5 mínutos e em caso de erro da DAG será disparada uma mensagem de erro no Slack. 

