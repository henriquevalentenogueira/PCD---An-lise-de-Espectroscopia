# PCD - Análise de Espectroscopia
Repositório para códigos em técnicas de análise dos resultados de espectroscopia.

## Leitura e plotagem dos arquivos
Minha ideia para o código era que fosse o mais fácil e acessível possível. Cada máquina de espectroscopia trabalha de uma maneira diferente e recebemos arquivos tanto em .xlsx quanto em .txt. A leitura desses arquivos é diferente, então defini duas funções que leem esses arquivos e coloquei dentro de uma função maior:
 
# Criando uma função para leitura, análise e plotagem de arquivos em txt: 
A função terá 12 argumentos, sendo que somente três são essenciais: o nome do arquivo, o eixo x e o eixo y. Os outros argumentos são parâmetros da plotagem e a definição se encontrar o máximo e o mínimo da função é necessário.
```python
def plotar_graficotxt(arquivo, x, y, marker='', color='black', label='', figsize=(12,8), xlabel='', ylabel='', title='', maximo=True, minimo=True):
```
Acima, estão os doze parâmetros da função: o arquivo escolhido, o eixo x, o eixo y, a escolha de marcador da linha, a cor da linha, o nome da linha, o tamanho da figura, o nome para o eixo x, o nome para o eixo y, o título do gráfico, se precisa achar o máximo e se precisa encontrar o mínimo. 
Notaremos que o arquivo não precisa ser nomeado como '*.txt', o que facilita o usuário.
```python
    import matplotlib.pyplot as plt
    import pandas as pd
    import numpy as np
    cor_maximo = ''
    cor_minimo = ''
```
Agora, definiremos um dicionário básico para a escolha das cores.
```python
    cores = {range(625, 741): "vermelho",
    range(590, 626): "laranja",
    range(565, 591): "amarelo",
    range(500, 566): "verde",
    range(440, 486): "azul",
    range(380, 441): "violeta"}
```
Começaremos agora a leitura do arquivo
```python
    arquivo = f"{arquivo}.txt"
    df = pd.read_csv(arquivo)
```
Para a análise do ponto máximo e mínimo, tive que definir um valor mínimo e máximo, pois existem limitações na leitura pela máquina.
```python
    ponto_maximox, ponto_maximoy = df.loc[df['Wavelength (nm)'] >= 240].sort_values(by='Absorbance', ascending=False).iloc[0, 0], df.loc[df['Wavelength (nm)'] >= 240].sort_values(by='Absorbance', ascending=False).iloc[0, 1]
    ponto_minimox, ponto_minimoy = df.loc[df['Wavelength (nm)'] >= 240].sort_values(by='Absorbance', ascending=True).iloc[0, 0], df.loc[df['Wavelength (nm)'] >= 240].sort_values(by='Absorbance', ascending=True).iloc[0, 1]
```
Plotagem do gráfico. Além de plotar, defini que, se o usuário quiser o ponto máximo e mínimo, marcaremos ele no gráfico.
```python
    ax = df.plot(x=x, y=y, marker=marker, color=color, label=label, figsize=figsize)
    plt.xlabel(xlabel)
    plt.ylabel(ylabel)
    plt.title(title)
    if maximo == True:
        plt.scatter(ponto_maximox, ponto_maximoy, color='red', label='Ponto máximo')
        plt.text(ponto_maximox, ponto_maximoy, f'({ponto_maximox:.2f}, {ponto_maximoy:.2f})', fontsize=10, ha='center', va='bottom', color='black')
        for faixa, cor in cores.items():
            if ponto_maximox in faixa:
                print('O ponto máximo de absorção da luz pela solução ocorre na cor ', cor)
            break 
    if minimo == True:
        plt.scatter(ponto_minimox, ponto_minimoy, color='blue', label='Ponto mínimo')
        plt.text(ponto_minimox, ponto_minimoy, f'({ponto_minimox:.2f}, {ponto_minimoy:.2f})', fontsize=10, ha='center', va='top', color='black')
        for faixa, cor in cores.items():
            if ponto_minimox in faixa:
                print('O ponto mínimo de absorção da luz pela solução ocorre na cor ', cor)
                break
    plt.legend(loc='best', fontsize='medium', frameon=True, shadow=True)
    plt.show()
```
## Criando uma função para os arquivos em xlsx. 
A estrutura do código não muda muito em relação ao código anterior. Por isso, comentarei somente a parte modificada.
```python
def plotar_graficoxlsx(arquivo, x, y, marker='', color='black', label='', figsize=(12,8), xlabel='', ylabel='', title='', maximo=True, minimo=True):
    import matplotlib.pyplot as plt
    import pandas as pd
    import numpy as np
    cores = {range(625, 741): "vermelho",
    range(590, 626): "laranja",
    range(565, 591): "amarelo",
    range(500, 566): "verde",
    range(440, 486): "azul",
    range(380, 441): "violeta"}
```
A alteração foi aqui: transformei a string 'arquivo' em uma string 'arquivo.xlsx', e a li pelo read_xlsx.
arquivo = f"{arquivo}.xlsx"
```python
    df = pd.read_xlsx(arquivo)
    ponto_maximox, ponto_maximoy = df.loc[df['Wavelength (nm)'] >= 240].sort_values(by='Absorbance', ascending=False).iloc[0, 0], df.loc[df['Wavelength (nm)'] >= 240].sort_values(by='Absorbance', ascending=False).iloc[0, 1]
    ponto_minimox, ponto_minimoy = df.loc[df['Wavelength (nm)'] >= 240].sort_values(by='Absorbance', ascending=True).iloc[0, 0], df.loc[df['Wavelength (nm)'] >= 240].sort_values(by='Absorbance', ascending=True).iloc[0, 1]
    ax = df.plot(x=x, y=y, marker=marker, color=color, label=label, figsize=figsize)
    plt.xlabel(xlabel)
    plt.ylabel(ylabel)
    plt.title(title)
    if maximo == True:
        plt.scatter(ponto_maximox, ponto_maximoy, color='red', label='Ponto máximo')
        plt.text(ponto_maximox, ponto_maximoy, f'({ponto_maximox:.2f}, {ponto_maximoy:.2f})', fontsize=10, ha='center', va='bottom', color='black')
        for faixa, cor in cores.items():
            if ponto_maximox in faixa:
                print('O ponto máximo de absorção da luz pela solução ocorre na cor ', cor)
            break 
    if minimo == True:
        plt.scatter(ponto_minimox, ponto_minimoy, color='blue', label='Ponto mínimo')
        plt.text(ponto_minimox, ponto_minimoy, f'({ponto_minimox:.2f}, {ponto_minimoy:.2f})', fontsize=10, ha='center', va='top', color='black')
        for faixa, cor in cores.items():
            if ponto_minimox in faixa:
                print('O ponto mínimo de absorção da luz pela solução ocorre na cor ', cor)
                break
    plt.legend(loc='best', fontsize='medium', frameon=True, shadow=True)
    plt.show()
    


