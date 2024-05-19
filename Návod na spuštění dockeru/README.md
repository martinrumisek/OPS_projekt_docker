# Instalace dockeru
V tomto návodu se zaměříme na instalaci Dockeru (nikoliv zakládání kontejnerů). Bereme v potaz, že se snažíme Docker nainstalovat na virtuální stroj s operačním systémem Debian 12.

## Připojení k VM přes PuTTY
Proč použít PuTTY? Má to hned několik důvodů. Debian spuštěný přes Live CD/DVD nemá přednastavenou českou klávesnici a PuTTY nám umožňuje vkládat text. Pro zprovoznění SSH potřebujeme provést následující kroky:

1. Prvně aktualizujeme seznam balíčků: `sudo apt update`
2. Nainstalujeme balíček SSH: `sudo apt install ssh`
3. Pokud používáme Live CD/DVD, tak si změníme heslo: `sudo passwd user`
4. Zjistíme IP adresu virtuálního stroje: `ip a`
5. V PuTTY se připojíme pomocí zjištěné IP adresy (na port 22) a daného uživatelského jména a hesla

## Instalace balíčku docker.io
Pro nainstalování Dockeru je potřeba stáhnout odpovídající balíček: `sudo apt install docker.io`