
# Construção de mapas interativos para os dados do Censo

Extração com Python de dados do IBGE e plotagem de mapas utilizando Plotly


## Overview
Esse é um projeto de visualização de dados, que se utiliza dos resultados de variação populacional do Censo de 2022, extraídos do porta SIDRA através da biblioteca Sidrapy para a plotagem de um mapa interativo utilizando Plotly.
O projeto também acompanha um vídeo gravado para o nosso canal [FEA.dev](https://www.youtube.com/@FEADev), onde mostramos passo a passo esse processo.

[Clique para assistir ao vídeo 👇](https://www.youtube.com/watch?v=arLyMJSFG-M)
![Capa do vídeo IBGE para o YT](https://github.com/fea-dev-usp/IBGE/assets/90975619/f2c246fe-6518-4bc4-891a-f28fcb6f79a9)

## Instalação

Foram utilizadas as seguintes dependências:

__Extração de dados:__
```bash
  import requests
  import wget
  import zipfile
  import sidrapy

  from urllib.request import urlopen
  from io import BytesIO
```

__Manipulação de dados:__

```bash
  import numpy as np 
  import pandas as pd
  import json
```

__Visualização de dados:__
```bash
  import matplotlib.pyplot as plt 
  import plotly.express as px
```
Caso falte alguma dependência na sua máquina, basta instalar com o comando pip
```bash
# Ex:
  pip install sidrapy
```

    
## Dados
Os dados foram extraidos diretamente do IBGE utilizando-se sidrapy. Essa é uma parte um tanto "traiçoeira" dado que os parâmetros da função são preenchidos através de códigos específicos

```bash
    pop = sidrapy.get_table(
      table_code='4709',              # t (table_code) - é o código da tabela referente ao indicador e a pesquisa;
      territorial_level='6',          # n (territorial_level) - especifica os níveis territoriais;
      ibge_territorial_code='all',    # n/ (ibge_territorial_code) - inserido dentro do nível territorial, especificar o código territorial do IBGE;
      period='all',                   # p (period) - utilizado para especificar o período;
      variable='all'                  # v (variable) - para especificar as variáveis desejadas;
    )
```
A tabela utilizada e os parâmetros definidos podem ser consultados aqui:
- Tabelas Censos - https://sidra.ibge.gov.br/pesquisa/censo-demografico/demografico-2022/primeiros-resultados-populacao-e-domicilios
- Parâmetros da API -> Consulta: https://apisidra.ibge.gov.br/

### GeoJSON
Boa parte da dificuldade deste projeto está em encontrar as coordenadas necessárias para a plotagem do mapa. Ao contrário do mapa dos EUA, que já está embutido no Plotly, aqui precisamos de todas as coordenadas que delimitam cada município do Brasil.

Assim, extraimos direto da página de malhas territoriais do IBGE, as delimitações de cada cidade e geramos um [geojson_2022](https://github.com/fea-dev-usp/IBGE/blob/master/geojson_2022.json). Inclusive, esperamos que essa seja outra contribuição do nosso projeto para a comunidade. 

O código desse processo pode ser conferido no notebook [getting_json_ibge](https://github.com/fea-dev-usp/IBGE/blob/master/getting_json_ibge.ipynb).
## Resultados

Para a plotagem, utilizamos a função choropleth_mapbox e o resultado pode-se observar a seguir:

![Mapa](https://github.com/fea-dev-usp/IBGE/assets/90975619/27ec5572-3a62-4ce4-bf3b-c45f308e1c36)

