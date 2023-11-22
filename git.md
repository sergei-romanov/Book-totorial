**Git**

Подробный видео гайд по установке https://www.youtube.com/watch?v=GsG5roSGha0&t=1s&ab_channel=KonstantinShibkov
```
# windows
  https://git-scm.com
  
# linux
  sudo apt install git-all

# macOS
  git --version
```

- Настройка git
```
# глобальные дефолтные параметры 
git config --global user.name "Sergey Romanov"             
git config --global user.email romanov.java@gmail.com
git config --global core.editor "vim" сменить нано или другую фигню

# Параметры установки окончаний строк 
 # linux/mac
git config --global core.autocrlf input
git config --global core.safecrlf warn
# windows
git config --global core.autocrlf true
git config --global core.safecrlf warn

# Установка отображения unicode
git config --global core.quotepath off
```
- Уровни настройки:
```
# параметры на всю систему
--system 

# для пользователя
--global 
    # examplegit 
    config --global user.name "Sergey Romanov"

# для проекта
--local(default)
```
- настройка .gitcofig
```
# linux /home/sergey/.gitconfig 

[user]
	name = Sergey Romanov
	email = romanov.java@gmail.com
[core]
	editor = vim
	autocrlf = input
	safecrlf = warn
	quotepath = off
[alias]
  co = checkout
  ci = commit
  st = status
  br = branch
  hist = log --pretty=format:\"%h %ad | %s%d [%an]\" --graph --date=short
  type = cat-file -t
  dump = cat-file -p
```
**Git command**

- Создание репозитория
```
# переходим в нужный каталог и создаем там репозиторий
  git init

# в гите создаем репозиторий и выполняем привязку
  git init --initial-branch=main
  git remote add origin http://gitlab.romanov.digital/sergei/tutorial-book.git
  git push
```
- добавления ssh-ключей на github.com
```
# Создание ssh-ключей
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
# Дальше будет несколько вопросов. На все вопросы нужно нажимать Enter.

# Запуск агента ssh, который следит за ключами
$ eval "$(ssh-agent -s)"

# Добавления нового ssh-ключа в агент
$ ssh-add ~/.ssh/id_rsa

$ cat ~/.ssh/id_rsa.pub

Добавьте ssh-ключ в аккаунт Github. https://github.com/settings/keys
```
- gitignore example
```
# Ignore everything
*
# Don't ignore directories, so we can recurse into them
!*/
# Don't ignore .gitignore and *.foo files
!.gitignore
!*.foo
```
standard file
```
# Compiled class file
*.class

# Log file
*.log

# BlueJ files
*.ctxt

# Mobile Tools for Java (J2ME)
.mtj.tmp/

# Package Files #
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar

# virtual machine crash logs, see http://www.java.com/en/download/help/error_hotspot.xml
hs_err_pid*
```

- добавление файлов в индекс
```
# добавление файлов
git add nameFile1 nameFile2 nameFile3  

# добавление всех файлов в текущем каталоге
git add . 

# добавить все файлы в текущем каталоге с раширением .java
git add *.java  

# добавить все файлы в проекте с расширением .java
git add "*.java"   

# добавить все файлы в папке
gtt add nameDirectory

# добавление файла если он находится в гит игноре или каталоге гит игноре(дальше с ними будет идти работа как с обычными файлами)  (-f флаг для игнора предупреждений гит)
git add -f nameFile 

# выборочное добавления изменений в файле в индекс(когда часть кода тру но не весь)
git add -p nameFile 

# показывает измененные куски файлов и спрашивает, что с ними сделать
git add -i

# снять индексацию
git reset HEAD nameFile
```

