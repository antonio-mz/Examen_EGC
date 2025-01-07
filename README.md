# Configuración de Git y SSH para GitHub

```bash
sudo apt install git
git config --global user.name "<tu nombre y apellidos entrecomillados>"
git config --global user.email <tu email>"
ssh-keygen -t rsa -b 4096
cd ~/.ssh
cat id_rsa.pub
# Ve a GitHub -> Settings -> SSH and GPG keys -> New SSH key
# Pega la clave pública (id_rsa.pub) que se mostró al ejecutar el comando anterior.
