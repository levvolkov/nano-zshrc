# «Aвтоматизизация процесса резервного копирования файла `.zshrc`»

Процесс создания резервной копии файла `.zshrc`, автоматизизация процесса резервного копирования и использование Git для управления версиями вкючает в себя создания резервной копии и настройку автоматизации с помощью cron. 

#### 1. Настройка репозитория Git для управления версиями .zshrc
```bash
   # Создаст директорию, для хранения версии файла конфигурации:
   mkdir -p ~/Backup-zshrc/dotfiles

   # Перейдет в созданную директорию:
   cd ~/Backup-zshrc/dotfiles

   # Инициализирует репозиторий Git:
   git init

   # Скопирует файл .zshrc в директорию ~/dotfiles/
   # -v (verbose), чтобы увидеть, что именно происходит:
   cp -v ~/.zshrc ~/Backup-zshrc/dotfiles/.zshrc

   # Покажет все скрытые файлы
   ls -la ~/Backup-zshrc/dotfiles

   # Добавит файл .zshrc в индекс:
   git add .zshrc

   # Зафиксирует изменения:
   git commit -m "Initial commit of .zshrc"
```

#### 2. Создание скрипта для автоматического резервного копирования и управления версиями
```bash
   # Создаст новый файл скрипта:
   nano ~/Backup-zshrc/backup_zshrc.sh
```
- Добавляем весь следующий код в файл:
```bash
   #!/bin/bash

   # Копируем .zshrc в папку dotfiles
   cp ~/.zshrc ~/Backup-zshrc/dotfiles/.zshrc

   # Переходим в директорию dotfiles
   cd ~/Backup-zshrc/dotfiles

   # Добавляем файл .zshrc в индекс:
   git add .zshrc

   # Создаем коммит с изменениями
   git commit -m "Backup .zshrc on $(date +%F-%H%M%S)"
```

#### 3. Сделать скрипт исполняемым
```bash
   # Сделает скрипт исполняемым:
   chmod +x ~/Backup-zshrc/backup_zshrc.sh
```

#### 4. Назначение nano как редактор для работы со всеми текстовыми файлами
```bash
   # Откроет nano редактор
   nano ~/.zshrc

   # Устанавливает nano в качестве текстового редактора по умолчанию
   export EDITOR=nano

   # Соохранение изменений
   source ~/.zshrc 
```

#### 5. Настройка автоматического выполнения резервного копирования с помощью cron
```bash
   # Откроет crontab для редактирования:
   crontab -e

   # Укажет cron выполнять скрипт каждый день в 2:00:
   0 2 * * * ~/Backup-zshrc/backup_zshrc.sh
```

#### 6. Проверка работоспособности
```bash
   # Для проверки, работает ли скрипт:
   ~/Backup-zshrc/backup_zshrc.sh

   # Перейдет в директорию dotfiles:
   cd ~/Backup-zshrc/dotfiles

   # Для проверки истории коммитов:
   git log
```

#### 7. Просмотр изменений в коммитах
- После выполнения команды `git log`, видно список коммитов. Чтобы посмотреть различия между коммитами, выполните следующую команду:
```bash
   # Просмотр изменений в последнем коммите:
   git show HEAD

   # Просмотр изменений между двумя конкретными коммитами (два хэша, разделенные двумя точками ..):
   git diff f737af6b..0471d741

   # Или, нужно посмотреть изменения между последним коммитом и предпоследним:
   git diff HEAD~1 HEAD
```

#### 8. Откат к предыдущей версии
В случае если нужно откатиться к предыдущей версии файла `.zshrc`, можно это сделать, используя следующую команду:
```bash
   # Восстановит файл из конкретного коммита:
   git checkout f737af6b -- .zshrc
```
> Это восстановит файл .zshrc в состоянии, в котором он был на момент указанного коммита. Однако имейте в виду, что после этой команды нужно будет сделать новый коммит, чтобы сохранить изменения.

#### 9. Создание нового коммита после отката
После восстановления файла, для сохранения этой версии:
```bash
   # Добавит файл в индекс:
   git add .zshrc

   # Зафиксирует изменения с новым сообщением:
   git commit -m "Revert to previous version of .zshrc"
```
> Эти команды позволит управлять версионностью файла .zshrc и восстанавливать его в случае необходимости. 


<details>
  <summary>Хэш коммита</summary>

   Хэш коммита – это уникальный идентификатор, который Git присваивает каждому коммиту. Он генерируется на основе содержимого коммита, включая изменения, автора, дату и другие метаданные.

Хэш представляет собой длинную строку символов, состоящую из цифр и букв (в основном, это 40-значная строка в шестнадцатеричном формате).
Используя этот хэш, можно ссылаться на конкретные коммиты в различных командах Git, например, для просмотра, возврата или сравнения.

**Пример использования**
- Когда видите строчку, подобную этой:
```bash
   f737af6b82f191bbb91bb3022fac45d9bdad8480..0471d741f54c27f1fa25ac2de025552925c159f9
```
Это означает, что вы сравниваете изменения, сделанные от коммита с хэшем `f737af6...` до коммита с хэшем `0471d741....` Хэши позволяют точно указать, какие коммиты вы хотите анализировать.
</details>

Настроен автоматизированный процесс резервного копирования файла `.zshrc` с использованием Git для управления версиями. Можно просматривать и восстанавливать предыдущие версии файла `.zshrc`, просматривая историю коммитов.


