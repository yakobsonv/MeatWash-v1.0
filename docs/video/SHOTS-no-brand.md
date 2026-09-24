# MEATWASH — съёмочный комплект для Google Flow (без названия марки)

Промпты на 18 клипов: 6 общих планов машины и 12 услуг. Всё снимается на ОДНОЙ машине.

> Машина серии: **a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing года, тёмно-бордовый «бычья кровь», «китовый хвост», чёрные диски Fuchs, чёрные накладки бамперов, БЕЗ номерных знаков**
> Это та же машина, что сейчас стоит на сайте 3D-моделью (1975 a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, Sketchfab, CC-BY).
> Полное описание подставлено в каждый промпт слово в слово — иначе получится 18 разных машин.

---

## Что нужно знать про Flow до первой генерации

- Один канонический кадр машины — единственный надёжный якорь континуити. Veo 3.1 Quality НЕ поддерживает ingredients, только text-to-video и frames-to-video. Значит схема такая: сделать машину один раз изображением (Nano Banana Pro во Flow), утвердить её с Владом, и дальше подставлять этот кадр первым кадром во все 13 клипов. На Fast/Lite поверх этого добавлять те же 2–3 картинки в ingredients.
- Максимум 8 секунд на клип у Veo (10 у Omni). Тринадцать роликов — это тринадцать отдельных генераций, а не одна длинная сцена. Extend тут не помогает: он продолжает ту же сцену, а не переносит машину в новую услугу.
- Видео только 16:9 и 9:16. Квадрата нет. Под панель «Гараж услуг» мастера делать в 16:9 и кадрировать в CSS, либо отдельно перегенерировать вертикаль под мобильный — это удвоение бюджета кредитов.
- Базовое разрешение 720p. 1080p — апскейлом, бесплатно только подписчикам; 4K — 50 кредитов и только Ultra. Для сайта 1080p достаточно, но подписка Pro минимум нужна.
- Звук отключить в модели нельзя. Все 13 файлов придут с дорожкой — срезать на постобработке (ffmpeg -an), заодно это уменьшит вес для веба. На сайте видео всё равно muted+autoplay+playsinline+loop.
- ЖЁСТКОЕ ПРАВИЛО ПО НОМЕРАМ ложится на генерацию отдельной нагрузкой: модель дорисовывает номерные знаки сама. Запрет надо зашивать трижды — в канонический кадр машины, в текст каждого промпта, и в покадровую приёмку каждого клипа. Безопаснее кадрировать так, чтобы бампера в кадре не было: три четверти сзади, детальные планы, срез по колесу.
- Не называть в промпте марку. Политика Veo ограничивает реальные бренды и логотипы, а шильдики модель всё равно коверкает. Описывать силуэт обобщённо («классическое заднемоторное купе 80-х»), а не «a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing».
- Бюджет кредитов определяет выбор модели. Бесплатный тариф (50 кредитов в день) не потянет Quality вообще — одна генерация стоит 100. Рабочий минимум — AI Pro (1000/мес + 50/день) на Veo 3.1 Fast. Quality на все 13 услуг с дублями — это Ultra.
- Кредиты не переносятся ни по дням, ни по месяцам. Съёмку 13 роликов надо планировать как дозированную работу по дням, а не одним заходом.
- SynthID снимать нельзя и не нужно — он невидимый. Видимый знак по документации включается только для трёх стран, но перед планированием надо проверить на одном реальном скачивании, что на аккаунте Влада его нет.

### Факты, на которых построен комплект

| Факт | Уверенность |
|---|---|
| В Flow сегодня четыре видеомодели: Veo 3.1 Lite, Veo 3.1 Fast, Veo 3.1 Quality и Gemini Omni Flash 1.1. Более новой Veo (3.2 и т.п.) в интерфейсе нет. | высокая |
| Длительность клипа: у всех трёх Veo 3.1 — 4, 6 или 8 секунд. У Gemini Omni Flash 1.1 — 4, 6, 8 или 10 секунд. Восьмисекундный клип у Veo доступен только при 1080p/4K или при использовании референсных изображений. | высокая |
| Режимы по моделям: Lite — text-to-video, frames-to-video (первый / первый+последний), ingredients, extend. Fast — text-to-video, frames, ingredients, БЕЗ extend. Quality — только text-to-video и frames, БЕЗ ingredients и БЕЗ extend. Omni Flash 1.1 — text-to-video, frames, ingredients и video-to-video редактирование до 10 секунд; extend у неё «coming soon». | высокая |
| Соотношения сторон видео — только 16:9 и 9:16. Вертикаль есть, квадрата нет. Пропорции 1:1, 3:4 и 4:3 Flow добавил только для генерации и редактирования ИЗОБРАЖЕНИЙ (анонс марта 2026), на видео они не распространяются. | высокая |
| Разрешение: база — 720p. 1080p и 4K получаются апскейлом. Апскейл до 1080p бесплатен для подписчиков Plus/Pro/Ultra; апскейл до 4K стоит 50 кредитов и доступен только Ultra. У Omni есть дешёвый режим 360p (половина стоимости 720p), подписчикам Pro/Ultra апскейл 360p→720p за 0 кредитов. | высокая |
| Частота кадров 24 fps, контейнер MP4. | высокая |
| Звук генерируется всегда и выключить его нельзя: у Veo 3.1 аудио «Always on» по документации API. У Gemini Omni аудио тоже по умолчанию, но его можно направлять промптом («no dialogue», «silence», описание фоновых звуков). Практический вывод: дорожку срезать на постобработке. | высокая |
| Референсных изображений (ingredients) у Veo — до трёх на генерацию. | высокая |
| У Gemini Omni лимит ingredients выше, чем у Veo (сообщается про 7 референсов и до 10 изображений), но это не официальная страница Google, а PR к стороннему API-врапперу flow2api. | низкая |
| Механизмы continuity в Flow: (1) frames-to-video — свой первый кадр, либо первый+последний; (2) ingredients/references — до 3 картинок персонажа/объекта/стиля, держат идентичность при смене сцены; (3) последний кадр клипа как первый кадр следующего; (4) Scene Builder со сборкой цепочки клипов и «jump to». Extend продолжает ТУ ЖЕ сцену, для 13 разных услуг он не помогает. | высокая |
| Ingredients-to-video официально заявлен именно как удержание идентичности: «Keep your characters looking the same even as the setting changes», плюс переиспользование объекта, фона и текстур между сценами. | высокая |
| Extend работает только с видео, сгенерированными Veo. К продлённым клипам нельзя применять другие режимы правки — insert, remove, camera. | высокая |
| Каждый extend добавляет 7 секунд, повторять можно до 20 раз, предельная длина сцены ~148 секунд (8 + 7×20). Продление отдаёт только 720p. | средняя |
| Кредиты: 50 в день получают все, включая бесплатных. Сверху месячный пакет: AI Plus +200, AI Pro +1000, AI Ultra $100 +10000, AI Ultra $200 +25000. Неизрасходованные кредиты не переносятся ни с дня на день, ни с месяца на месяц. | высокая |
| Стоимость генерации: Veo 3.1 Lite — 10 кредитов (5 для Ultra); Veo 3.1 Fast — 20 (10 для Ultra); Veo 3.1 Quality — 100 кредитов за 8-секундную генерацию для всех тарифов без скидки Ultra; Gemini Omni Flash 720p — 7–15 кредитов в зависимости от длины (4–10 с), 360p — 4–7; видеоредактирование — 40 кредитов за генерацию. | высокая |
| Практический расчёт бюджета на 13 роликов: при реалистичных 3–5 дублях на услугу это 40–65 генераций. На Veo 3.1 Fast — 800–1300 кредитов (тариф Pro тянет за несколько дней). На Veo 3.1 Quality — 4000–6500 кредитов, то есть нужен Ultra. На бесплатном тарифе Quality недоступна в принципе: одна генерация стоит 100 кредитов при дневном лимите 50. | высокая |

### Где Flow чаще всего проваливается

- Вода, пена, брызги и отражения на лаке — прямо названные слабые места модели, а это ровно предмет съёмки автомойки. «Полировка кузова», «Керамическое покрытие» и «Антидождь на стёкла» будут самыми дорогими по числу дублей: капля на керамике и ровное отражение на полировке — это две вещи, которые Veo делает хуже всего.
- Модель галлюцинирует номерные знаки. Правило заказчика «ни одного номера» превращается в покадровую приёмку 13 клипов, а не в одну проверку. Особенно опасны фронтальные и задние планы.
- Текст в кадре коверкается: любые вывески, надписи на бутылках химии, шильдики выйдут мусором. В промптах явно запрещать читаемый текст, брендинг накладывать своей графикой в CSS поверх видео.
- Руки и точная мелкая моторика ломаются на крупных планах — а «ручная проработка», «точечная подкраска сколов», «оклейка плёнкой» это именно руки в кадре. Планировать такие услуги как планы без рук или с руками в расфокусе.
- Континуити на 13 клипах может всё равно поплыть: даже с одним первым кадром модель меняет оттенок кузова, форму фар и пропорции. Риск, что машина в «Химчистке салона» и в «Полировке» выглядит как две разные. Страховка — держать 2–3 услуги в одном длинном прогоне через extend там, где это возможно, и цветокоррекция на постобработке.
- Расхождение между документацией и практикой по видимому водяному знаку. Если знак всё-таки печатается, 13 роликов придётся переделывать или закрывать композицией — проверить ДО начала работы.
- Уход от Three.js ломает текущую механику «Гаража услуг»: сейчас эффекты СКЛАДЫВАЮТСЯ на одной машине (помыл + отполировал + покрыл керамикой). С 13 независимыми видео накопление эффектов невоспроизводимо — либо услуги становятся взаимоисключающими, либо нужна другая логика показа. Это продуктовое решение, а не техническое, и его надо задать Владу до генерации.
- Режим «Ролик» (?film=1) сейчас прогоняет услуги подряд одной непрерывной сценой. На 13 отдельных клипах он станет монтажом со склейками — нужен либо переход-кроссфейд, либо перегенерация цепочки через последний кадр → первый кадр, что дороже.
- Клип 8 секунд не зацикливается бесшовно — Veo не умеет loop. Для фоновых петель на сайте нужен либо кроссфейд в CSS, либо подбор клипов, которые начинаются и кончаются на похожем кадре.
- Вес видео: 13 роликов в 1080p вместо одной GLB-модели на 9 МБ. Нужен бюджет по весу страницы, poster-кадры, preload=none и ленивая подгрузка — иначе мобильный сайт станет тяжелее, чем был с 3D.

---

> ### ⚠ Если Flow откажется генерировать по названию марки
>
> В промптах машина названа прямо — «a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing». Так модель точнее
> попадает в силуэт. Но Veo ограничивает реальные автомобильные бренды, и промпт может
> быть отклонён или шильдики выйдут мусором.
>
> **Если это случится — замените в промпте название на описание силуэта:**
>
> ```text
> a classic 1970s rear-engined air-cooled sports coupe, wide flared rear arches,
> large flat whale-tail rear wing, deep oxblood burgundy paintwork,
> black five-spoke forged wheels, matte black rubber bumper trim,
> NO license plate on either bumper
> ```
>
> Рядом лежит **[SHOTS-no-brand.md](SHOTS-no-brand.md)** — тот же комплект, где замена
> уже сделана во всех 18 кадрах. Начинайте с основного файла; если Flow ругается —
> переходите на него и больше не возвращайтесь, иначе машина поедет между клипами.

---

## Как пользоваться

1. Сначала сделать **канонический кадр** машины — одну картинку, с которой стартуют все клипы.
   Ниже у каждого кадра в поле «Первый кадр» написано, что на нём должно быть.
2. Английский промпт вставляется в Flow **как есть**. Русский — чтобы вы понимали, что заказываете.
3. Negative-лист вставляется в поле негатива, если модель его принимает; если нет — он всё равно
   полезен как список того, что проверять в готовом клипе.
4. Звук Veo генерирует всегда и отключить его нельзя — дорожка срезается после: `ffmpeg -i in.mp4 -an -c:v copy out.mp4`.
5. Если кадр не выходит за 3-4 попытки — берите **запасной вариант**, он есть у каждого кадра.

---

# Первый экран и общие планы машины

## `hero-arrive` — Заглавный общий план: машина приехала

**6 с** · режим: frames-to-video (один и тот же канонический стоп-кадр подаётся И первым, И последним кадром) · **петля**

**Промпт для Flow:**

```text
[Cinematography] Locked-off tripod shot, 35 mm lens at f/2.8, camera 55 cm above the floor, static three-quarter front-left framing showing the whole car, absolutely no camera movement, no dolly, no zoom, no handheld drift. [Subject] a a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, deep oxblood burgundy paintwork, whale-tail rear spoiler, black Fuchs-style wheels, black rubber bumper trim, NO license plate on either bumper — the bumper faces are smooth unbroken black rubber with no plate, no plate recess, no plate holder and no mounting bolts. The car has just been driven in: the paint is dulled by a film of winter road grime, grey salt haze across the lower body, dirt streaks fanning back from the wheel arches, sills dark and wet. [Action] exactly one event and nothing else: a slow bank of cold vapour drifts into frame from the left, passes in front of the lower half of the car, and thins away to nothing. The car does not move, no water runs, the lights do not change, nothing enters or leaves the frame. [Context] an underground detailing bay: black ceiling with exposed red-painted steel beams, four long white LED strip lights running front to back, grey brick-paver floor with plain red and white painted lane stripes and no characters or pictograms on the floor, dark grey panelled walls, black hose reels on the wall, wet floor mirroring the ceiling lights, no windows, no daylight, no other vehicles. [Style & Ambiance] exposure set for the LED strips, background two stops darker than the car, one unbroken LED strip reflection running along the shoulder line of the body, cool grey-green grade with the red ceiling beams as the only warm accent, fine film grain, restrained and expensive, not advertising-glossy, no music, no dialogue, silence. [Timing] [00:00-00:01] frame completely still and clear, the vapour only just touching the left edge. [00:01-00:04] the vapour crosses in front of the sills and thins out. [00:04-00:06] the vapour is gone and the frame is identical to the opening frame in every detail.
```

**Negative:**

```text
license plate, number plate, registration plate, plate recess, plate holder, plate frame, any digits or numerals, readable text, painted characters or pictograms on the floor, signage, road signs, model badges, brand logos, watermark, subtitles, on-screen text, people, full human figures, hands, other vehicles, windows, daylight, camera shake, handheld drift, zoom, dolly move, drone shot, orbit around the car, lens flare, sparkle, slow motion, music, speech
```

**По-русски.** Камера на штативе, 35 мм, f/2.8, 55 см от пола, три четверти спереди-слева, полностью неподвижна. В кадре — a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing года, тёмно-бордовый «бычья кровь», «китовый хвост», чёрные диски Fuchs, чёрные накладки бамперов, БЕЗ номеров; поверхность бамперов — сплошная чёрная резина без ниши и рамки под номер. Машина только что заехала: плёнка зимней грязи, солевой налёт по низу, потёки от арок, мокрые пороги. Одно-единственное событие: холодный пар медленно входит слева, проходит перед нижней частью кузова и растворяется. Машина не двигается, вода не течёт, свет не меняется. Бокс: чёрный потолок с красными балками, четыре белые линейки LED, серая брусчатка с простыми красно-белыми полосами разметки без букв и пиктограмм, тёмно-серые панели стен, чёрные катушки шлангов, мокрый пол с отражением; без окон, без дневного света, без других машин. Экспозиция по линейкам, фон на два стопа темнее машины, одна непрерывная полоса отражения по плечевой линии, холодный серо-зелёный грейд, красные балки — единственный тёплый акцент, мелкое зерно. Тайминг: 0–1 с полная статика, пар только у левого края; 1–4 с пар проходит и истончается; 4–6 с пара нет, кадр в точности совпадает с первым.

