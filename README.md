# PCD - Análise de Espectroscopia
Repositório para códigos em técnicas de análise dos resultados de espectroscopia.

Minha ideia para o código era que fosse o mais fácil e acessível possível. Cada máquina de espectroscopia trabalha de uma maneira diferente e recebemos arquivos tanto em .xlsx quanto em .txt. A leitura desses arquivos é diferente, então defini duas funções que leem esses arquivos e coloquei dentro de uma função maior:
 # Definindo as bibliotecas importadas:
from glob import glob
import pandas as pd
import numpy as np
import os
import matplotlib.pyplot as plt
import shutil

def plotar_graficotxt(arquivo, x, y, marker='', color='black', label='', figsize=(12,8), xlabel='', ylabel='', title='', maximo=True, minimo=True):
    import matplotlib.pyplot as plt
    import pandas as pd
    import numpy as np
    cor_maximo = ''
    cor_minimo = ''
    cores = {range(625, 741): "vermelho",
    range(590, 626): "laranja",
    range(565, 591): "amarelo",
    range(500, 566): "verde",
    range(440, 486): "azul",
    range(380, 441): "violeta"}
    arquivo = f"{arquivo}.txt"
    df = pd.read_csv(arquivo)
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
    arquivo = f"{arquivo}.xlsx"
    df = pd.read_csv(arquivo)
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
    


def plot_grafico(arquivo, x, y, marker='', color='red', label='', figsize=(12,8), xlabel ='', ylabel = '', title = '', maximo = True, minimo = True):
    import os
    from glob import glob
    directory = os.getcwd()
    directory_txt = f"{directory}\\*.txt"
    directory_xlsx = f"{directory}\\*.xlsx"
    list_txt = [i for i in glob(directory_txt)]
    list_xlsx = [i for i in glob(directory_xlsx)]
    arquivo_txt = f"{directory}\\{arquivo}.txt"
    arquivo_xlsx = f"{directory}\\{arquivo}.xlsx"
    if arquivo_txt in list_txt:
        plotar_graficotxt(arquivo, x, y, marker,color,label,figsize,xlabel,ylabel,title, maximo, minimo)
    elif arquivo_xlsx in list_xlsx:
        plotar_graficoxlsx(arquivo, x, y, marker,color,label,figsize,xlabel,ylabel,title, maximo, minimo)


