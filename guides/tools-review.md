# Огляд 47 інструментів із рілсів і чатів

**Українська** | [English](en/tools-review.md)

Усе це прийшло з рілсів, відео і чатів за один день. Ми пройшли кожен пункт і в систему взяли лише кілька. Решта не обов'язково погана: вона або дублює те, що вже є, або потрібна під задачу, якої зараз нема.

**Як читати вердикт:**
- ✅ поставили собі
- 📌 корисне під конкретну задачу, не ставити наперед
- 📖 матеріал для навчання, ставити нічого не треба
- ❌ нам не підійшло, причина поруч

**Що перевірено (30.10.2026):** автор, ліцензія, остання активність, README. Код скіла-кандидата ми читали, а записи про вразливості дивились в [OSV](https://osv.dev) лише для пакетів. Для більшості пунктів це огляд, а не аудит. Зірки не доказ, чек-лист нижче. Кількість зірок тут не наводимо навмисно.

**Перед установкою будь-чого:** що запускається під час встановлення, версія закріплена на коміті, тільки в проєкт, а не глобально. Скіл це текст плюс скрипти, які агент виконує з твоїми правами.

## Скіли і фреймворки для Claude Code

| Що | Вердикт | Кому і коли | Застереження |
|---|---|---|---|
| [cloudflare/security-audit-skill](https://github.com/cloudflare/security-audit-skill) | ✅ | аудит власного коду з окремою перевіркою кожної знахідки | багато агентів, тому дорого. Без пісочниці сам не запускає чужий код. Ми скопіювали на закріпленому коміті, без `npx` |
| [obra/superpowers](https://github.com/obra/superpowers) | 📌 | хто хоче готовий процес «спека → план → TDD» | хук вантажить текст у кожну сесію, логотип за замовчуванням іде з їхнього сервера (є в README). Якщо процес уже є, бери окремий скіл |
| [msitarzewski/agency-agents](https://github.com/msitarzewski/agency-agents) | 📌 | бібліотека ролей: читай 1-2 під задачу | сотні файлів, частина під китайські платформи. Ми взяли одну: Discovery Coach. Застосунок і інсталятор не ставили |
| [aicodeking/leanrea](https://github.com/aicodeking/leanrea) | 📌 | розібрати, як влаштована програма. Це реверс-інжиніринг, а не клон сайту | заточений під macOS, бенчмарк авторський, один контриб'ютор. Розбирай лише те, що маєш право |
| [latent-spaces/brag](https://github.com/latent-spaces/brag) | 📌 | коротке відео про свій проєкт для LinkedIn чи портфоліо. `brag-slim` це один файл | потрібні ffmpeg і час на рендер |
| [Oldcircle/geo-sleuth](https://github.com/Oldcircle/geo-sleuth) | 📌 | знайти місце зйомки за фото | лише свої фото: ця сама техніка знаходить, де живе людина |
| [unclebob/swarm-forge](https://github.com/unclebob/swarm-forge) | 📌 ідея | варто вкрасти думку: QA перевіряє незалежно від автора коду | `zsh` + `tmux`, не для Windows, шість агентів, у репо нема ліцензії. Автор сам попереджає: токен SWARM купувати не треба |
| [google-research/harness2](https://github.com/google-research/harness2) | 📌 ідея | дослідникам: агент покращує власну обв'язку на задачі | Linux, Vertex AI, мережа без обмежень, не офіційний продукт Google |
| [ruvnet/ruflo](https://github.com/ruvnet/ruflo) | ❌ | | величезна надбудова, старт через `@latest`, моди працюють із правами акаунта |
| [affaan-m/ECC](https://github.com/affaan-m/ECC) | ❌ | | 71 агент, 314 скілів, хуки. Лягає поверх твоєї системи, а не поруч із нею |
| [morluto/rea](https://github.com/morluto/rea) | ❌ | | 207 інструментів MCP. Якщо реверс справді потрібен, спершу `leanrea` |
| [nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) | ❌ | | дублює [taste-skill і impeccable](../README.md#-що-ми-ставили-собі-і-що-відхилили), які вже стоять |

## Конектори

| Що | Вердикт | Кому і коли | Застереження |
|---|---|---|---|
| [Playwright](https://github.com/microsoft/playwright-mcp) | 📌 | керувати браузером і тестувати сайт | сам README радить кодинг-агентам CLI зі скілами замість MCP: так менше токенів |
| [Firecrawl](https://docs.firecrawl.dev/mcp-server) | 📌 | сторінка в чистий текст | [1 000 кредитів на місяць](https://www.firecrawl.dev/pricing) безкоштовно |
| [PostHog](https://posthog.com/docs/model-context-protocol) | 📌 | коли є продукт із користувачами | підключення безкоштовне, але частина інструментів платна |
| [Perplexity](https://docs.perplexity.ai/getting-started/pricing) | ❌ | | платний за запит, а пошук у Claude Code вже є |
| [Composio](https://docs.composio.dev/docs/authentication) | ❌ | | за замовчуванням OAuth-токени твоїх акаунтів лежать у них. Клієнтські акаунти туди не підключай |

## Скрапінг і браузерні агенти

| Що | Вердикт | Кому і коли | Застереження |
|---|---|---|---|
| [crawl4ai](https://github.com/unclecode/crawl4ai) | 📌 | локально отримати Markdown із сайтів | багато записів у OSV, зокрема критичні для Docker API. Свіжа закріплена версія, порт не назовні |
| [browser-use](https://github.com/browser-use/browser-use) | 📌 | агент, якому потрібен браузер | модель іде на кожен крок, тож дорожче за прямий скрипт |
| [Maxun](https://github.com/getmaxun/maxun) | 📌 | скрапер без коду на своєму сервері | AGPL |
| [ScrapeGraphAI](https://github.com/ScrapeGraphAI/Scrapegraph-ai) | 📌 | вказуєш сайт і поля, LLM їх витягує | платиш за модель на кожній сторінці. Збір лідів це персональні дані: RODO |

## Платформи і свій сервер

| Що | Вердикт | Кому і коли | Застереження |
|---|---|---|---|
| [Supabase](https://github.com/supabase/supabase) | 📌 | коли Google Sheets уже замало: Postgres, авторизація, API | |
| [Stirling-PDF](https://github.com/Stirling-Tools/Stirling-PDF) | 📌 | PDF локально, без онлайн-сервісів | частина коду пропрієтарна |
| [Dify](https://github.com/langgenius/dify) | 📌 | RAG і чат-боти на своєму сервері | ліцензія обмежує мультитенант |
| [Voicebox](https://github.com/jamiepine/voicebox) | 📌 | локальна озвучка і клон голосу | лише свій голос або з письмової згоди |
| [Coolify](https://github.com/coollabsio/coolify) | ❌ для нас | якщо сервера ще нема, це зручний старт | у нас compose + Caddy, панель додала б ще один вхід із доступом до сервера |
| [OpenHands](https://github.com/OpenHands/OpenHands) | ❌ для нас | постійно ввімкнені агенти на сервері | задачі нема |
| [Open WebUI](https://github.com/open-webui/open-webui) | ❌ для нас | свій чат для локальних моделей | дуже багато записів в OSV, своя ліцензія |
| [Langflow](https://github.com/langflow-ai/langflow) | ❌ для нас | візуальний конструктор агентів | багато записів в OSV, зокрема RCE. Не виставляй у мережу |

## Запуск безкоштовно

| Що | Вердикт | Для чого |
|---|---|---|
| [free-for-dev](https://github.com/ripienaar/free-for-dev) | 📖 | список сервісів із безкоштовним тарифом |
| [awesome-selfhosted](https://github.com/awesome-selfhosted/awesome-selfhosted) | 📖 | що можна підняти самому |
| [FreeDomain](https://github.com/DigitalPlatDev/FreeDomain) | 📌 | піддомен для демо. Для бізнесу купи свій домен, він належить тобі |

## Навчання

| Що | Вердикт | Для чого |
|---|---|---|
| [freeCodeCamp](https://github.com/freeCodeCamp/freeCodeCamp) | 📖 | безкоштовні сертифікати, серед них Python і Relational Databases, плюс англійська для розробників |
| [system-design-primer](https://github.com/donnemartin/system-design-primer) | 📖 | підготовка до співбесід із проєктування систем |
| [project-based-learning](https://github.com/practical-tutorials/project-based-learning) | 📖 | вчитись через свій проєкт |
| [build-your-own-x](https://github.com/codecrafters-io/build-your-own-x) | 📖 | зібрати технологію з нуля, щоб зрозуміти |
| [developer-roadmap](https://github.com/nilbuild/developer-roadmap) | 📖 | карта навичок під роль |
| [free-programming-books](https://github.com/EbookFoundation/free-programming-books) | 📖 | безкоштовні книжки, є розділ українською |

## Дві ідеї без репо

**Opus планує, Sonnet пише, Haiku шукає.** Вбудований `opusplan` вмикає Opus лише в режимі плану, решта роботи йде на Sonnet. Спершу подивись `/usage`: якщо в ліміти не впираєшся, якість важливіша. Дешевшу модель варто ставити точково: у скілі чи субагенті є поле `model`, а `context: fork` запускає скіл в окремому потоці.

**Модель має пам'ятати міркування між кроками.** Для Claude API це правило, а не порада: у циклі з інструментами блоки міркувань останнього ходу треба повертати без змін, інакше запит падає. Це важливо, коли пишеш власного агента. Claude Code робить це сам.

## Відео: як не впиратись у ліміти

[Austin Marchese](https://www.youtube.com/watch?v=SFh6MMe-XcM), 18 хв, англійською, є реклама. Корисні звички: `/usage` показує, куди йдуть ліміти, `/context` у порожньому чаті показує, що вантажиться завжди, `/clear` при зміні задачі, вимкнути непотрібні MCP. Скіли зі скриптами замість повторюваної роботи моделі теж добрий рецепт. Обіцянку 60-90% перевір на своїх командах: [RTK](../README.md#-що-ми-ставили-собі-і-що-відхилили) на наших замірах дав 0,25%. Заміну тексту картинками ми не беремо: це обхід з ризиком втратити точність.
