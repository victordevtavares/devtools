# Git

git config --global user.name "Nome de Usuario"
git config --global user.email "email@email.com"
__________________________________________________________
## Update forked repo with the upstream:
1. git remote add upstream https://www.github.com/ACCOUNT/REPO.git

1. git fecth upstream

1. git rebase upstream/BRANCH -> normally, branch == master

1. git push -> It will update your repo with the upstream content
__________________________________________________________
## Resolver problema de alocação de memória:

### Aumentar RAM:
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
sudo swapon --show

### Aumentar o postBuffer:
git config --global https.postBuffer 1007152000