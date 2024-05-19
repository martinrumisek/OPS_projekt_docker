# Spuštění kontejneru s MySQL
## Vytvoření kontejneru v dockeru
Vyžaduje více místa na disku (není daná přesná hodnota)

![image](https://github.com/martinrumisek/OPS_projekt_docker/assets/91115756/99385a2a-7719-442c-965b-06a4d0d2fa0e)


Po instalaci dockeru v Putty stačí do příkazového řádku napsat tento kód
```html
sudo docker run -d --name mysql-container -e MYSQL_ROOT_PASSWORD=my-secret-pw -p 3306:3306 mysql:latest
```
###  Vysvětlení kódu

1. **`sudo`**: Spustí příkaz jako admin.
2. **`docker run`**: Spustí nový Docker kontejner.
3. **`-d`**: Spustí kontejner na pozadí.
4. **`--name mysql-container`**: Pojmenuje kontejner `mysql-container`.
5. **`-e MYSQL_ROOT_PASSWORD=my-secret-pw`**: Nastaví heslo pro MySQL uživatele `root`.
6. **`-p 3306:3306`**: Nastavuje port 3306 na hostitelském systému na port 3306 v kontejneru.
7. **`mysql:latest`**: Použije nejnovější verzi oficiálního MySQL.

## DBeaver
Desktopová aplikace která spravuje databáze
### Nastavení připojení k databázi
Edit connection (F4, pravé tlačítko na databázi a vybrat Edit Connection)

Překliknout "Connect by" z Host na URL

Do textového pole URL za text který tam už je napsat `?allowPublicKeyRetrieval=true`

V "Username" a "Password" napsat výše uvedené jméno uživatele a heslo

![image](https://github.com/martinrumisek/OPS_projekt_docker/assets/91115756/72bce242-ef74-482a-bd7d-0095afdc77a2)
