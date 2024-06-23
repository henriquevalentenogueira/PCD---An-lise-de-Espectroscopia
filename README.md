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

# Criando uma função para leitura, análise e plotagem de arquivos em txt: 
A função terá 12 argumentos, sendo que somente três são essenciais: o nome do arquivo, o eixo x e o eixo y. Os outros argumentos são parâmetros da plotagem e a definição se encontrar o máximo e o mínimo da função é necessário.