**Зачем этот кадр.** Это точка «до»: грязь и соль на дорогом лаке — повод, по которому человек едет на мойку. Пар даёт кадру жизнь, не трогая ни машину, ни свет, поэтому петля честная.

**Если не выходит.** Сначала главное про машину: BMW M5 в промптах быть не должно — его в этом проекте нет вообще. M5 живёт в другом репозитории (/Users/vladislavakobson/Автомойка, src/components/WashHero.tsx и public/img/foam-m4*.jpg) — это отдельный Next.js-сайт. В MeatWash-v1.0 на сайте a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing: dist/assets/porsche-930.glb и dist/assets/img/hero-hq.webp. Во всех тринадцати промптах машина одна — a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, слово в слово.
Отдельно проверить с Владом: hero-hq.webp — это узкий классический 911 без «китового хвоста» и без раздутых арок, и стоит он в деревянной клубной комнате, а не в индустриальном боксе. Если канонический кадр делать «от сайта», описание машины придётся менять под узкий кузов; если оставляем строку с «китовым хвостом» — канонический кадр генерим с нуля и сайт потом подтягиваем под него. Решать до генерации, иначе первый экран и видео разойдутся.
Если пар не идёт:
1) убрать пар совсем, событие — одна капля грязной воды срывается с нижней кромки порога (не с бампера), 4 с;
2) заменить событие на медленное угасание и возврат одной потолочной линейки — модель тянет свет стабильнее пара;
3) крайний вариант — статичный канонический кадр постером, первый экран от этого не ломается.

---

## `hero-side-pass` — Проезд камеры вдоль борта

**6 с** · режим: frames-to-video (задаём ТОЛЬКО первый кадр — кроп канонического стоп-кадра по переду борта; последний кадр не задаём)

**Промпт для Flow:**

```text
[Cinematography] Camera on a motorised floor slider, 50 mm lens at f/2.0, lens axis 45 cm above the floor, one single continuous lateral move parallel to the car's flank, left to right, constant speed, easing out at the end. No zoom, no tilt, no pan, no rotation around the car, no handheld motion. The framing is tight on the flank: both bumpers stay outside the frame for the whole shot. [Subject] a a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, deep oxblood burgundy paintwork, whale-tail rear spoiler, black Fuchs-style wheels, black rubber bumper trim, NO license plate on either bumper — washed and dry, paint deep and even, the flared rear arch and the edge of the whale-tail spoiler the only shapes that leave the flank. [Action] exactly one event and nothing else: the slider travels a short distance, from the front door shut line to the flared rear arch, and the reflection of the long white ceiling LED strip runs along the shoulder line of the body and breaks on the edge of the spoiler. Nothing else moves: no water, no vapour, no people, no doors, no wheels turning. [Context] an underground detailing bay: black ceiling with exposed red-painted steel beams, four long white LED strip lights running front to back, grey brick-paver floor with plain red and white painted lane stripes and no characters or pictograms on the floor, dark grey panelled walls, black hose reels on the wall, wet floor giving a second softer reflection under the car, no windows, no daylight, no other vehicles. [Style & Ambiance] exposure set for the LED strips, background two stops darker than the car, the ceiling strip stays visible inside the reflection at all times, cool grey-green grade with the red ceiling beams as the only warm accent, shallow depth of field so the far wall stays soft, fine film grain, restrained and expensive, not advertising-glossy, no music, no dialogue, silence. [Timing] [00:00-00:01] static on the front door shut line. [00:01-00:05] the slider moves at constant speed and the light line travels along the body. [00:05-00:06] the move eases out and stops on the rear arch and the spoiler edge.
```

**Negative:**

```text
license plate, number plate, registration plate, plate recess, plate holder, plate frame, any digits or numerals, readable text, painted characters or pictograms on the floor, signage, road signs, model badges, brand logos, watermark, subtitles, on-screen text, people, full human figures, hands, other vehicles, windows, daylight, camera shake, handheld drift, zoom, tilt, pan, orbit around the car, drone shot, flying camera, lens flare, sparkle, slow motion, music, speech
```

**По-русски.** Камера на моторизованном напольном слайдере, 50 мм, f/2.0, ось объектива 45 см от пола. Одно непрерывное боковое движение параллельно борту, слева направо, ровная скорость, мягкое замедление в конце. Ни зума, ни наклона, ни панорамы, ни облёта. Кадр плотный по борту: оба бампера всю съёмку остаются за рамкой. Машина — a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing года, тёмно-бордовый «бычья кровь», «китовый хвост», чёрные диски Fuchs, чёрные накладки бамперов, БЕЗ номеров; вымыта и высушена, лак глубокий и ровный. Одно событие: слайдер проходит короткий отрезок — от стыка передней двери до раздутой задней арки, и отражение потолочной LED-линейки бежит по плечевой линии и обрывается на кромке «хвоста». Больше не двигается ничего. Бокс и свет — те же, что в остальных кадрах: чёрный потолок с красными балками, четыре белые линейки, серая брусчатка с простой разметкой без букв, тёмно-серые панели, мокрый пол со вторым мягким отражением. Экспозиция по линейкам, фон на два стопа темнее, линейка всегда читается внутри отражения, холодный серо-зелёный грейд, малая глубина резкости. Тайминг: 0–1 с статика на стыке двери; 1–5 с ход слайдера; 5–6 с замедление и остановка на задней арке и «хвосте».

**Зачем этот кадр.** Бегущая по борту полоса света — единственное, что честно показывает качество лака на видео. Это кадр-обещание: так выглядит машина после полировки и керамики.

**Если не выходит.** Единственный клип набора без петли — на сайте держим последний кадр или уводим кроссфейдом 320 мс.
Если слайдер у Veo превращается в облёт или кузов «дышит» формой по ходу движения:
1) укоротить до 4 с и урезать ход вдвое — только от задней двери до арки;
2) заперть камеру и двигать свет: вдоль борта проводят LED-жезл, эффект бегущей полосы тот же, у модели выходит стабильнее;
3) собрать проезд из двух статичных кадров (перед борта / зад борта) с CSS-кроссфейдом — ровно та механика, что уже работает в WashHero.
Плашки по номерам здесь нет по построению кадра: оба бампера за рамкой, это самый безопасный клип набора — его и генерить первым, чтобы проверить сходство машины дёшево.

---

## `hero-hood-reflection` — Крупный план переднего капота с отражением

**6 с** · режим: frames-to-video (кроп канонического стоп-кадра по капоту подаётся И первым, И последним кадром) · **петля**

**Промпт для Flow:**

```text
[Cinematography] Locked-off tripod shot, 85 mm lens at f/4, camera looking down the front luggage lid at a shallow 20-degree angle (the car is rear-engined, this is the front lid), the frame filled by the lid and the base of the windscreen, absolutely no camera movement. The front bumper and the whole plate area are outside the frame. [Subject] the front luggage lid of a a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, deep oxblood burgundy paintwork, whale-tail rear spoiler, black Fuchs-style wheels, black rubber bumper trim, NO license plate on either bumper — the lid is clean and completely dry, the dark paint reading almost black except where it reflects the ceiling. [Action] exactly one event and nothing else: the bay lighting moves on a dimmer, and the reflection of the ceiling LED strip in the paint closes from a soft broken halo into one hard-edged straight line of light, then opens softly back to exactly how it started. No water, no sheeting, no droplets, no hands, no cloth, no polisher, nothing enters the frame. [Context] an underground detailing bay: black ceiling with exposed red-painted steel beams and four long white LED strip lights, present only as reflections in the paint, no windows, no daylight, no other vehicles. [Style & Ambiance] exposure set for the reflected LED strip, background two stops darker than the paint, cool grey-green grade with the red ceiling beams faintly visible as the only warm streak inside the reflection, fine film grain, restrained and expensive, not advertising-glossy, no music, no dialogue, silence. [Timing] [00:00-00:01] static, the reflection soft and broken. [00:01-00:03] the reflected light line tightens into a single hard edge. [00:03-00:05] it softens back. [00:05-00:06] static, the reflection and the framing identical to the opening frame.
```

**Negative:**

```text
license plate, number plate, registration plate, plate recess, plate holder, plate frame, any digits or numerals, readable text, badges, brand logos, watermark, subtitles, on-screen text, hands, people, gloves, cloth, towel, polisher or any tool in frame, water sheeting, running water, droplets, foam, other vehicles, windows, daylight, camera shake, handheld drift, zoom, dolly, drone shot, rainbow lens flare, sparkle, slow motion, music, speech
```

**По-русски.** Камера на штативе, 85 мм, f/4, смотрит вдоль переднего багажника под углом 20° (двигатель сзади — это передняя крышка), кадр заполнен крышкой и основанием лобового стекла, камера полностью неподвижна. Передний бампер и вся зона номера — за рамкой. Машина — a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing года, тёмно-бордовый «бычья кровь», «китовый хвост», чёрные диски Fuchs, чёрные накладки бамперов, БЕЗ номеров; крышка чистая и полностью сухая, тёмный лак почти чёрный везде, кроме отражения. Одно событие: свет в боксе идёт на диммере, и отражение потолочной линейки в лаке собирается из мягкого рваного ореола в одну жёсткую прямую кромку, а затем мягко возвращается ровно в исходное состояние. Ни воды, ни стекающей плёнки, ни капель, ни рук, ни салфетки, ни машинки. Бокс присутствует только отражением: чёрный потолок, красные балки, четыре белые линейки. Экспозиция по отражённой линейке, фон на два стопа темнее лака, холодный серо-зелёный грейд, красные балки — единственная тёплая полоска внутри отражения, мелкое зерно. Тайминг: 0–1 с статика, отражение мягкое и рваное; 1–3 с кромка собирается; 3–5 с мягко расходится обратно; 5–6 с статика, кадр в точности как первый.

**Зачем этот кадр.** Ровная жёсткая кромка отражения — это и есть язык, на котором детейлинг доказывает качество лака. Клиент видит «паутинки нет» без единого слова.

**Если не выходит.** Здесь я вырезал стекающую плёнку воды из оригинального варианта — это одновременно худшее место Veo (плёночная вода плюс отражение) и нарушение петли: мокрое→сухое обратно не возвращается, а кадр был помечен «первый = последний». Теперь событие световое и обратимое, петля честная.
Если и световой ход ломается (мигание, «дышащий» лак):
1) оставить только подъём света без возврата, 4 с, петлю снять, на сайте держать последний кадр;
2) снять самим: штатив, телефон, 4 с, в боксе кто-то ведёт рукой диммер — кадр статичный, ИИ здесь не обязателен и выйдет правдивее;
3) два стоп-кадра (мягкое/жёсткое отражение) с кроссфейдом на сайте.
Если всё-таки хочется воды — переносить её на стекло, а не на лак: на стекле капля читается проще и это сразу продаёт «Антидождь».

---

## `hero-clean` — Ступень st-clean: машина вымыта

**6 с** · режим: frames-to-video (один и тот же канонический стоп-кадр «чистой» ступени подаётся И первым, И последним кадром) · **петля**

**Промпт для Flow:**

```text
[Cinematography] Locked-off tripod shot, 35 mm lens at f/2.8, camera 55 cm above the floor, static three-quarter front-left framing — the exact same framing, height, distance and lens as the arrival shot, absolutely no camera movement. [Subject] a a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, deep oxblood burgundy paintwork, whale-tail rear spoiler, black Fuchs-style wheels, black rubber bumper trim, NO license plate on either bumper — the bumper faces are smooth unbroken black rubber with no plate, no plate recess, no plate holder and no mounting bolts. The car is freshly washed: the paint clean and saturated, the body still damp, the black bumper trim and the wheels clean down to the barrel, a shallow sheet of standing water on the floor under the car. [Action] exactly one event and nothing else: a single drop falls from the lowest point of the rear wheel arch into the standing water under the car, one ring spreads outward across the reflection of the ceiling LED strips in the wet floor and fades out, and the water is flat and mirror-still again. The car does not move, no water runs down the body, the lights do not change, no hose, no pressure washer, no hands. [Context] an underground detailing bay: black ceiling with exposed red-painted steel beams, four long white LED strip lights running front to back, grey brick-paver floor with plain red and white painted lane stripes and no characters or pictograms on the floor, a black drain grate under the car, dark grey panelled walls, black hose reels on the wall, no windows, no daylight, no other vehicles. [Style & Ambiance] exposure set for the LED strips, background two stops darker than the car, one unbroken LED strip reflection along the shoulder line of the body at the same height in frame as in the arrival shot, cool grey-green grade with the red ceiling beams as the only warm accent, fine film grain, restrained and expensive, not advertising-glossy, no music, no dialogue, silence. [Timing] [00:00-00:01] static, the floor water flat and the two LED reflections straight. [00:01-00:03] the drop falls and one ring spreads across the reflection. [00:03-00:05] the ring fades out and the water flattens. [00:05-00:06] static, identical to the opening frame.
```

**Negative:**

```text
license plate, number plate, registration plate, plate recess, plate holder, plate frame, any digits or numerals, readable text, painted characters or pictograms on the floor, signage, model badges, brand logos, watermark, subtitles, on-screen text, people, full human figures, hands, hose, pressure washer or any tool in frame, foam, running water down the body, other vehicles, windows, daylight, camera shake, handheld drift, zoom, dolly, drone shot, lens flare, sparkle, slow motion, music, speech
```

**По-русски.** Камера на штативе, 35 мм, f/2.8, 55 см от пола, три четверти спереди-слева — ровно та же крупность, высота, дистанция и оптика, что в кадре приезда, камера неподвижна. Машина — a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing года, тёмно-бордовый «бычья кровь», «китовый хвост», чёрные диски Fuchs, чёрные накладки бамперов, БЕЗ номеров; поверхность бамперов — сплошная чёрная резина без ниши и рамки под номер. Только что вымыта: лак чистый и насыщенный, кузов ещё влажный, накладки и диски чистые до внутренней части, под машиной тонкий слой стоячей воды. Одно событие: с нижней точки задней арки срывается одна капля в лужу под машиной, по отражению потолочных линеек расходится одно кольцо и затухает, вода снова зеркально ровная. Машина не двигается, по кузову вода не течёт, свет не меняется, ни шланга, ни аппарата, ни рук. Бокс и грейд — те же, что во всём наборе; полоса отражения по плечевой линии на той же высоте кадра, что в кадре приезда. Тайминг: 0–1 с статика, вода ровная; 1–3 с капля и кольцо; 3–5 с кольцо гаснет; 5–6 с статика, кадр совпадает с первым.

**Зачем этот кадр.** Ступень «чистая» в лестнице состояний: та же точка съёмки, что и «грязная», поэтому разница читается мгновенно при переключении. Зеркальный пол доказывает, что бокс чистый — это продаёт дороже, чем сама машина.

**Если не выходит.** Стекающие по кузову нити воды я убрал: они и выглядели приклеенными, и ломали петлю. Капля в лужу — одно событие, начинается и кончается на ровной воде, петля настоящая.
Если кольцо на воде выходит резиновым:
1) убрать каплю, событие — «отражение линеек в полу чуть подрагивает и выпрямляется», 4 с;
2) без события вовсе, 4 с — но тогда это уже не видео, а постер, и честнее отдать постер;
3) снять самим: штатив, пипетка, 6 секунд — этот кадр реально снимается за десять минут.
Кадрирование по номерам: перед машины в кадре, поэтому формулировка «сплошная чёрная резина без ниши под номер» в промпте обязательна — просто negative модель здесь пробивает.

---

## `hero-gloss-final` — Финальный кадр: готовая машина, общий план

**8 с** · режим: frames-to-video (один и тот же канонический стоп-кадр «готовой» машины подаётся И первым, И последним кадром) · **петля**

