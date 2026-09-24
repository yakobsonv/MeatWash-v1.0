# Шаг 1 — мастер-кадр машины

**Это первое, что генерируется, и до его утверждения ничего больше делать нельзя.**
Из этого одного файла потом растут все остальные опорные картинки и все 18 клипов.

Машина: mid-1970s заднемоторное купе, тёмно-бордовое, раздутые задние арки, плоский спойлер-поднос.
Марка нигде не называется намеренно — Imagen и Veo ограничивают реальные автобренды,
и если фильтр сработает после утверждения мастера, придётся переделывать всю серию.

## Перед первой генерацией

С НУЛЯ. Это единственная генерация семьи и опорная картинка всех 18 клипов — сначала утверждается она, только потом делается всё остальное.

В Flow: Settings → Media Watermark ВЫКЛЮЧИТЬ до генерации (кадр идёт и первым, и последним кадром петли, метка будет мигать на стыке). Nano Banana Pro, Text-to-Image, 16:9, максимальное доступное разрешение (4K) — из этого файла режутся детальные кропы, на превью 1K их качество не проверяется. Ingredients ПУСТЫЕ: wash-foam.jpg, studio-bay.jpg, wash-rinse.jpg, process-pressure.jpg, wheels.jpg не подкладывать — оттуда лезут чужая машина, человек с копьём и читаемая разметка.

ЧТО ИСПРАВЛЕНО ПРОТИВ ПРЕДЫДУЩЕЙ ВЕРСИИ ПРОМПТА (важно, иначе вернёте себе те же грабли):
1. Из тела промпта убран блок [FRAME EXCLUDES] со словами «no licence plate, no plate recess, frame or mounting bolts» — три упоминания объекта внутри позитивного промпта тянут к нему внимание модели. Вместо него [NOT IN FRAME], где про бампер сказано позитивно, и слова «plate» нет ни разу. Единственная отсечка осталась одной строкой в вечном негативе.
2. Убрана просьба нарисовать одну набухшую каплю, висящую на кромке задней арки. Она (а) противоречила собственному же негативу, где стоят «water beads, droplets in mid-air, falling drop», (б) на масштабе «машина в 2/3 ширины» это объект в несколько пикселей, Nano Banana его либо не рисует, либо размазывает в блик. Каплю рожает Veo в клипе — от мастера нужна только пустая ровная вода и мокрая тёмная кромка арки.
3. Из негатива состояния убрано «tiled floor»: пол клубной комнаты — именно крупноформатная каменная плитка, запрет резал собственную библию мира.
4. Из негатива убраны «modern wide-body sports car» и «whale-tail-less narrow body»: первое сужает раздутые задние арки (нашу главную примету), второе — двойное отрицание, модели читают его наизнанку. Заменено на прямые опознавательные страховки: «narrow rear arches, missing rear deck spoiler, post-1980s body shape».

Сгенерировать 4 варианта, выбрать по приёмке, СКАЧАТЬ файл и проверять по скачанному на 100%, а не по превью. Утверждённый файл сохранить под именем cf-hero-clean и дальше не трогать: cf-hero-dirty делается правкой поверх него, и любая перегенерация мастера обнуляет сходство всей серии.

## Промпт

Вставить целиком, как есть.

