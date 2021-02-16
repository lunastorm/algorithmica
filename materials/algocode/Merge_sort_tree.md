#### Задача

Дан массив $a_0,\\ \\ldots,\\ a_{n-1}$. Требуется ответить на $q$
запросов:

  - $?\\ l,\\ r,\\ x$ — количество элементов на $a_l,\\ \\ldots,\\
    a_r$, меньших $x$.

#### Решение

Если подотрезок массива отсортирован, то мы умеем отвечать на такой
запрос за $O(\\log n)$ с помощью [бинпоиска](Бинпоиск "wikilink").
Построим [дерево отрезков](Дерево_отрезков "wikilink") на нашем
массиве, а в каждой вершине будем хранить отсортированную версию
соответствующего подотрезка массива. Тогда ответ на запрос работает за
$O(\\log^2 n)$ — мы делаем обычный get-запрос в дереве отрезков,
отличающийся лишь получением ответа на подотрезке, потому что
там мы будем делть [бинпоиск](Бинпоиск "wikilink").

#### Построение

Для того, чтобы быстро построить merge sort tree, воспользуемся идеей
[merge sort'а](Сортировка_слиянием "wikilink") (собственно, отсюда и
название). Для каждой вершины будем хранить копию ее подотрезка (о
затратах на память поговорим позже). Если вершина отвечает за отрезок
длины 1, то ее массив уже отсортирован. А если у сыновей вершиины
отсортированы массивы, то мы их можем просто слить за линейное
время.

#### Время работы и используемая память

Каждый элемент массива будет храниться в $O(\\log n)$ массивах — на пути
от листа до корня, поэтому наша структура будет занимать $O(n \\log n)$
памяти. Построение работает за $O(n \\log n)$ (см.
[тут](Сортировка_слиянием#Асимптотика "wikilink")), а
ответ на запрос за $O(\\log^2 n)$.

``` c++ numberLines
struct segtree {
    vector<int> val;
};

segtree t[4 * MAXN];

void build(int v, int tl, int tr) {
    if (tl + 1 == tr) {
        t[v].val.push_back(a[tl]);
        return;
    }
    int tm = (tl + tr) / 2;
    build(2 * v, tl, tm);
    build(2 * v + 1, tm, tr);
    t[v].val.resize(t[2 * v].val.size() + t[2 * v + 1].val.size());
    merge(t[2 * v].val.begin(), t[2 * v].val.end(),
              t[2 * v + 1].val.begin(), t[2 * v + 1].val.end(),
              t[v].val.begin());
}

int get(int v, int tl, int tr, int l, int r, int x) {
    if (tl >= r || l >= tr) {
        return 0;
    }
    if (l <= tl && tr <= r) {
        return lower_bound(t[v].val.begin(), t[v].val.end(), x - 1) - t[v].val.begin();
    }
    int tm = (tl + tr) / 2;
    return get(2 * v, tl, tm, l, r, x) + get(2 * v + 1, tm, tr, l, r. x);
}
```

Версия данной задачи с запросами изменения потребует [более тяжелых
структур](Двумерные_структуры_данных "wikilink").

[Категория:Конспект](Категория:Конспект "wikilink") [Категория:Структуры
данных для запросов на
отрезке](Категория:Структуры_данных_для_запросов_на_отрезке "wikilink")