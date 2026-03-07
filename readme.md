# Sukram Projekt

## Autor ja rollid
Markus Ulejev - serveri admin, GitHub admin ja deploy seadistamine

## Valitud serverivariant
Valisin self-hosted variandi, kasutades Hyper-V virtuaalmasinat Arch Linuxiga.

## Miks see variant
Valisin selle variandi, sest see võimaldab paremini aru saada, kuidas päris Linux server, Nginx, SSH ja Git-põhine deploy toimivad. Lisaks olin just eelmine nädal ka selle VM valmis sättinud ning olin seda veidi edasi näppinud ja tuttavamaks saanud. Tundus loogiline ja mõistlik seda siis ka kasutada ja proovida.

## Serveri info
- Server: Hyper-V virtuaalmasin
- OS: Arch Linux
- Veebiserver: Nginx
- Veebilehe asukoht serveris: /usr/share/nginx/html
- Deploy script: /home/markus/deploy.sh

## Kuidas deploy töötab
Muudatused tehakse lokaalselt ja lükatakse GitHubi.  
Serveris olev deploy script teeb `git pull` käsu ja reloadib Nginx teenuse, et uus sisu jõuaks veebilehele.

## Probleemid ja lahendused
- `deploy` kasutajal puudusid sudo õigused → kasutati `markus` kasutajat
- Git ei olnud alguses installitud → paigaldati eraldi
- Git clone andis autentimise vea → kontrolliti URL-i ja repo ligipääsu
- Private IP tõttu ei saa GitHub Actions otse serverisse ligi → self-hosted variandi puhul kasutatakse `git pull` loogikat
- /usr/share/nginx/html kuulus vahepeal valele kasutajale, tuli premission denied → muuta omanik õigeks kasutajaks
- peale git paigaldamist ei töödanud git clone ikka → terminali restartimine aitas


## Veebileht
Praegu töötab lokaalses võrgus:

http://172.xx.xxx.xx
http://172.xx.xxx.xxx

Tegi uue local ip, esimene kord kasutasin ülemist ja teine kord alumist.

## Deploy testimine ja töövoog

Deploy loogika testimiseks loodi eraldi branch, kuhu tehti muudatus. Seejärel commititi ja pushiti muudatus GitHubi, avati Pull Request ning merge'iti see `main` harusse. Pärast merge'i käivitus GitHub Actions workflow, mis kinnitas, et deploy pipeline reageerib õigel sündmusel.

Serveripoolne uuendus toimus `deploy.sh` skripti abil, mis teeb `git pull` käsu ja reloadib Nginx teenuse. Testimise käigus kinnitati, et GitHubi repo on serveriga seotud, `git pull` töötab korrektselt ning muudatused jõuavad pärast deploy käivitamist veebiserverisse.

Kuna server asub Hyper-V virtuaalmasinas ja kasutab private IP-aadressi, ei saa GitHub Actions praeguses lahenduses otse serverisse ühenduda. Seetõttu kasutatakse self-hosted variandi puhul `git pull` põhist deploy loogikat, kus server tõmbab ise GitHubist uue koodi.


Workflow deploy test- tegin readme faili brauseris lahti (http://172.xx.xxx.../readme.md), muutus oli seal faili lõpus olemas, kui testisin, et GitHub muudatused jõuaks Linuxi veebiserverisse.