**Промпт для Flow:**

```text
[Cinematography] Locked-off tripod shot, 28 mm lens at f/4, camera 70 cm above the floor, wide static three-quarter rear-left framing that shows the whole car, the ceiling beams and the floor markings, absolutely no camera movement. [Subject] a a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, deep oxblood burgundy paintwork, whale-tail rear spoiler, black Fuchs-style wheels, black rubber bumper trim, NO license plate on either bumper — the bumper faces are smooth unbroken black rubber with no plate, no plate recess, no plate holder and no mounting bolts. The car is finished: dry, deep glossy paint, tyres dressed matte black, the wide rear arches and the whale-tail spoiler clearly readable against the dark wall. [Action] exactly one event and nothing else: the bay lighting breathes on a dimmer — it rises from half power to full, holds, and eases back down to exactly the half power it started at. As it rises the long ceiling LED strips brighten, their reflections build along the flank and across the engine lid, the wet floor picks up a second reflection and the red ceiling beams come out of the dark; as it falls everything returns to the opening state. The car does not move, no water, no vapour, no hands, nothing enters the frame. [Context] an underground detailing bay: black ceiling with exposed red-painted steel beams, four long white LED strip lights running front to back, grey brick-paver floor with plain red and white painted lane stripes and no characters or pictograms on the floor, dark grey panelled walls, black hose reels on the wall, no windows, no daylight, no other vehicles. [Style & Ambiance] restrained and expensive, not advertising-glossy: the background stays two stops darker than the car even at full power, cool grey-green grade with the red ceiling beams as the only warm accent, no sparkle, no glints, no lens flare, fine film grain, no music, no dialogue, silence. [Timing] [00:00-00:01] static at half light. [00:01-00:04] the light rises smoothly to full. [00:04-00:05] holds at full. [00:05-00:07] it eases back down. [00:07-00:08] static at half light, identical to the opening frame.
```

**Negative:**

```text
license plate, number plate, registration plate, plate recess, plate holder, plate frame, any digits or numerals, readable text, painted characters or pictograms on the floor, signage, road signs, model badges, brand logos, watermark, subtitles, on-screen text, people, full human figures, hands, other vehicles, windows, daylight, strobing, flickering light, lens flare, sparkle effects, glints, camera shake, handheld drift, zoom, dolly, drone shot, slow motion, music, speech
```

**По-русски.** Камера на штативе, 28 мм, f/4, 70 см от пола, широкий статичный план три четверти сзади-слева: вся машина, балки потолка и разметка пола, камера неподвижна. Машина — a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing года, тёмно-бордовый «бычья кровь», «китовый хвост», чёрные диски Fuchs, чёрные накладки бамперов, БЕЗ номеров; поверхность бамперов — сплошная чёрная резина без ниши и рамки под номер. Готовая: сухая, глубокий глянец, резина в матовом чернении, широкие задние арки и «китовый хвост» чётко читаются на тёмной стене. Одно событие: свет в боксе дышит на диммере — поднимается с половины до полной, держится и плавно возвращается ровно на ту же половину. На подъёме линейки разгораются, отражения выстраиваются по борту и по крышке двигателя, мокрый пол даёт второе отражение, красные балки выходят из темноты; на спаде всё возвращается в исходное. Машина не двигается, ни воды, ни пара, ни рук. Бокс и грейд — как во всём наборе; фон остаётся на два стопа темнее машины даже на полном свете; без бликов, искр и засветок. Тайминг: 0–1 с статика на половине; 1–4 с плавный подъём; 4–5 с полка на полном; 5–7 с плавный спад; 7–8 с статика на половине, кадр совпадает с первым.

**Зачем этот кадр.** Финал лестницы: «вот так вы забираете машину». Дыхание света вместо рекламного блеска — ровно тот сдержанный дорогой тон, который отличает премиальный детейлинг от мойки у метро.

**Если не выходит.** В исходнике кадр был помечен петлёй, но свет в нём шёл в одну сторону — первый и последний кадры не совпадали. Я сделал полный вдох-выдох: half → full → half, петля стала настоящей, а ощущение «разгорания» сохранилось.
Если диммер у Veo превращается в мигание:
1) оставить только спад с полного до половины — обратный ход модель делает ровнее, петлю снять, на сайте держать последний кадр;
2) заменить событие движением воздуха: лёгкая пыль или пар в контровом луче, машина статична;
3) статичные 8 секунд без события не брать — это скринсейвер; лучше отдать постер и не проигрывать вовсе.
Это самый дорогой кадр по номерам: три четверти сзади, задний бампер крупно в кадре. Приёмку делать покадрово, не на глаз.

---

## `hero-bead` — Ступень st-bead: результат защиты (вода на готовой машине)

**6 с** · режим: frames-to-video (задаём ТОЛЬКО первый кадр — макро-кроп канонического стоп-кадра по задней арке; на Fast/Lite те же 2–3 референса добавить в ingredients)

**Промпт для Flow:**

```text
[Cinematography] Locked-off tripod shot, 100 mm macro lens at f/5.6, camera level with the rear quarter panel, the frame filled by the top of the flared rear arch and the base of the whale-tail spoiler, absolutely no camera movement. The rear bumper and the whole plate area are outside the frame. [Subject] the rear quarter of a a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, deep oxblood burgundy paintwork, whale-tail rear spoiler, black Fuchs-style wheels, black rubber bumper trim, NO license plate on either bumper — the paint is dry, finished and protected, and a scatter of tight round water beads is already sitting on the panel from the opening frame, each bead holding a single point of light from the ceiling LED strip. [Action] exactly one event and nothing else: three of the largest beads roll down the curve of the arch and leave clean dry tracks behind them, the rest of the beads stay exactly where they are. No mist, no spray, no water arriving, no coating, no bottle, no cloth, no hands, nothing enters the frame. [Context] an underground detailing bay, present only as the reflected white LED strip and the dark grey panelled wall behind, no windows, no daylight, no other vehicles. [Style & Ambiance] real time, not slow motion; clinical and restrained, background two stops darker than the paint, cool grey-green grade, the beads read as glass not as glitter, fine film grain, no music, no dialogue, silence. [Timing] [00:00-00:01] static, the beads sitting still on dry protected paint. [00:01-00:05] three large beads roll down the curve, leaving dry tracks. [00:05-00:06] everything settles and holds still.
```

**Negative:**

```text
license plate, number plate, registration plate, plate recess, plate holder, plate frame, any digits or numerals, readable text, badges, brand logos, watermark, subtitles, on-screen text, hands, people, gloves, spray bottle, cloth, towel or any tool in frame, mist, spray, water jet, foam, slow motion, glitter, sparkle, bokeh balls, other vehicles, windows, daylight, camera shake, handheld drift, zoom, dolly, drone shot, lens flare, music, speech
```

**По-русски.** Камера на штативе, макро 100 мм, f/5.6, на уровне задней боковины, кадр заполнен верхом раздутой задней арки и основанием «китового хвоста», камера неподвижна. Задний бампер и вся зона номера — за рамкой. Машина — a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing года, тёмно-бордовый «бычья кровь», «китовый хвост», чёрные диски Fuchs, чёрные накладки бамперов, БЕЗ номеров; лак сухой, обработанный и защищённый, и уже с первого кадра на панели лежит россыпь плотных круглых капель, в каждой — одна точка света от потолочной линейки. Одно событие: три самые крупные капли скатываются по изгибу арки, оставляя за собой чистые сухие дорожки, остальные капли остаются на месте. Никакой взвеси, никакого распыла, вода в кадр не прилетает, ни состава, ни флакона, ни салфетки, ни рук. Бокс присутствует только отражённой белой линейкой и тёмно-серой панельной стеной. Реальное время, не слоумо; клинично и сдержанно, фон на два стопа темнее лака, холодный серо-зелёный грейд, капли читаются как стекло, а не как блёстки. Тайминг: 0–1 с статика, капли лежат; 1–5 с три крупные скатываются; 5–6 с всё успокаивается.

**Зачем этот кадр.** Капля, которая собирается в шарик и уходит, не оставляя следа, — это дословно то, что обещает «Керамическое покрытие» и «Антидождь». Один макро-кадр закрывает две услуги из тринадцати.

**Если не выходит.** Самый дорогой клип набора по числу дублей, поэтому я срезал с него всё лишнее. В исходнике было три события подряд (взвесь садится → капли собираются → скатываются → панель сухая) и при этом пометка «петля» — Veo такую цепочку не удержит. Оставил одно событие: капли уже лежат, три крупные скатываются. Петлю снял честно: скатывание необратимо, на сайте держим последний кадр или кроссфейд.
Порядок отступления:
1) уменьшить до 4 с и скатывать одну каплю, а не три;
2) перенести кадр на стекло вместо лака — на стекле капля читается проще и это сразу «Антидождь»;
3) снять самим в боксе: пульверизатор, штатив, макро на телефон, 6 секунд — этот кадр правдивее любой генерации и стоит ноль кредитов;
4) статичный макро-постер с каплями и подписью услуги.
Первый кадр для этого клипа — не общий канонический стоп-кадр, а его макро-кроп по задней арке; кроп делать в редакторе, а не генерировать заново, иначе уплывёт оттенок бордового.

---

# Мойка

## `three-phase` — Трёхфазная мойка — выдержка пены

**6 с** · режим: frames-to-video (первый и последний кадр = ОДИН И ТОТ ЖЕ канонический стоп-кадр «930 под пеной, бампера вне кадра») · **петля**

**Промпт для Flow:**

```text
Locked-off tripod shot, absolutely no camera movement, no zoom, no pan, no handheld drift, fixed focus throughout. 35mm lens, f/4, camera 60 cm above a wet grey paver floor. Front three-quarter framing of a a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, deep oxblood burgundy paintwork, whale-tail rear spoiler, black Fuchs-style wheels, black rubber bumper trim, NO license plate on either bumper. The frame is cropped at the front wheel arch: neither bumper is inside the frame at any moment — only the flank, the roofline, the flared rear arch and the whale-tail are visible. Setting: an underground detailing bay — dark low concrete ceiling, insulated silver round ventilation ducts, long continuous white LED tubes running the length of the ceiling, dark grey corrugated wall panels, a steel floor drain grate, grey brick pavers wet and mirror-like, background exposed two stops darker than the car, cool grey-green ambience, bare walls and bare floor with no signage and no painted marks. The car is already blanketed in thick white snow foam; the dwell has just begun. A narrow ribbon of bare deep oxblood burgundy paint is already open along the shoulder line and stays exactly that size for the whole clip. ONE EVENT ONLY: the foam dwells and creeps under its own weight. [00:00-00:02] the foam blanket sits still, heavy and matte; fine steam drifts slowly left to right. [00:02-00:05] the foam creeps a few centimetres downward — thick tongues elongate along the door and the rear arch, two heavy clots let go of the lower sill and drop out of the bottom of frame onto the wet pavers. [00:05-00:06] the creep stops, the blanket holds exactly the thickness and pattern of the first frame, steam still drifting. Nothing else moves. Lighting: the white LED tubes read as one unbroken highlight along the shoulder line at a constant height of frame; the bare burgundy ribbon is the only saturated colour; steam backlit; no orange-teal grade, no lens flare. 24fps, quiet documentary detailing-studio look, expensive, not a commercial.
```

**Negative:**

```text
license plates, plate frames, blank plate panels, empty plate recess, any digits, any letters, any readable text, badges, model script, grille emblems, signage, floor stencils, painted letters or numbers on the floor, watermarks, subtitles, captions, full human figures, people, faces, hands, gloves, forearms, brushes or lances entering frame, camera movement, pan, zoom, rack focus, handheld shake, drone shot, dolly move, timelapse, slow-motion glamour, sparkle particles, glitter, rainbow refraction, orange-teal grade, lens flare, glossy TV-commercial look, rotating brushes, automatic tunnel wash gantry, a second car in frame, sedan body, four-door body, SUV body, modern grille, foam pouring in from above
```

**По-русски.** Статичный штатив, ни движения камеры, ни зума, фикс-фокус. 35 мм, f/4, камера в 60 см над мокрой брусчаткой. Передние три четверти a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, тёмно-бордовый «бычья кровь», «китовый хвост», чёрные диски Fuchs, чёрные накладки бамперов, БЕЗ номеров — кадр обрезан по передней арке, ни один бампер в кадр не попадает. Подземный бокс: тёмный бетонный потолок, серебристые круглые воздуховоды, длинные белые LED-трубки, серые гофропанели стен, решётка трапа, мокрая серая брусчатка, фон на два стопа темнее, голые стены и пол без надписей. Машина уже целиком под густой белой пеной, выдержка только началась; вдоль плечевой линии уже открыта узкая полоса голого бордового лака — она не меняется весь клип. ОДНО СОБЫТИЕ: пена стоит и оплывает под своим весом. 0–2с — пена неподвижна, матовая, пар тянет слева направо. 2–5с — пена сползает на пару сантиметров: языки вытягиваются по двери и задней арке, два тяжёлых сгустка срываются с порога и уходят вниз из кадра. 5–6с — сползание прекращается, пена держит ту же толщину и рисунок, что на первом кадре, пар идёт. Больше ничего не движется. Свет: LED-трубка одной непрерывной бликовой линией на постоянной высоте кадра, единственный насыщенный цвет — бордовая полоса лака, пар в контровом. 24 к/с, сдержанно, дорого, без рекламного глянца.

**Зачем этот кадр.** Услуга продаётся словом «выдержка» — и кадр показывает именно её: пена стоит на лаке и работает, а не летит красиво мимо. Это честная, узнаваемая картинка трёхфазки и единственный вариант, который переживает генерацию: падающая сверху завеса пены — самый частый провал модели. Важно: ни wash-foam.jpg, ни studio-bay.jpg в ingredients не класть — на первом BMW M5, на втором Mercedes G-Class с читаемой разметкой на полу; отсюда и брался чужой силуэт.

**Если не выходит.** Канонический кадр: генерить в Nano Banana Pro ТЕКСТОМ по описанию бокса выше плюс кроп нашего бокса БЕЗ машины (потолок с трубками, стена, брусчатка). Фото wash-foam.jpg / process-pressure.jpg / studio-bay.jpg целиком не подавать — там M5, GLC с человеком и G-Class; они и тянут серию к чужой машине. Лестница отступления: (1) убрать срыв сгустков, оставить только дрейф пара над неподвижной пеной — почти гарантированный проход; (2) сузить до одной двери и заднего крыла: меньше геометрии — меньше шансов, что кузов поплывёт; (3) на Fast/Lite добавить ingredients: утверждённый канонический кадр 930 плюс кроп бокса без машины; (4) крайний вариант — не генерировать: канонический кадр постером и лёгкий CSS-дрейф пара поверх. Если Veo блокирует марку — первую строку заменить на «a 1975 rear-engined German turbo coupe, deep oxblood burgundy, large whale-tail rear wing, wide rear arches, black five-spoke wheels», машину всё равно держит опорный кадр.

---

## `complex` — Комплексная с воском — горячая вода на корме

**6 с** · режим: frames-to-video (первый = последний кадр: заднее крыло и «китовый хвост» под тонкой плёнкой горячей воды) · **петля**

**Промпт для Flow:**

