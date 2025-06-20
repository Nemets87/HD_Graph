# HD_Graph

добрый день ! как на linux mint для Ноутбук ASUS ROG Strix G18 G814JVR-N6010 серый и Монитор LG UltraGear 27GR83Q-B черный сделать вывод изображения на два экрана - отключив или сделав гибридрый режим работы для 4060 чтобы акб ноутбука не садился быстро - как бы работал ноутбук как apple macbook m1 pro


Добрый день! Рассмотрю подробно настройку вашего ноутбука ASUS ROG Strix G18 G814JVR-N6010 с RTX 4060 на Linux Mint для работы с двумя мониторами и максимальной экономии энергии.

## Настройка двух мониторов в Linux Mint

### Базовая настройка через графический интерфейс

Linux Mint предоставляет встроенные инструменты для настройки нескольких мониторов[1][2]. Для доступа к настройкам дисплея:

1. Нажмите кнопку меню в левом нижнем углу и введите "Display"[1]
2. Откроется окно настроек дисплея, где вы сможете настроить два монитора[1]
3. По умолчанию мониторы будут работать в режиме "зеркалирования", но вы можете отключить опцию "Same Image in all Monitors" для расширенного рабочего стола[3]

### Совместимость с LG UltraGear 27GR83Q-B

Монитор LG UltraGear 27GR83Q-B хорошо поддерживается в Linux[4][5]. Для оптимальной работы рекомендуется установить драйвер монитора и утилиту OnScreen Control с официального сайта LG[5].

## Настройка гибридной графики NVIDIA Optimus

### Проблемы совместимости ASUS ROG Strix G18

ASUS ROG Strix G18 имеет известные проблемы с Linux, особенно с гибридной графикой[6][7]. Некоторые пользователи сталкиваются с тем, что GPU NVIDIA не определяется системой[6].

### Установка NVIDIA драйверов

Для Linux Mint 22 рекомендуется использовать следующие версии NVIDIA драйверов[8]:
- 560.xx (560.35.03 NFB) - новейший
- 550.xx (550.120) - стабильный
- 535.xx (535.183.01) - LTS версия

Установка через Driver Manager[9]:
1. Откройте "Driver Manager" через меню
2. Выберите подходящую версию NVIDIA драйвера
3. Нажмите "Apply Changes" и перезагрузите систему

### Настройка PRIME для переключения GPU

Linux Mint поддерживает NVIDIA PRIME для переключения между интегрированной и дискретной графикой[10][11]:

```bash
# Проверить текущий статус
prime-select query

# Переключиться на Intel GPU (экономия энергии)
sudo prime-select intel

# Переключиться на NVIDIA GPU (производительность)
sudo prime-select nvidia
```

После каждого переключения требуется перезагрузка системы[10].

### Альтернативные решения

Для более гибкого управления рассмотрите использование:
- **Bumblebee** - классическое решение для Optimus[12]
- **EnvyControl** - современный инструмент управления[13]
- **supergfxctl** - для ASUS ноутбуков[14]

## Оптимизация энергопотребления

### Встроенные средства Linux Mint

Linux Mint 22 включает power-profiles-daemon по умолчанию[15][16]. Для доступа к настройкам энергосбережения:

1. Нажмите Super (Windows) + введите "Power Management"[17][18]
2. Настройте профили для работы от сети и батареи[17]
3. Установите таймауты для экрана и режима сна[17]

### Профили энергосбережения

Power-profiles-daemon предоставляет три режима[16][19]:
- **Performance** - максимальная производительность
- **Balanced** - баланс производительности и энергосбережения  
- **Power Saver** - максимальная экономия энергии

### Установка TLP для расширенного управления

TLP обеспечивает более детальный контроль энергопотребления[16][20]:

```bash
# Установка TLP
sudo apt install tlp

# Остановка power-profiles-daemon (во избежание конфликтов)
sudo systemctl stop power-profiles-daemon.service
sudo systemctl mask power-profiles-daemon.service

# Запуск TLP
sudo tlp start
```

TLP автоматически оптимизирует настройки CPU, GPU, дисков, USB и Wi-Fi для экономии энергии[20].

### Ограничение мощности RTX 4060

Для дополнительной экономии энергии можно ограничить мощность GPU[21][22]:

```bash
# Проверить текущие лимиты мощности
nvidia-smi -q -d POWER

# Установить лимит мощности (например, 100W вместо стандартных 115W)
sudo nvidia-smi -pl 100
```

### Динамическое управление питанием NVIDIA

Включите RTD3 для автоматического отключения GPU при простое[23]:

```bash
# Добавить в /etc/modprobe.d/nvidia.conf
options nvidia "NVreg_DynamicPowerManagement=0x02"
```

Это позволит GPU автоматически переходить в режим минимального энергопотребления при отсутствии нагрузки[23].

## Рекомендуемая конфигурация

### Для работы от батареи (как MacBook M1 Pro)

1. **Переключиться на Intel GPU**:
   ```bash
   sudo prime-select intel
   ```

