# Schritt 85 — Bericht

## Gebaut

**Der Hinweis geht durch die Übersetzung.** In `client.js:10136` steht jetzt
`h('span', …, uebersetze(darfHinweis))` statt der rohen Ausgabe. `darfGrund`
ging an zwei Stellen durch `uebersetze` (`client.js:10134`, `10204`); in den
Board-Einstellungen stand er aber roh (`client.js:2382`). Auch dort steht
jetzt `uebersetze(String(daten.darfGrund || …))`, sodass der Grund aus
`/board/data` überall in der Sprache des Dashboards erscheint.

**Die neun Sätze stehen in allen sieben Wörterbüchern.** Die acht Konstanten
`GRUND_*`/`HINWEIS_*` kommen aus `rechte.js`, die Vorgabe
`Kein Schreibrecht für dieses Board.` aus `index.js:2396`. Ein einmaliges
Skript hat die Konstanten importiert und die Vorgabe aus dem Quelltext
gelesen; in die Wörterbücher wurden die deutschen Schlüssel nicht abgetippt,
sondern als Werte der Konstanten übernommen. Danach trägt jedes Wörterbuch
614 statt 605 Schlüssel, `de.json` bleibt leer:

| Wörterbuch | Schlüssel |
|---|---:|
| `texte/en.json` | 614 |
| `texte/es.json` | 614 |
| `texte/fr.json` | 614 |
| `texte/ru.json` | 614 |
| `texte/ja.json` | 614 |
| `texte/ar.json` | 614 |
| `texte/zh.json` | 614 |
| `texte/de.json` | 0 |

**Die Prüfung.** `test/nach85.mjs` liest die acht Konstanten und die Vorgabe
über den neuen Helfer `test/rechte-texte.js` und verlangt je Wörterbuch:
Vorhandensein, nicht-leeren Wert, eine vom Deutschen verschiedene Fassung
und dieselben Platzhalter. Dazu kommen die Schlüsselzahl 614, die Gegenprobe
(ein aus `en` entfernter Satz macht die Prüfung rot) und die Client-Seite
(`uebersetze(darfHinweis)`, kein rohes `darfHinweis`, `daten.darfGrund` in
den Einstellungen durch `uebersetze`).

**Die Schlüsselzahlen.** In `test/nach66.mjs`, `test/nach82.mjs`,
`test/nach83.mjs`, `test/nach84.mjs` und `test/markt48.mjs` sind die
Wörterbuchschlüssel auf 614 gehoben. `grep -rn "605" test/` findet keine
Stelle mehr.

## Die neun Sätze

### 1. `Keine Anmeldung: es fehlt ein Token.`

- **en:** Not signed in: a token is missing.
- **es:** Sin sesión: falta un token.
- **fr:** Aucune connexion : il manque un jeton.
- **ru:** Нет входа: отсутствует токен.
- **ja:** 未ログイン: トークンがありません。
- **ar:** لا تسجيل دخول: الرمز مفقود.
- **zh:** 未登录：缺少 Token。

### 2. `Melde dich in den Einstellungen unter GitHub mit einem Token an. Ohne Anmeldung bleibt dieses Board nur lesbar.`

- **en:** Sign in with a token under GitHub in Settings. Without signing in, this Board stays read-only.
- **es:** Inicia sesión con un token en Ajustes, en GitHub. Sin sesión, este Board solo se puede leer.
- **fr:** Connectez-vous avec un jeton sous GitHub dans les réglages. Sans connexion, ce Board reste en lecture seule.
- **ru:** Войдите с токеном в настройках, раздел GitHub. Без входа этот Board останется только для чтения.
- **ja:** 設定のGitHubでトークンを使ってログインしてください。ログインしない場合、このBoardは読み取り専用のままです。
- **ar:** سجّل الدخول برمز في الإعدادات ضمن GitHub. بدون تسجيل الدخول يبقى هذا Board للقراءة فقط.
- **zh:** 请在设置中的 GitHub 下使用 Token 登录。未登录时，此 Board 保持只读。

### 3. `Kein Schreibrecht: der angemeldete Login darf dieses Repository nicht beschreiben.`

