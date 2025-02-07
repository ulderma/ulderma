import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Задаем пути к файлам
bacteria_file = r"D:\333\pr.xlsx"  # Файл с бактериями
eukaryote_file = r"D:\333\eu.csv"  # Файл с эукариотами

# Чтение данных
bacteria_data = pd.read_excel(bacteria_file)
eukaryote_data = pd.read_csv(eukaryote_file)
