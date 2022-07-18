# SSh generálása és másolása

        ssh-keygen

# nagyobb bizotnságú key generálása:

        ssh-keygen -t ed25519 -C " default"


- Ha a felhasználói SSH-könyvtár nem létezik, hozza létre a mkdir
és állítsa be a megfelelő engedélyeket, fontos hogy ne a  rootnak adjunk ssh jogokat, biztonsági okokból.
De a user legyen sudoer.

        mkdir -p ~/.sshchmod 0700 ~/.ssh

- Nyisson meg egy szövegszerkesztőt
és illessze be a 4. lépésben a kulcspár generálásakor másolt nyilvános kulcsot a:

        nano ~/.ssh/authorized_keys
        
- A teljes nyilvános kulcs szövegének egyetlen sorban kell lennie

- Futtassa a következőt chmod parancsot, hogy csak a felhasználó tudja olvasni és írni:

        chmod 0600 ~/.ssh/authorized_keys

- Widowson az ssh konyvtar: c: \Users\user\.ssh
- vagy c: ~\.ssh

- Jelentkezzen be a távoli szerverre, és nyissa meg az SSH konfigurációs fájlt:

        sudo nano /etc/ssh/sshd_config

Keresse meg a következő direktívákat, és módosítsa az alábbiak szerint:

#SSH Tesztelése

- SSH kapcsolat lebontása. Ujra kapcsolodás ha nem kér jelszót akkor rendben van

# Kapcsolat és SSH Biztonságossá tétele

        /etc/ssh/sshd_config

PasswordAuthentication no
ChallengeResponseAuthentication no
UsePAM no

Ha elkészült, mentse el a fájlt, és indítsa újra az SSH szolgáltatást a következő beírásával:

sudo systemctl restart ssh

Ezen a ponton a jelszó alapú hitelesítés le van tiltva.



# Miért kell letiltanunk a root bejelentkezést SSH-n keresztül?


Adminisztráció Biztonság 

1. Áttekintés

Linux rendszergazdákként azt tanítják nekünk, hogy nagyon rossz gyakorlat és biztonsági hiba az SSH . De pontosan mi teszi ezt rossz gyakorlattá?

Ebben az oktatóanyagban először elmagyarázzuk, miért jelent biztonsági problémát az SSH-n keresztüli root-bejelentkezés engedélyezése. Ezen ismeretek birtokában bemutatunk néhány bevált gyakorlatot.
2. A Rossz

A gyökér a szuperfelhasználói fiók Unix és Linux alapú rendszerekben. Miután elértük a root fiókot, teljes rendszerhozzáféréssel rendelkezünk. Mivel a felhasználónév mindig root, és a hozzáférési jogok korlátlanok, ez a fiók a legértékesebb célpont a hackerek számára.

Nagyon sok bot keresi az internetet szabad SSH-portokkal rendelkező rendszerek után. Amikor találnak egyet, megpróbálnak bejelentkezni a szokásos felhasználónevekkel, és megpróbálják kitalálni a jelszót.

Képzelje el, hogy egy botnak szerencséje van, és kitalálja a root jelszót. Mivel a root hozzáférést biztosít az egész géphez, a gépet jelenleg elveszettnek kell tekinteni.
freestar

A hatás sokkal kisebb lett volna, ha a feltört felhasználó jogosultság nélküli hozzáféréssel rendelkezik. Ekkor a jogsértést feloldják, és csak erre a felhasználóra korlátozzák.

Vegye figyelembe, hogy a root nem az egyetlen gyakori felhasználónév. Ugyanez vonatkozik az olyan alkalmazások egyéb gyakori felhasználóneveire, mint az Apache , MySQL stb.
3. A legjobb gyakorlatok

Most, hogy tudjuk, hogy rossz az SSH-n keresztüli root bejelentkezés engedélyezése, ideje néhány mérést végezni. Nézzünk meg néhány bevált gyakorlatot.
3.1. Root SSH letiltása

Először is letiltjuk az SSH root bejelentkezéseket. Ezt az SSH démon konfigurációjának szerkesztésével tehetjük meg, amely általában az /etc/ssh/sshd_config fájlban található. Meg kell győződnünk arról, hogy tartalmazza a következő sort:

PermitRootLogin no

Továbbá, mivel nem akarjuk kizárni magunkat, gondoskodunk arról, hogy normál felhasználónk továbbra is be tudjon jelentkezni felhasználónévvel:

AllowUsers username

vagy csoportonként:

AllowGroups groupname

Miután elmentettük a változtatásokat, újra kell indítanunk a sshdszolgáltatást, hogy azok hatékonyak legyenek.
3.2. Használj sudo-t

Adminisztrációs célból időnként továbbra is rootként kell végrehajtanunk bizonyos feladatokat. Fel kell venni a szokást, hogy ehhez a sudo .

A sudo segítségével rootként működhetünk anélkül, hogy rootként kellene lennünk. Ennek van egy kevésbé nyilvánvaló előnye is. n keresztül végrehajtott összes feladatot sudo saját használatunkban hajtjuk végre, nem pedig az általános root fiókban, ezért a naplókban a saját felhasználónevünk alatt fog megjelenni.
3.3. Használjon SSH kulcsokat

Bár a ritka felhasználói nevek miatt kevésbé valószínű, a normál felhasználói fiókok továbbra is ki vannak téve a botok általi jelszókitalálásnak. Emellett az emberek hajlamosak gyenge jelszavakat választani, vagy újra felhasználni jelszavaikat, hogy könnyebben megjegyezhetőek legyenek.
freestar

Míg a jelszavak kitalálása enyhíthető erős jelszavak kiválasztásával (nehezebb, mint amilyennek látszik) vagy a sikertelen bejelentkezési kísérletek korlátozásával, a legjobb, ha teljesen megszabadul a jelszavaktól.

Jelszavak helyett SSH-kulcsokat használhatunk a bejelentkezéshez. A beállítás után le kell tiltanunk a jelszavas bejelentkezést az /etc/sshd_config fájlban :

PasswordAuthentication no

és indítsa újra az sshd szolgáltatást.

A jelszavakkal ellentétben a privát kulcsokat gyakorlatilag lehetetlen kitalálni. Mivel a privát kulcs ellopása sokkal nehezebb, mint egy (gyenge) jelszó kitalálása, az SSH-kulcsok használata eleve a biztonságosabb választás.
4. Következtetés

Ebben a cikkben láthattuk, miért rossz az SSH-n keresztüli root bejelentkezés engedélyezése. A root bejelentkezések letiltása mellett meg kell vizsgálnunk a rendszereink védelmét a jelszavas bejelentkezések teljes letiltásával.

Az SSH-kulcsok és a sudo használata nagyszerű lépés rendszereink biztonságosabbá tételében.

Forrás cikk:

https://www.baeldung.com/linux/root-login-over-ssh-disable