```text
Locked-off tripod shot, absolutely no camera movement, no zoom, no pan, no handheld drift, fixed focus throughout. 50mm lens, f/2.8, camera 70 cm above a wet grey paver floor. Tight rear-quarter framing of a a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, deep oxblood burgundy paintwork, whale-tail rear spoiler, black Fuchs-style wheels, black rubber bumper trim, NO license plate on either bumper. The frame holds only the rear quarter panel, the flared rear arch, the top of the black Fuchs wheel and the trailing edge of the whale-tail — the rear bumper is completely outside the frame. Setting: an underground detailing bay — dark low concrete ceiling, insulated silver round ventilation ducts, long continuous white LED tubes, dark grey corrugated wall panels, a steel floor drain grate, grey brick pavers wet and mirror-like, background exposed two stops darker than the car, cool grey-green ambience, bare walls and bare floor with no signage and no painted marks. A thin even film of hot wax-carrying water already lies on the burgundy quarter panel; warm vapour rises off the paint. ONE EVENT ONLY: the hot water sheets off one panel. [00:00-00:01] the film lies still and even, steam rising slowly. [00:01-00:04] the film breaks at the top of the panel and slides down as one smooth wide curtain, leaving the paint behind it deep, dark and glossy with the white LED tube mirrored in it as one hard clean line; the water runs off the lower edge of the panel and out of the bottom of frame. [00:04-00:06] a fresh warm rinse arrives from just outside the top of frame and rebuilds exactly the same even film as in the first frame; steam keeps rising. Nothing else moves. Lighting: the white LED tube mirrored as one unbroken highlight along the shoulder line at a constant height of frame; cool grey-green ambience; steam backlit; no orange-teal grade, no lens flare. 24fps, quiet documentary detailing-studio look, expensive, not a commercial.
```

**Negative:**

```text
license plates, plate frames, blank plate panels, empty plate recess, any digits, any letters, any readable text, badges, model script, engine-lid script, signage, floor stencils, watermarks, subtitles, captions, full human figures, people, faces, hands, gloves, forearms, a lance or hose in frame, camera movement, pan, zoom, rack focus, handheld shake, drone shot, timelapse, slow-motion water beauty-shot, splashing, sparkle particles, glitter, rainbow refractions, orange-teal grade, lens flare, foam, suds, a second car in frame, sedan body, four-door body, SUV body, reflections of people or of a studio crew in the paint
```

**По-русски.** Статичный штатив, никакого движения камеры, зума и рэк-фокуса. 50 мм, f/2.8, камера в 70 см над мокрой брусчаткой. Плотный кадр задней четверти a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, тёмно-бордовый «бычья кровь», «китовый хвост», чёрные диски Fuchs, чёрные накладки бамперов, БЕЗ номеров — в кадре только заднее крыло, раздутая арка, верх чёрного диска Fuchs и задняя кромка «китового хвоста»; задний бампер вне кадра полностью. Бокс тот же: тёмный бетонный потолок, серебристые воздуховоды, белые LED-трубки, серые гофропанели, решётка трапа, мокрая брусчатка, фон на два стопа темнее, голые стены и пол без надписей. На бордовом крыле уже лежит ровная плёнка горячей воды с воском, над лаком поднимается пар. ОДНО СОБЫТИЕ: вода сходит с одной панели. 0–1с — плёнка ровная и неподвижная, пар идёт. 1–4с — плёнка рвётся вверху и сходит одной широкой шторой, оставляя за собой глубокий тёмный глянец с жёсткой белой линией LED-отражения; вода уходит через нижнюю кромку вниз из кадра. 4–6с — сверху из-за кадра приходит тёплый ополаскиватель и восстанавливает ровно ту же плёнку, что на первом кадре, пар продолжается. Больше ничего не движется. 24 к/с, сдержанно, дорого, без рекламного глянца.

**Зачем этот кадр.** Воск продаётся поведением воды на лаке: сошла — и лак остался тёмным и зеркальным. Корма 930 с «китовым хвостом» — главная примета сходства во всей серии, поэтому именно её показываем крупно, и именно здесь опорный кадр окупается лучше всего.

**Если не выходит.** Сходящая вода плюс отражения — второе по сложности после пены, дублей будет больше среднего. Лестница: (1) убрать сход воды и оставить только подъём пара над глянцевой кормой плюс один медленный ручеёк по кромке — событие слабее, но проходит почти всегда и зацикливается идеально; (2) сузить кадр до одного заднего крыла без «хвоста» — меньше геометрии, меньше дрейфа; (3) на Fast/Lite ingredients: канонический кадр кормы 930 плюс кроп бокса без машины (wash-rinse.jpg целиком НЕ подавать — там M5); (4) если глянец выходит пластиковым — снять без воды: медленный проход пара по сухой глянцевой корме, а воск доказать подписью и ценой. При блокировке марки — обобщённая формулировка, как в three-phase.

---

## `reagents` — Детейлинг от реагентов — соль уходит из арки

**6 с** · режим: frames-to-video (первый = последний кадр: макро порога и арки, серый солевой самотёк уже идёт) · **петля**

**Промпт для Flow:**

```text
Locked-off camera standing on the floor, absolutely no camera movement, no zoom, no pan, fixed focus throughout. 35mm lens, f/4, lens 18 cm above wet grey pavers, extreme low angle. The frame is filled by the lower rear flank of a a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, deep oxblood burgundy paintwork, whale-tail rear spoiler, black Fuchs-style wheels, black rubber bumper trim, NO license plate on either bumper. Only the rocker sill, the flared rear arch lip and the edge of the black Fuchs wheel are in frame — no bumper of any kind is inside this frame. Above them, far out of focus, the dark low concrete ceiling, the insulated silver round ventilation ducts and one long white LED tube of the underground detailing bay; dark grey corrugated wall panels, a steel floor drain grate, grey brick pavers wet and mirror-like, background two stops darker, cool grey-green ambience, bare floor with no painted marks. ONE EVENT ONLY: salt-laden water drains out of the wheel arch by itself. [00:00-00:01] a steady stream of chalky grey-white runoff is already pouring from the inside lip of the arch onto the pavers. [00:01-00:04] the flow thickens: pale grey salt-laden water pours out of the arch, splits around a paver joint and floods the stone in front of the lens, then drains away toward the grate; where the water has passed, the burgundy sill is clean, dark and wet. [00:04-00:06] the flow eases back to exactly the same steady stream as in the first frame. No lance, no jet, no arm, no hand, no tool ever enters the frame — the water simply drains under gravity. Lighting: hard white LED light raking along the sill so the wet edge glows; deep shadow inside the arch; cool grey-green ambience; fine mist backlit; no orange-teal grade, no lens flare. 24fps, technical, honest, quiet, no glamour.
```

**Negative:**

```text
license plates, plate frames, blank plate panels, any digits, any letters, any readable text, badges, signage, floor stencils, painted numbers on the floor, chemical bottles with labels, watermarks, subtitles, captions, full human figures, people, faces, hands, gloves, forearms, a pressure lance, a hose, a brush, any tool entering frame, a high-pressure jet, camera movement, pan, zoom, rack focus, handheld shake, drone shot, timelapse, slow motion, sparkling clean-water beauty shot, foam curtain, snow, street exterior, daylight, a second car in frame, sedan body, SUV body, orange-teal grade, lens flare
```

**По-русски.** Камера стоит на полу, полная статика, без зума и рэк-фокуса. 35 мм, f/4, объектив в 18 см над мокрой брусчаткой, предельно низкий ракурс. Кадр заполнен нижней задней частью борта a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, тёмно-бордовый «бычья кровь», «китовый хвост», чёрные диски Fuchs, чёрные накладки бамперов, БЕЗ номеров — в кадре только порог, кромка раздутой задней арки и край чёрного диска Fuchs, бамперов в кадре нет вообще. Выше, в сильном расфокусе, тёмный бетонный потолок, серебристые воздуховоды и одна белая LED-трубка того же подземного бокса; серые гофропанели, решётка трапа, мокрая брусчатка, фон на два стопа темнее, пол без разметки. ОДНО СОБЫТИЕ: солёная вода сама вытекает из арки. 0–1с — из внутренней кромки арки уже идёт ровный меловато-серый самотёк на брусчатку. 1–4с — поток густеет: бледно-серая солевая вода льётся из арки, обтекает шов брусчатки, заливает камень перед объективом и уходит к трапу; там, где вода прошла, бордовый порог чистый, тёмный, мокрый. 4–6с — поток возвращается ровно к тому же состоянию, что на первом кадре. Ни копья, ни струи, ни руки, ни инструмента в кадре нет — вода идёт самотёком. Свет: жёсткий белый LED скользит по порогу, мокрая кромка светится, внутри арки глубокая тень. 24 к/с, технично, честно, без глянца.

**Зачем этот кадр.** Обещание услуги дословно: «соль уходит из порогов и арок» — и в кадре видно, как из арки вытекает именно серая солевая вода, а за ней остаётся чистый бордовый порог. Кузова почти нет, поэтому это самый дешёвый по дублям кадр группы: ломаться тут почти нечему.

**Если не выходит.** Струю и копьё я из основного варианта убрал намеренно: любой инструмент в кадре тянет за собой руку, а руки — названное слабое место модели. Лестница: (1) если самотёк выглядит вяло — добавить узкую струю, входящую из левого края кадра, но СТРОГО как воду без корпуса копья, и сразу закладывать вдвое больше дублей; (2) если модель всё равно дорисовывает руку — жёстко кропнуть верхнюю треть и оставить только порог с брусчаткой; (3) на Fast/Lite ingredients: канонический кадр порога 930 плюс кроп мокрой брусчатки, process-pressure.jpg НЕ подавать — там Mercedes GLC и человек с копьём в кадре, он и приводит чужую машину и руки; (4) крайний вариант — статичный макро-постер арки: эта услуга объясняется текстом лучше любой другой в группе. При блокировке марки — обобщённая формулировка, как в three-phase.

---

## `wheels` — Диски и шины — бордовое стекание по спицам

**8 с** · режим: frames-to-video (первый = последний кадр: колесо крупно, реакция уже идёт ровным потоком) · **петля**

**Промпт для Flow:**

```text
Locked-off macro shot, camera completely still, fixed focus, no rack focus, no movement of any kind. 85mm macro lens, f/2.8, camera 35 cm above wet grey pavers, perpendicular to the wheel. The frame is filled by one front black Fuchs-style wheel of a a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, deep oxblood burgundy paintwork, whale-tail rear spoiler, black Fuchs-style wheels, black rubber bumper trim, NO license plate on either bumper. Only the wheel, the wet black tyre sidewall and a narrow crop of the burgundy front arch are in frame — no bumper and no body panel beyond the arch is visible. Behind the spokes the inner barrel sits in soft focus, still grey with brake dust. Setting: the same underground detailing bay — dark ceiling, one long white LED tube reflected as a single hard highlight on the tyre shoulder, dark grey wall panels far out of focus, grey brick pavers wet and mirror-like, background two stops darker, cool grey-green ambience. The iron-dissolving wheel cleaner is already reacting when the clip starts. ONE EVENT ONLY: the chemical keeps bleeding and dripping. [00:00-00:02] deep burgundy-purple streaks are already running down the black spoke faces in a steady rhythm; a thin film of white foam lies on the pavers below. [00:02-00:06] the bleeding continues at the same rate: fresh streaks bloom out of the spoke faces and out of the inner barrel, gather on the lower rim lip and drip onto the wet pavers, where they spread in marbled dark red veins through the white foam and drift slowly out of the bottom of frame. [00:06-00:08] the rhythm stays identical to the first two seconds; one last drop leaves the rim lip exactly as at the start. Nothing else moves. Lighting: hard raking white LED light from upper left so each wet spoke edge carries a bright rim; the background falls to black; the burgundy runoff is the only saturated colour in frame. 24fps, macro, clinical, quiet.
```

**Negative:**

```text
license plates, plate frames, any digits, any letters, any readable text, tyre sidewall lettering, tyre size markings, brand markings on the wheel, brake caliper lettering, centre-cap emblem, chemical bottles with labels, signage, watermarks, subtitles, captions, full human figures, people, faces, hands, gloves, forearms, brushes entering frame, wheel off the car, detached wheels lying on the floor, tiled floor, camera movement, pan, zoom, rack focus, handheld shake, drone shot, timelapse, wheel rotation, car driving, blood-like gore look, bright magenta, neon pink, sparkles, glitter, rainbow reflections, orange-teal grade, lens flare, steam clouds, a second car in frame
```

**По-русски.** Статичное макро, камера абсолютно неподвижна, фикс-фокус, без рэк-фокуса. 85 мм макро, f/2.8, камера в 35 см над мокрой брусчаткой, перпендикулярно колесу. Кадр заполнен одним передним чёрным диском Fuchs автомобиля a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, тёмно-бордовый «бычья кровь», «китовый хвост», чёрные диски Fuchs, чёрные накладки бамперов, БЕЗ номеров — в кадре только диск, мокрая чёрная боковина шины и узкий кроп бордовой передней арки; ни бампера, ни других панелей кузова не видно. За спицами в мягком расфокусе внутренняя полка обода, ещё серая от тормозной пыли. Бокс тот же: тёмный потолок, одна белая LED-трубка отражается жёстким бликом на плече шины, серые панели стен в сильном расфокусе, мокрая брусчатка, фон на два стопа темнее. Очиститель уже реагирует в момент начала клипа. ОДНО СОБЫТИЕ: химия продолжает течь и капать. 0–2с — тёмно-бордово-фиолетовые потёки уже ровно идут по чёрным спицам, внизу на брусчатке тонкая плёнка белой пены. 2–6с — стекание идёт с тем же темпом: новые потёки проступают на спицах и с внутренней полки, собираются на нижней кромке обода и капают на мокрую брусчатку, расходясь мраморными тёмно-красными прожилками по белой пене и медленно уходя вниз из кадра. 6–8с — ритм тот же, что в первые две секунды; последняя капля срывается с кромки обода ровно как в начале. Больше ничего не движется. Единственный насыщенный цвет в кадре — бордовое стекание. 24 к/с, макро, клинично, тихо.

**Зачем этот кадр.** Единственный кадр группы, где цвет самого события совпадает с фирменным #861A22: бордовое стекание по чёрным спицам читается как подпись бренда, а не как спецэффект. Плюс это буквальная демонстрация того, за что платят, — что диск чистится и с внутренней стороны.

**Если не выходит.** Я убрал из кадра смену состояния: и первый, и последний кадр — уже идущее стекание, иначе петля физически не сходится (реакция необратима). Самый вероятный сбой — цвет: модель уводит потёки в ярко-малиновый или в «кровь». Лестница: (1) цвет чинить не промптом, а грейдом — снять нейтрально-тёмным и подтянуть оттенок к #861A22 в ffmpeg одним проходом на весь клип; (2) если потёки на спицах рвутся — сузить до трёх спиц и нижней кромки обода, стекание по кромке модель держит лучше; (3) на Fast/Lite ingredients: канонический кадр колеса 930 плюс кроп лужи с бордовым стеканием. ВАЖНО: wheels.jpg целиком НЕ подавать — там два снятых оранжевых многоспицевых диска на кафеле и бутылки химии с этикетками, это не наш диск и не наш пол; из него годится только кроп самой лужи как цветовой референс; (4) этот клип единственный, который дешевле снять телефоном в боксе вживую, чем гонять через Flow, — и он же приоритет 1, поэтому проверять его надо первым. При блокировке марки — обобщённая формулировка, как в three-phase.

---

# Кузов и лак

## `polish` — Полировка кузова — световая полоса собирается в кромку

**6 с** · режим: frames-to-video (ОДИН утверждённый первый кадр + событие текстом; кадр «после» — только если он СДЕЛАН ПРАВКОЙ того же самого кадра в Nano Banana Pro, а не сгенерирован заново)

**Промпт для Flow:**

