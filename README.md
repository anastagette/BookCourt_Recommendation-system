# BookCourt_Recommendation-system
Варианты реализации рекомендательной системы для проекта BookCourt.

### Задача
Пользователь в инструменте «Книжный Тиндер» формирует свой портрет книжных предпочтений. Книги могут быть в состоянии: прочитал понравилась, прочитал не понравилась, хочу прочесть, не читал.
Каждая книга обладает набором характеристик, которые задают контекст портрета пользователя. Список книг конкретного пользователя, которые он оценил таким путем формируют его портрет.

Рекомендации формируются на основе алгоритма:
Книги пользователя -> Список наиболее вероятных книг, которые понравятся пользователю.

Необходимо:
определить входные параметры для обучения модели;
предложить свое видение архитектуры этой модели и алгоритм (можно описать словами, схема);


### User-based recommendation system
Имеются данные о пользователях и книгах в состоянии: пользователь прочитал понравилась, прочитал не понравилась, хочет прочесть, не читал.

Определим для каждого состояния класс:
0 - прочитал понравилась,
1 - прочитал не понравилась,
2 - хочет прочесть,
3 - не читал.

Представим входные данные в виде матрицы N x M, где N - количество пользователей, M - количество книг.
Пример (5 пользователей, 6 книг):
[[1, 0, 2, 3, 1, 0]
 [0, 1, 2, 1, 0, 0]
 [1, 1, 0, 0, 1, 3]
 [2, 2,  1, 0, 3, 1]
 [3, 3, 0, 0, 1, 1]]
У каждого пользователя каждая книга отмечена одним из вышеуказанных классов.

Для каждого пользователя находим его ближайших соседей, посчитав между ними косинусное сходство. Ищем, какие среди непрочитанных пользователем книги (класс 3) были отмечены его соседями как понравившиеся (класс 0). Выводим эти книги как рекомендованные к прочтению в порядке от самого близкого соседа до самого дальнего.


### Content-based recommendation system
На входе датасет из описаний книг и ранее описанная матрица N x M, где N - количество пользователей, M - количество книг.

Делаем предобработку описаний книг (приведение текста в нижний регистр, лемматизация, удаление знаков пунктуации). Векторизуем книги методом TF-IDF.

Выбираем векторы книг, которые пользователь отметил как понравившиеся. Кластеризуем эти книги. Находим центр каждого кластера. Выбираем непрочитанные пользователем книги. Для каждого центра кластера находим k ближайших непрочитанных пользователем книг методом подсчета косинусного сходства между векторами описаний книг. На выходе выдаем наиболее близкие к центрам кластеров книги.
