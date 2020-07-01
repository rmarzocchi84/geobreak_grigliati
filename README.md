# Istruzioni

Con l'evoluzione della libreria PROJ alle versioni 5 e 6 è stato introdotto un file sqlite denominato proj.db che contiene tra gli altri elementi una tabella denominata *grid_transformation*, con l'elenco di tutte le trasformazioni fra sistemi di riferento

Volendo usare un grigliato nei software OsGeo (QGIS, ogr2ogr, etc) su S.O. Windows è necessario seguire questa semplice procedura: 

1) copiare il file con i grigliati nella cartella contenente i grigliati 

Es. *C:\OSGeo4W64\share\proj*

2) inserire le righe corrispondenti all'interno della tabella denominata *grid_transformation*. Io consiglio di usare il SW DB Browser che consente di accedere al file SQLITE e lanciare una query SQL

Ad esempio con questa istruzione inserisco la conversione da ETRF2000 a Roma40

```
INSERT INTO "grid_transformation"
VALUES ('PROJ','EPSG_6706_TO_EPSG_4265_GENOVA','RDN2008 TO Monte Mario (ROMA40) - Area di Genova',
'Per l''area di Genova - USO GRIGLIATI IGM',
'LOCALE',
'EPSG','9615','NTv2',
'EPSG','6706','EPSG','4265',
'EPSG','1127', 
NULL,
'EPSG','8656','Latitude and longitude difference file',
'44080835_44400922_F00_R40.gsb',
NULL,NULL,NULL,NULL,NULL,NULL,NULL,0);
```

Con questa i grigliati per la trasformazione inversa:

```
INSERT INTO "grid_transformation"
VALUES ('PROJ','EPSG_4265_TO_EPSG_6706_GENOVA','Monte Matio (Roma40) to RDN2008 - Area di Genova',
'Per l''area di Genova - USO GRIGLIATI IGM',
'LOCALE',
'EPSG','9615','NTv2',
'EPSG','4265','EPSG','6706',
'EPSG','1127', 
NULL,
'EPSG','8656','Latitude and longitude difference file',
'44080835_44400922_R40_F00.gsb',
NULL,NULL,NULL,NULL,NULL,NULL,NULL,0);
```

3) usare QGIS o ogr2ogr o GRASS per le trasformazioni tra SR avendo cura di definire un nuovo CRS che contenga il parametro **nadgrids**

es. codice ogr2ogr senza uso grigliati

```
.\ogr2ogr.exe -f "ESRI Shapefile" -s_srs EPSG:3003 -t_srs EPSG:7791 output.shp input.shp
```

es. codice ogr2ogr con uso grigliati

```
.\ogr2ogr.exe -f "ESRI Shapefile" -s_srs '+proj=tmerc +lat_0=0 +lon_0=9 +k=0.9996 +x_0=1500000 +y_0=0 +ellps=intl +nadgrids=../share/proj/44080835_44400922_R40_F00.gsb +units=m +no_defs' -t_srs EPSG:7791 output.shp input.shp
```



Discorso analogo si potrebbe fare per PostgreSQL/PostGIS!

Se avete commenti o suggerimenti aprite *issues* su questo repository o contattatemi via mail (roberto.marzocchi@gter.it)