```text
[FORMAT] Photorealistic automotive still photograph, 16:9 landscape, one single uncropped frame — no collage, no split screen, no border, no inset. Maximum resolution: this is the master frame of a series and detail crops will be cut out of it later. This is the OPENING frame of a cinema shot, not a finished poster: calm, static, nothing has happened yet, and there is room left in the frame for an event to occur.

[CAMERA] Locked-off tripod. One 50 mm lens at f/4 — normal perspective, never a wide angle. Lens axis 1.0 m above the floor, level, zero tilt, zero roll, horizon dead straight. The whole car stands inside the frame and occupies about two thirds of the frame width, placed slightly right of centre, with clear dark air above the roofline, empty reflecting floor to the left of it, and empty reflecting floor running toward the camera in the lower left. Focus held on the car, which is sharp from nose to tail; the room behind falls away gently. No wide-angle stretch of the near front corner, no exaggerated foreshortening, true photographic proportions.

SUBJECT CAR (use verbatim, never name a manufacturer or model): A mid-1970s rear-engined European sports coupe, compact and unusually wide for its length. Deep oxblood "bull's blood" dark burgundy paint, glossy and freshly polished, almost black in shadow and glowing ruby only where light touches it. Short overhangs, low nose that slopes down between two raised front wings; a single round headlamp stands upright in each raised wing crown, chrome-ringed, with a small amber indicator lens set low beside it. One-piece curved windscreen, thin bright window surrounds, a steeply raked fastback roofline that runs in one unbroken curve down to a short vertical tail. Hugely flared rear wheel arches, much wider than the front arches, with a hard sculpted lip; a flat, wide, tray-shaped rear deck spoiler sits low on the engine lid, rectangular in plan, its outer edge finished with a thick black rubber lip. Black rubber-faced impact bumpers front and rear, with black rubber bellows at each end. Wheels: five-spoke forged alloys, spokes painted satin black, with a polished stepped outer rim lip; low-profile black tyres, rear tyres visibly wider than the fronts. No stripes, no decals, no visible script or emblem anywhere on the body. Immaculate original condition, period-correct, no modern additions.

BUMPERS (use verbatim): The front bumper is one single continuous black rubber beam running wing to wing; its central section is unbroken, smooth and featureless, with nothing fixed onto it and nothing recessed into it — the rubber face stays clean and uninterrupted across the whole centre. The rear bumper is the same single clean rubber beam, equally smooth and unbroken across its centre. The car is turned roughly 40 degrees to the lens axis, the front left corner nearest camera, so the bumper is seen edge-on and in perspective and its face never squares up to the camera.

CLUB ROOM (use verbatim): A private members' club room converted into a car room — a gentlemen's lounge, not a garage. Walls are dark stained oak panelling: raised rectangular panels below a moulded dado rail, tall fielded panels above, flat fluted pilasters breaking the wall into bays, the grain warm brown-black and low in sheen. One bay holds a glazed display cabinet with a dark timber frame; behind the glass, small chromed and brass trophies and figurines catch tiny points of light. A low sideboard of the same dark oak carries more brass and silver objects and a table lamp with a cream pleated fabric shade on a dark ceramic base; a second identical lamp stands further down the room. Framed prints hang salon-style in clusters on the panelling: small black-and-white and sepia images in slim dark frames, faces and contents indistinct. A pair of antique brass wall sconces with small clear bulbs glows hot amber against the wood. One black enamel dome pendant on a long stem hangs from the ceiling over the open floor, its bulb visible, throwing a soft pool of light straight down. A deep-buttoned dark brown leather chesterfield sofa with rolled arms and brass stud trim sits against the right-hand wall, its leather cracked and lived-in. The floor is large-format dark graphite stone tile, semi-polished, with fine grout lines and a mirror-wet sheen that returns long warm reflections of every lamp. No windows, no daylight, no visible ceiling grid, no signage of any kind. The room is mostly darkness: tungsten light pools on wood, brass and floor, and everything more than a few metres from a lamp falls to near-black, with the ceiling and upper corners fully black. A large dark cloth banner hangs on the far right wall — keep it out of frame, or far behind the plane of focus, heavily blurred, reading only as a dark rectangle with a faint warm smudge; no letterform is legible.

[STATE — freshly washed, and nothing is happening yet] The car has just been washed in the bay behind and rolled into the club room to be looked at. The paint is clean, saturated and evenly damp with one fine even sheen across every panel; the black rubber is clean; the wheels are clean right down to the inner rim lip, with no brake dust and no grime in the spokes. On the stone tile directly under and around the car lies a shallow film of water the car carried in with it — perfectly flat and mirror-still, holding an undisturbed warm reflection of the lamps, with no ripples, no rings, no expanding circles and no splashes anywhere in it. Nothing on the bodywork is moving: no beads running, no rivulets, no water sheeting down a panel, no drips caught in mid-air, no foam, no suds, no spray, no steam and no vapour anywhere in the frame. The air is completely clear.

[LIGHT & GRADE] One single colour temperature only: warm tungsten between 2900 K and 3100 K. No cold source, no daylight, no white LED, no blue or green fill anywhere. A few warm pools of light — the ceiling pendant, the pair of brass sconces, the table lamps — sit in near-blackness; the fall-off into shadow is two to three stops, the ceiling and the upper corners go fully black, and there is almost no fill, so the shadows stay warm brown-black rather than grey. On the bodywork the overhead light registers as TWO long continuous highlight streaks — one running the length of the shoulder line, one along the roof — each a single unbroken band, never doubled, never split into segments, with no starburst and no sparkle. Under the car lies a long warm mirrored reflection on the stone and a soft anchoring shadow. The palette is limited to four colours only: oxblood, brass and amber, dark oak brown, graphite black — no teal, no green, no cold grey. Saturation is restrained everywhere except the red bodywork, which is the only carrier of colour in the picture. Grade: soft S-curve, deep but not crushed blacks with a warm bias, gentle highlight roll-off, a faint amber halo on the brightest speculars, fine even grain. Restrained, expensive, editorial — not advertising-glossy: no HDR, no bloom, no lens flare, no vignette theatrics.

[BANNER AND TEXT] The large dark cloth banner on the far right wall is either completely out of frame or far behind the plane of focus and heavily blurred, reading only as a dark rectangle with a faint warm smudge on it — no letterform, no word, no shape of a letter is legible. There is no readable text anywhere in the picture: not on the car, not on the tyres, not on the walls, not in the framed prints, not on the floor.

[COMPOSITION FOR VIDEO — leave the event somewhere to happen] In the clip that follows, one drop lets go of the lowest point of the rear wheel arch and sends a single ring across the reflection of the lamps. That event belongs to the clip and NOT to this frame. In this frame the water beneath and behind the rear wheel is an empty, flat, unbroken mirror with nothing in it yet, and the lowest edge of the rear arch lip simply reads as wet and dark — do not draw a separate droplet hanging off it, and do not draw a droplet anywhere else. Keep the floor around the car clear and simple, keep empty dark air above the roofline, keep the lower left quarter of the frame as plain reflecting floor. Do not render the later state either: no ring already spreading, no disturbed water, no drop in the air.

[NOT IN FRAME] Across the whole front of the car and the whole rear of the car the rubber beams stay clean, continuous and featureless: nothing rectangular, nothing pale, nothing recessed, nothing bolted on, no bracket, no dark rectangular patch. No digits and no characters anywhere in the picture; no readable text of any kind; no badge, emblem, script or signage; no watermark; no people, figures, hands, or reflections of a crew or a camera in the paint; no tools, hose, hose reel, trolley or bottles; no second vehicle; no windows and no daylight; no painted line or marking on the floor; no motion blur; no camera tilt.
```