2. **Установить профиль энергосбережения**:
   ```bash
   powerprofilesctl set power-saver
   ```

3. **Настроить автоматическое отключение экранов**:
   - Встроенный экран: 5-10 минут
   - Внешний монитор: 10-15 минут

### Для работы с высокой производительностью

1. **Переключиться на NVIDIA GPU**:
   ```bash
   sudo prime-select nvidia
   ```

2. **Установить производительный профиль**:
   ```bash
   powerprofilesctl set performance
   ```

## Дополнительные оптимизации

### Настройки ядра для энергосбережения

Добавьте параметры ядра в GRUB[24]:
```bash
# В /etc/default/grub добавить:
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash i915.i915_enable_rc6=1 i915.i915_enable_fbc=1"
```

### Отключение неиспользуемых устройств

- Отключите Bluetooth, если не используется[24]
- Настройте автосуспенд для USB устройств[24]
- Уменьшите яркость экрана[17]

При правильной настройке ваш ASUS ROG Strix G18 на Linux Mint сможет работать от батареи значительно дольше, приближаясь по энергоэффективности к MacBook M1 Pro при обычных задачах.

-------------

Чтобы настроить вывод изображения на два экрана (ноутбук + внешний монитор) с энергосбережением как на MacBook M1 Pro в Linux Mint, выполните следующие шаги:

### 1. **Настройка гибридного режима NVIDIA (Optimus)**
Цель: Использовать интегрированную графику для экономии энергии, активируя NVIDIA RTX 4060 только для требовательных задач.

#### **Шаги:**
1. **Установите драйверы NVIDIA:**
   ```bash
   sudo apt update
   sudo apt install nvidia-driver-535 nvidia-prime-applet  # Рекомендуемая версия
   ```
   Перезагрузите систему.

2. **Переключитесь в гибридный режим:**
   - Откройте **NVIDIA X Server Settings** → **PRIME Profiles**.
   - Выберите **On-Demand**.
   - Или через терминал:
     ```bash
     sudo prime-select on-demand
     ```
   - Перезагрузите ноутбук.

3. **Проверьте режим:**
   ```bash
   prime-select query  # Должно показать "on-demand"
   glxinfo | grep "OpenGL renderer"  # В обычном режиме должен показывать Intel/AMD iGPU
   ```

---

### 2. **Настройка двух мониторов**
#### **Вариант A: Только внешний монитор (для экономии энергии)**
1. Подключите монитор LG через HDMI/DisplayPort.
2. Откройте **Системные настройки** → **Дисплеи**.
3. Выберите внешний монитор, установите:
   - **Режим:** Основной дисплей.
   - Разрешение и частоту обновления (например, 2560x1440@144Hz).
4. Для встроенного экрана ASUS выберите **Отключить**.

#### **Вариант B: Расширенный рабочий стол**
1. В настройках дисплеев перетащите мониторы согласно их физическому расположению.
2. Настройте разрешение для каждого.
3. Нажмите **Применить**.

---

### 3. **Оптимизация энергопотребления**
#### **Отключите NVIDIA при простое:**
```bash
sudo apt install auto-cpufreq
sudo systemctl enable auto-cpufreq
sudo systemctl start auto-cpufreq
```

#### **Дополнительные настройки:**
1. **TLP для управления питанием:**
   ```bash
   sudo apt install tlp tlp-rdw
   sudo systemctl enable tlp
   ```

2. **Отключите подсветку клавиатуры ROG:**
   ```bash
   echo 0 | sudo tee /sys/class/leds/asus::kbd_backlight/brightness
   ```

---

### 4. **Автоматизация (опционально)**
**Скрипт для переключения режимов:**
```bash
#!/bin/bash
# Переключение на внешний монитор + отключение встроенного
xrandr --output HDMI-1 --primary --mode 2560x1440 --rate 144 \
       --output eDP-1 --off
```
Сохраните как `external-only.sh`, сделайте исполняемым:
```bash
chmod +x external-only.sh
```
Запускайте при подключении монитора.

---

### 5. **Закрытие крышки без сна (как MacBook)**
1. Откройте **Системные настройки** → **Электропитание**.
2. Установите:
   - **При закрытии крышки:** Действие не требуется.
   - **При простое:** Выключать экран через 1-2 минуты.

---

### Итог:
- **Графика:** Режим `On-Demand` (NVIDIA включается только для игр/рендеринга).
- **Мониторы:** Внешний LG как основной, встроенный отключен для экономии энергии.
- **Питание:** `auto-cpufreq` + `TLP` снижают расход батареи.
- **Крышка:** Закрытие не влияет на работу с внешним монитором.

> **Проверка энергопотребления:**
> ```bash
> sudo powertop  # Следите за разрядом батареи (раздел "Discharge rate")
> ```

Если дискретная карта активна в простое, проверьте запущенные приложения:
```bash
nvidia-smi  # Убивайте процессы, использующие NVIDIA
```
