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

http://172.26.146.76