- commit
```
# первая строка заголовок, через пустую строку начиная с * можно конкретизировать * add feature A  * Fix feature B и тд
git commit  

# для краткости без отркытия редактора
git commit - m "текст комита" 

# добавление изменений всех файлов без команды add но только если файл до этого был проиндексирован
git commit -am "text"  

# добавление по указаному пути
git commit - m "text" paths   
```
- info
```
# проверка версии гит
git --version 

# посмотреть все настройки
git config --list 

# посмотрите информацию о последнем коммите
git show 

# просмотр разницы (что конкретно было изменено в файлах) между рабочим каталогом и индексом (staged area)
git diff

# показывает какие изменения пойдут в коммит
git diff --cached

# просмотр разницы между последним коммитом и индексом
git diff --staged

# узнать текущий статус файлов (статус репозитория)
git status 

git log история коммитов 
git log --pretty=oneline вывод в однострочном формате коммитов

# удобный вывод лога
git log --pretty=format:"%h %ad | %s%d [%an]" --graph --date=short
    # определяет формат вывода. --pretty="..."
    # укороченный хэш коммита (7 символов) %h     
    # дополнения коммита («головы» веток или теги) %d     
    # дата коммита %ad 
    # комментарий %s      
    # имя автора %an    
    # отображает дерево коммитов в виде ASCII-графика --graph 
    # сохраняет формат даты коротким и симпатичным --date=short

# ищет совпадение с указанной строкой во всех файлах проекта
git grep
```

- редактирование(local репозиторий)
```
# откат комита (после тильды можно узать колличество комтиов на сколько нужно назад) данные из комита остаются в индексе
git reset --soft @~
    # запись месседжа из откаченого комита в новый (-c копирование сообщения)
    git commit -c ORIG_HEAD

# если забыли добавить файл в коммит делаем (упрощеный аналог команды выше)
git add nameFile
git commit --amend

# (mix) откатывает последний коммит и снимает его изменения с индекса
git reset @~

# поменять месседж последнего коммита
git commit --amend -m "Text"

#быстрое восстановление последней версии файла до индексации
git restore nameFile

# отмена изменений в файлах если они еще непроиндексированы
git checkout nameFile1 nameFile2 

# отмены индексации изменения (противоположность git add)
git reset nameFile 

# отмена последнего комита(однако он будет видет в истории ветки)
git revert HEAD 

# откат к последнему коммиту
git reset --hard hashCommit

# удаление файла из коммита, но не из индекса
git rm --cash nameFile 
```

- работа с удаленным репозиторием(remote репозиторий)
```
git remote add origin https...
git branch -m main
git push -u origin main

# извлекать новые коммиты из удаленного репозитория, но не сливает их с наработками в локальных ветках
git fetch 

# слитие изменений
git merge origin/master 

# скачивает из внешнего репозитория новые коммиты и добавляет их в локальный репозиторий,
эквивалентна комбинации git fetch и git merge. (лучше использовать с флагом --rebase)
git pull 
```
- работа с ветками
```
# создание новой ветки
git branch name 

# показать все ветки
git branch -a 

# переключение на созданную ветку
git checkout <name> 
git switch <name>

# переименовать локальную ветку
git branch -m oldname newname

# шорткат создание и сразу переход (co = checkout alias)
git co -b name 

# переход между ветками
git co name 

# слияние текущей ветки с указанной
git merge nameBranch 
    # если конфликт: открываем файл с конфликтом фиксим делаем адд и коммит "merge nameBranch fixed conflict"

# перебазирование(перенос изменений из одной ветки в другую) rebase для кратковременных, локальных веток, а слияние для веток в публичном репозитории.
git rebase nameBranch 

# удаление всех эксперентов(возвращение в состояние ласт комита)
git checkout -f 

# сохранение изменений без коммита для переключения между ветками
git stash  

# вернуть незакомиченые данные
git stash pop 

# добавление локальной ветки, которая отслеживает удаленную ветку.
git branch --track style origin/style 
```

- удаление и переименование
```
# удаляет файл
git rm nameFile 

# удаляет директорию (-r флаг для директории)
git rm - r nameDirectory 

# удаляет из индекса, но оставляет в рабочем каталоге (--cached операция с индексом, а не рабочей директорией)
git rm - r --cached nameDirectory 

# переименования файла
git mv nameFile1 nameFile2 

# сброс изменений в отслеживаемых файлах
git reset --hard

# удаление неотслеживаемых файлов и директорий из рабочей области
git clean -dxf
```
- добавить 2ой удаленный репозиторий
```
git remote add origin http://gitlab.localdomain/back/pie-platform.git
origin заменяем на нужное название пример staging
```