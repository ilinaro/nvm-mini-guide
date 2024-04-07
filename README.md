### Управление версиями Node.js и NPM с помощью NVM


```ls-remote``` выводит на экран все доступные версии:

```
        v0.1.14
        v0.1.15
        ...
        
        v20.11.1   (LTS: Iron)
        v20.12.0   (LTS: Iron)
        v20.12.1   (Latest LTS: Iron) <--- нас интресует последняя стабильная версия
        v21.0.0
        ...

```


Данная команда позволяет получить последнюю поддерживаемую версию npm для текущей версии Node.js:

```
nvm install-latest-npm 
```

Команда nvm alias позволяет установить версию по умолчанию.

```
nvm alias default 20.12.1
```


Мы можем проверить текущую версию с помощью следующей команды:
```
nvm current
```

```.nvmrc``` установить желаемую версию NodeJS для правильной работы вашего проекта:
```
node -v > .nvmrc
```

Для автозагрузки указанной версии узла

В моем случае я использую, zsh поэтому мне нужно добавить это в конец моего ```.zshrc``` файла:

```
# place this after nvm initialization!
autoload -U add-zsh-hook
load-nvmrc() {
  local node_version="$(nvm version)"
  local nvmrc_path="$(nvm_find_nvmrc)"

  if [ -n "$nvmrc_path" ]; then
    local nvmrc_node_version=$(nvm version "$(cat "${nvmrc_path}")")

    if [ "$nvmrc_node_version" = "N/A" ]; then
      nvm install
    elif [ "$nvmrc_node_version" != "$node_version" ]; then
      nvm use
    fi
  elif [ "$node_version" != "$(nvm version default)" ]; then
    echo "Reverting to nvm default version"
    nvm use default
  fi
}
add-zsh-hook chpwd load-nvmrc
load-nvmrc
```
