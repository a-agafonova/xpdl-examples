### Описание
Правило ищет упоминания государственных проектов и программ, осуществляемых на национальном, федеральном и региональном уровнях.

### Примеры найденных сущностей
* был принят новый **национальный проект «Жилье и городская среда»**
* внесены изменения в **государственную программу «Современное образование Ленинградской области»**

### Инструкция
* Создать новый проект в платформе [PolyAnalyst](https://www.megaputer.ru/produkti/)
* Загрузить в проект файл [**Данные_Гос_проект.txt**](Данные_Гос_проект.txt)
* Добавить узел **Извлечение сущностей**, в настройках узла:
	 * отключить стандартные сущности
	 * создать новую пользовательскую сущность
	 * открыть **Редактор правил** новой сущности и скопировать в него правило [**Гос_проект.txt**](...):
	```
	rule: гос_проекты 
	{
	 
		query: {phrase({lemma(adjective)}:level,			 		//Ищем уровень проекта или программы: НАЦИОНАЛЬНЫЙ, РЕГИОНАЛЬНЫЙ и т.д.
						{проект or программа}:type, 			 	//Ищем ключевые слова:	ПРОЕКТ или ПРОГРАММА				
						"\"", {repeat(.)}:name, "\"")}:project		//Ищем название проекта или программы: ЧИСТАЯ ВОДА, ЭКОЛОГИЯ и т.д.
		
		result: Результат = norm($project)
			attribute: Название = $name
			attribute: Уровень = title(norm($level))
			attribute: Тип = title(norm($type))
	}
	```
	 * выполнить узел

### Структура сущности
* **Результат** - общий вывод найденной сущности
* **Название** - полное название проекта или программы
* **Уровень** - уровень, на котором осуществляются проект или программа (национальный, федеральный, региональный и т.д.)
* **Тип** - тип найденной сущности (проект или программа)

### Отчет узла
Отчет узла **Извлечение сущностей** выглядит следующим образом:

| Результат | Название | Уровень | Тип |
| ------ | ------ |------ |------ |
| национальный проект «Демография»| «Демография» |Национальный |Проект |
| региональный проект «Финансовая поддержка семей при рождении детей» | «Финансовая поддержка семей при рождении детей» |Региональный |Проект |

В тексте подсвечиваются следующие фрагменты:
> В рамках **национального проекта "Демография"**, **регионального проекта "Финансовая поддержка семей при рождении детей"** 3000 семей получили денежные выплаты в связи с рождением первого ребенка. 