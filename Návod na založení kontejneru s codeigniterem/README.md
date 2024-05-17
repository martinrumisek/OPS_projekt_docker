# Vytvoření kontejneru a zprovoznění PHP frameworku CodeIgniter 4
Tento návod se zaměřuje na založení kontejneru v Dockeru. Tento kontejner bude obsahovat PHP (v8.2) s následujícími rozšířeními:

- mysqli
- pdo
- pdo_mysql
- zip
- gd
- mbstring
- intl

V rámci tohoto kontejneru není řešeno žádné hostování databáze. Následně naimportovaný projekt (z repozitáře na GitHubu) obsahuje pouze výchozí stránku CodeIgniteru a slouží pouze pro ukázku.

Vytvořený kontejner má pouze jediný otevřený port (HTTP - 80).

## Vytvoření kontejneru
Následujícím příkazem vytvoříme nový kontejner s názvem "web". Poběží v "detached" režimu (přepínač -d), což znamená, že může běžet na pozadí. Následně otevřeme port 80 pro protokol HTTP. Pokud chceme, aby se na web dalo dostat z jiného portu, můžeme upravit první část argumentu (např. 8080:80). Později ale budeme muset při návštěvě stránky za IP adresu i port (např. 192.168.2.1:8080). V poslední řadě vybereme obraz pro tvorbu kontejneru. V našem případě použijeme PHP s verzí 8.2 a web server Apache.

`sudo docker run -itd --name web -p 80:80 php:8.2-apache`

Další příkaz nás přesune do terminálu samotného kontejneru s názvem "web".

`sudo docker exec -it web bash`

## Instalace potřebných PHP rozšíření
Pro úspěšné spuštění CodeIgniter 4 projektu je nutné nainstalovat potřebná rozšíření. V první řadě ale musíme aktualizovat seznam dostupných balíčků pomocí následujícího příkazu.

`apt update`

Nyní je čas nainstalovat samotné balíčky.

`apt install -y libzip-dev libpng-dev libjpeg-dev libfreetype6-dev libonig-dev libicu-dev`

Na základě těchto balíčků můžeme nainstalovat rozšíření do PHP (nejdůležitějšími jsou mbstring a intl, které CodeIgniter vyžaduje k samotnému spuštění).

`docker-php-ext-install mysqli pdo pdo_mysql zip gd mbstring intl`

Teď povolíme přepisovací mód v Apache, který je důležitý pro mnohé frameworky (včetně CodeIgniteru 4)

`a2enmod rewrite`

## Instalace Gitu a naklonování projektu
V kontejneru nemáme předem nainstalovaný Git, proto si jej musíme nyní stáhnout. 

`apt install git`

Po úspěšném nainstalování Gitu si můžeme naklonovat prázdný CodeIgniter 4 projekt z repozitáře umístěného na GitHubu. Předem se musíme ujistit, zda se nacházíme ve složce /var/www/html/ (výchozí složka pro Apache), neboť se tento repozitář klonuje přímo do aktuální složky (a to označuje poslední parametr). 

`git clone https://github.com/David-Citron/blank-codeigniter4 .`

## Konfigurace projektu
Přichází čas na již samotnou konfiguraci projektu. V první řadě potřebujeme udělit celému projektu práva pro zapisování, čtení i spouštění. Pomocí přepínače -R zajístíme, aby měly stejné práva všechny podsložky a jejich soubory. Opět se nejprve musíme ujistit, že se nacházíme ve složce /var/www/html/.

`chmod -R 777 .`

Pro následující krok budeme potřebovat editor textových souborů. Doporučuji nainstalovat nástroj Nano.

`apt install nano`

Pomocí nainstalovaného editoru otevřeme hlavní konfigurační soubor v CodeIgniteru a změníme proměnnou $baseURL na IP adresu (stroje, na kterém běží docker - v našem případě virtuální stroj). Důležité je, aby byla baseURL ve správném formátu. Nesmíme vynechat začátek ani konec (např. ht<span>tp://192.168.2.1/</span>)

`nano app/Config/App.php`

## Spuštění serveru
Aby se všechny změny provedly, tak je potřeba restartovat Apache. Tento restart bohužel ukončí chod kontejneru, takže po odeslání příkazu je následně potřeba jednou zmáčknout ENTER a vrátíme se zpět (mimo kontejner).

`service apache2 restart`

Pokud se nacházíme mimo kontejner, tak můžeme znovu kontejner zapnout. Následujícím příkazem zapneme kontejner s názvem "web".

`sudo docker start web`

## Zobrazení webu
Pro zobrazení webu stačí pouze v URL řádku našeho prohlížeče zadat stejnou adresu jako u proměnné baseURL v konfiguračním souboru (např.: 192.168.2.1). Pokud jsme si při vytváření kontejneru definovali jiný port než 80, tak jej potřebujeme zmínit na konci URL (např.: 192.168.2.1:8080).