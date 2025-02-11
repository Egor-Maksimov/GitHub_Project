Проект "Обучение с учителем: качество модели"

Цели проекта:

Разработать модель, которая предскажет вероятность снижения покупательской активности постоянных клиентов интернет-магазина «В один клик» в следующие три месяца.

План проекта:

1. Загрузка данных: Загрузить данные из таблиц market_file.csv, market_money.csv, market_time.csv и money.csv. Проверить соответствие данных описанию.
2. Предобработка данных: Провести необходимую предобработку данных.
3. Исследовательский анализ данных: Провести анализ данных из каждой таблицы. Отсеять клиентов с покупательской активностью не менее трёх месяцев.
4. Объединение таблиц: Объединить таблицы market_file.csv, market_money.csv и market_time.csv. Разделить данные о выручке и времени на сайте по периодам.
5. Корреляционный анализ: Провести корреляционный анализ признаков в итоговой таблице для моделирования.
6. Разработка модели: Построить модель для предсказания вероятности снижения покупательской активности.
7. Анализ сегментов: Выделить сегменты покупателей с высокой вероятностью снижения активности и высокой прибыльностью. Разработать персонализированные предложения для этих сегментов.

С помощью пайплайнов были обучены модели и подобраны оптимальные гиперпараметры на каждом наборе данных. В качестве гиперпараметров - кодировщиков категориальных признаков использовались OneHotEncoder и OrdinalEncoder. Для масштабирования числовых параметров подбирались методы StandardScaler() и MinMaxScaler(). 

Обучались модели 4-мя методами:
1. LogisticRegression() - метод логистической регрессии с подбором гиперпараметров регуляризации l1_ratio и C
2. SVC() - метод опорных векторов с подбором гиперпараметров степени DEGREE_RANGE для полиномиального ядра и регуляризации C_RANG для полиномиального и линейного ядер
3. KNeighborsClassifier() - метод k ближайших соседей с подбором количества определяющих соседей n_neighbors в качестве гиперпараметра
4. DecisionTreeClassifier() - дерево решений с подбором гиперпараметра глубины max_depth
В качестве сравнения для выбора модели и подбора гиперпараметров использовалась метрикка Roc-Auc. Лучшие метрики показали линейные модели, обученные на основе набора со среднемесячной выручкой.

Для дальнейшего использования была выбрана модель логистической регрессии с балансным уровнем регуляризации L1_ratio на уровне 0.5 и с силой регуляризации C = 0.3. Категориальные признаки решено кодировать методом OneHotEncoder(), количественные признаки масштабировать методом MinMaxScaler(). Рассчитанная метрика Roc-Auc для этой модели: 0.9275.

Были определены наиболее значимые для модели признаки, определяющие вероятность снижения активности:

1. среднее количество страниц, которые просмотрел покупатель за один визит на сайт за последние 3 месяца.
2. время, проведённое на сайте в минутах за текущий и прошедший месяцы
3. Среднее кол-во категорий, которые покупатель просмотрел за визит в течение последнего месяца.
4. среднемесячная доля покупок по акции от общего числа покупок за последние 6 месяцев.
5. среднемесячное значение маркетинговых коммуникаций компании, которое приходилось на покупателя за последние 6 месяцев.
6. общее число неоплаченных товаров в корзине за последние 3 месяца.

Изучена группа клиентов с высокой вероятностью снижения покупательской активности и наиболее высокой прибыльностью. Был выделен сегмент покупателей с уровнем прибыльности выше среднего (4000). Учитывая исследования влияния признаков на вероятность снижения активности методами Shap, были предложены следующие решения для уменьшения риска снижения активности:

1. Разработать структуру сайта, стимулирующую увеличение количества просмотренных страниц за сеанс:
- улучшить навигацию сайта, поиск, фильтрацию
- поработать со связанным контекстом
- поработать с рекомендациями
2. Увеличить продуктовый ассортимент, это также должно задержать клиента на маркет-плейсе
3. Повысить маркетинговую активность при работе с клиентами, увеличить количество предложений, рекламы. Особенно с клиентами, предпочитающими покупки по акции.