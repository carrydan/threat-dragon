## Об устройстве репозитория

Репозиторий состоит из следующих основных компонентов:

- **cupcake/**: Содержит иконки и изображения, используемые в проекте.
- **docs/**: Документация проекта, включая файлы для сайта, такие как `_config.yml` и страницы Markdown.
- **td.server/**: Серверная часть, содержащая исходный код и тесты.
- **td.vue/**: Исходный код пользовательского интерфейса на Vue.js, а также тесты и конфигурационные файлы.
- **ThreatDragonModels/**: Примеры моделей угроз, представленных в формате JSON.
- **node_modules/**: Зависимости Node.js, установленные для проекта.
- **Dockerfile**: Инструкция для создания Docker-образа.
- **package.json и package-lock.json**: Файлы, содержащие информацию о зависимостях и скриптах для работы с проектом.
- **README.md**: Основная документация проекта.
- **contributing.md, code_of_conduct.md, security.md**: Документы, описывающие правила для контрибьюторов, кодекс поведения и политику безопасности.

Эти компоненты вместе обеспечивают работу приложения Threat Dragon, включая серверную часть, клиентский интерфейс и документацию.

## CI/CD Pipeline

Автоматизированный процесс сборки и деплоя настроен с помощью GitHub Actions. Пайплайн запускается при пушах и pull request в ветки `main` и `my-diplom`.

### Этапы пайплайна:

1. **Сборка (`build`):**

   - **Установка зависимостей и тестирование:**
     - Устанавливаются зависимости и запускаются тесты для фронтенда и бэкенда.
   - **Сборка приложения:**
     - Фронтенд и бэкенд собираются для подготовки к деплою.
   - **Сборка и публикация Docker-образа:**
     - Создается Docker-образ приложения и публикуется в Docker Hub под именем `carrydan/threat-dragon:${{ github.sha }}`.

2. **Деплой (`deploy`):**

   - **Условие запуска:**
     - Этап деплоя выполняется только при пуше в ветку `main`.
   - **Деплой в Kubernetes:**
     - Используется `kubectl` для обновления образа в Kubernetes-кластере.
     - Образ обновляется на последнюю версию, собранную в этапе `build`.

### Настройка секретов:

- **`DOCKER_USERNAME` и `DOCKER_PASSWORD`:**
  - Необходимы для аутентификации в Docker Hub.
- **`KUBE_CONFIG`:**
  - Содержит закодированный в base64 файл конфигурации для доступа к Kubernetes-кластеру.

Файл конфигурации пайплайна находится по пути `.github/workflows/ci-cd.yml`.