- **en:** No write permission: the signed-in login may not write to this repository.
- **es:** Sin permiso de escritura: el usuario con sesión iniciada no puede escribir en este repositorio.
- **fr:** Pas de droit d'écriture : l'identifiant connecté ne peut pas écrire dans ce dépôt.
- **ru:** Нет права записи: вошедший логин не может записывать в этот репозиторий.
- **ja:** 書き込み権限がありません: ログイン中のアカウントはこのリポジトリに書き込めません。
- **ar:** لا صلاحية كتابة: اسم الدخول المسجّل لا يستطيع الكتابة في هذا المستودع.
- **zh:** 没有写入权限：已登录的账号不能写入此仓库。

### 4. `Nur wer bei GitHub schreiben darf, kann dieses Board ändern. Ein Token mit Schreibrecht hilft.`

- **en:** Only someone allowed to write on GitHub can change this Board. A token with write permission helps.
- **es:** Solo quien puede escribir en GitHub puede modificar este Board. Un token con permiso de escritura ayuda.
- **fr:** Seule une personne autorisée à écrire sur GitHub peut modifier ce Board. Un jeton avec droit d'écriture aide.
- **ru:** Изменить этот Board может только тот, кому разрешено писать на GitHub. Поможет токен с правом записи.
- **ja:** GitHubで書き込みが許可されている人だけがこのBoardを変更できます。書き込み権限のあるトークンが必要です。
- **ar:** لا يمكن تعديل هذا Board إلا لمن يُسمح له بالكتابة على GitHub. ويساعد رمز بصلاحية الكتابة.
- **zh:** 只有在 GitHub 上有写入权限的人才能修改此 Board。带有写入权限的 Token 会有帮助。

### 5. `Das Schreibrecht ist nicht prüfbar: GitHub ist nicht erreichbar.`

- **en:** Write permission cannot be checked: GitHub cannot be reached.
- **es:** No se puede comprobar el permiso de escritura: no se puede acceder a GitHub.
- **fr:** Le droit d'écriture ne peut pas être vérifié : GitHub est injoignable.
- **ru:** Право записи проверить нельзя: GitHub недоступен.
- **ja:** 書き込み権限を確認できません: GitHubに接続できません。
- **ar:** لا يمكن التحقق من صلاحية الكتابة: GitHub غير متاح.
- **zh:** 无法检查写入权限：GitHub 无法连接。

### 6. `Prüfe die Verbindung und lade das Board neu; solange bleibt es nur lesbar.`

- **en:** Check the connection and reload the Board; until then it stays read-only.
- **es:** Comprueba la conexión y recarga el Board; mientras tanto, solo se puede leer.
- **fr:** Vérifiez la connexion et rechargez le Board ; jusque-là, il reste en lecture seule.
- **ru:** Проверьте соединение и перезагрузите Board; до этого он останется только для чтения.
- **ja:** 接続を確認してBoardを再読み込みしてください。それまでは読み取り専用のままです。
- **ar:** تحقّق من الاتصال وأعد تحميل Board؛ إلى أن يحدث ذلك يبقى للقراءة فقط.
- **zh:** 请检查连接并重新加载 Board；在此之前它保持只读。

### 7. `Die Anmeldung ist nicht prüfbar: der Token wird von GitHub abgewiesen.`

- **en:** The sign-in cannot be checked: GitHub rejects the token.
- **es:** No se puede comprobar la sesión: GitHub rechaza el token.
- **fr:** La connexion ne peut pas être vérifiée : GitHub refuse le jeton.
- **ru:** Вход проверить нельзя: GitHub отклоняет токен.
- **ja:** ログインを確認できません: GitHubがトークンを拒否します。
- **ar:** لا يمكن التحقق من تسجيل الدخول: GitHub يرفض الرمز.
- **zh:** 无法检查登录状态：GitHub 拒绝该 Token。

### 8. `Trage in den Einstellungen unter GitHub einen gültigen Token ein.`

- **en:** Enter a valid token under GitHub in Settings.
- **es:** Introduce un token válido en Ajustes, en GitHub.
- **fr:** Saisissez un jeton valide sous GitHub dans les réglages.
- **ru:** Введите действительный токен в настройках, раздел GitHub.
- **ja:** 設定のGitHubで有効なトークンを入力してください。
- **ar:** أدخل رمزًا صالحًا في الإعدادات ضمن GitHub.
- **zh:** 请在设置中的 GitHub 下输入有效的 Token。

### 9. `Kein Schreibrecht für dieses Board.`

