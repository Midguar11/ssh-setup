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


widowson az ssh konyvtar: \Users\user\.ssh> 