## Negative

Если в интерфейсе нет отдельного поля — дописать в конец промпта только ЧАСТЬ 1.

```text
ЧАСТЬ 1 — ВЕЧНАЯ (переносится во все кадры и клипы серии без изменений):
ETERNAL NEGATIVE (every shot): licence plate, any text, letters, numbers, captions, watermark, signage, banner lettering, readable logo, manufacturer badge, emblem, brand mark, painted floor lines or markings, stickers, people, faces, hands, arms, crew, reflections of a crew or camera, tripod, hose reel, tool trolley, bottles, compressor, second car, wide-angle distortion, fisheye, warped body proportions, tilted horizon, HDR glow, plastic CGI render, video-game look, oversaturation, teal shadows, blue or green cast, blown highlights, flat low-contrast grade, daylight, window light, noise, low resolution, extra wheels, extra headlamps, deformed wheels, melted reflections.

ЧАСТЬ 2 — СОСТОЯНИЕ ИМЕННО ЭТОГО КАДРА (в клипы мойки НЕ переносить, иначе запретит саму мойку):
STATE NEGATIVE (cf-hero-clean only): foam, suds, soap, snow foam, shampoo, running water, water sheeting down the body, rivulets, streaks of water on the paint, water beads, droplets in mid-air, falling drop, spray, splash, ripples, rings or expanding circles in the floor water, disturbed water, steam, vapour, mist, fog, smoke, dirt, road grime, salt haze, dust, brake dust in the spokes, mud, swirl marks, polishing haze, water spots, drying marks, scratches, dents, rust, snow, street, outdoors, industrial workshop, concrete wash bay, public car wash, drain grate, bright cherry or scarlet red paint, metallic flake paint, chrome bumpers, modern car, post-1980s body shape, narrow rear arches, missing rear deck spoiler, broken or segmented highlight streak.

(Снято против прошлой версии: «tiled floor» — пол клуба и есть каменная плитка; «modern wide-body sports car» — сужал раздутые задние арки; «whale-tail-less narrow body» — двойное отрицание, читается наизнанку; «motion blur» — уже стоит в вечной части.)
```