```text
Locked-off tripod shot, 50mm lens at f/4, camera 55 cm above the floor, framing only the rear quarter panel and the flared rear arch of a a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, deep oxblood burgundy paintwork, whale-tail rear spoiler, black Fuchs-style wheels, black rubber bumper trim, NO license plate on either bumper; the bumper itself is cropped out of frame entirely, so no plate, plate recess or plate frame can appear anywhere in the picture. The car stands in the one dark detailing bay used across the whole series: matte black ceiling with dark red steel beams, a single long white LED strip overhead, dark vertical matte wall panels, wet grey paved floor holding a second reflection; warm key from the LED strip, cold blue rim light from behind, background two stops darker than the car, the burgundy reading near-black in shadow and blood-red only where the light lands. ONE event only: the reflection of the overhead LED strip lying along the shoulder line of the burgundy panel changes character, and nothing else in the frame moves. [00:00-00:01.5] hold — the reflected strip is a soft milky feathered halo, a fine circular swirl haze catching the light around it. [00:01.5-00:04.5] the swirl haze dissolves from left to right and the reflected strip tightens into one hard straight mirror-sharp line with a clean edge; the oxblood burgundy goes deeper and darker. [00:04.5-00:06] hold on the sharp reflection, the panel completely still. Nothing enters the frame: no polishing machine, no rotating pad, no cloth, no hand, no water, no foam. Camera is locked off on a tripod: no dolly, no zoom, no handheld shake, no rack focus, no camera drift. Restrained documentary detailing look, no music, no speech, no on-screen text.
```

**Negative:**

```text
license plates, plate frames, plate recesses, empty plate holders, any rectangle mounted on a bumper, any text, letters, digits or timecode anywhere in frame, logos, badges, brand marks, watermarks, subtitles, people, faces, bodies, hands, gloves, fingers, extra fingers, reflections of a film crew or a tripod, polishing machine, rotating pad, buffing wheel, cloth, spray bottle, water, foam, suds, droplets, a modern sedan, a BMW, any car other than the described 1970s rear-engined coupé, changed body colour, changed arch shape, changed wheel design, lens flares, star flares, sparkles, bokeh balls, slow-motion stock look, orange-teal grade, camera drift, zoom, handheld shake, music, speech. (рус.: ни одного номера, рамки или ниши под номер, ни одной буквы и цифры в кадре, ни людей, ни рук, ни машинки и тряпки, ни воды и пены, ни другой машины, ни другого цвета кузова, ни бликов-звёзд, ни дрожания камеры, ни звука.)
```

**По-русски.** Штатив, жёстко зафиксированная камера, 50 мм, f/4, 55 см над полом. В кадре только заднее крыло и раздутая арка a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing года, тёмно-бордовый «бычья кровь», «китовый хвост», чёрные диски Fuchs, чёрные накладки бамперов, БЕЗ номерных знаков; бампер срезан кадрировкой целиком, чтобы зона номера физически не попала в картинку. Тот же тёмный бокс, что и во всей серии: чёрный матовый потолок с тёмно-красными балками, одна длинная белая LED-линейка сверху, вертикальные матовые панели стен, мокрый серый пол с отражением; тёплый ключ от линейки, холодный контровой сзади, фон на две ступени темнее машины, бордовый почти чёрный в тени и кроваво-красный только на свету. ОДНО событие: отражение потолочной линейки на плечевой линии крыла меняет характер, больше в кадре не движется ничего. 0–1,5 с — отражение молочное, размытое, вокруг тонкая круговая паутинка. 1,5–4,5 с — муть сходит слева направо, отражение собирается в одну прямую зеркальную линию с чёткой кромкой, бордовый становится глубже и темнее. 4,5–6 с — выдержка на чистом отражении, крыло неподвижно. В кадр НИЧЕГО не входит: ни машинки, ни круга, ни тряпки, ни руки, ни воды, ни пены. Камера абсолютно статична. Сдержанная документальная подача, без музыки, без речи, без надписей.

**Первый кадр.** Канонический кадр серии, вариант «до»: заднее крыло и раздутая задняя арка a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing цвета «бычья кровь», камера 55 см над мокрым серым полом, объектив 50 мм. Бампер ПОЛНОСТЬЮ вне кадра — зоны номера в кадре нет. Отражение потолочной LED-линейки лежит вдоль плечевой линии крыла молочным размытым ореолом, вокруг него тонкая круговая паутинка. Тёмный бокс, чёрный матовый потолок с тёмно-красными балками, вертикальные матовые панели стен, холодный контровой сзади, фон на две ступени темнее кузова.

**Последний кадр.** Тот же кадр, изменённый ТОЛЬКО правкой в Nano Banana Pro (Edit поверх первого кадра, не новая генерация): паутинка снята, отражение LED-линейки — одна прямая зеркальная линия с чистой кромкой, бордовый глубже и темнее. Геометрия, свет, рамка, отражения фона — пиксель в пиксель как в первом кадре. Если правкой добиться не удаётся — второй кадр НЕ подставлять вообще: два независимо сгенерированных кадра Veo превратит в морфинг кузова.

**Зачем этот кадр.** Полировку продаёт ровно одна вещь — кромка отражения. Пока свет в лаке растекается молочным пятном, машина выглядит уставшей; как только он собирается в прямую линию, зритель сам достраивает «лак как зеркало» и вспоминает, что у него на крыле такое же пятно. Цена 40 000 ₽ оправдывается не процессом, а этим финальным кадром.

**Если не выходит.** 0) ПРО МАШИНУ: герой серии — не BMW M5. На сайте стоит 3D-модель a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, /Users/vladislavakobson/MeatWash-v1.0/dist/assets/porsche-930-optimized.glb, лак в сцене задан цветом #44080f (dist/js/scene.js, материал «Meatwash oxblood lacquer»). Снимки с BMW M4/M5 лежат в другом репозитории (/Users/vladislavakobson/Автомойка/public/img) и к героическому автомобилю отношения не имеют — их в референсы Flow НЕ подкладывать. 1) Если Flow режет марку — заменить только первые слова на «a classic 1970s rear-engined German coupé, flared rear arches, whale-tail rear spoiler, deep oxblood burgundy paintwork, black Fuchs-style wheels, black rubber bumper trim, NO license plate on either bumper», всё остальное в промпте не трогать. Арка и «китовый хвост» держат узнаваемость сами. 2) Полировальной машинки в промпте больше нет осознанно: вращающийся круг Veo стабильно достраивает рукой и предплечьем, а руки с инструментом — названное слабое место модели. Не возвращать её даже «на пробу», это сгоревшие кредиты. 3) Если переход паутинка→кромка не читается, снять клип про свет: камера стоит, сама LED-линейка медленно едет вдоль крыла, отражение меняет характер от движения света. Движется свет, кузов стоит. 4) Второй кадр подставлять ТОЛЬКО как правку первого в Nano Banana Pro. Два кадра, сгенерированных порознь, Veo сращивает морфингом: поплывёт форма арки и оттенок лака. 5) Полный провал — берём два утверждённых кадра и делаем кроссфейд 600 мс прямо на сайте, без видео: приём уже работает в WashHero. 6) Петли нет осознанно: обратное проигрывание вернёт паутинку — это антиреклама. Постер = ПОСЛЕДНИЙ кадр (отполировано), клип играет один раз по входу во вьюпорт или по выбору услуги и замирает на финале. 7) Скачанный файл прогнать через ffmpeg -an (Veo всегда кладёт звук, отключить в модели нельзя) и покадрово проверить на номера, буквы и цифры. 8) Снимать 16:9, кадрировать под панель «Гараж услуг» в CSS; вертикаль отдельно не генерировать, это удвоение кредитов.

---

## `chips` — Сколы и подкраска — скол закрывается, отражение идёт непрерывно

**4 с** · режим: frames-to-video (ОДИН первый кадр со сколом + событие текстом; второго кадра не подставлять)

**Промпт для Flow:**

```text
Extreme macro, locked-off tripod, 100mm macro lens at f/5.6, shallow depth of field, camera 20 cm from the leading edge of the front fender of a a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, deep oxblood burgundy paintwork, whale-tail rear spoiler, black Fuchs-style wheels, black rubber bumper trim, NO license plate on either bumper; the bumper and the whole plate area are far outside this frame. The frame is filled almost entirely by burgundy paint curving away. The same dark detailing bay as the rest of the series is present here only as reflections: the single long white LED strip laid as a soft warm band across the top third of the paint, cold blue rim light along the bottom edge, background falling to black, the burgundy reading near-black in shadow and blood-red only where the light lands. ONE event only: a single 2 mm stone chip closes, and nothing else in the frame moves. [00:00-00:01] hold — the chip reads as a tiny bright crater in the burgundy, grey primer at its bottom catching the light, and the reflected warm band is broken where it crosses the crater. [00:01-00:03] a small glossy bead of matching oxblood burgundy paint wells up inside the crater from below and levels out flush with the surrounding paint. [00:03-00:04] hold — the surface reads continuous and the reflected warm band now runs unbroken straight across the place where the chip was. Nothing enters the frame: no applicator, no brush, no touch-up pen, no tool tip, no hand. Camera is locked off on a tripod: no dolly, no zoom, no handheld shake, no rack focus, no camera drift. Clinical, restrained documentary macro, no music, no speech, no on-screen text.
```

**Negative:**

```text
license plates, plate frames, plate recesses, any rectangle mounted on a bumper, any text, letters, digits or timecode anywhere in frame, logos, badges, brand marks, watermarks, people, faces, hands, gloves, fingers, extra fingers, applicator, brush, touch-up pen, syringe, cotton swab, any tool tip entering frame, reflections of an operator or a tripod, paint colour that does not match the body, running drip, smeared blob, wet spray, water droplets, a modern sedan, a BMW, any car other than the described 1970s rear-engined coupé, changed body colour, lens flares, star flares, sparkles, focus pull, zoom, handheld shake, camera drift, slow-motion stock look, orange-teal grade, music, speech. (рус.: ни одного номера и рамки, ни одной буквы и цифры, ни людей и рук, ни аппликатора и кисти, ни подтёка и кляксы, ни несовпадения цвета краски с кузовом, ни перефокусировки и дрожания, ни звука.)
```

**По-русски.** Крупное макро, штатив, камера жёстко зафиксирована, 100 мм макро, f/5.6, малая глубина резкости, 20 см от передней кромки крыла a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing года, тёмно-бордовый «бычья кровь», «китовый хвост», чёрные диски Fuchs, чёрные накладки бамперов, БЕЗ номерных знаков; бампер и вся зона номера далеко за кадром. Кадр почти целиком залит уходящим бордовым лаком. Тот же тёмный бокс, что во всей серии, присутствует только отражениями: потолочная LED-линейка мягкой тёплой полосой по верхней трети лака, холодная контровая кромка снизу, фон в чёрное, бордовый почти чёрный в тени и кроваво-красный на свету. ОДНО событие: один скол 2 мм закрывается, больше в кадре ничего не движется. 0–1 с — скол читается крошечным светлым кратером, на дне серый грунт ловит свет, тёплая полоса отражения разорвана там, где пересекает кратер. 1–3 с — внутри кратера снизу набухает маленькая глянцевая капля бордового в цвет кузова и выравнивается вровень с лаком. 3–4 с — выдержка: поверхность непрерывна, тёплая полоса отражения идёт через бывший скол без разрыва. В кадр НИЧЕГО не входит: ни аппликатора, ни кисти, ни карандаша-подкраски, ни кончика инструмента, ни руки. Камера абсолютно статична. Клинически сдержанное документальное макро, без музыки, без речи, без надписей.

**Первый кадр.** Макро-кадр Nano Banana Pro: кадр почти целиком залит бордовым лаком передней кромки крыла a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, камера 20 см, 100 мм макро, малая глубина резкости. По верхней трети — мягкая тёплая полоса отражённой потолочной LED-линейки, внизу холодная контровая кромка, фон уходит в чёрное. В центре один скол 2 мм: маленький светлый кратер, на дне серый грунт ловит свет. Бампера и зоны номера в кадре нет.

**Последний кадр.** Не подставлять. Событие ведёт текст. Макро-кадр «после» отличался бы от «до» тысячей случайных мелочей в фактуре лака, и Veo вместо заливки скола сделает морфинг всей поверхности. Если событие совсем не идёт — см. fallback 3.

**Зачем этот кадр.** Клиент боится не скола, а перекраса всего крыла. Четыре секунды макро показывают ровно обещание подписи «точечно по месту»: работа размером два миллиметра, вокруг лак не тронут. Непрерывная полоса отражения в финале — это доказательство, что место ремонта не видно под светом, а именно по свету скол обычно и находят.

