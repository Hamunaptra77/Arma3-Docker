# Arma3-Docker-Server
**Docker Image für den [Arma3](https://arma3.com/) Dedicated Server incl. WINE**

Das Image selbst enthält WINE und Steamcmd sowie ein Skript „entrypoint.sh“, das die Installation des dedizierten Arma3-Servers über Steamcmd startet.

Wenn Sie das Image ausführen, mounten Sie das Volume /home/user/Steam, um die Arma3-Installation beizubehalten und zu vermeiden, dass es bei jedem Containerstart heruntergeladen wird.
Beispielaufruf:

```
cd /home
mkdir -p gamedir
docker run -di -p 2302:2302/udp -p 2303:2303/udp -p 2304:2304/udp -p 2305:2305/udp -p 2306:2306/udp -p 2344:2344/udp -p 2344:2344/tcp -p 2345:2345/tcp --restart unless-stopped -v $PWD/gamedir:/home/user/Steam hamunaptra77/Arma3-server

```

Nach dem Starten des Servers können Sie die Datei „dedicated.yaml“ unter „gamedir/steamapps/common/Arma3 – Dedicated Server/dedicated.yaml“ bearbeiten.
Nach der Bearbeitung müssen Sie den Docker-Container neu starten.

Der DedicatedServer-Ordner wurde mit /server verknüpft, sodass Sie mit z:/server/Saves auf Spielstände verweisen können (z. B. den Spielstand mit dem Namen „The\_Game“):

```

# cp -r /..../Saves/Games/The_Game 'gamedir/steamapps/common/Arma3 - Dedicated Server/Saves/Games/'
# you might want a symlink for games: ln -s 'gamedir/steamapps/common/Arma3 - Dedicated Server/Saves/Games'
docker run -di -p 2302:2302/udp -p 2303:2303/udp -p 2304:2304/udp -p 2305:2305/udp -p 2306:2306/udp -p 2344:2344/udp -p 2344:2344/tcp -p 2345:2345/tcp --restart unless-stopped -v $PWD/gamedir:/home/user/Steam hamunaptra77/Arma3-server -dedicated 'z:/server/Saves/Games/The_Game/dedicated.yaml'

```

Um Argumente an den Steamcmd-Befehl anzuhängen, verwenden Sie `-e "STEAMCMD=..."`. z.B.: `-e "STEAMCMD=+runscript /home/user/Steam/addmods.txt"`.