## Приёмка — проверить по скачанному файлу, не по превью

1. Машина целиком в кадре, занимает ~2/3 ширины, чуть правее центра; над крышей есть пустой тёмный воздух, левый нижний угол — голый отражающий пол.
2. Разворот ~40°, ближе всего передний левый угол; плоскость переднего бампера НЕ смотрит в камеру.
3. Оба бампера — сплошные чёрные резиновые балки: в центре ничего не закреплено и ничего не утоплено, нет рамки, прямоугольного углубления, болтов, тёмного прямоугольного пятна и цифр.
4. Перспектива нормальная: ближний передний угол не раздут, кузов не гнётся — если раздут, это 35 мм и кадр в брак.
5. Приметы на месте: круглые фары в гребнях поднятых крыльев с хромкольцом и янтарным поворотником, раздутые задние арки шире передних, плоский спойлер-поднос с чёрной резиновой губой, пятиспицевые чёрные диски со ступенчатой полированной кромкой, задние шины шире передних.
6. Цвет — тёмно-бордовая «бычья кровь»: не алый, не коричневый, не чёрный; красный кузов — единственный носитель цвета в кадре.
7. Свет один тёплый (2900–3100K); потолок и верхние углы в нуле; тени тёплые чёрно-коричневые; ни синего, ни зелёного, ни дневного.
8. На борту и по крыше — по одной длинной непрерывной полосе блика, не разорванной на сегменты и не удвоенной.
9. Под машиной длинное тёплое зеркальное отражение в полу и мягкая тень-якорь.
10. Ничего не происходит: ни пены, ни пара, ни распыла, ни капель в воздухе, ни висящей капли; вода на полу ровная, без ряби и кругов.
11. Читаемого текста нет нигде; знамя MEATWASH вне кадра или тёмным нечитаемым пятном в расфокусе.
12. Нет людей, рук, отражения съёмочной группы в лаке; нет шлангов, катушек, тележек, бутылок, второй машины; нет разметки на полу.
13. Файл СКАЧАН и проверен на 100%: колесо, фара, кромка арки и линия плеча держат резкость — из них режутся кропы; по превью 1K не принимать.
14. Watermark выключен: на кадре нет метки Flow (он идёт и первым, и последним кадром петли).

## Если вышло не то

Если на переднем или заднем бампере всё же вылезла рамка / прямоугольное углубление / тёмное прямоугольное пятно — НЕ добавлять в промпт новых слов про номер и не плодить синонимы в негативе (именно это его и притягивает). Вместо этого: (1) довернуть машину — заменить «roughly 40 degrees» на «roughly 45 degrees» в блоке BUMPERS и сгенерить заново; (2) если не помогло — 50 градусов и камеру чуть левее, чтобы передняя балка ушла в ещё более острый ракурс. Позитивную формулировку про сплошную балку не сокращать.

Если ближний передний угол раздут, а кузов «гнётся» — модель ушла в широкоугольник: вынести «One 50 mm lens at f/4, normal perspective, no wide angle» в самое начало промпта, сразу после [FORMAT].

Если машина ушла в треть ширины — повторить «about two thirds of the frame width» ещё раз в конце [CAMERA]; кадр с мелкой машиной не принимать, кроп колеса из него будет мыльным.

Если полоса блика разорвалась на сегменты или удвоилась — убрать из [LIGHT & GRADE] упоминание настольных ламп как источников на кузове и оставить формулировку «TWO long continuous highlight streaks» отдельным предложением в конце абзаца.

Если в кадр лезет читаемая надпись на знамени — дописать в [BANNER AND TEXT] «the banner is completely out of frame» и убрать альтернативу с расфокусом.

Если все 4 варианта плохи — чинить промпт и перегенерировать МАСТЕР сейчас, пока ничего вниз по цепочке не сделано. После утверждения мастера перегенерация запрещена: она обнуляет сходство всей серии, включая cf-hero-dirty и все кропы.

Одной каплей на кромке арки мастер не занимается принципиально: на масштабе 2/3 ширины Nano Banana рисует её либо бликом, либо не рисует вовсе. Каплю делает Veo в клипе; если капля нужна статичной картинкой — это отдельный близкий кроп, а не мастер.
