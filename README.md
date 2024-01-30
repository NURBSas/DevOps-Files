# DevOps-Files
Visi DevOpso užrašai

Dažniausiai naudojamos Linux Command'os

1. ls: _katalogo turinio sąrašas_
2. cd: _keisti katalogą_
3. pwd: _spausdinti darbinį katalogą_
4. mkdir: _sukurkite katalogą_
5. touch: _sukurkite failą_
6. cp: _failų ir katalogų kopijavimas_
7. mv: _perkelkite arba pervardykite failus ir katalogus_
8. rm: _pašalinti failus ir katalogus_
9. find: _ieškoti failų ir katalogų_
10. grep: _ieškokite šablonų failuose_
11. cat: _sujunkite ir rodykite failu turinį_
12. less: _peržiūrėkite failo turinį puslapis po puslapio_
13. head: _rodyti pirmąsias failo eilutes_
14. tail: _rodyti paskutines failo eilutes_
15. vi/vim: _teksto tvarkyklė_
16. nano: _teksto tvarkyklė_
17. tar: _archyvuoti ir suglaudinti failus_
18. gzip: _suspausti failus_
19. gunzip: _išskleiskite failus_
20. wget: _atsisiųsti failus iš interneto_
21. curl: _perkelti duomenis į serverį arba iš jo_
22. ssh: _SSH nuotolinis prisijungimas_
23. scp: _saugiai kopijuoti failus tarp kompiuterių_
24. chmod: _keisti failo teises_
25. chown: _pakeisti failo nuosavybę_
26. chgrp: _keisti grupės nuosavybės teisę_
27. ps: _Rodyti vykdomus procesus_
28. top: _stebėti sistemos išteklius ir procesus_
29. kill: _nutraukti procesus_
30. df: _rodyti diske naudojimą vietą_
31. du: _apskaičiuoti failų ir katalogų vietą_
32. free: _rodyti atminties naudojimą_
33. uname: _rodyti sistemos informaciją_
34. ifconfig: _rodyti tinklo sąsajas_
35. ping: _patikrinti tinklo ryšį, papinginti_
36. netstat: _tinklo statistika_
37. iptables: _ugniasienės administravimas_
38. systemctl: _tvarkyti sistemos servisus_
39. journalctl: _sistemos žurnalas_
40. crontab: _suplanuoti cron darbus_
41. useradd: _sukurti vartotoją_
42. passwd: _pakeisti vartotojo slaptažodį_
43. su: _perjungti vartotoją_
44. sudo: _vykdykite komandą kaip nurodytas vartotojas_
45. usermod: _redaguoti vartotojo paskyrą_
46. ​​groupadd: _sukurti grupę_
47. groupmod: _keisti grupę_
48. id: _rodyti vartotojo ir grupės informaciją_
49. ssh-keygen: _generuokite SSH raktus_
50. rsync: _sinchronizuoja failus ir katalogus_
51. diff: _palyginti failus eilutė po eilutės_
52. patch: _pritaikyti pataisą failams_
53. tar: _išskleisti failus iš archyvo_
55. nc: _Netcat – tinklo įrankis_
56. wget: _atsisiųsti failus iš interneto_
57. whois: _ieškoti LDAP domeno vartotojo informacijos_
58. dig: _DNS paieškos įrankis_
59. sed: _srauto redaktorius, skirtas manipuliuoti tekstu_
60. awk: _raštų nuskaitymo ir apdorojimo kalba_
61. sort: _rūšiuoti eilutes tekstiniame faile_
62. cut: _ištraukti dalis iš failų eilučių_
63. wc: _žodžių, eilučių, simbolių ir baitų skaičius_
64. tee: _peradresuoti išvestį į kelis failus arba komandas_
65. history: _komandų istorija_
66. source: _vykdyti komandas iš failo dabartiniame apvalkale_
67. alias: _kurti komandų slapyvardžius_
68. ln: _kurti nuorodas tarp failų_
69. uname: _spausdinti sistemos informaciją_
70. lsof: _atidaryti failų ir procesų sąrašą_
71. mkfs: _sukurti failų sistemą_
72. mount: _prijungti failų sistemą_
73. umount: _atjungti failų sistemą_
74. ssh-agent: _tvarkyti SSH raktus atmintyje_
76. tr: _išversti simbolius_
78. paste: _sujungti failų eilutes_
79. uniq: _pranešti arba praleisti pasikartojančias eilutes_


## Ansible konfiguravimas Windows 10 mašinose:

Prieš paleidžiant konfiguracinį skriptą: ConfigureRemotingForAnsible.ps1 *(https://github.com/AlbanAndrieu/ansible-windows/blob/master/files/ConfigureRemotingForAnsible.ps1)*

Būtina pasirūpinti leidimais: __Set-ExecutionPolicy -ExecutionPolicy Unrestricted__
Kitu atveju fail. ;)

Aišku būtina jį paleisti administratoriaus teisėmis na bet čia jau turėtu būti savaime aišku. ;)

Then on the left pane the group editor can be expanded. Expand it and navigate to Computer Configuration -> Administrative Templates -> Windows Components.

[enter image description here](https://i.stack.imgur.com/84JjK.png)

Then to Windows PowerShell.

[enter image description here](https://i.stack.imgur.com/N5HCq.png)

So select Turn on Script Execution. Change configuration to Enabled and specify Allow all scripts in Execution Policy.
[enter image description here](https://i.stack.imgur.com/n3WQh.png)

Confirm by hitting Ok and close the Management Console.

## Ansible konfiguravimas Linux mašinose:

Norint prisijungti prie Linux serverio naudojant tik vartotojo vardą ir slaptažodį būtina užsiinstaliuoti __sshpass__ modulį _sudo apt-get install sshpass_ Jūsų serveryje.

Inventory faile rašome:

    [jūsų serverio:vars]
    ansible_connection = ssh
    ansible_user = vartotojas
    ansible_ssh_pass = slaptažodis

Bet šis metodas tinkamas tik testavimui. ;)

__Playbook failai YML formatu__

Kai nori užinstaliuoti Chocolatey! *Choco.yml*

    ---
    - name: "Chocolatey"
      hosts: windows serverio vardas
      tasks:
        - name: Install chocolatey
          win_chocolatey:
              name:
               - chocolatey
               - chocolatey-core.extension
              state: present

Ir atskirų programų iš šokolado instaliavimas.

    ---
    - hosts: windows serverio vardas
      tasks:
      - name: Install Admin Tools
        win_chocolatey:
            name: '{{ item }}'
            state: present
            pinned: yes
            architecture: x86
            force: yes
        with_items:
        - 7zip
        - procexp
        - putty
        - notepadplusplus.install
        - windirstat
        - git
        - winscp