- **en:** No write permission for this Board.
- **es:** Sin permiso de escritura para este Board.
- **fr:** Pas de droit d'écriture pour ce Board.
- **ru:** Нет права записи для этого Board.
- **ja:** このBoardには書き込み権限がありません。
- **ar:** لا صلاحية كتابة لهذا Board.
- **zh:** 此 Board 没有写入权限。

Die Begriffe folgen den vorhandenen Wörterbüchern: **Board** bleibt in allen
sieben Sprachen stehen, **Repository** wird en/es/fr/ru/ja/ar/zh als
Repository/Repositorio/Dépôt/Репозиторий/リポジトリ/المستودع/仓库 geführt,
**Station** als Station/Estación/Station/Станция/ステーション/محطة/站点. Für
„Token“ bleibt es bei Token/Jeton/токен/トークン/الرمز/Token, wie schon in den
bestehenden Sätzen.

## Prüfung

```
$ node --check client.js
(keine Ausgabe, Rückgabewert 0)

$ node --test test/nach85.mjs
ok Übersetzungen: 9 Sätze des Servers in sieben Wörterbüchern, 614 Schlüssel je Wörterbuch, de.json leer, darfHinweis durch uebersetze, Gegenprobe rot
ℹ tests 1
ℹ pass 1
ℹ fail 0

$ node --test test/markt48.mjs
ok Sprache: Deutsch als Ausgang, unbekannt leer, 400/405, Sprachdateien deckungsgleich
ℹ tests 1
ℹ pass 1
ℹ fail 0

$ node --test test/*.mjs
ℹ tests 81
ℹ pass 81
ℹ fail 0
```

Die 80 bestehenden Prüfungen und die neue Datei `test/nach85.mjs` ergeben
81 Prüfungen; die Vorgabe „80/80“ war der Stand vor diesem Schritt.

**Gegenprobe, wirklich ausgeführt.** Einmal `Keine Anmeldung: es fehlt ein
Token.` aus `texte/en.json` entfernt: `test/nach85.mjs` meldet
`fail 1`; nach dem Zurückspielen wieder `pass 1`. Ebenso die alte rohe
Zeile `h('span', …, darfHinweis)` eingesetzt: `fail 1`, danach
wiederhergestellt und wieder `pass 1`.

**Weitere Prüfungen.** Alle sieben Wörterbücher tragen dieselben 614
Schlüssel (keine fehlenden, keine überzähligen), `de.json` ist leer. Die
Platzhalter der neun Sätze sind leer und bleiben leer. `test/nach85.mjs`
prüft die neun Sätze zeichengleich gegen die Quelle, nicht gegen eine
abgeschriebene Liste; nur die Vorgabe Satz 9 ist im Test zusätzlich als Text
genannt, wie in der Aufgabe erlaubt.

## Abweichungen

1. **81 statt 80 Prüfungen.** Die neue Datei `test/nach85.mjs` kommt zur
   Suite hinzu; die 80 bestehenden Prüfungen laufen weiter. Ich habe keine
   alte Prüfung entfernt, nur um die Zahl zu halten.

2. **Die Wörterbuchprüfungen der alten Schritte mussten mitziehen.** `nach66`,
   `nach82`, `nach83`, `nach84` und `markt48` vergleichen die Wörterbücher
   mit den Client-Schlüsseln und hätten die neun Sätze sonst als „unbekannte
   Schlüssel“ abgewiesen. Sie lesen die neun Sätze jetzt über denselben
   Helfer `test/rechte-texte.js` und zählen 614. Der Helfer liegt als `.js`
   neben den Prüfungen, damit `node --test test/*.mjs` ihn nicht als eigene
   Prüfung ausführt.

3. **Orte im Client.** Die rohe Hinweisstelle lag nach den späteren Schritten
   bei `client.js:10136`, nicht bei 9921 wie im Schritt notiert. Beim Audit
   kam eine zweite rohe Stelle hinzu: `daten.darfGrund` in den
   Board-Einstellungen (`client.js:2382`) stand ohne `uebersetze` da.
   Geändert wurden nur diese beiden Stellen.

4. **Kein unabhängiges Gegenlesen.** Die Übersetzungen habe ich beim
   Eintragen Satz für Satz geprüft; ein zweiter Prüfer hat sie nicht
   gegengelesen. Die Qualität einer Übersetzung kann kein Test messen — das
   bleibt eine offene Grenze.

