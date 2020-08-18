# Компонент для парсинга и отображения телеметрических данных

Логика работы задается через конфигурационный файл формата json.
Файл содержит список схем, используемых в работе.

Схема содержит обязательные для определения параметры, такие как:
- Name (название схемы, отображается в заголовке таблицы)
- Id (идентификационный номер, используется для получения таблицы от компонента типа UserControl, через метод GetControlByUid(int)
- Type
- Params

### Используемые типы схем (параметр Type)
- KeyValueTable
- BitsTable
- MasksTable

## Параметры

### Форматы
- DEC
- HEX
- BIN

### Задаем подсветку в файле конфигурации

- простая
```json
"Backlight": {
			"Type": "BasicBacklight",
			"Colors" : [ 
				"#ffff00",
				"#1fddff"
			]
		},
```

- с диапазоном
```json
"Backlight": {
	"Type" : "RangeBacklight",
	"Bigger" : {
		"Value" : 100,
		"Color" : "#c3bbec"
	},
	"Lower": {
		"Value" : 90,
		"Color" : "#aaddee"
	},
	"Equal" : {
		"Value" : 95,
		"Color" : "#ffccaa"
	}
},
```

### Output
Сортируем байты в соответствии с заданным порядком

```json
"Output": [
	7,
    6,
    5,
    4
],
```


### Использование TelemetryUI.dll

```c#
var teleUI = new TeleUI();

teleUI.Init("scheme.config.json");

ContentControl.Content = teleUI.GetControlByUid("Id");

teleUI.Process(byte[]);

teleUI.Clear(); //если нужно очистить данные

teleUI.Refresh(); //если нужно перезагрузить компонент с измененной схемой
```


### Использование TelemetryUI с автообновлением файла конфигурации

```c#
var cfl = new ConfigFileListener(Dispatcher, TeleUI);

//При обновлении файла вызывает TeleUI.Refresh() из потока GUI

cfl.Dispose();
```
