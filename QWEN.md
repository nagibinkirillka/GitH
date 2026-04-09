# Home Assistant - Knowledge Base

## Подключение к Home Assistant

### REST API
- **URL**: `http://192.168.10.150:8123`
- **Token**: `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiIwNTM1NTBiYjdiOGE0ZjczODNlOTFlMDZlOTdhOGU3YyIsImlhdCI6MTc3NTU4NTc5NywiZXhwIjoyMDkwOTQ1Nzk3fQ.fPyNByqM5LEhGy7YZHfdqprUp8vaCkgJc8uB1sCfGPw`
- **Internal URL**: `http://192.168.10.150:8123`
- **External URL**: `https://dom.homeass72.pp.ru`

### MCP Server
- **URL**: `http://192.168.10.150:9583/private_VObDQKwk3OTWiqqxTn0tAg`
- **Config**: `z:\.qwen\settings.json` (mcpServers.home-assistant)

### Информация о системе
- **Location**: Smart Home
- **Timezone**: Asia/Yekaterinburg
- **Country**: RU
- **Currency**: RUB
- **Version**: 2026.4.1

---

## Команды для управления устройствами

### Получить статус устройства
```powershell
powershell -Command "Invoke-RestMethod -Uri 'http://192.168.10.150:8123/api/states/<ENTITY_ID>' -Headers @{'Authorization'='Bearer <TOKEN>'; 'Content-Type'='application/json'}"
```

### Получить все устройства
```powershell
powershell -Command "Invoke-RestMethod -Uri 'http://192.168.10.150:8123/api/states' -Headers @{'Authorization'='Bearer <TOKEN>'; 'Content-Type'='application/json'}"
```

### Включить/выключить switch
```powershell
# Включить
powershell -Command "Invoke-RestMethod -Uri 'http://192.168.10.150:8123/api/services/switch/turn_on' -Method POST -Headers @{'Authorization'='Bearer <TOKEN>'; 'Content-Type'='application/json'} -Body '{\"entity_id\": \"switch.<ID>\"}'"

# Выключить
powershell -Command "Invoke-RestMethod -Uri 'http://192.168.10.150:8123/api/services/switch/turn_off' -Method POST -Headers @{'Authorization'='Bearer <TOKEN>'; 'Content-Type'='application/json'} -Body '{\"entity_id\": \"switch.<ID>\"}'"
```

### Перезагрузить конфигурацию
```powershell
# Reload core config
powershell -Command "Invoke-RestMethod -Uri 'http://192.168.10.150:8123/api/services/homeassistant/reload_core_config' -Method POST -Headers @{'Authorization'='Bearer <TOKEN>'; 'Content-Type'='application/json'}"
```

### Проверить API
```powershell
powershell -Command "Invoke-RestMethod -Uri 'http://192.168.10.150:8123/api/' -Headers @{'Authorization'='Bearer <TOKEN>'; 'Content-Type'='application/json'}"
```

---

## Структура конфигурации

### Путь к конфигурации
- **В Home Assistant**: `/config/includes`
- **Локально (сетевая папка)**: `Z:\`

### Структура папок
```
Z:\ (includes)
├── Room_Balkon/          # Балкон
│   ├── balkon_setting.yaml
│   └── balkon_ya_int.yaml
├── Room_Bathroom/        # Ванная
│   ├── bathroom_setting.yaml
│   └── bathroom_ya_int.yaml
├── Room_Bedroom/         # Спальня
│   ├── bedroom_setting.yaml
│   └── bedroom_ya_int.yaml
├── Room_Children/        # Детская
│   ├── children_setting.yaml
│   └── children_ya_int.yaml
├── Room_Entrance/        # Прихожая
├── Room_Kitchen/         # Кухня
├── Room_Koridor/         # Коридор
├── Room_Toilet/          # Туалет
├── System/               # Системные настройки
└── old/                  # Старые конфиги
```

### Формат файлов
- `*_setting.yaml` - основные настройки комнаты (customize, template)
- `*_ya_int.yaml` - интеграции с Яндексом

### Пример структуры customize (children_setting.yaml)
```yaml
children_setting:
  homeassistant:
    customize:
      switch.0xa4c138b8531d196c:
        friendly_name: "Координатор детская"
        icon: mdi:zigbee
      switch.0x60a423fffeb046fa_left:
        friendly_name: "Выключатель детская - большой свет"
        icon: mdi:lightbulb