**Если не выходит.** 0) ПРО МАШИНУ: герой — a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing с сайта (dist/assets/porsche-930-optimized.glb, лак #44080f), не BMW M5 и не любой другой автомобиль из папок с фотографиями. 1) Если Flow режет марку — та же подмена первых слов на «a classic 1970s rear-engined German coupé, flared rear arches, whale-tail rear spoiler, deep oxblood burgundy paintwork, black Fuchs-style wheels, black rubber bumper trim, NO license plate on either bumper»; на макро марка не видна вовсе, подмена безболезненна. 2) Аппликатор из промпта убран осознанно: руки и точная мелкая моторика — главная поломка модели, инструмент в макро вырастает в лишние пальцы и вторую руку. Возвращать его нельзя. 3) Если «капля набухает сама» выглядит как пузырь или лак «дышит» — снять text-to-video чистое макро лака без события (4 с, камера стоит, живёт только отражение), а «до/после» показать на сайте вертикальной шторкой между двумя постерами Nano Banana Pro. 4) Если кратер в кадре не читается из-за глубины резкости — увеличить скол в кадре до 3–4 мм, а не переходить на более длинный объектив: смена оптики уводит фактуру лака от остальной серии. 5) Петли нет: обратная петля вернёт скол. Постер = последний кадр (закрыто), клип играет один раз по выбору услуги. 6) ffmpeg -an после скачивания, затем покадровая приёмка на текст и цифры. 7) Снимать 16:9, кадрировать в CSS.

---

## `headlights` — Полировка фар — стекло проясняется, свет получает кромку

**6 с** · режим: frames-to-video (ОДИН первый кадр с мутной фарой + событие текстом; кадр «после» — только правкой того же кадра в Nano Banana Pro)

**Промпт для Flow:**

```text
Locked-off tripod shot, 85mm lens at f/2.8, camera 45 cm above the floor, three-quarter front framing cropped tight on the round left headlamp and the burgundy fender around it of a a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, deep oxblood burgundy paintwork, whale-tail rear spoiler, black Fuchs-style wheels, black rubber bumper trim, NO license plate on either bumper; the bumper is cropped out below the lamp, so no plate, plate recess or plate frame can appear anywhere in the picture. The car stands in the one dark detailing bay used across the whole series, here darkened: matte black ceiling with dark red steel beams, the single long white LED strip overhead dimmed to a low cold glow, dark vertical matte wall panels, wet grey paved floor holding a second reflection, background three stops darker than the lamp, the burgundy reading near-black in shadow and blood-red only where the light lands. The headlamp is switched on at a low steady output and is the brightest thing in frame; its brightness never changes — no flicker, no pulsing, no strobing, no exposure shift. ONE event only: the glass of that lamp clears, and nothing else in the frame moves. [00:00-00:02] hold — the round lens looks yellowed and cloudy, and the light leaving it is a diffuse milky bloom with no edge. [00:02-00:04.5] the cloudiness clears across the lens from left to right: the glass goes clear, the bloom collapses into a defined bright disc with a sharp edge, and the reflection of the dimmed overhead LED strip appears as one crisp line in the glass. [00:04.5-00:06] hold on the clear lens and the defined disc, everything still. Nothing enters the frame: no polisher, no pad, no cloth, no hand. Camera is locked off on a tripod: no dolly, no zoom, no handheld shake, no rack focus, no camera drift. Restrained documentary detailing look, no music, no speech, no on-screen text.
```

**Negative:**

```text
license plates, plate frames, plate recesses, empty plate holders, any rectangle mounted on a bumper, any text, letters, digits or timecode anywhere in frame, logos, badges, brand marks, watermarks, people, faces, hands, gloves, fingers, polishing machine, pad, cloth, spray, water, foam, modern LED or projector headlamp optics, angel eyes, rectangular or oval headlamp, additional driving lamps, flicker, strobing, pulsing brightness, exposure pumping, a beam pattern or cut-off line projected on the wall or the floor, fog, smoke, haze machine, lens flares, star flares, sparkles, a modern sedan, a BMW, any car other than the described 1970s rear-engined coupé, changed body colour, changed headlamp shape, camera drift, zoom, handheld shake, slow-motion stock look, orange-teal grade, music, speech. (рус.: ни одного номера и рамки, ни одной буквы и цифры, ни людей и рук, ни машинки и тряпки, ни современной оптики и ангельских глазок, ни мерцания и скачков яркости, ни луча на стене, ни дыма, ни другой машины, ни другой формы фары, ни дрожания камеры, ни звука.)
```

**По-русски.** Штатив, камера жёстко зафиксирована, 85 мм, f/2.8, 45 см над мокрым серым полом, три четверти спереди, плотно на круглую левую фару и бордовое крыло вокруг неё: a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing года, тёмно-бордовый «бычья кровь», «китовый хвост», чёрные диски Fuchs, чёрные накладки бамперов, БЕЗ номерных знаков; бампер срезан кадрировкой ниже фары, зоны номера в картинке нет вообще. Тот же тёмный бокс серии, только притушенный: чёрный матовый потолок с тёмно-красными балками, потолочная LED-линейка убавлена до слабого холодного свечения, вертикальные матовые панели стен, мокрый пол с отражением, фон на три ступени темнее фары, бордовый почти чёрный в тени. Фара включена на малой РОВНОЙ мощности и самая яркая в кадре; её яркость не меняется — ни мерцания, ни пульсации, ни скачков экспозиции. ОДНО событие: стекло фары проясняется, больше в кадре ничего не движется. 0–2 с — стекло желтоватое и мутное, свет вокруг него размытое молочное свечение без границы. 2–4,5 с — муть сходит слева направо: стекло становится прозрачным, свечение собирается в плотный яркий диск с чёткой кромкой, в стекле появляется чёткая линия отражённой потолочной линейки. 4,5–6 с — выдержка на прозрачном стекле, всё неподвижно. В кадр НИЧЕГО не входит: ни машинки, ни круга, ни тряпки, ни руки. Камера абсолютно статична. Сдержанная документальная подача, без музыки, без речи, без надписей.

**Первый кадр.** Кадр Nano Banana Pro: три четверти спереди, плотно на круглую левую фару и бордовое крыло вокруг неё, a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, камера 45 см над мокрым серым полом, 85 мм, f/2.8. Бампер срезан кадрировкой ниже фары — зоны номера в кадре нет. Фара включена на малую ровную мощность и является самым ярким объектом кадра; стекло желтоватое и мутное, свет вокруг него — размытое молочное свечение без границы. Потолочная LED-линейка бокса приглушена до слабого холодного свечения, вертикальные матовые панели стен, мокрый пол с отражением.

**Последний кадр.** Тот же кадр, изменённый ТОЛЬКО правкой в Nano Banana Pro: стекло прозрачное, свечение собрано в плотный диск с чёткой кромкой, в стекле чётко отражается потолочная линейка. Яркость фары и экспозиция кадра те же — иначе Veo сделает из перехода пульсацию. Не получилось правкой — второй кадр не подставлять.

**Зачем этот кадр.** Мутная фара — единственный дефект, который владелец видит каждый вечер сам. Шесть секунд показывают не процесс, а результат за 5 000 ₽: стекло, в котором снова отражается линейка света, и свечение с чёткой кромкой вместо тумана. Это продаёт ощущением безопасности ночью, а не блеском.

**Если не выходит.** 0) ПРО МАШИНУ: снимаем ту же машину, что стоит на сайте 3D-моделью — a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, dist/assets/porsche-930-optimized.glb, лак #44080f. BMW M5/M4 из фотобанка второго репозитория в кадр и в референсы не идут ни при каких условиях. 1) Если Flow режет марку — подмена первых слов на «a classic 1970s rear-engined German coupé, flared rear arches, whale-tail rear spoiler, deep oxblood burgundy paintwork, black Fuchs-style wheels, black rubber bumper trim, NO license plate on either bumper»; круглая фара и арка держат узнаваемость. 2) ФАКТИЧЕСКАЯ МИНА: у настоящего 930 фары СТЕКЛЯННЫЕ и поликарбонат там не желтеет. Поэтому в промпте написано «yellowed and cloudy» и слова polycarbonate нет, а термин живёт только в подписи на сайте. Если Влад захочет буквальной правды — либо править подпись услуги в dist/js/config.js (сейчас «Мутный поликарбонат снова даёт чёткий пучок света»), либо честно снять этот один клип на современной машине и сказать, что он единственный не с героем. 3) Светотеневой границы на стене в кадре больше нет осознанно: луч на стене плюс включённая фара — это экспозиционные качели, Veo начинает пульсировать яркостью и «дышать» формой фары. Не возвращать. 4) Если модель всё равно мерцает или рисует современную оптику — выключить фару совсем: событие остаётся тем же (муть сходит, в стекле появляется чёткое отражение потолочной линейки), только стекло больше не источник света. Это самый устойчивый вариант, при третьем неудачном дубле переходить на него сразу. 5) Второй кадр — только правкой первого в Nano Banana Pro, с той же яркостью фары. Отдельно сгенерированный кадр «после» даст скачок экспозиции. 6) Полный провал — два кадра и кроссфейд 600 мс на сайте, как в WashHero. 7) Петли нет: обратная петля вернёт муть. Постер = последний кадр, клип играет один раз по выбору услуги. 8) ffmpeg -an, затем покадровая приёмка: три четверти спереди — самый опасный ракурс по номерам, проверять каждый кадр нижней трети. 9) Снимать 16:9, кадрировать в CSS.

---

# Салон

## `interior` — Химчистка салона — чистая полоса идёт по ткани

**6 с** · режим: frames-to-video (подаём ТОЛЬКО первый кадр — канонический кадр салона; последний кадр НЕ подаём, событие описано текстом)

**Промпт для Flow:**

```text
Locked-off tripod shot, no camera movement of any kind. 50mm lens at f/2.8, shallow depth of field. Camera sits just outside the open driver's door and looks down across the driver's seat. Interior only: no bodywork, no bumpers, no wheels, no exterior of the car anywhere in frame.

SUBJECT: the cabin of a a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, deep oxblood burgundy paintwork, whale-tail rear spoiler, black Fuchs-style wheels, black rubber bumper trim, NO license plate on either bumper. Period-correct interior: black leather seat bolsters, houndstooth-check cloth centre panels, a thin three-spoke steering wheel with a plain unmarked hub, five round gauges left dark and unreadable, a vertical leather door pull strap, charcoal carpet.

ACTION - one single event, nothing else happens: the cloth centre panel of the driver's seat begins dull, flat and grey, the nap flattened, a fine dust bloom across the weave. Across the clip one clean groomed lane travels slowly and steadily down that panel, from the top of the backrest to the front of the cushion. Behind the advancing edge the cloth is visibly deeper, darker and evenly brushed in a single direction; ahead of the edge it stays dull and grey. The edge stays crisp, straight and parallel to the seat stitching. A soft dark shadow of an off-screen tool travels with the edge. Nothing else in the cabin moves. No tool, no hose, no hands, no arms, no people in frame at any moment. The clip ends with the whole centre panel groomed and dark, the leather bolster beside it clean and matte, the cabin completely still.

CONTEXT: a dark detailing bay - matte black walls, black ceiling, vertical dark panels, one long white LED strip overhead as the single warm key light, a cooler light source behind the car and out of frame, polished wet concrete floor holding one soft reflection, just visible past the door sill.

STYLE: restrained, documentary, expensive. Warm key light, cool rim light, cool grey-green grade. Fine film grain, real surface texture. No stylisation, no slow motion, no sparkle, no lens flare. Silent, no dialogue, no music.
```

**Negative:**

```text
license plate, number plate, bumper, car exterior, body panel, badge, emblem, logo, readable text, letters, numbers, watermark, subtitles, timestamp, UI overlay, person, face, human body, hand, hands, arm, glove, fingers, tool in frame, hose, clear tube, spray, mist, foam, water jet, suds, droplets, steam, handheld camera shake, camera move, zoom, dolly, pan, tilt, drone, slow motion, sparkle particles, lens flare, cartoon, CGI look, plastic shine, oversaturated colour, colour shift, seat changing shape, upholstery pattern changing
```

**По-русски.** Камера жёстко на штативе, никакого движения. 50 мм, f/2.8, малая глубина резкости. Камера стоит снаружи открытой водительской двери и смотрит сверху вниз на водительское кресло. В кадре ТОЛЬКО салон: ни кузова, ни бамперов, ни колёс, ни номера.

ОБЪЕКТ: салон a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing года, тёмно-бордовый «бычья кровь», «китовый хвост», чёрные диски Fuchs, чёрные накладки бамперов, БЕЗ номерных знаков. Салон эпохи: чёрные кожаные боковины кресла, центральные вставки в «гусиную лапку», тонкий трёхспицевый руль с гладкой ступицей без надписей, пять круглых приборов — тёмные и нечитаемые, вертикальная кожаная петля-ручка двери, тёмно-серый ковролин.

ДЕЙСТВИЕ — одно событие, больше ничего: центральная тканевая вставка водительского кресла в начале тусклая, плоская, серая, ворс примят, по плетению пыльная дымка. За клип по вставке сверху вниз — от верха спинки к переднему краю подушки — ровно и медленно проходит одна чистая уложенная полоса. Позади наступающей границы ткань заметно темнее, глубже и причёсана в одну сторону; впереди границы остаётся тусклой и серой. Граница резкая, прямая, параллельная строчке. Вместе с границей идёт мягкая тень инструмента, который сам за кадром. Больше в салоне не двигается ничего. Ни инструмента, ни шланга, ни рук, ни людей в кадре нет ни в одном кадре. Клип заканчивается: вся вставка причёсана и потемнела, кожаная боковина рядом чистая и матовая, салон неподвижен.

СРЕДА: тёмный детейлинг-бокс — матовые чёрные стены и потолок, вертикальные тёмные панели, одна длинная белая LED-полоса сверху как единственный тёплый ключевой свет, холодный источник позади машины за кадром, полированный мокрый бетон с одним мягким отражением, чуть видный за порогом.

СТИЛЬ: сдержанно, документально, дорого. Тёплый ключ, холодный контровой, холодный серо-зелёный грейд. Мелкое зерно, настоящая фактура. Без стилизации, без слоу-мо, без блёсток и бликов. Без звука.

**Первый кадр.** Канонический кадр салона 911 (930) из Nano Banana Pro: открытая водительская дверь, камера снаружи смотрит вниз на водительское кресло. Чёрные кожаные боковины, центральная вставка в «гусиную лапку» — тусклая, серая, ворс примят, по плетению лёгкая пыльная дымка. В кадре только салон: ни бампера, ни крыла, ни колеса, ни номера. Приборы тёмные, нечитаемые, ступица руля без надписей.

**Последний кадр.** Не подаём. Если Влад всё же хочет два кадра — второй делать РЕДАКТИРОВАНИЕМ первого файла в Nano Banana (меняется только состояние ткани: вставка тёмная, ворс уложен в одну сторону), а не отдельной генерацией, иначе поедут свет и ракурс и Veo начнёт морфить салон.

**Зачем этот кадр.** Химчистку покупают глазами: человек должен увидеть границу между «до» и «после» на одной ткани, в одном кадре, без монтажа. Движущаяся чистая полоса — это и есть доказательство работы, и она честнее любого крупного плана с пеной: видно, что ворс уложен, а не просто «мокро».

**Если не выходит.** 1) ГЛАВНАЯ ПРАВКА ПРОТИВ ЧЕРНОВИКА: из кадра убраны насадка экстрактора, прозрачный шланг с грязной водой и рука в перчатке. Veo стабильно ломает именно это трио (геометрия инструмента, жидкость в прозрачной трубке, пальцы). Событие переписано на то, что модель тянет уверенно: смена фактуры + ползущая тень. Возвращать инструмент в кадр — только если первые дубли выйдут «мёртвыми».
2) Если полоса не читается совсем (ткань не темнеет) — усилить контраст в каноническом кадре: сделать исходную вставку заметно пыльнее и светлее, чтобы модели было от чего отталкиваться.
3) Если Flow всё-таки просит два кадра — второй получить РЕДАКТИРОВАНИЕМ первого файла в Nano Banana (меняется только ткань), не новой генерацией: иначе свет и ракурс уедут и Veo даст морфинг салона.
4) Если клип не выходит за 3–4 дубля — взять два утверждённых стоп-кадра (до/после) и кроссфейдить 320 мс тем же механизмом, что уже работает в WashHero: для до/после этого достаточно.
5) Крайний вариант — v:0 в stage.json: услуга едет постером с ценой и кнопкой записи. Это штатное состояние реестра, а не поломка сайта.
ВАЖНО ПРО ПЕТЛЮ: клип принципиально не зацикливается (первый кадр ≠ последний). На сайте играет один раз с удержанием последнего кадра, loop выключен. Завернуть «до/после» в петлю — значит стереть продажу: зритель увидит, что чистое опять становится грязным.
ПРИЁМКА: смотреть покадрово начало и конец — Veo любит дорисовать в проёме двери кусок крыла с бампером; любой намёк на внешнюю часть кузова = дубль в брак.

---

## `leather` — Кожа и пластик — свет идёт по зерну

**6 с** · режим: frames-to-video (подаём только первый кадр; петля собирается на монтаже обрезкой по совпадающему кадру, не в модели) · **петля**

**Промпт для Flow:**

```text
Locked-off tripod shot, no camera movement of any kind. 100mm macro lens at f/4. Extreme close-up on the top of the dashboard where the leather crash pad meets the door card. Interior only: no bodywork, no bumpers, no wheels, no exterior of the car anywhere in frame.

SUBJECT: the cabin of a a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, deep oxblood burgundy paintwork, whale-tail rear spoiler, black Fuchs-style wheels, black rubber bumper trim, NO license plate on either bumper. In frame: the black leather dashboard crash pad with a fine even pebble grain, a matte black plastic vent surround, the black leather door card behind it. No gauges, no badges, no lettering, no numbers anywhere in frame.

ACTION - one single event, nothing else happens: the frame opens in low ambient light, the grain reading deep matte black, the plastic surround an even charcoal, no highlight anywhere. One hard narrow band of white light then travels steadily from the left edge of frame to the right, raking low across the surface. Where it passes, every pore and crease of the grain lifts for a moment and settles again, the leather answering with depth and suppleness rather than shine; the plastic surround holds one even matte tone as the band crosses it, with no glossy patch, no greasy hotspot and no chalky grey bloom. The band stays a soft even stripe and never becomes a mirror. The band leaves the right edge of frame and the surface returns to the same low ambient matte black it started in. The camera, the dashboard and the door card never move.

CONTEXT: a dark detailing bay - matte black walls, black ceiling, vertical dark panels, one long white LED strip overhead as the single warm key light, a cooler light source behind the car and out of frame; the moving light source itself is entirely off camera and nothing else in the room is lit.

STYLE: restrained, documentary, expensive, almost still-life. Warm key light, cool rim light, cool grey-green grade. Fine film grain, real surface texture. No stylisation, no slow motion, no sparkle, no bokeh lights, no lens flare. Silent, no dialogue, no music.
```

**Negative:**

