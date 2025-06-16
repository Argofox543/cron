# Plánování úloh v cronu
Obchodní akademie, Vyšší odborná škola a Jazyková škola s právem státní jazykové zkoušky Uherské Hradiště

Radim Vaculík
15.05.2025
## Co je to cron?
-   Jedná se o systémového démona, který se v Linuxových a Unixových systémech stará o automatické spouštění naplánovaných úloh​
    
-   Běží neustále na pozadí a každou minutu kontroluje, jestli má něco spustit podle naplánovaných pravidel​
    
-   Ideální pro opakované úlohy, které se mají spouštět ve specifický čas nebo interval
## K čemu se používá
-   Zálohování souborů​
    
-   Čištění logů nebo cache​
    
-   Automatické aktualizace​
    
-   Odesílání e-mailových reportů​
    
-   Spouštění skriptů nebo serverových úloh​
    
-   Restartování služeb

## Kde se konfigurují úlohy a jejich zobrazení
-   Konfigurace:​
    
	  	Pro běžného uživatele: crontab –e (uživatelský plán)​
    
	   	Systémové úlohy: /etc/crontab nebo /etc/cron.d/​
    
	   	Speciální složky: /etc/cron.hourly/, /etc/cron.daily/, atd.​
    
    
-   Zobrazení:​
    
		Osobního seznamu: crontab –l​
    
		Systémového seznamu: cat /etc/crontab​
    
## Zápis úloh
-   Po otevření seznamu s příkazem **crontab –e** se ​  
    zobrazí následující upravitelný soubor.​ (viz. screenshot)
    
-   Každý řádek má tento formát:

		* * * * * příkaz/skript​
    
-   Každá hvězdička/asterisk představuje čas:​  
 
		(minuty 0-59, hodiny 0-23, den v měsíci 1-31,​  měsíc 1-12, den v týdnu 0-7)​
    
-   Pokud není zadaný určitý čas tak se opakuje každý interval (hodinu, den, ...)

 ## Příklad 1. (jednoduchý výpis)
 
 -   Vytvoříme jednoduchý skriptový soubor např. "Hello.sh" a do něj vložíme následující příkaz:​  
    
    echo "Skript byl aktivován $(date)" > /home/uzivatel/hello.txt​
    
-   Nesmíme zapomenou na to aby byl soubor spustitelný:​ 

		chmod +x hello.sh​
    
-   Následně přejdeme do konfigurace a nastavíme aby se skript spouštěl každou minutu​  
    
	    * * * * * /home/uzivatel/hello.sh​
    
-   Nebo každý týden v poledne​  
    
	    0 12 * * 1 /home/uzivatel/hello.sh
## Příklad 2. (zálohování)
-   Vytvoříme soubor "zaloha.sh" která bude​  
    obsahovat následující skript (viz. screenshot)​
    
-   Proměnné **ZDROJ** a **CIL** odkazují na složky​
    
-   V proměnné **DATUM** bude datum ve formátu:​
		
		Datum=$(date +%Y-%m-%d_%H-%M)  
	    (rok, měsíc, den, hodina a minuta)​
- V proměnné ZALOHA bude cesta k výslednému souboru 

	  ZALOHA="$CIL/zaloha_$DATUM.tar.gz"
    
-   Pomocí **tar –czf**  vytvoříme a zazipujem soubor.​
	
		tar -czf "$ZALOHA" "$ZDROJ"
    
-   Následně zajistíme aby byl soubor spustitelný a poté přidáme do cronu​: ​  
    
	    chmod +x zaloha.sh 
	 
		* */1 * * * /home/uzivatel/zaloha.sh 
- spouštění každou hodinu
- Bonus: pokud máme zálohy starší než 7 dní tak je můžeme smazat

	  find "$CIL" -name "zlaoha_*.tar.gz" -mtime +7 -delete

  ## Přílohy
  ![Bez názvu2](https://github.com/user-attachments/assets/19d4286c-8a87-43f0-881e-402f699456b2)
![Bez názvu2](https://github.com/user-attachments/assets/19d4286c-8a87-43f0-881e-402f699456b2)
![Bez názvu1](https://github.com/user-attachments/assets/cd01d546-fd2c-4870-9bcc-3ab768959abd)
![Bez názvu1](https://github.com/user-attachments/assets/cd01d546-fd2c-4870-9bcc-3ab768959abd)