```

---

## Устройства в детской (Room_Children)

### Switches
| Entity ID | Friendly Name | Icon |
|-----------|---------------|------|
| switch.0xa4c138b8531d196c | Координатор детская | mdi:zigbee |
| switch.0x60a423fffeb046fa_left | Выключатель детская - большой свет | mdi:lightbulb |
| switch.0x60a423fffeb046fa_right | Выключатель детская - малый свет | mdi:lightbulb |
| switch.0xa4c13865ff117cba | Розетка в детской | mdi:power-socket-eu |
| switch.0xa4c1389ac97dd356 | Кондиционер в детской | mdi:power-socket-eu |
| switch.0xa4c1380b215141b6 | Розетка кондиционер детская | mdi:power-socket-eu |
| switch.0xa4c138c8b951870d | Розетка детская шлюз | mdi:power-socket-eu |

### Sensors
- sensor.0xa4c13865ff117cba_energy - Розетка в детская энергия
- sensor.0xa4c13865ff117cba_power - Розетка в детская мощность
- sensor.detskaya_temperature - Температура детская
- sensor.detskaya_humidity - Влажность детская

### Climate
- climate.detskaya_ac - Кондиционер - детская

### Media
- media_player.lg_webos_tv_uk6300plb - Телевизор LG в детской
- media_player.yandex_station_m007e85001yzgb - Яндекс станция в детской

---

### Датчик туалета (0x00158d000ad604da)
- sensor.0x00158d000ad604da_temperature - Средняя температура в туалете
- sensor.0x00158d000ad604da_humidity - Средняя влажность туалет
- sensor.0x00158d000ad604da_battery - Датчик влажности туалет

---

## Все розетки с энергомониторингом

### Детская (Room_Children)
| Switch | Friendly Name | Sensors |
|--------|---------------|---------|
| switch.0xa4c138c8b951870d | Холодильник | sensor.0xa4c138c8b951870d_energy, sensor.0xa4c138c8b951870d_power |
| switch.0xa4c1380b215141b6 | Холодильная камера | sensor.0xa4c1380b215141b6_energy, sensor.0xa4c1380b215141b6_power |
| switch.0xa4c13865ff117cba | Детская зона телевизора | sensor.0xa4c13865ff117cba_energy, sensor.0xa4c13865ff117cba_power |
| switch.0xa4c1389ac97dd356 | Кондиционер в детской | sensor.0xa4c1389ac97dd356_energy |
| switch.0xa4c138b8531d196c | Координатор детская | - |

### Спальня (Room_Bedroom)
| Switch | Friendly Name | Sensors |
|--------|---------------|---------|
| switch.0xa4c13800371f889b | Спальная комп | sensor.0xa4c13800371f889b_energy, sensor.0xa4c13800371f889b_power |
| switch.0xa4c13883ea7103db | Кондиционер в спальной | sensor.0xa4c13883ea7103db_energy, sensor.0xa4c13883ea7103db_power |

---

## Полезные ссылки
- Документация HA: https://www.home-assistant.io/
- MCP интеграция: https://www.home-assistant.io/integrations/mcp/

---

**Последнее обновление**: 2026-04-07

## Qwen Added Memories
- Home Assistant: URL=http://192.168.10.150:8123, MCP URL=http://192.168.10.150:9583/private_VObDQKwk3OTWiqqxTn0tAg, Token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiIwNTM1NTBiYjdiOGE0ZjczODNlOTFlMDZlOTdhOGU3YyIsImlhdCI6MTc3NTU4NTc5NywiZXhwIjoyMDkwOTQ1Nzk3fQ.fPyNByqM5LEhGy7YZHfdqprUp8vaCkgJc8uB1sCfGPw
- Конфигурация Home Assistant находится в Z:\ (это /config/includes в контейнере). Структура: Room_* папки для комнат, System для системных настроек. Файлы *_setting.yaml содержат customize и templates.
