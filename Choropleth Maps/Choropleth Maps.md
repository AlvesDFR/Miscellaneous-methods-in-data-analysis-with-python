# World Choropleth Map


```python
import plotly.graph_objects as go
import pandas as pd
from pycountry import countries
```


```python

# Carregar o DataFrame
df = pd.read_csv('PATH/species_count_by_countries.csv')

```


```python
print(df.columns)
```

    Index(['Unnamed: 0', 'TERRITORY1', 'specie'], dtype='object')
    


```python
# Função para converter nomes de países para ISO Alpha-3
def get_iso_alpha_3(country_name):
    try:
        return countries.lookup(country_name).alpha_3
    except LookupError:
        return None

# Adicionando uma nova coluna com os códigos ISO Alpha-3
df['iso_alpha_3'] = df['TERRITORY1'].apply(get_iso_alpha_3)

# Verifique se todos os países foram corretamente mapeados
print(df.head())
```

       Unnamed: 0 TERRITORY1  specie iso_alpha_3
    0           1     Alaska       2        None
    1           2    Albania       2         ALB
    2           3    Algeria       3         DZA
    3           4  Argentina       3         ARG
    4           5  Australia       7         AUS
    


```python
# Criar o mapa coroplético com dados
data = go.Choropleth(
    locations=df['iso_alpha_3'],
    locationmode='ISO-3',
    z=df['specie'],
    text=df['TERRITORY1'],
    colorscale='Viridis',
    colorbar=dict(title='Number of Species'),
    marker_line_color='black',  # Cor dos contornos dos países
    marker_line_width=0.5  # Espessura dos contornos
)

```


```python

# Atualizar o layout para mostrar contornos dos países e configurar o mapa
layout = go.Layout(
    title='Number of Invasive Species',
    geo=dict(
        showframe=False,
        projection=dict(type='natural earth'),
        showcoastlines=True,
        coastlinecolor='Black',
        showland=True,
        landcolor='rgb(255, 255, 255)',  # Cor branca para países sem dados
        showocean=True,
        oceancolor='rgb(204, 204, 255)',
        showlakes=True,
        lakecolor='rgb(200, 200, 200)',  # Cor cinza para os lagos
        countrycolor='Black',  # Cor dos contornos dos países
        countrywidth=0.5,  # Espessura dos contornos dos países
        showcountries=True  # Certifica que todos os países sejam exibidos
    )
)

# Criar o mapa
choromap = go.Figure(data=[data], layout=layout)
choromap.show()
```


