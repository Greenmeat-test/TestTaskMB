# TestTaskMB

Вводные данные:

у нас мультизональный кластер (три зоны), в котором пять нод
приложение требует около 5-10 секунд для инициализации
по результатам нагрузочного теста известно, что 4 пода справляются с пиковой нагрузкой
на первые запросы приложению требуется значительно больше ресурсов CPU, в дальнейшем потребление ровное в районе 0.1 CPU. По памяти всегда “ровно” в районе 128M memory
приложение имеет дневной цикл по нагрузке – ночью запросов на порядки меньше, пик – днём
хотим максимально отказоустойчивый deployment
хотим минимального потребления ресурсов от этого deployment’а


#  Обяснение решения
По сравнение с предыдыдущей версией решения было добавлдено два варианта. (autoscaler, cronjob)

Оба варианта используют Horizontal Pod Autoscaler (HPA) для автоматического масштабирования приложения в зависимости от загрузки CPU. Это позволяет управлять количеством реплик приложения в реальном времени, чтобы обеспечить отказоустойчивость и эффективное использование ресурсов.

Первый вариант использует CronJob для управления количеством реплик приложения в определенные часы суток, что учитывает дневной цикл нагрузки. Второй вариант использует ScaleDown Policy в HPA, чтобы обеспечить стабильность системы перед масштабированием вниз и избежать резких снижений количества реплик, что может привести к потере доступности приложения.