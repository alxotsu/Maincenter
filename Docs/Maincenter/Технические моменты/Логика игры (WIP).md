# **Общая задача**
Создать систему классов, способную обеспечить игровую локику


# **Игровой режиссер:**

## Общее назначение:
Задача игрового режиссера - комбинировать между собой методы из других классов, получая на выходе то, что необходимо отобразить игроку

## Логика процесса:

При нажатии на кнопку "Начать игру" в игровом меню происходит обращение к классу Игрового режиссера.

**Старт игрового процесса:**
1. Инициализация Игрового режиссера
2. Игровой режиссер проверяет наличие сохранения в выбранной ячейке
	1. Есть сохранение:
		1. Загрузить данные из файла
	2. Нет сохранения:
		1. Создать файл сохранения
		2. Записать в файл параметры по умолчанию согласно выбранной сложности и режиму
3. Игровой режиссер загружает интерфейс перехода между главным меню и игровым интерфейсом с 100% прозрачностью
4. Игровой режиссер выгружает интерфейс главного меню и ставит прозрачность интерфейса перехода на 0%
5. Игровой режиссер загружает игровой интерфейс с 100% прозрачностью
6. Игровой режиссер выгружает интерфейс загрузки и ставит прозрачность игрового интерфейса на 0%
7. Игровой режиссер проверяет наличие ссылки на файл карты в файле сохранения
	1. Отобразить на мониторе радара эффект загрузки карты
	2. Есть файл:
		1. Загрузить карту по ссылке
		2. Скрыть эффект загрузки
		3. Отобразить карту
	3. Нет файла:
		1. Создать карту
		2. Создать события на карте
		3. Сохранить карту
		4. Вернуться к пункту "Есть файл"
8. Игровой режиссер инициализирует логику событий на карте
9. Игровой режиссер проверяет загруженные файлы
10. Игровой режиссер ожидает действий игрока, реагируя в зависимости от его действий

**Игрок посещает точку события:**


# **Менеджер файлов:**

## Общее назначение:
Задача менеджера файлов - управлять игровым содержимым, создавать, модифицировать и удалять файлы в зависимости от вызовов класса Игровой режиссер.

### Список методов ++ функционал
1. **Основные методы - вывод данных:**
	1. GameInstance_createMap - создает карту игрового мира
		1. Параметры:
			1. GameInstance_getGameStage
			2. GameInstance_getGameDifficulty
			3. PlayerState_getPlayerStats (частично)
				1. PlayerState_getReputation
				2. PlayerState_getRank
				3. PlayerState_getNetWorth
		2. Функционал:
			1. Получает на вход значения, подставляет их в формулу генерации с использованием процедурно генерируемого ключа
			2. Создает карту согласно полученным данным и ключу
			3. Присваивает карте значение параметра GameInstance_GameStage по формуле: GameInstance_GameStage(загруженной карты)+1, иначе 0
			4. Сохраняет карту в виде файла
	2. GameInstance_LoadMap
		1. Тип: string? (имя файла)
		2. Функционал: загружает созданную карту на игровую сцену
	3. GameInstance_unloadMap
		1. Тип: string? (имя файла)
		2. Функционал: выгружает созданную карту с игровой сцены
	4. GameInstance_updateMap
		1. Тип: string? (имя файла)
		2. Функционал: обновляет загруженную карту 
	5. GameInstance_deleteMap (необходимо?)
		1. Тип: string? (имя файла)
		2. Функционал: удаляет карту
	6. GameInstance_createEvent
		1. Параметры:
			1. GameInstance_getGameStage
			2. GameInstance_getGameDifficulty
			3. GameInstance_getEventList
			4. PlayerState_getPlayerStats (частично)
				1. PlayerState_getReputation
				2. PlayerState_getRank
				3. PlayerState_getNetWorth
		2. Функционал: 
			1. Создает событие согласно формуле генерации события
			2. Делает запись в файл текущей карты о созданном событии
			3. Создает ссылку внутри файла на шаблон события
			4. Заполняет параметры события
			5. Сохраняет файл карты
	7. GameInstance_loadEvent
		1. Тип: string? (имя файла)
		2. Функционал: загружает событие в GameInstance
	8. GameInstance_unloadEvent
		1. Тип: string? (имя файла)
		2. Функционал: 
			1. Сохраняет событие в файл карты (перезаписывает если уже есть событие с таким ID)
			2. Выгружает событие из GameInstance
	9. GameInstance_updateEvent
		1. Тип: string? (имя файла)
		2. Функционал: обновляет параметры события в GameInstance
	10. GameInstance_deleteEvent
		1. Тип: string? (имя файла)
		2. Функционал:
			1. Удаляет событие из файла карты с одноименным ID (если есть)
			2. Удаляет событие из GameInstance
2. **Вспомогательные методы - загрузка данных в и их обработка**
	1. GameInstance_getGameStage
		1. Тип: int
		2. Функционал: получает значение текущей игровой стадии
	2. GameInstance_changeGameStage
		1. Тип: int
		2. Функционал: изменяет значение игровой стадии при переходе на следующую карту (равное значению игровой стадии в карте)
	3. GameInstance_saveGameStage
		1. Параметры: int
		2. Функционал: сохраняет значение игровой стадии в карте
	4. GameInstance_getGameDifficulty
		1. Тип: int
		2. Функционал: загружает значение игровой сложности в GameInstance
	5. GameInstance_getEventList
		1. Тип: list
		2. Функционал: загружает список игровых событий из файла карты в GameInstance
	6. PlayerState_getPlayerStats - получает параметры игрока из файла PlayerState.txt
		1. PlayerState_getReputation
			1. Тип: int
			2. Функционал: получает значение репутации игрока из файла
		2. PlayerState_getRank
			1. Тип: int
			2. Функционал: получает значение ранга игрока из файла
		3. PlayerState_getCurrencyMetal
			1. Тип: int
			2. Функционал: получает значение кол-ва металла из файла
		4. PlayerState_getInventory
			1. Тип: list
			2. Тип данных элемента массива: dictionary
			3. Функционал: получает списка инвентаря игрока из файла
		5. PlayerState_getNetWorth
			1. Тип данных: int
			2. Функционал: получает значение общей ценности игрока из файла
	7. PlayerState_changeReputation
		6. Тип: int
		7. Функционал: модифицирует параметр репутации
	8. PlayerState_cnangeRank
		1. Тип: int
		2. Функционал: модифицирует параметр ранга
	9. PlayerState_cnangeCurrencyMetal
		1. Тип: int
		2. Функционал: модифицирует параметр "Металл"
	10. PlayerState_cnangeResources
		1. Тип: list
		2. Тип данных элемента массива: int
		3. Функционал:  модифицирует параметр ресурсов (1 из или несколько)
	11. PlayerState_addItem
		1. Тип: string
		2. Функционал:  добавляет предмет
	12. PlayerState_modifyItem
		1. Тип: string
		2. Функционал: изменяет параметры предмета
	13. PlayerState_deleteItem
		1. 1. Тип: string
		2. Функционал:  удаляет предмет