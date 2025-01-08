# Configuración de Git y SSH para GitHub

```bash
sudo apt install git
```

```bash
git config --global user.name "<tu nombre y apellidos entrecomillados>"
```
```bash
git config --global user.email "<tu email>"
```


```bash
ssh-keygen -t rsa -b 4096
cd ~/.ssh
cat id_rsa.pub
```
#### Ve a GitHub -> Settings -> SSH and GPG keys -> New SSH key
#### Pega la clave pública (id_rsa.pub) que se mostró al ejecutar el comando anterior.

## Links utiles
[Instalacion sistema](https://docs.uvlhub.io/installation/manual_installation)

## Crear otra vez bd
```bash
sudo mysql -u root -p
```
```bash
DROP DATABASE uvlhubdb;
DROP DATABASE uvlhubdb_test;
DROP USER 'uvlhubdb_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

