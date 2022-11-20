Host azure_key
  HostName vs-ssh.visualstudio.com
  AddKeysToAgent yes
  User git
  IdentityFile ~/.ssh/azure_key
  IdentitiesOnly yes
  HostkeyAlgorithms +ssh-rsa
  PubkeyAcceptedKeyTypes=ssh-rsa


Host personal_github_ssh_key
  HostName github.com
  AddKeysToAgent yes
  User git
  IdentityFile ~/.ssh/personal_github_ssh_key
  IdentitiesOnly yes