```text
license plate, number plate, bumper, car exterior, body panel, badge, emblem, logo, readable text, letters, numbers, watermark, subtitles, timestamp, person, face, human body, hand, hands, arm, glove, fingers, cloth, applicator pad, spray bottle, tool in frame, handheld camera shake, camera move, zoom, dolly, pan, tilt, rack focus, drone, slow motion, glossy wet greasy shine, mirror reflection, oily plastic, cheap dressing sheen, sparkle particles, bokeh lights, lens flare, cartoon, CGI look, oversaturated colour, leather changing grain or colour, frozen static frame with no light movement
```

**По-русски.** Камера жёстко на штативе, никакого движения. Макро 100 мм, f/4. Очень крупно: верх торпедо там, где кожаная накладка сходится с картой двери. В кадре ТОЛЬКО салон: ни кузова, ни бамперов, ни колёс, ни номера.

ОБЪЕКТ: салон a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing года, тёмно-бордовый «бычья кровь», «китовый хвост», чёрные диски Fuchs, чёрные накладки бамперов, БЕЗ номерных знаков. В кадре: чёрная кожаная накладка торпедо с мелким ровным зерном, матовая чёрная пластиковая окантовка дефлектора, за ней чёрная кожаная карта двери. Ни приборов, ни шильдиков, ни букв, ни цифр.

ДЕЙСТВИЕ — одно событие, больше ничего: кадр открывается на низком ровном свете, зерно читается глубоким матовым чёрным, пластик — ровный графит, бликов нет нигде. Затем одна узкая жёсткая полоса белого света ровно идёт от левого края кадра к правому, скользя вдоль поверхности. Там, где она проходит, каждая пора и складка зерна на мгновение поднимается и снова успокаивается — кожа отвечает глубиной и мягкостью, а не блеском; пластиковая окантовка держит один ровный матовый тон, без глянцевого пятна, без жирного «зайца» и без серого мелового налёта. Полоса остаётся мягкой ровной лентой и ни разу не превращается в зеркало. Полоса уходит за правый край, и поверхность возвращается ровно в то же низкое матовое состояние, с которого начинала. Камера, торпедо и карта двери не двигаются.

СРЕДА: тёмный детейлинг-бокс — матовые чёрные стены и потолок, вертикальные тёмные панели, одна длинная белая LED-полоса сверху как тёплый ключ, холодный источник позади машины за кадром; сам движущийся источник полностью за кадром, больше в помещении ничего не освещено.

СТИЛЬ: сдержанно, дорого, почти натюрморт. Тёплый ключ, холодный контровой, холодный серо-зелёный грейд. Мелкое зерно, настоящая фактура. Без стилизации, без слоу-мо, без блёсток, без боке, без бликов. Без звука.

**Первый кадр.** Макро-кадр из Nano Banana Pro: верх торпедо 911 (930) — чёрная кожаная накладка с мелким ровным зерном, матовая чёрная пластиковая окантовка дефлектора, за ней чёрная кожаная карта двери. Свет низкий, ровный, бликов нет вообще. В кадре нет приборов, шильдиков, надписей, рук и ничего от внешней части машины.

**Последний кадр.** Не подаём. Одинаковый первый и последний кадр в Flow даёт почти статичный клип — петлю режем в ffmpeg по кадру, совпадающему с первым.

**Зачем этот кадр.** Кожу и пластик продают не «блеском», а тем, что поверхность жива: зерно видно, тон ровный, жирного глянца нет. Проход света — единственный способ показать это за шесть секунд, и он же отстраивает нас от дешёвых моек, где «уход за кожей» заканчивается силиконовым блеском.

**Если не выходит.** 1) ГЛАВНЫЙ РИСК ЧЕРНОВИКА СНЯТ: одинаковые первый и последний кадр больше не подаём — на таком задании Veo отдаёт почти статичный клип. Подаём ТОЛЬКО первый кадр, событие описываем текстом, а петлю режем в ffmpeg по кадру, совпадающему с первым (плюс кроссфейд 200–320 мс на стыке). Петля собирается на монтаже, а не в модели — поэтому loop на сайте включён честно.
2) Если ход света вообще не читается (кадр мёртвый) — поднять контраст события: в промпте увеличить ширину и яркость полосы и укоротить клип до 4 с, чтобы модель не успевала «застыть».
3) Если и это не идёт — заменить событие на один проход аппликатора в чёрной перчатке по торпедо, матовая поверхность темнеет за ним. Но тогда в кадре появляются руки (слабое место модели), клип играет один раз, loop выключается.
4) Если ломается макро-фактура (кожа «пластилином», зерно плывёт) — уйти на средний план: кожаная карта двери с ручкой целиком, тот же проход света; на среднем плане модель держит фактуру заметно лучше.
5) Крайний вариант — v:0 в stage.json, услуга едет постером с ценой и кнопкой записи.
ПРИЁМКА: проверять покадрово, что пластик нигде не ушёл в глянец (это ровно то, что мы продаём как «не жирный блеск»), и что модель не дорисовала на окантовке вентиляции надпись или значок.

---

# Защита

## `ceramic` — Керамическое покрытие — вода стоит шаром и уходит, панель остаётся сухой

**6 с** · режим: frames-to-video (первый кадр = последний кадр: ОДИН И ТОТ ЖЕ канонический стоп-кадр сухого бордового заднего плеча 930 с целой белой линией потолочной LED-линейки). На Veo 3.1 Fast/Lite поверх кадров те же 3 картинки в ingredients: канонический кадр 930, кадр бокса, кадр мокрого пола. На Quality — только кадры, ingredients там нет. · **петля**

**Промпт для Flow:**

```text
Locked-off medium close-up on a tripod. Absolutely no camera movement: no dolly, no pan, no tilt, no zoom, no handheld drift. 85mm lens, f/4, shallow depth of field, focus held on the paint surface. Constant real-time speed for the whole clip — no slow motion, no speed ramp.

Subject: the rear shoulder and upper quarter panel of a a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, deep oxblood burgundy paintwork, whale-tail rear spoiler, black Fuchs-style wheels, black rubber bumper trim, NO license plate on either bumper. Only the rear shoulder, the top of the rear arch and the leading edge of the whale-tail spoiler in the top right corner are inside this framing — the rear bumper, the bumper face, the plate recess and the tail lights are entirely OUT of frame. The paint is clean, freshly coated, mirror-deep.

Context — the same detailing bay in every clip of this series: a dark ceiling crossed by dark red-brown steel I-beams, one long white LED linear fixture overhead, a white vertically ribbed corrugated wall far behind with NO lettering and NO signage on it, a grey paver floor left wet so it holds a second dim reflection. The overhead LED fixture reads on the paint as a single hard unbroken white line running along the shoulder of the panel, held at exactly the same height of frame for the whole clip. Warm key light from that overhead line, cold blue-grey fill from behind the car, background exposed two stops darker than the car.

Action — one single event, nothing else happens:
[00:00-00:01] the panel is dry and completely still, the white LED line unbroken along the shoulder.
[00:01-00:02] a soft mist passes once from outside the top of frame and lands on the panel ALREADY as tight round beads standing high on the surface, each bead catching one pinpoint of the LED line. The beads appear formed — do not show droplets spreading, stretching or contracting.
[00:02-00:04.5] the beads gather weight, merge and run off downward in fast straight tracks, out of the bottom of frame, carrying dust with them. The paint behind them is dry — no film of water, no streak, no residue.
[00:04.5-00:06] the panel is dry again and perfectly still, the white LED line unbroken, the frame identical to the very first frame.

Style and ambiance: matte, restrained, expensive product-film look. Warm key, cold backlight, deep near-black blacks. No sparkle, no rainbow refraction, no advertising gloss. No people anywhere. Silent: no dialogue, no music, no sound design.
```

**Negative:**

```text
license plate, number plate, registration plate, plate recess, plate bracket, plate frame, rear bumper, bumper face, tail light, any digits, any numbers, any text, lettering on the wall, signage, bottle labels, stickers, logos, badges, model script, watermark, timecode, UI overlay, people, faces, full human body, hands, gloves, camera movement, dolly, pan, tilt, zoom, handheld shake, slow motion, speed ramp, lens flare, rainbow refraction, sparkle, glitter, foam, soap suds, shampoo, spray jet, pressure washer lance, hose, rain, syrupy water, jelly-like water, water that stretches, streaks, drying marks, water spots, orange-teal grade
```

**По-русски.** Камера намертво на штативе, 85 мм, f/4, средний крупный план, реальное время без замедления. В кадре — заднее плечо и верх задней арки a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing года, тёмно-бордовый «бычья кровь», «китовый хвост» входит в правый верхний угол, чёрные диски Fuchs, чёрные накладки бамперов, БЕЗ номерных знаков; задний бампер и ниша под номер вне кадра целиком. Бокс тот же, что во всех клипах: тёмный потолок с тёмно-красными балками, одна белая LED-линейка сверху, белая рифлёная стена без единой надписи, мокрый серый пол с отражением. Линейка лежит на лаке одной непрерывной белой линией на одной и той же высоте кадра. 0–1 с — панель сухая и неподвижная. 1–2 с — сверху один раз проходит лёгкая взвесь и ложится на лак СРАЗУ готовыми тугими шариками, каждый ловит точку света; каплю не «лепим» на глазах. 2–4.5 с — шарики набирают вес, сливаются и уходят вниз быстрыми прямыми дорожками за нижнюю рамку, унося пыль; за ними лак сухой, без плёнки и разводов. 4.5–6 с — панель снова сухая и неподвижная, белая линия целая, кадр совпадает с первым. Тёплый ключ, холодная подсветка сзади, матовый дорогой грейд, никаких людей, без звука.

**Первый кадр.** Канонический стоп-кадр (Nano Banana Pro, утверждён с Владом один раз на всю серию): сухое заднее плечо 930 в бордо, кромка «китового хвоста» в правом верхнем углу, белая линия LED-линейки целой полосой вдоль плеча, бампера и ниши под номер в кадре нет, фон на две ступени темнее.

**Последний кадр.** Тот же файл, что и первый кадр — байт в байт. Это и есть механизм петли: крайние кадры совпадают физически, поэтому клип играет атрибутом loop без кроссфейда, и кузов не «дышит» формой к концу.

**Зачем этот кадр.** Керамику нельзя показать — её можно только доказать. Доказательство ровно одно: вода не растекается плёнкой, а стоит шаром и уходит, оставляя сухой лак. Кадр показывает результат за 15 000 ₽ буквально: было сухо — полилось — снова сухо, без разводов. Плюс «китовый хвост» в углу держит узнаваемость машины с сайта.

**Если не выходит.** 1) Сократил с 8 до 6 с и убрал замедление: смешивать 0.5x и реальное время внутри одного клипа — вторая инструкция, которая конкурирует с физикой воды, а вода у Veo и так слабое место. Формирование капли из растекания тоже убрано — модель лепит из него кисель; капли приходят уже готовыми. 2) Если и так вода идёт киселём или мыльным пузырём — резать до 4 с, оставив ТОЛЬКО скат (0–0.5 покой, 0.5–3.5 скат, 3.5–4 покой): скат готовых капель модель тянет заметно лучше, чем их рождение. 3) Если последний кадр не возвращается сухим (самый вероятный сбой) — не перегенерировать: обрезать хвост на первом сухом кадре и закрыть стык кроссфейдом 0.4 с в CSS, петля сохраняется, кредиты нет. 4) Если модель всё равно дорисовывает бампер или нишу под номер — поднять рамку выше, срез по верхней кромке арки, спойлер занимает верхнюю треть кадра. 5) Если Flow упрётся в политику по брендам на слове a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing — заменить во всём промпте на «a 1975 rear-engined German sports coupe with a wide rear arch and a whale-tail rear spoiler, deep oxblood burgundy paintwork, black Fuchs-style five-spoke wheels, black rubber bumper trim, NO license plate on either bumper» и менять ОДИНАКОВО во всех 13 промптах, иначе серия рассыплется на 13 разных машин. 6) Совсем крайний случай — снять практически: реальная панель с керамикой в боксе, пульверизатор, телефон в макро на штативе, 6 с, свет от той же потолочной линейки, грейд подогнать под остальные клипы. 7) Звук Veo генерирует всегда — срезать ffmpeg -an на постобработке. 8) Актёр во всех 13 клипах один: 930 с сайта (dist/assets/porsche-930-optimized.glb). Никакой другой машины в кадре нет.

---

## `rain` — Антидождь — сухой фронт уезжает по стеклу, вода не размазывается

**6 с** · режим: frames-to-video (первый кадр = последний: чистое сухое лобовое с резким отражением LED-линейки в верхней трети). Событие полностью укладывается внутрь клипа и возвращает кадр в исходное состояние — петля честная. На Fast/Lite добавить в ingredients канонический кадр 930 и кадр бокса. · **петля**

**Промпт для Flow:**

```text
Locked-off shot on a tripod. No camera movement of any kind: no dolly, no pan, no tilt, no zoom, no handheld drift. 50mm lens, f/2.8, real time throughout.

Camera stands outside the car, slightly above the base of the windscreen and about ten degrees off the centre line. The frame is cut at the bottom edge of the glass: the hood, the wiper cowl, the front bumper and any license plate recess are COMPLETELY out of frame. Inside the frame there is only the windscreen glass filling almost the whole picture, the two A-pillars, the roof edge and the dark empty cabin behind the glass.

Subject: the windscreen of a a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, deep oxblood burgundy paintwork, whale-tail rear spoiler, black Fuchs-style wheels, black rubber bumper trim, NO license plate on either bumper. Of the car, only the oxblood burgundy A-pillars, the roof edge and the black rubber trim around the glass are visible — the spoiler, the wheels and both bumpers are outside this framing. The cabin behind the glass is dark and empty: nobody inside, no driver, no silhouette.

Context — the same detailing bay in every clip of this series: a dark ceiling crossed by dark red-brown steel I-beams, one long white LED linear fixture overhead, a white vertically ribbed corrugated wall far behind with NO lettering and NO signage on it, a grey paver floor left wet so it holds a second dim reflection. The LED fixture reads in the glass as a single hard white line across the upper third of the windscreen, in exactly the same position for the whole clip. Warm key light from that overhead line, cold blue-grey fill from behind the car, background two stops darker than the car.

Action — one single event:
[00:00-00:01] the glass is clear and dry, dead still, the reflected white LED line sharp and unbroken.
[00:01-00:02] clean water arrives once from outside the top of frame and covers the whole glass in a thin even sheet; the reflected LED line goes to a blurred smear.
[00:02-00:04.5] a dry front sweeps across the glass from lower left to upper right as ONE clean straight boundary moving at a steady speed. Ahead of the boundary the glass is still wet and smeared; behind it the glass is instantly clear and the reflected LED line is sharp again. The boundary travels out of the frame. The wipers never move — no wiper blade ever crosses the glass.
[00:04.5-00:06] the glass is clear and dry, the reflected white LED line sharp and unbroken, the frame identical to the first frame.

Style and ambiance: matte, restrained, documentary-clean. Warm key, cold backlight, deep near-black blacks. No advertising gloss, no slow motion, no lens flare. No people, and no reflection of a camera, tripod or crew in the glass. Silent: no dialogue, no music, no sound design.
```

**Negative:**

```text
license plate, number plate, registration plate, plate recess, plate bracket, hood, bonnet, wiper cowl, front bumper, any digits, any numbers, any text, lettering on the wall, signage, stickers, inspection sticker on the glass, logos, badges, watermark, timecode, UI overlay, people, faces, driver, silhouette in the cabin, hands, reflection of a camera, reflection of a tripod, reflection of a crew, moving wipers, windshield wiper blade, wiper arm, camera movement, dolly, pan, zoom, handheld shake, slow motion, raindrops running down the glass, streaks, syrupy water, jelly-like water, lens flare, orange-teal grade
```

