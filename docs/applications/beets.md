# Beets

Homepage: [https://beets.io/](https://beets.io/)

Beets catalogs your collection, automatically improving its metadata as it goes using the MusicBrainz database.

## Usage
1. Regroup music of a same album in a folder.
2. Upload the folder on you NAS's import folder.
3. Within 15 minutes, Beets should run, tag and move your music in your music library. You can check the timer status by running this command on your nas `systemctl list-timers`.

## Specific Configuration
You can configure the plugins used by Beets by redefine the `beets_plugins` variable. You can get a list of available plugins in the Beets documentation [https://beets.readthedocs.io/en/v1.3.17/plugins/index.html].

### Smart playlists
You can enable Smart playlists by adding `smartplaylist` to the `beets_plugins` variable. Once the plugin is added, simply specify names and queries of your playlists in the `beets_smartplaylists` variable. Here is a sample on the expected format:
```yaml
beets_smartplaylists:
    - name: "all.m3u"
      query: ""
    - name: "other.m3u"
      query: "artist:other"
```
The `query` section uses the Beets query language, which the full documentation can be accessible online [https://beets.readthedocs.io/en/v1.3.17/reference/query.html]

## Troubleshooting
### The timer ran but nothing has been moved
Beets uses MusicBrainz to tag your music by using the best candidate possible and applying it automatically. It needs a similarity score of 96% in order to perform automatic tagging. Many reasons can explain this, like Beets picking the wrong release on MusicBrainz or missing tracks. In order to fix this, you can connect to your NAS and run this command: `docker exec -ti -u abc beets beet import /downloads/` This will launch the import from the `import` folder in interactive mode. Beets will prompt you for each albums found that doesn't reach the required similarity score.

### Tags of imported music are wrong
It is possible that Beets took the wrong MusicBrainz release when importing. You can rerun the import command but on your music/band/album directory `docker exec -ti -u abc beets beet import /music/<your\ band\ name>/<album\ name>`. Search for your artist on MusicBrainz (https://musicbrainz.org/), pick the album and choose the correct release. Once this is done, extract the release ID in the URL and use it in beets.

### What if my artist or album cannot be found on MusicBrainz
When manually importing your music, if there's already tags on the tracks, you can tell Beets to use the tags 'As-Is'. It is generally the option 'U'. However, if artists or album names are not consistent (case included), those tracks wont be grouped together. Make sure tags are 'somewhat' clean before using this option.