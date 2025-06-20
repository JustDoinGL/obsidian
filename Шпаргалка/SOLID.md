## Single responsibility principle / SRP
[[Принцип единственной ответственности]]

Принцип единственной обязанности / ответственности (**single responsibility principle / SRP**) означает, что каждый объект должен иметь одну обязанность и эта обязанность должна быть полностью инкапсулирована в класс. Все его сервисы должны быть направлены исключительно на обеспечение этой обязанности.

![https://habrastorage.org/r/w1560/webt/ug/2v/ts/ug2vtsbxvspdx0elsmexemp3kxm.png](https://habrastorage.org/r/w1560/webt/ug/2v/ts/ug2vtsbxvspdx0elsmexemp3kxm.png) 
## Open-closed principle / OCP
[[Принцип открытости-закрытости]]

Принцип открытости / закрытости (**open-closed principle / OCP)** декларирует, что программные сущности (классы, модули, функции и т. п.) должны быть открыты для расширения, но закрыты для изменения. Это означает, что эти сущности могут менять свое поведение без изменения их исходного кода.

![https://habrastorage.org/r/w1560/webt/ir/sm/eb/irsmeboddq2dcx1eaky5qo83v64.png](https://habrastorage.org/r/w1560/webt/ir/sm/eb/irsmeboddq2dcx1eaky5qo83v64.png)
## Liskov substitution principle / LSP
[[Принцип подстановки Барбары Лисков]]

Принцип подстановки Барбары Лисков (**Liskov substitution principle / LSP**) в формулировке Роберта Мартина: «функции, которые используют базовый тип, должны иметь возможность использовать подтипы базового типа не зная об этом».

![https://habrastorage.org/r/w1560/webt/hj/dt/a-/hjdta-bs2bvk2ga_dabxajfqjnk.png](https://habrastorage.org/r/w1560/webt/hj/dt/a-/hjdta-bs2bvk2ga_dabxajfqjnk.png)
## Interface segregation principle / ISP
[[Принцип разделения интерфейса]]

Принцип разделения интерфейса (**interface segregation principle / ISP**) в формулировке Роберта Мартина: «клиенты не должны зависеть от методов, которые они не используют». Принцип разделения интерфейсов говорит о том, что слишком «толстые» интерфейсы необходимо разделять на более маленькие и специфические, чтобы клиенты маленьких интерфейсов знали только о методах, которые необходимы им в работе. В итоге, при изменении метода интерфейса не должны меняться клиенты, которые этот метод не используют.

![https://habrastorage.org/r/w1560/webt/v8/co/dn/v8codny8xpy355zcqvfro-7ep8a.png](https://habrastorage.org/r/w1560/webt/v8/co/dn/v8codny8xpy355zcqvfro-7ep8a.png)
## Dependency inversion principle / DIP
[[Принцип инверсии зависимостей]]

Принцип инверсии зависимостей (**dependency inversion principle / DIP**) — модули верхних уровней не должны зависеть от модулей нижних уровней, а оба типа модулей должны зависеть от абстракций; сами абстракции не должны зависеть от деталей, а вот детали должны зависеть от абстракций.

![https://habrastorage.org/r/w1560/webt/du/7r/0u/du7r0use6rfrrqr8wc9a2qcvf2y.png](https://habrastorage.org/r/w1560/webt/du/7r/0u/du7r0use6rfrrqr8wc9a2qcvf2y.png)