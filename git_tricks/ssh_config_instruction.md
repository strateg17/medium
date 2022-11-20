Вирішив доповнити дане завдання по налаштування ssh доступів до наших репозиторіїв. Для цього можна скористатися цим гайдом: https://learn.microsoft.com/en-us/azure/devops/repos/git/use-ssh-keys-to-authenticate?view=azure-devops

Проте у ньому є багато відритих питань, особливо, що стосується користування декількома доступами. Тому необхідно виконати наступний алгоритм:

виконуємо команду:
ssh-keygen \
    -m PEM \
    -t rsa \
    -b 4096 \
    -C "azureuser@myserver" \
    -f ~/.ssh/<ssh_key_file_name>\
    -N mypassphrase
ssh-keygen = the program used to create the keys
-m PEM = format the key as PEM
-t rsa = type of key to create, in this case in the RSA format
-b 4096 = the number of bits in the key, in this case 4096
-C "azureuser@myserver" = a comment appended to the end of the public key file to easily identify it. Normally an email address is used as the comment, but use whatever works best for your infrastructure.
-f ~/.ssh/mykeys/myprivatekey = the filename of the private key file, if you choose not to use the default name. A corresponding public key file appended with .pub is generated in the same directory. The directory must exist.
-N mypassphrase = an additional passphrase used to access the private key file.
ls -al ~/.ssh (бачимо файли з однаковими назвами і різними закінченнями)
cat <ssh_key_file_name>.pub - копіюємо цей публічний ключ та вставляємо його у AzureDevOps SSH Pubilc Keys. Він має мати вигляд:
ssh-rsa XXXXXXXXXXc2EAAAADAXABAAABAXC5Am7+fGZ+5zXBGgXS6GUvmsXCLGc7tX7/rViXk3+eShZzaXnt75gUmT1I2f75zFn2hlAIDGKWf4g12KWcZxy81TniUOTjUsVlwPymXUXxESL/UfJKfbdstBhTOdy5EG9rYWA0K43SJmwPhH28BpoLfXXXXXG+/ilsXXXXXKgRLiJ2W19MzXHp8z3Lxw7r9wx3HaVlP4XiFv9U4hGcp8RMI1MP1nNesFlOBpG4pV2bJRBTXNXeY4l6F8WZ3C4kuf8XxOo08mXaTpvZ3T1841altmNTZCcPkXuMrBjYSJbA8npoXAXNwiivyoe3X2KMXXXXXdXXXXXXXXXXCXXXXX/ azureuser@myserver
Після чого можна проводити конфігурацію локальних налаштувань ключів:
Відкриваємо файл config для ssh ключів:
vim ~/.ssh/config
У ньому прописуємо наступні налаштування (тут наводжу приклад власного файлу):
Host azure-work
  HostName vs-ssh.visualstudio.com
  AddKeysToAgent yes
  User git
  IdentityFile ~/.ssh/azure_key
  IdentitiesOnly yes
  HostkeyAlgorithms +ssh-rsa
  PubkeyAcceptedKeyTypes=ssh-rsa
Host github-personal
  HostName github.com
  AddKeysToAgent yes
  User git
  IdentityFile ~/.ssh/personal_github_ssh_key
  IdentitiesOnly yes
Тепер можна додати наші ключі до ssh-agent:
ssh-add -K ~/.ssh/azure_key
ssh-add -K ~/.ssh/personal_github_ssh_key
Проводимо тестування чи зв'язок встановлено коректно:
ssh -T azure-work
ssh -T github-personal 
ОБОВ'ЯЗКОВО АКТИВУЄМО SSH:
eval `ssh-agent`
