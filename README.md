# news_classification

Предсказание новостных тематик

Задача предсказания тем новостей, в рамках открытого буткемпа "First Step in NLP: 2.0" от ФКН ВШЭ.

Описание соревнования - https://www.kaggle.com/competitions/news-scraping-competition

## Задача  

1. Спарсить обучающие данные из открытых новостных источников (например, РИА-новости, Lenta.ru и любые другие сайты) и обучить на них модель для прогноза темы новости (только по указанным темам: 'Общество/Россия', 'Экономика', 'Силовые структуры', 'Бывший СССР', 'Спорт', 'Забота о себе', 'Строительство', 'Туризм/Путешествия', 'Наука и техника')
2. Применить обученную модель к тестовым новостям и спрогнозировать тему (ее числовой код). Пример файла с прогнозом - base_submission_news.csv

 <img width="399" alt="image" src="https://github.com/lteplova/news_classification/assets/38242392/3fccc4a3-0c1c-4fb3-821e-4aca75befb9c">

 ## Данные 

Был выполнен парсинг данных с сайта lenta.ru и fontanka.ru.
Код парсинга находится в ноутбуке news_parsing.ipynb
Далее был получен [train.df](https://www.kaggle.com/datasets/luydmilateplova/lenta-f) (это датафрейм, содержащий 83934 строки и 4 колонки - `link/text/target/text_prep`)
text_prep - колонка с предобработанным текстом
В данных есть дисбаланс:  

<img width="206" alt="image" src="https://github.com/lteplova/news_classification/assets/38242392/f9dc001c-53b8-40ed-88df-1adf35dd3c29">
<img width="965" alt="image" src="https://github.com/lteplova/news_classification/assets/38242392/6bc1b52e-9b0c-4ad3-9166-05c69fd3b5c5">

 ## Решение  

Для парсинга новостей используется библиотеки `requests, BeautifulSoup`, но в этом случае загрузка данных работает довольно долго. Есть второй вариант для  lenta.ru  - использование API, позволяет за несколько минут получить около 10000 статей.  

Векторизация текста с помощью `TfidfVectorizer` (проверялся вариант `Word2Vec`, но не показал положительного прироста метрики)
Классификацию пробуем несколькими способами, в итоге самые лучшие и быстрые результаты показали модели классификации из `sklearn`

 <img width="259" alt="image" src="https://github.com/lteplova/news_classification/assets/38242392/7cf40ca7-a34a-4ecd-9494-2c85db737e45">

Вариант с транформерами (модель `DeepPavlov/rubert-base-cased`) не дала улучшения, при этом гораздо более ресурсозатратная как по времени, так и по процессорному времени.

Моя метрика на тестовых данных (которые модель не видела - 0.85652)  

## Используемые технологии  

* requests
* BeautifulSoup
* sklearn.feature_extraction (TfidfVectorizer)
* sklearn.linear_model (LogisticRegression, SGDClassifier)
* sklearn.preprocessing (MaxAbsScaler, StandardScaler)
* xgboost
* gensim.models (Word2Vec)
* transformers: AutoTokenizer, BertForSequenceClassification
* torch
