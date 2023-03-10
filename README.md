# Изображение проекции полиэдра

Построение изображения полиэдра с удалением невидимых линий — пример
классической задачи, для успешного решения которой необходимо знакомство
с основами вычислительной геометрии. Данный материал является первым из
трёх, посвящённых этому проекту.

## Постановка задачи

В настоящее время во многих областях науки и техники используются
различные методы моделирования геометрических объектов в трёхмерном
пространстве. Один из способов моделирования состоит в том, чтобы
аппроксимировать реальный объект набором выпуклых плоских
многоугольников. Такой набор мы будем называть *полиэдром*,
многоугольники — *гранями*, а их стороны — *рёбрами* полиэдра.
Вот формальные определения введённых терминов.

Полиэдр (polyedr)
:    Множество выпуклых плоских многоугольников.

Грань полиэдра (facet)
:    Выпуклый плоский многоугольник.

Ребро полиэдра (edge)
:    Любая из сторон одной из граней.

Вершина полиэдра (vertex)
:    Любая из вершин произвольной грани.

Примерами полиэдров могут служить поверхности куба, призмы и пирамиды
(включая усечённую), листы полураскрытой веером книги и даже просто
два-три плоских выпуклых многоугольника, расположенных в трёхмерном пространстве
друг относительно друга произвольным образом.

При изображении полиэдра реально рисуются не сами многоугольники, а
линии, их ограничивающие, то есть их рёбра. Если, однако, изобразить
*все* рёбра полиэдра, то получится весьма запутанная, непривычная для
человека картина. Для получения удобного для человека рисунка нужно при
построении изображения учитывать, что некоторые рёбра могут оказаться
полностью или частично *невидимыми*, так как их могут загородить грани
полиэдра.

> Здесь следует запустить программу, которая строит изображения
> нескольких несложных полиэдров, и убедиться в том, насколько
> *неправильным* является рисование всех рёбер полиэдра.

Таким образом, при построении изображения надо *удалять* (не рисовать)
части рёбер, которые не видны. По этой причине рассматриваемая задача в
литературе часто называется задачей *удаления невидимых линий*.

Изображение полиэдра зависит от того, *откуда* на него посмотреть. 
Это направление (взгляда) должно быть задано в точной постановке задачи.
Направление, противоположное направлению взгляда, будем называть
*вектором проектирования* и считать, что он направлен вверх. Этого
всегда можно добиться, осуществив *поворот* полиэдра вместе с заданным
вектором проектирования. Для того чтобы упростить терминологию,
договоримся называть в дальнейшем параллельные вектору проектирования
прямые и плоскости *вертикальными*, а ортогональные
ему — *горизонтальными*.

> Здесь полезно «покрутить/повертеть» простейший полиэдр.
>
> Сначала используя программу, изображающую **все рёбра**,
> а затем — программу, **удаляющую невидимые рёбра**.

Рассматриваемая задача, таким образом, сводится к следующей: построить
проекцию полиэдра с удалением невидимых линий на *бесконечно высокую*
горизонтальную плоскость. Требуемое плоское изображение при этом задано
не однозначно, так как вращения вокруг вертикальной оси приводят к
поворотам итоговой картинки. Будем поэтому считать, что полная
постановка задачи кроме самого полиэдра содержит также задание вращения
трёхмерного пространства.

## Задание полиэдра

При задании полиэдра всю информацию о нём разделяют на две части. Первая
из них задаёт так называемый *абстрактный* полиэдр. При этом
определяется только количество граней, рёбер и вершин в нём с указанием
того, какие именно рёбра принадлежат отдельным граням, а
вершины — рёбрам. Вторая часть информации, называемая *метрической*,
определяет координаты всех вершин полиэдра в пространстве.

Информация, представляющая полиэдр, является достаточно объёмной, и
поэтому было бы весьма неудобно вводить её, используя клавиатуру.
Гораздо разумнее создать описание полиэдра в специальном файле, а
программе передать его имя. Вот как будет выглядеть этот файл,
представляющий поверхность единичного куба с центром в начале координат
и гранями, параллельными координатным плоскостям:

    200.0   45.0    45.0    30.0
    8       6       24
    -0.5    -0.5    0.5
    -0.5    0.5     0.5
    0.5     0.5     0.5
    0.5     -0.5    0.5
    -0.5    -0.5    -0.5
    -0.5    0.5     -0.5
    0.5     0.5     -0.5
    0.5     -0.5    -0.5
    4       1    2    3    4
    4       5    6    2    1
    4       3    2    6    7
    4       3    7    8    4
    4       1    4    8    5
    4       8    7    6    5

Можете ли вы объяснить содержимое этого файла?

Первая строка содержит коэффициент гомотетии и три угла Эйлера, задающие
вращение пространства (о преобразованиях пространства будет сказано чуть
позже). Вторая строка говорит о том, что у куба 8 вершин, 6 граней и
*24* ребра. Число 24, вдвое превышающее реальное количество рёбер у
куба, является результатом того факта, что каждое из рёбер куба
принадлежит ровно *двум* граням. Следующая часть файла содержит
метрическую информацию — координаты всех восьми вершин полиэдра, а в
заключительной части файла для каждой из шести граней куба указываются
количество её вершин и перечисляются их номера.


![Шахматный король](images/king.png)