**По-русски.** Камера намертво на штативе, 50 мм, f/2.8, реальное время. Стоим снаружи, чуть выше основания стекла, под углом ~10°; рамка срезана по нижней кромке стекла — капот, жабо, передний бампер и ниша под номер ВНЕ кадра полностью. В кадре только лобовое стекло почти во весь кадр, две стойки, кромка крыши и тёмный пустой салон. Машина — a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing года, тёмно-бордовый «бычья кровь», «китовый хвост», чёрные диски Fuchs, чёрные накладки бамперов, БЕЗ номерных знаков; от машины видны только бордовые стойки, кромка крыши и чёрный уплотнитель. В салоне никого. Бокс тот же: тёмный потолок с тёмно-красными балками, белая LED-линейка, белая рифлёная стена без надписей, мокрый серый пол. Линейка отражается в стекле одной резкой белой линией в верхней трети, на одном месте весь клип. 0–1 с — стекло чистое и сухое, линия резкая. 1–2 с — сверху один раз приходит вода и накрывает стекло тонкой ровной плёнкой, отражение размазывается. 2–4.5 с — по стеклу слева-снизу направо-вверх ровно уезжает ОДНА прямая граница сухого: перед ней ещё вода, за ней стекло сразу чистое и линия снова резкая; граница уходит за рамку. Дворники не двигаются вообще. 4.5–6 с — стекло чистое и сухое, белая линия резкая и целая, кадр совпадает с первым. Тёплый ключ, холодная подсветка сзади, без замедления, без бликов, без людей и без отражения съёмочной группы в стекле, без звука.

**Первый кадр.** Канонический стоп-кадр: чистое сухое лобовое 930, бордовые стойки по краям, тёмный пустой салон, одна резкая белая линия LED-линейки в верхней трети стекла, капота и ниши под номер в кадре нет.

**Последний кадр.** Тот же файл, что первый кадр. Стекло возвращается в чистое состояние достовернее, чем лак, поэтому именно здесь петля самая надёжная в группе.

**Зачем этот кадр.** Антидождь продаётся не каплями, а скоростью: вода не ползёт и не размазывается, а срывается, и видимость возвращается мгновенно. Уезжающая граница сухого — это буквально то, что водитель видит на трассе за 3 000 ₽. Пустой тёмный салон и отсутствие дворников в кадре говорят: работает химия, а не щётки.

**Если не выходит.** 1) Главная правка: из основного промпта убран «срыв воды потоком воздуха». Считать физику отрыва — самая рискованная просьба во всей группе, Veo делает из неё кисель. Вместо этого в основу поднят бывший запасной ход: уезжающая граница сухого. Модели несравнимо проще вести ровный край, чем моделировать отрыв. Воздуходувка как событие уходит в запас. 2) Если и граница плывёт — сделать её ещё проще: половина стекла уже сухая с самого первого кадра, граница просто медленно уезжает за рамку, воды на стекле никогда не прибавляется; тогда крайние кадры перестают совпадать, петля снимается (loop=false) и стык закрывается кроссфейдом 0.4 с в CSS. 3) Если модель дорисовывает капот, жабо или нишу под номер — опустить камеру и подать стекло почти фронтально, чтобы оно занимало кадр от края до края, кромку крыши срезать. 4) Если в стекле проступает отражение камеры или человека — увести угол ещё на 10–15° и добавить в промпт строку «the glass reflects only the ceiling LED line and the dark ceiling, nothing else». 5) Если и это не идёт — снять практически в боксе: воздуходувка вдоль стекла, телефон на штативе, 6 с, в салоне никого, отражение той же потолочной линейки. 6) Совсем крайний вариант — два канонических кадра (мокрое стекло → чистое стекло) и кроссфейд в браузере тем же механизмом, который уже работает в героблоке: движение отдаёт браузер, генерация не нужна вовсе. 7) Если Flow упрётся в бренд — тот же единый дебренд, что в остальных 12 промптах: «a 1975 rear-engined German sports coupe…». 8) Звук срезать ffmpeg -an. 9) Машина во всех вариантах одна — 930 с сайта, других машин в серии нет.

---

## `film` — Оклейка зон риска — ракель гонит волну по кромке капота, лак становится глубже

**6 с** · режим: frames-to-video (первый кадр = последний: тот же передний край капота и верх фары). Плёнка прозрачна, поэтому крайние кадры честно идентичны — петля сходится сама, никакого «до/после» здесь не нужно. На Fast/Lite сверху ingredients: канонический кадр 930 + кадр бокса. Самый рискованный клип группы, закладывать 4–5 дублей. · **петля**

**Промпт для Flow:**

```text
Locked-off close-up on a tripod. No camera movement whatsoever: no dolly, no pan, no tilt, no zoom, no handheld drift. 50mm lens, f/4, focus held on the paint surface, real time throughout.

Subject: the leading edge of the front hood and the upper half of the round headlight pod of a a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing, deep oxblood burgundy paintwork, whale-tail rear spoiler, black Fuchs-style wheels, black rubber bumper trim, NO license plate on either bumper. The spoiler, the wheels and both bumpers are outside this framing: the frame is cut above the bumper line and the lower edge of the picture is the top of the black rubber bumper trim. NO bumper face, NO license plate, NO plate recess, NO plate bracket and NO fog lights are visible anywhere in frame.

Context — the same detailing bay in every clip of this series: a dark ceiling crossed by dark red-brown steel I-beams, one long white LED linear fixture overhead, a white vertically ribbed corrugated wall far behind and out of focus with NO lettering and NO signage on it, a grey paver floor left wet so it holds a second dim reflection. The LED fixture reads on the hood as a single hard white line running across the panel at a fixed height of frame. Warm key light from that overhead line, cold blue-grey fill from behind the car, background two stops darker than the car.

Action — one single event, one moving object and nothing else:
[00:00-00:00.5] the hood edge and the headlight are still, the reflected white LED line unbroken, the surface reads as bare paint.
[00:00.5-00:04.5] a soft black squeegee blade enters from the right edge of frame — the blade alone, with NO hand, NO fingers, NO glove, NO arm and NO person attached to it — and travels slowly and evenly across the hood edge toward the headlight at one constant speed, no jitter, no stop, no re-grip. Ahead of the blade a wet vein of clear slip solution is pushed forward as a single moving bead of liquid. Behind the blade the paint is dry and the reflected white LED line becomes deeper and cleaner. No seam, no edge, no line, no bubble and no haze appears anywhere on the surface.
[00:04.5-00:06] the blade has left the frame to the left; the panel is still again, the reflected white LED line unbroken, the frame identical to the first frame.

Style and ambiance: matte, restrained, technical, expensive. Warm key, cold backlight, deep near-black blacks. No advertising gloss, no slow motion, no lens flare. Silent: no dialogue, no music, no sound design.
```

**Negative:**

```text
license plate, number plate, registration plate, plate recess, plate bracket, bumper face, front bumper, fog light, turn signal lens, any digits, any numbers, any text, lettering on the wall, bottle labels, signage, stickers, logos, badges, model script, watermark, timecode, UI overlay, hand, fingers, thumb, glove, arm, wrist, forearm, person, face, full human body, second person, two hands, extra fingers, camera movement, dolly, pan, zoom, handheld shake, slow motion, air bubbles under the film, visible film edge, visible seam, cut line, white haze, milky film, peeling corner, foam, soap suds, spray jet, lens flare, orange-teal grade
```

**По-русски.** Камера намертво на штативе, 50 мм, f/4, крупный план, реальное время. В кадре — передняя кромка капота и верхняя половина круглой фары a classic 1970s rear-engined air-cooled sports coupe with a wide flared rear arches and a large flat rear wing года, тёмно-бордовый «бычья кровь», «китовый хвост», чёрные диски Fuchs, чёрные накладки бамперов, БЕЗ номерных знаков; спойлер, диски и оба бампера вне кадра, рамка срезана выше бамперной линии, нижний край кадра — верх чёрной накладки. Ни бампера, ни номера, ни ниши, ни кронштейна, ни противотуманок в кадре нет. Бокс тот же: тёмный потолок с тёмно-красными балками, белая LED-линейка сверху, белая рифлёная стена без надписей в расфокусе, мокрый серый пол. Линейка лежит на капоте одной резкой белой линией на постоянной высоте кадра. 0–0.5 с — панель неподвижна, линия целая, поверхность читается как голый лак. 0.5–4.5 с — из правого края кадра входит мягкое чёрное лезвие ракеля — ТОЛЬКО лезвие, без руки, без пальцев, без перчатки, без предплечья и без человека — и ровно, с одной скоростью идёт по кромке капота к фаре; перед лезвием катится прозрачный вал смачивающего раствора одной сплошной жилой; за лезвием лак сухой, а отражённая белая линия становится глубже и чище; ни стыка, ни кромки, ни пузыря, ни мути нигде не появляется. 4.5–6 с — лезвие ушло за левый край, панель снова неподвижна, линия целая, кадр совпадает с первым. Тёплый ключ, холодная подсветка сзади, матовый технический грейд, без замедления и бликов, без звука.

**Первый кадр.** Канонический стоп-кадр: передняя кромка капота и верх круглой фары 930 в бордо, одна резкая белая линия LED-линейки поперёк капота, нижний край кадра — верх чёрной накладки, бампера и номера нет.

**Последний кадр.** Тот же файл, что первый кадр. Плёнка прозрачна, поэтому «после» визуально не отличается от «до» ничем, кроме глубины отражения, — крайние кадры совпадают без натяжки и петля сходится сама.

**Зачем этот кадр.** Плёнку невозможно показать — её видно только по тому, что стыка НЕ видно, а лак стал глубже. Поэтому доказательство здесь — не материал, а инструмент, идущий ровно по той зоне, за которую платят: кромка капота и фара, то самое место, куда прилетает первым. 10 000 ₽ читаются как «защищено именно там, где бьёт».

**Если не выходит.** 1) Главная правка: рука убрана из ОСНОВНОГО промпта и понижена в запас. Руки и мелкая моторика — прямая слабость Veo, а 8 с ведения ракеля перчаткой почти гарантируют плывущие пальцы и перехват. В основе теперь только лезвие, входящее из-за рамки: один движущийся объект, ноль анатомии. Длительность срезана с 8 до 6 с, вход ракеля сдвинут на 0.5 с. 2) Если «летающий» ракель читается неестественно или модель всё равно дорисовывает руку — вернуть исходный вариант как дубль: одна кисть в чёрной нитриловой перчатке, входящая справа, без предплечья выше запястья, без человека; держать руку в лёгком расфокусе и не показывать пальцы крупно. 3) Если и это плывёт — доказательство светом без инструмента вовсе: камера стоит, по капоту медленно проходит отражение белой линейки, и на кромке фары на секунду проступает тонкая дуга среза плёнки, 6 с. 4) Если модель рисует пузыри, муть или видимый стык — добавить в промпт «the surface stays optically identical before and after the blade, only the reflection deepens» и усилить негатив, который уже перечисляет пузыри и кромку. 5) Если не идёт ничего — снять практически: реальная оклейка в боксе, телефон на штативе, макро по кромке капота, 6 с, руки в чёрных перчатках, никого в кадре выше запястья. 6) Передний план — самое опасное место по номерам во всей серии; каждый дубль принимать покадрово, а не «на глаз». 7) Если Flow упрётся в бренд — тот же единый дебренд во всех 13 промптах: «a 1975 rear-engined German sports coupe…». 8) Звук срезать ffmpeg -an. 9) Машина во всех вариантах одна — 930 с сайта.

---

# Порядок съёмки

Снимать по возрастанию сложности: сначала то, на чём проверяется сходство машины,
и только потом дорогое — вода, пена, отражения.

1. **`wheels`** — Диски и шины (8 с) — Тест-клип и гейт правдоподобности. Статичная макро-камера, колесо на весь кадр, событие — очиститель стекает бордовым по спицам (это буквально #861A22). Самый сильный и самый безопасный кадр набора: номерной рамки в кадре нет физически. Снимается ПЕРВЫМ, до всякого кода по видео.
2. **`st-arrive`** — Состояние: как приехала (6 с) — Первый экран и базовая ступень лестницы. Машина стоит грязная в их подземном боксе, камера заперта, движется только пар и капля с трубы. Первый кадр = последний. Постер этого клипа — LCP-элемент сайта.
3. **`three-phase`** — Трёхфазная мойка (6 с) — Глава 01 THE BODY и услуга одновременно. 3/4 спереди, камера 40–50 см, событие — приход пены сверху кадра, отражение потолочной линейки на плечевой линии кузова. Опора — их же wash-foam.jpg.
4. **`st-clean`** — Состояние: вымыта (6 с) — Вторая ступень лестницы. Тот же запертый кадр, что st-arrive, машина чистая, по кузову сходит остаточная вода. Включает складывание в «Гараже»: отметил мойку — машина изменилась.
5. **`interior`** — Химчистка салона (6 с) — Глава 02 THE INTERIOR и услуга. Сверху на валик сиденья, свет вскользь, чтобы читалось направление ворса; событие — тёмная чистая полоса за проходом экстрактора. Рук в кадре нет или они в расфокусе.
6. **`st-gloss`** — Состояние: отполирована (6 с) — Третья ступень и финальный кадр первого экрана (позиция final). Тот же запертый кадр, лак глубокий, линейка потолка отражается жёсткой кромкой.
7. **`headlights`** — Полировка фар (6 с) — Фара включена в затемнённом боксе: было рассеянное мутное свечение, стало резкая светотеневая граница. Кадр срезан по крылу — бампера и места номера нет.
8. **`leather`** — Кожа и пластик (6 с) — Макро по зерну кожи со скользящим светом, переход из серо-пепельного в матово-глубокий. Глянца в финале быть не должно — блестящая кожа читается дёшево.
9. **`complex`** — Комплексная с воском (6 с) — Кузов, диски и салон одним визитом. Событие — финишный горячий воск ложится плёнкой на плечо кузова. Кадр смежный с three-phase, дубли дешевеют.
10. **`reagents`** — Детейлинг от реагентов (6 с) — Единственный работающий ракурс — с пола, 15–20 см, порог и арка на полкадра. С уровня глаз смысл «туда, куда пена не достаёт» не читается вообще.
11. **`chips`** — Сколы и подкраска (4 с) — Самый короткий клип набора. Экстремальный макро, камера неподвижна, движется только аппликатор. Скол 1–2 мм, любое движение камеры его теряет.
12. **`st-bead`** — Состояние: под керамикой (8 с) — Четвёртая, верхняя ступень лестницы. Вода на капоте стянута в шары, каждый ловит линейку точкой света. Единственное место, где слоумо оправдано. Одна из четырёх «тяжёлых» генераций — вода и отражения это названные слабые места Veo.
13. **`ceramic`** — Керамическое покрытие (8 с) — Глава 04 THE PROTECTION и услуга. Статичный макро, капли падают и стягиваются. Тяжёлая генерация; запасной план — снять телефоном в реальном боксе при своём свете.
14. **`polish`** — Полировка кузова (8 с) — Глава 03 THE REFLECTION и услуга. Субъект кадра — не лак, а форма отражённой световой полосы: до прохода машинки размытый ореол, после — жёсткая кромка. Живая движущаяся граница вместо малярной ленты. Тяжёлая генерация, запасной план — телефон.
15. **`rain`** — Антидождь на стёкла (6 с) — На статичной машине не читается: нужна воздуходувка вдоль стекла, чтобы вода срывалась сама. Камера снаружи через стекло. Тяжёлая генерация, запасной план — телефон.
16. **`film`** — Оклейка зон риска (8 с) — Плёнка невидима — видно движущийся под ракелем мокрый вал слип-раствора. Руки в кадре обрезаны по инструменту. Последняя по очереди: и по сложности, и потому что у неё открыт вопрос по цене.
