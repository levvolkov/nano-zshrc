# Полное руководство по установке DeepSeek через Docker с удобным веб-интерфейсом

### 🔹 1. Установка Docker Desktop
- Скачайте и установите [Docker Desktop](https://www.docker.com/products/docker-desktop/) для вашей ОС (Windows/macOS/Linux). 
- После установки запустите Docker Desktop (🐳) и дождитесь его полной загрузки.


### 🔹 2. Установка Ollama в Docker
  - Ollama — это инструмент для работы с локальными LLM (включая DeepSeek).
  - Загрузка [образа Ollama](https://hub.docker.com/r/ollama/ollama):
```bash
   docker pull ollama/ollama
```

- Проверка загруженного образа:
```
   docker images | grep ollama
```

- Запуск [контейнера Ollama](https://hub.docker.com/r/ollama/ollama) на Центральном Процессоре (CPU):
```bash
   docker run -d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
```

- Проверка работы Ollama:
```bash
   docker ps 
   curl http://localhost:11434 
```

### 🔹 3. Установка модели DeepSeek
- Проверка списка моделей (если уже есть другие): 
```bash
   docker exec ollama ollama ls
```

- Загрузка и запуск DeepSeek 7B: 
```bash
   docker exec ollama ollama pull deepseek-r1:7b
   docker exec ollama ollama run deepseek-r1:7b
```

> Примечание:
> - Если хотите использовать [другую версию](https://ollama.com/search), укажите её вместо `deepseek-r1:7b`.
> - Модель займет несколько гигабайт (зависит от версии).
> - Если нужна работа DeepSeek прямо в терминале:
> ```bash
>    # Скачает модель 
>    docker exec ollama ollama pull deepseek-r1:7b
>
>    # -it — позволяет взаимодействовать с моделью в интерактивном режиме (ввод/вывод в терминале).
>    docker exec -it ollama ollama run deepseek-r1:7b
> ```

### 🔹 4. Установка Open WebUI [Open WebUI](https://github.com/open-webui/open-webui?tab=readme-ov-file#installation-with-default-configuration) (графический интерфейс) для удобства
- Запуск Open WebUI:
```bash
   docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
```

- Доступ к интерфейсу:
  - Откройте в браузере: `http://localhost:3000`.
  - Зарегистрируйтесь (первый пользователь автоматически становится администратором).
  - В настройках выберите модель deepseek-r1:7b и начинайте общение!

### 🔹 5. Дополнительные команды
- Остановка контейнеров:
```bash
   docker stop ollama open-webui
```
- Удаление контейнеров (если нужно переустановить)
```bash
   docker rm ollama open-webui
```

> ### ⚠️ Возможные проблемы
> #### Медленная работа.
> DeepSeek 7B требует минимум 8-16 ГБ ОЗУ. Если система слабая, следует рассмотреть другие модели.