5. **`index.js` und `rechte.js` sind unverändert.** Ihre Zeitstempel liegen
   vor diesem Schritt; die deutschen Sätze bleiben die Schlüssel.

## Audit der sichtbaren Reste

**Behoben:**

- `texte/ru.json`: `dein-name` → `ваше-имя`, `dein-name auf GitHub` →
  `ваше-имя на GitHub`.
- `texte/zh.json`: `dein-name` → `你的名字`, `dein-name auf GitHub` →
  `你在 GitHub 上的名字`, `/pfad/zum/workflow` → `/path/to/workflow`.
- `texte/ar.json`: `dein-name` → `اسمك`, `dein-name auf GitHub` →
  `اسمك على GitHub` (vorher „your-name على GitHub“).
- `texte/zh.json`: `Token gelöscht.` → `Token 已删除。` statt `令牌已删除。`;
  die übrigen zh-Sätze sagen sonst ebenfalls „Token“.
- `client.js:2382`: Der Grund in den Board-Einstellungen stand roh da
  (`String(daten.darfGrund || …)`); er geht jetzt durch `uebersetze`.
  Das war eine sichtbare Stelle außerhalb der neun Sätze und ohne Grund.

**Bleibt, mit Grund:**

- **Serverfehler in `error`.** `index.js` trägt weitere deutsche Texte, die
  nicht zu den neun Sätzen gehören, zum Beispiel
  `Kein Schreibrecht für dieses Vorhaben.` (`index.js:3201`) und
  `Kein Schreibrecht an <repo>` (`index.js:4780`). Sie stehen in
  Fehlerantworten; die Aufgabe nennt neun Sätze, und `error`-Texte sind im
  Schritt ausdrücklich als Rest benannt. Würden sie sichtbar, bräuchten sie
  eigene Schlüssel.
- **Andere Servertexte als die neun.** `darfGrund` und `darfHinweis` werden
  nur übersetzt, wenn der Server zeichengleich einen der neun Sätze schickt.
  Ein künftiger neuer Grund fällt auf Deutsch zurück, bis er im Wörterbuch
  steht; `test/nach85.mjs` liest die aktuellen Konstanten und wird bei einer
  Änderung rot.
- **Modellaufträge und technische Werte** bleiben deutsch, wie in
  `test/nach66.mjs` mit 26 Ausnahmen und Begründung festgehalten. Der
  Schritt fügt keine sichtbare Zeichenkette außerhalb von `uebersetze` hinzu.
- **Beispielpfade.** `ja` und `en` tragen bei `/pfad/zum/workflow` weiter
  `/path/to/workflow`; das ist Englisch, kein deutscher Rest, und als
  Beispielpfad vertretbar. Die übrigen Sprachen haben eigene Pfade.

## Offene Reste

- Ein unabhängiges Gegenlesen der 63 Übersetzungen (neun Sätze × sieben
  Sprachen) steht aus; hier wurde nur stichprobenartig geprüft.
- Die `error`-Texte des Servers außerhalb der neun Sätze bleiben deutsch
  (siehe Audit). Sie sind nicht Teil dieses Schritts.
- Die englische Seite wurde nicht im Browser besichtigt. Geprüft ist die
  Stelle im Quelltext und der Wörterbuchwert; `uebersetze` liest genau diese
  Zuordnung. Der laufende Web-Prozess liefert die Wörterbücher bei jeder
  Anfrage neu (`/board/sprache`), ein Neustart ist für diese Änderung nicht
  nötig.

## Geänderte Dateien

- `client.js` (zwei Stellen: 2382 und 10136)
- `texte/en.json`, `texte/es.json`, `texte/fr.json`, `texte/ru.json`,
  `texte/ja.json`, `texte/ar.json`, `texte/zh.json` (je neun Schlüssel,
  plus Audit-Werte)
- `test/nach85.mjs` (neu)
- `test/rechte-texte.js` (neu, Helfer)
- `test/nach66.mjs`, `test/nach82.mjs`, `test/nach83.mjs`, `test/nach84.mjs`,
  `test/markt48.mjs` (Schlüsselzahl und Server-Sätze)

Keine Commits, keine Installation. `index.js` und `rechte.js` blieben
unberührt.
