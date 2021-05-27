# Process notes (about parsing Notes)

## Where are the notes stored

The sqlite database is here: `~/Library/Group\ Containers/group.com.apple.notes/NoteStore.sqlite`

At least in macOS Catalina. Other versions of macOS apparently have it [elsewhere](https://apple.stackexchange.com/questions/111633/where-do-my-notes-written-in-the-notes-application-on-my-mac-get-saved).

## Datasette

Installed using `brew install datasette`

Pointed Datasette at the database with:

```bash
datasette ~/Library/Group\ Containers/group.com.apple.notes/NoteStore.sqlite -o
```

Key table seems to be `ZICCLOUDSYNCINGOBJECT`

### Facet by folder

http://127.0.0.1:8001/NoteStore/ZICCLOUDSYNCINGOBJECT?_facet=ZFOLDER (only works if Datasette is running locally.)

Folder `56` looks like the one I want. (It's my "projects" folder.)

### Query titles & snippets

My query: 

```sql
select ZFOLDER,ZMODIFICATIONDATE1,ZIDENTIFIER,ZTITLE1,ZSNIPPET
from ZICCLOUDSYNCINGOBJECT
where ZFOLDER = 56
order by ZMODIFICATIONDATE1 desc
```

... which is this URL (if Datasette is running locally):

http://127.0.0.1:8001/NoteStore?sql=select+ZFOLDER%2CZMODIFICATIONDATE1%2CZIDENTIFIER%2CZTITLE1%2CZSNIPPET%0D%0Afrom+ZICCLOUDSYNCINGOBJECT%0D%0Awhere+ZFOLDER+%3D+56%0D%0Aorder+by+ZMODIFICATIONDATE1+desc

