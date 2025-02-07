import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Задаем пути к файлам
bacteria_file = r"D:\333\pr.xlsx"  # Файл с бактериями
eukaryote_file = r"D:\333\eu.csv"  # Файл с эукариотами

# Чтение данных
bacteria_data = pd.read_excel(bacteria_file)
eukaryote_data = pd.read_csv(eukaryote_file)

# Вывод первых строк для проверки
print("Данные бактерий:")
print(bacteria_data.head())

print("\nДанные эукариотов:")
print(eukaryote_data.head())

# Проверка уникальных значений для MYCTU и CHLVA
print("\nПоиск MYCTU в данных бактерий:")
for column in bacteria_data.columns:
    if 'MYCTU' in str(bacteria_data[column].unique()):
        print(f"Найдено в столбце {column}:")
        print(bacteria_data[column].unique()[:10])

print("\nПоиск CHLVA в данных эукариотов:")
for column in eukaryote_data.columns:
    if 'CHLVA' in str(eukaryote_data[column].unique()):
        print(f"Найдено в столбце {column}:")
        print(eukaryote_data[column].unique()[:10])

# Фильтрация данных по выбранным организмам
bacteria_filtered = bacteria_data[bacteria_data['Name.4'].str.contains('MYCTU', na=False)]  # MYCTU в Name.4
eukaryote_filtered = eukaryote_data[eukaryote_data['Name.1'].str.contains('CHLVA', na=False)]  # CHLVA в Name.1

# Вывод отфильтрованных данных
print("\nОтфильтрованные данные для MYCTU (Mycobacterium tuberculosis):")
print(bacteria_filtered.head())

print("\nОтфильтрованные данные для CHLVA (Chlorella variabilis):")
print(eukaryote_filtered.head())

# Извлечение длин белков
bacteria_lengths = bacteria_filtered['Length.5']  # Длины белков для MYCTU в Length.5
eukaryote_lengths = eukaryote_filtered['Length']  # Длины белков для CHLVA в Length

# Проверка наличия данных
if bacteria_lengths.empty or eukaryote_lengths.empty:
    print("Ошибка: Отсутствуют данные для одного из организмов!")
else:
    # Вывод статистики
    print("\nСтатистика для MYCTU (Mycobacterium tuberculosis):")
    print(bacteria_lengths.describe())

    print("\nСтатистика для CHLVA (Chlorella variabilis):")
    print(eukaryote_lengths.describe())

    # Совместная гистограмма
    plt.figure(figsize=(10, 6))
    sns.histplot(bacteria_lengths, bins=50, color='blue', alpha=0.5, label='MYCTU')
    sns.histplot(eukaryote_lengths, bins=50, color='green', alpha=0.5, label='CHLVA')
    plt.title('Совместная гистограмма длин белков (MYCTU vs CHLVA)')
    plt.xlabel('Длина белка')
    plt.ylabel('Частота')
    plt.legend()
    plt.savefig(r"D:\333\histogram.png")  # Сохранение графика
    plt.show()

    # Ящики с усами
    plt.figure(figsize=(8, 6))
    data = [bacteria_lengths, eukaryote_lengths]
    labels = ['MYCTU', 'CHLVA']
    sns.boxplot(data=data)
    plt.title('Ящики с усами для длин белков (MYCTU vs CHLVA)')
    plt.xlabel('Организмы')
    plt.ylabel('Длина белка')
    plt.xticks(ticks=[0, 1], labels=labels)
    plt.savefig(r"D:\333\boxplot.png")  # Сохранение графика
    plt.show()

    # Эмпирические функции распределения
    plt.figure(figsize=(10, 6))
    sns.ecdfplot(bacteria_lengths, label='MYCTU')
    sns.ecdfplot(eukaryote_lengths, label='CHLVA')
    plt.title('Эмпирические функции распределения (MYCTU vs CHLVA)')
    plt.xlabel('Длина белка')
    plt.ylabel('Кумулятивная частота')
    plt.legend()
    plt.savefig(r"D:\333\ecdf.png")  # Сохранение графика
    plt.show()
