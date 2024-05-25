# Детектирование кольцевых галактик с помощью DL

Студент: Андрей Шан avshan@edu.hse.ru

Научный руководитель: Ольга Касьяновна Сильченко sil@sai.msu.ru

## Задача
Создание нового каталога кольцевых галактик с помощью нейронных сетей. 

## Примеры результатов

Примеры результатов классификации могут быть просмотрены [тут](https://ring-galaxy-detection.readthedocs.io/ru/latest/)

## Описание работы

### Target mining

Получить большой каталог кольцевых галактик не удалось, по заверениям автора он не будет готов к публикации еще длительное время. В связи с этим взял [Galactic Rings Revisited. I. CVRHS Classifications of 3962 Ringed Galaxies from the Galaxy Zoo 2 Database](https://arxiv.org/abs/1707.06589) (3962 галактик). Так как галактики были собраны из проекта Galaxy Zoo, данный каталог лучше всего подходит для работы с уже существующей инфраструктурой работы с галактиками. В результате парсинга удалось получить **3960 галактик**. 

Далее я предпринял попытку соотнести полученные галактики с более новым проектом Galaxy Zoo DECALS. таким образоv в Galaxy Zoo DECALS удалось найти **1239 галактик** из 3960 на предыдущем шаге, далее **gzd5**. 

### train test val split

Из-за малого количества положительных примеров данных, и крайне большого количества отрицательных был применен и undersampling (positive x 1-) и oversampling (x3).

Данные были разделены на train val и test в пропорциях 70/10/20. Баланс классов составляет 1/10.

Размеры выборок train - 28620, val - 4049, test - 8218

### Обучение

Были дообучены модели архитектуры ConvNext. Были обучены модели с комбинацией признаков:

- Размер модели: 15м, 58м, 88м параметров
- Количество размороженных блоков: 1 или 2
- Lr Scheduler: константа и косинус

Все замеренные метрики обучения доступны на [WandB](https://wandb.ai/mlds-avshan/rings-galaxy-detection/overview).

Как лучшая была выбрана модель gzd5-convnext_small-1-cosine, изза низкого переобучения и высокого aucroc и accuracy

# Инференс

После выбора лучшей модели был произведен инференс модели. На галактика которые не были ранее размечены как кольцевые.

# Создание каталога

Для создания каталога необходимо было выбрать порог классификации. Для нахождения порога была выбрана оптимизация F-score.  Таким образом был создан каталог из 4664 галактик. 

Каталог доступен в данном репозитории под именем ```ring_galaxies_catalogue.csv```