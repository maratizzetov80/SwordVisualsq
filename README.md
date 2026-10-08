# SwordVisuals (Fabric, клиент, MC 1.21.11)

Косметические эффекты и утилиты. Только клиент, без читерских функций.

## Сборка
1. Положи в корень `gradlew`, `gradlew.bat` и папку `gradle/wrapper` из шаблона
   https://github.com/FabricMC/fabric-example-mod (нужен Gradle 9.x для Loom 1.14).
2. `./gradlew build` — jar будет в `build/libs/swordvisuals-1.0.0.jar`.
3. Запуск в dev-окружении: `./gradlew runClient`.
4. Проверь версии в `gradle.properties` на https://fabricmc.net/develop

## Управление
- Right Shift — меню. ЛКМ по модулю — вкл/выкл, ПКМ — настройки, СКМ — назначить клавишу (Esc — снять).
- Вкладка Settings → «Редактор HUD»: ЛКМ двигает (сетка 5px), ПКМ скрывает/показывает.
- Zoom — удерживать C, скриншот — F9 (меняется в меню).
- Цвет: клик по строке открывает HSV-палитру (квадрат S/V + полоса оттенка).
- Settings: профили (`config/swordvisuals-profiles/`), экспорт/импорт через буфер обмена.
- Конфиг: `config/swordvisuals.json`.

## Разделы меню
- **Visuals**: SwordTrail, SwordGlow, SwordAura, TargetESP, SwingAnimation, HitParticles, HitMarker, KillEffect, TotemPop, ParticleTrail, WorldParticles, CustomCrosshair, DamageTint
- **Utilities**: Zoom, Optimization, Freelook (зажать Left Alt), Fullbright, ScreenshotHelper, ChatTimestamps
- **HUD**: Watermark, Coordinates, FPS/Ping, ArmorHUD, PotionHUD, Keystrokes, CPS, ItemCounter, TargetHUD
- **World**: TimeChanger, FogChanger, CustomSky, WeatherChanger
- **Options**: редактор HUD, сохранение/сброс, экспорт/импорт, профили

Optimization временно меняет настройки графики и возвращает их при выключении/выходе.
Модули Fog/Sky/Time, SwingAnimation и Freelook используют миксины из `swordvisuals.env.mixins.json` (require=0):
если Mojang поменяет метод, отключится только этот модуль.

## Что проверять при первой компиляции
Код не компилировался в этой среде (нет доступа к maven.fabricmc.net). Места, где в 1.21.9–1.21.11
менялись сигнатуры, и ошибки наиболее вероятны:
- `Screen#mouseClicked/mouseDragged/mouseReleased/keyPressed` (`Click`, `KeyInput`) — ClickGuiScreen, HudEditScreen
- `Window#getHandle()` — InputHelper
- `ScreenshotRecorder.saveScreenshot(...)` — ScreenshotHelper
- имена методов в миксинах: `renderCrosshair`, `getFov`, `onEntityStatus`, `EntityStatusS2CPacket#getEntity`
- `HudRenderCallback` устарел → при желании перейти на `HudElementRegistry`
- Чат игроков (не системный) для ChatTimestamps лучше делать миксином на `ChatHud#addMessage`.
Все ошибки легко найти по `./gradlew build` и сверить через `./gradlew genSources`.
