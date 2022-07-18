# SSh generálása:

        ssh-keygen

# nagyobb bizotnságú key generálása:

        ssh-keygen -t ed25519 -C " default"


Ha a felhasználói SSH-könyvtár nem létezik, hozza létre a mkdir
és állítsa be a megfelelő engedélyeket:

mkdir -p ~/.sshchmod 0700 ~/.ssh

Nyisson meg egy szövegszerkesztőt
és illessze be a 4. lépésben a kulcspár generálásakor másolt nyilvános kulcsot a ~/.ssh/authorized_keys:

nano ~/.ssh/authorized_keys

A teljes nyilvános kulcs szövegének egyetlen sorban kell lennie.

Futtassa a következőt chmod
parancsot, hogy csak a felhasználó tudja olvasni és írni a ~/.ssh/authorized_keys:

chmod 0600 ~/.ssh/authorized_keys


widowson az ssh konyvtar: \Users\user\.ssh

Jelentkezzen be a távoli szerverre, és nyissa meg az SSH konfigurációs fájlt:

sudo nano /etc/ssh/sshd_config

Keresse meg a következő direktívákat, és módosítsa az alábbiak szerint:

/etc/ssh/sshd_config

PasswordAuthentication no
ChallengeResponseAuthentication no
UsePAM no

Ha elkészült, mentse el a fájlt, és indítsa újra az SSH szolgáltatást a következő beírásával:

sudo systemctl restart ssh

Ezen a ponton a jelszó alapú hitelesítés le van tiltva.

