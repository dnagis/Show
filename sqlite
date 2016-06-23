Geographic Information Software

for file in *.tar.xz; do tar -xf $file -C /; done

tout compilé sans open-gl

GRASS
-cmake
	export CXXFLAGS='-I/usr/include/c++/5.1.0/ -I/usr/include/c++/5.1.0/x86_64-vinvin-linux-gnu/'
-proj4: proj.4-master.zip (with proj-datumgrid-1.3.zip support??? ce qu'il disent sur le wiki grass
	./configure --prefix=/usr --libdir=/usr/lib64 
-GDAL/OGR
	libjpeg-> http://www.ijg.org/files/jpegsrc.v8d.tar.gz ./configure --prefix=/usr --libdir=/usr/lib64 --enable-shared
	libTIFF-> après libjpeg ftp://ftp.remotesensing.org/pub/libtiff/tiff-4.0.6.tar.gz export CXXFLAGS... et ./configure --prefix=/usr --libdir=/usr/lib64
	export CXXFLAGS='-I/usr/include/c++/5.1.0/ -I/usr/include/c++/5.1.0/x86_64-vinvin-linux-gnu/'
	./configure --prefix=/usr --libdir=/usr/lib64
	il faut le binding gdal-python (chez pypi, astuces: python setup.py install --user et CPPFLAGS 
	et mettre le fichier usr/lib64/python2.7/site-packages/easy-install.pth, et il faut que numpy soit installé avec les headers (pas nécesssaire au runtime though)
-Python: sqlite3 et NumPy compilés dans python.tar.xz voir python.configures

-wxPython 3.0.2
	export PKG_CONFIG_PATH=/usr/X11R7/lib/pkgconfig:/usr/X11R7/share/pkgconfig
	export CXXFLAGS='-I/usr/include/c++/5.1.0/ -I/usr/include/c++/5.1.0/x86_64-vinvin-linux-gnu/'
	export CPPFLAGS='-I/usr/include/c++/5.1.0/ -I/usr/include/c++/5.1.0/x86_64-vinvin-linux-gnu/'	
	mkdir bld; cd wxPython
	python build-wxpython.py --build_dir=../bld -> va planter mais va créer les fichiers à modifier
	build/tools/build-wxwidgest.py commenter --with-opengl et --enable-mediactrl (gstreamer)
	(trouvé chez http://weerapurage.org/blog/2013/11/12/compile-wxpython-2-dot-9-5-dot-0-in-linux/)
	pour désactiver le build de glcanvas: wxPython/config.py BUILD_GLCANVAS = 0 (pas = 1, sinon pb src/gtk/glcanvas_wrap.cpp -> wxGLCanvas' not declared in this scope
	python build-wxpython.py --build_dir=../bld --prefix=/usr (--install ne pas faire car après je ne sais plus quoi mettre comme fichiers)
	
	install, chaud patate... python >>> import wx il faut le faire depuis root sinon tu peux avoir des fausses joies si tu es dans le dir de build
	Premier essai: cp le wx/ de wxPython/ dans /usr/lib64/python2.7/site-packages/ ....mais....
	pb avec wxversion.py, qui cherche un dir "wx-[0-9].*" avec un subdir wx/ dedans 
	python >>> import wxversion puis wxversion.getInstalled() wxversion.ensureMinimal('2.4') attention select doit être exact match pour fonctionner...
	Deuxième essai: mkdir wx-3.0 dans site-packages/, y mettre le wx/ avec un symlink wx->wx-3.0/wx et cp wxPython/wxversion/wxversion.py dans .../site-packages/
	
	
	
	pour demo: copier le dir wxPython/demo dans /root et y faire python demo/demo.py
	
	grass au configure wxwidgets il veut wx-config, qui est dans bld/ où je fais make install après avoir modifié la libdir en lib64 dans la Makefile
	petite erreur qui bloque wxrc: dans lib64/ ln -s lib64wx_baseu_xml-3.0.so.0 libwx_baseu_xml-3.0.so.0 et 
	ln -s lib64wx_baseu-3.0.so.0 libwx_baseu-3.0.so.0
	il faut les includes que tu récupères après le make install dans bld, et il y a un fichier setup.h perdu (grass ne le trouve pas)
	cp usr/lib64/wx/include/gtk2-unicode-3.0/wx/setup.h usr/include/wx-3.0/wx/
	
	****intermediate thoughts on wxPython****
	pratique pour bidouiller un module pendant que tu es en ligne de commande: reload(wx) soit reload(nom_du_module)
	dans wxPython/ -> tar -cJf wxPython_vinvinux.tar.xz -T installed_files.txt -> bof, python setup.py install-> bof aussi, pas clair
	dans bld/ make install DESTDIR pour les librairies?? finalement a pas l'air indispensable
	histoire du /usr/lib dû au lib32 pour android: sfs indispensable pour studio, je vire le sfs au moins pour builder wxPython
	
	
-grass 7
export PKG_CONFIG_PATH=/usr/X11R7/lib/pkgconfig:/usr/X11R7/share/pkgconfig
export CXXFLAGS='-I/usr/include/c++/5.1.0/ -I/usr/include/c++/5.1.0/x86_64-vinvin-linux-gnu/'
cp ../geo/*.h /usr/include/rpc #(sinon dans lib/ plante car pas de rpc/types.h ni xdr.h http://linux.die.net/include/rpc/xdr.h|types.h)
./configure --prefix=/usr --libdir=/usr/lib64 --with-opengl=no --with-fftw=no --with-freetype=no --with-wxwidgets=/usr/bin/wx-config --with-pthread --with-readline
il faut gnu tar pour le make install
Il faut des symlinks dans ~/.grass7/toolboxes vers /usr/grass-7.0.0/gui/wxpython/xml/main_menu.xml et /usr/grass-7.0.0/gui/wxpython/xml/toolboxes.xml
sinon erreur python xml parse tree menutree , heureusement j'ai trouvé http://osgeo-org.1560.x6.nabble.com/g-gui-not-launching-in-grass70-after-install-td5092922.html qui 
m'a orienté vers les toolboxes

Runtime:
**************REFERENCE GRASS TOP LEVEL******************
https://grass.osgeo.org/grass70/manuals/index.html
**************REFERENCE GRASS TOP LEVEL******************
Des rasters pour le background:
Chez natural earth télécharger un cultural (10m par exple)
File>Import Vector>Common Input Format>Dir>Browse>Là où tu as unzippé le cultural
Cocher les layers country (31) et populated places (villes, =10)
Sur le display zoom out max (icone Zoom to Selected map layers)
Pour faire apparaitre les labels des populated places: Layer Manager -> Map Layers 
Clic Droit sur populated places>Properties>Labels. Nb molette permet de zoomer/dezoomer.
--Pour les WMS="Web Map Service":
http://osm-wms.de/; https://grass.osgeo.org/grass70/manuals/r.in.wms.html; https://grasswiki.osgeo.org/wiki/WMS
g.region n=45 s=42 w=3 e=8 res=0.001 -p
r.in.wms url=http://irs.gis-lab.info -c pour voir les layers disponibles
r.in.wms url=http://irs.gis-lab.info layers=osm --overwrite output=osm
curl "http://129.206.228.72/cached/osm?LAYERS=osm_auto:all&STYLES=&SRS=EPSG%3A4326&FORMAT=image%2Fpng&SERVICE=WMS&VERSION=1.1.1&REQUEST=GetMap&BBOX=-178.217598,18.924782,-66.969271,71.406235&WIDTH=1080&HEIGHT=1080" -o "image.png"

http://www.ing.unitn.it/~grass/docs/tutorial_64_en/index.html
http://grassbook.org/datasets/datasets-3rd-edition/ -> je download GRASS 7 LOCATION nc_spm_08 (145MB)
je tar -xf nc_spm_08_grass7.tar.gz /root/grassdata/ , et il faut chown -R root:root /root/grassdata/ sinon permission denied
puis http://grasswiki.osgeo.org/wiki/GRASS_6_Tutorial/Getting_Started

grass70 -text #après ça ressemble beaucoup à du bash...

Pour de la commandline direct:
https://pvanb.wordpress.com/2011/04/25/creating-a-map-using-the-command-line/
http://www.ing.unitn.it/~GRASS/docs/tutorial_64_en/htdocs/introduzione/monitor.html
d.mon --h; liste les available monitors
d.mon start=wx0
****************************************************************************************************************************
*****libspatialite 4.4.0_RC1:
-prerequisites:
	-proj.4
	-GEOS: http://trac.osgeo.org/geos/
		export CPPFLAGS='-I/usr/include/c++/5.1.0/ -I/usr/include/c++/5.1.0/x86_64-vinvin-linux-gnu/'
		./configure --prefix=/usr --libdir=/usr/lib64
		install via DESTDIR pour modifier libgeos[_c].la : /usr/lib64/libstdc++.la et pas ...../vinux/...../libstdc++.la

libspatialite 4.4.0_RC1:
	unset CPPFLAGS
	./configure --prefix=/usr --libdir=/usr/lib64 --disable-freexl
	runtime: sqlite3> SELECT load_extension('mod_spatialite'); ou sqlite3> .load mod_spatialite
	modifier mod_spatialite.la et libspatialite.la: /usr/lib64/libstdc++.la et pas ...../vinux/...../libstdc++.la
	
	si --disable-geos libspatialite pb DFX: 4.3.0a: ld -lspatialite: undefined reference to `gaia[CreateDxfParser,ParseDxfFile_r,DestroyDxfParser]` https://groups.google.com/forum/#!topic/spatialite-users/ptdh7Gl2TfY:
		The problem is that, the load_dxf function that was added in libspatialite 4.3 (in spatialite.c) is not in a #ifndef OMIT_GEOS / #endif block while it uses functions (gaiaCreateDxfParser) that are only defined (in dxf_parser.c) if OMIT_GEOS is not set.

spatialite-tools-4.4.0-RC1 ./configure --prefix=/usr --libdir=/usr/lib64 --disable-freexl --disable-readosm
	plante dans shp_sanitize car undef ref à gaiaMakeValid et gaiaMakeValidDiscarded qui devraient être dans /usr/lib64/libspatialite.so
	https://git.osgeo.org/gogs/rttopo/librttopo --->>> plus tôt peut être? car gg_rttopo.c y fait appel dans libspatialite/src/gaiageo/
	readosm au passage: https://www.gaia-gis.it/fossil/readosm/index
	
runtime spatialite: table = Points colonne = Geometry 	
SELECT DISTINCT Srid(Geometry) FROM Points; -> -1 = unset
UPDATE geometry_columns SET srid=4326;
UPDATE Points SET Geometry = SetSRID(Geometry, 4326); (tu peux pas le faire avant geometry_columns sinon Error: Points.Geometry violates Geometry constraint [geom-type or SRID not allowed]
	avec ça .dropkml fonctionne, mais pas geojson
	
INSERT INTO Points (lat, long) 
select one.*, two.lat, two.lon
from cellid as one
join cells as two
on (one.CELLID = two.cell and one.MNC = two.net and one.RADIO = two.radio and one.LAC = two.area)
order by id;	

Select AddGeometryColumn ('Points', 'Geometry', 4326, 'POINT', 2);
UPDATE geometry_columns SET srid=4326;

UPDATE Points SET Geometry=MakePoint(cast(long as REAL), cast(lat as REAL),  4326);
****************************************************************************************************************************
sqlite3
.separator ;
.import SelStim.csv selstim
.schema selstim
.mode column	
.headers on
.width 50 50
.output foobar.csv
.output #pour revenir à l'output de base
.read requete.sql
changer juste un record, en identifier par rowid ce qui sera très pratique pour faire à chaque ligne.
update matable set Numero_Dossier='zobi' where rowid=4;

merge de deux fichiers.db: en bash:
sqlite3 fichier_a_inserer.db .dump | sqlite3 fichier_destination_finale.db

puis inner join de deux db pour avoir lat et long
[INSERT INTO Points (lat, long)] #au cas où tu veuilles remplir une nouvelle table, simplement à ajouter au début
select one.*, two.lat, two.lon
from cellid as one
join cells as two
on (one.CELLID = two.cell and one.MNC = two.net and one.RADIO = two.radio and one.LAC = two.area)
order by id;

#smo: inner join avec 2 tables sur numero dossier mdf (format NNNN00022 et NNNN)
select Adresse, Numero_de_dossier_MediFirst, "Num dossier" from smo inner join adr where substr(smo.Numero_de_dossier_MediFirst,-5,-5)=adr.[Num dossier];

select Numero_Dossier, Nom,  Nb_ovocytes_retenus from selstim inner join location where selstim.Adresse_Patient_Principal=location.adresse and location.dist_1 < 10;

#chaud davoir le format YYYY-MM-DD à partir du DD/MM/YYYY de medifirst:
select "Code Postal homme" from smo where (substr([Date de l'examen], 7, 4) || "-" || substr([Date de l'examen], 4, 2) || "-" || substr([Date de l'examen], 1, 2))  >= "2016-10-26";

****************************************************************************************************************************
code postal avec geoloc en open source:
http://public.opendatasoft.com/explore/dataset/correspondance-code-insee-code-postal/?tab=table
http://giscollective.org/geolite-location-databases/
****************************************************************************************************************************
API Keys: Geocoding - GEOPY - OpenCellID - Google
opencellid: 789443d4-f4be-4eee-a929-b88587f272ca (mail amu.fr code bassu)
api.ign.fr pour avoir la clé de l'API: login vincent.achard@imbe.fr pwd geopy -> marche plus en 2016
google dev console -> clé api serveur récup le 16 01 16 -> AIzaSyCeo21STvv5YsB3fcKUSuyNJfd_bfD7K-4 #ne pas oublier de l'activer (présentation, google maps geocoding api -> activer)
google maps api documentation: https://developers.google.com/maps/documentation/geocoding/intro?hl=fr
et google python: https://developers.google.com/maps/web-services/client-library?hl=fr#usage_python

python install geopy (juste copier le dir geopy/geopy dans /usr/lib64/python27/site-packages:

from geopy.geocoders import IGNFrance || from geopy.geocoders import GoogleV3
geolocator = IGNFrance(api_key="obxbfbdgkqp281ewwlqrietu", referer="localhost", scheme='http')

geolocator = GoogleV3(api_key='AIzaSyCeo21STvv5YsB3fcKUSuyNJfd_bfD7K-4', domain='maps.google.fr') #sans le domain 13100 tombe en malaysie!
monprox={"https":"dr20961:4cht4L16@proxyweb.aphm.ap-hm.fr:8080"} #pour l'APHM
geolocator = GoogleV3(api_key='AIzaSyCeo21STvv5YsB3fcKUSuyNJfd_bfD7K-4', domain='maps.google.fr', proxies=monprox)  #pour l'APHM, sinon virer le proxies=
geolocator = IGNFrance(api_key="obxbfbdgkqp281ewwlqrietu", referer="localhost", proxies=monprox) #pour l'APHM, sinon virer le proxies=

con = None
con=lite.connect('smo.db')
con.text_factory = str #str ou unicode(defaut)
cur=con.cursor()
cur.execute("DROP TABLE IF EXISTS location")
cur.execute("create table location(address TEXT, latitude REAL, longitude REAL)")

#cur.execute("select [Code Postal homme] from smo where (substr([Date de l\'examen], 7, 4) || \"-\" || substr([Date de l\'examen], 4, 2) || \"-\" || substr([Date de l\'examen], 1, 2))  >= \"2016-01-20\"")
cur.execute("select Ville,Adresse,[Code postal] from smo inner join adr where substr(smo.Numero_de_dossier_MediFirst,-5,-5)=adr.[Num dossier] \
and (substr([Date_de_l\'examen], 7, 4) || \"-\" || substr([Date_de_l\'examen], 4, 2) || \"-\" || substr([Date_de_l\'examen], 1, 2)) >= \"2013-06-01\"");


rows=cur.fetchall()
monset=set(rows) #'uniq' la list rows

for row in monset:
	if row[0] is not None:
		#if re.search('[0-9]', row[0]):
		time.sleep(.5) #sinon j'ai une erreur de l'API à la fac (faut dire que ça envoie les watts niveau vitesse...)
		adresse_complete=row[0]+' '+row[1]+' '+row[2]
		print 'on va tenter le geocodage de:'+adresse_complete
		location=geolocator.geocode(adresse_complete)
		if location is not None:
			print(location.address,location.latitude,location.longitude)
			cur.execute("insert into location(address, latitude, longitude) values(?, ?, ?)", (location.address,location.latitude,location.longitude))	
			con.commit()
****************************************************************************************************************************
extraction MEDIFIRST #ticket adresse postale dans requête smo ouvert le 23/11/15: AMP-195, contournement avec le bouton export coord patients

#spermogrammes: extraction que pour 3 ans max: +eurs extractions et sed 1d in.csv > out.csv (vire premiere ligne) puis cat 1.csv 2.csv 3.csv > total.csv pour append
#trimmer que les colonnes que je veux recup (passer par gnumeric et en première ligne faire une suite de nombres) et pas en double,
#virer les diacritiques et autres joyeusetés unicode (convertir en ASCII) et remplacer les espaces par des underscores dans la 1ère colonne:
cat smo.csv | cut -d";" -f 2,5-13,26-30,52,55-59,114,132 | iconv -f ISO88591 -t ASCII//TRANSLIT | sed '1 s/ /_/g' > _smo.csv
iconv -f ISO88591 -t ASCII//TRANSLIT <in >out aussi pour le csv des adresses ("export coord patients" de mdf) sinon plante avec l'api

sed -i 's/[^"];[^"]//' SelStim.csv #virer les délimiteurs qui se trouvent dans les guillemets
cut -d";" -f 1-4,9-11,17,18,24-27,33,34,40,41,44,45,78,79,90,130,134,137-146,156-157 SelStim.csv #récup que certaine colonnes pour selstim
cut -d";" -f 1,9 essai.csv | sort | uniq | wc -l # récup que num_dossier et adresse et compte du nombre de lignes
virer les guillemets (utile??) avec: sed -i 's/"//g' essai.csv
grep -ao '[^"];[^"]' SelStim.csv #-a pour text sinon csv=binary car \000 NULL qq part
****************************************************************************************************************************
sqlite vers grass
grass70 -text
v.in.db table=location database=/initrd/mnt/dev_save/packages/geo/sql/selstim.db x=longitude y=latitude output=patients
l'exporte dans ~/grassdata/NewLocation/root/vector/patients et tu l'importes avec file>map display>add vector
****************************************************************************************************************************
Points convertis avec www.GeoFree.fr. Le 10/11/2015 20:34:15
Systeme départ :Lambert 93
Systeme arrivée :Geographique WGS84
					X départ;	Y départ; 	X Arrivée; 	Y Arrivée
VIGNIERES 			861438;		6311115;	5.0087948	43.8805598
AVIGNON MAIRIE 		844683;		6318549;	4.8024978	43.9510810
ARLES 				831701;		6287702;	4.6328675	43.6760988
TOULON 				938333;		6229559;	5.9272618	43.1255292
Cannes BROUSSAILLES 1023661;	6282154;	7.0064805	43.5645345
Port de BOUC 		860589;		6257826;	4.9815982	43.4014186
NICE 				1045339;	6298600;	7.2853533	43.7020726

insert into prelevement values ('vignieres', 43.8805598, 5.0087948);
insert into prelevement values ('avignon', 43.9510810, 4.8024978);
insert into prelevement values ('arles', 43.6760988, 4.6328675);
insert into prelevement values ('toulon', 43.1255292, 5.9272618);
insert into prelevement values ('cannes', 43.5645345, 7.0064805);
insert into prelevement values ('port_de_bouc', 43.4014186, 4.9815982);
insert into prelevement values ('nice', 43.7020726, 7.2853533);
v.in.db table=prelevement database=/initrd/mnt/dev_save/packages/geo/sql/selstim.db x=longitude y=latitude output=prelevement

#########################################NODEJS#############################################
vinvinux:
nodejs.org node-v4.2.4

./configure --shared-zlib --shared-openssl --prefix=/usr
export CXXFLAGS='-I/usr/include/c++/5.1.0/ -I/usr/include/c++/5.1.0/x86_64-vinvin-linux-gnu/'
out/deps/v8/tools/gyp/mksnapshot.target.mk virer la ligne -fuse-ld=gold \ (voir ci dessous, erreur collect2...)
CC=gcc make puis CC=gcc make install
20 minutes de build

Problème = pas de --libdir au configure: npm n installe qu en /usr/lib/node_modules. Je fouille pas il suffit de déplacer les fichiers.
Ne pose problème que pour les install globales comme bower: npm install bower -g -> installe en /usr/lib, il suffit de déplacer en /usr/lib64  
et de gérer le bin (symlink en /usr/bin), 

pb avec ld pas trouvé: "collect2: fatal error: cannot find 'ld'" au bout d un quart d heure environ (tenté sans shared libs: idem)
Solution: dans out/deps/v8/tools/gyp/mksnapshot.target.mk
ce qui fait planter dans la ligne de commande g++ c est un -fuse-ld=gold (enlever li 122 dans LDFLAGS_Release)

ancien: rpi:
node-v.4.1.1/out/Makefile:
CC.target ?= gcc
CXXFLAGS.target ?= $(CXXFLAGS) -I/usr/include/c++/5.1.0 -I/usr/include/c++/5.1.0/x86_64-vinvin-linux-gnu


./configure --dest-cpu=arm --with-arm-float-abi=hard --dest-os=linux --prefix=${DEV_SAVE}cross/arm-linux-gnueabihf/
node-v.4.1.1/out/Makefile: 
CC.target ?= arm-linux-gnueabihf-gcc
CFLAGS.target ?= $(CFLAGS)
CXX.target ?= arm-linux-gnueabihf-g++
CXXFLAGS.target ?= $(CXXFLAGS) -I/usr/include/c++/5.1.0/
LINK.target ?= $(LINK)
LDFLAGS.target ?= $(LDFLAGS)
AR.target ?= arm-linux-gnueabihf-ar
#######################################BITBUCKET-GIT#####################################
#Basculer du WIFI APHM vers HotSpot Android (sinon le proxy APHM bloque...)
#unset http_proxy; wpa_cli reconfigure; kill -USR2 `pidof udhcpc`; kill -USR1 `pidof udhcpc`
git clone https://vachard@bitbucket.org/_larry/geoloc.git
git clone https://vachard@bitbucket.org/_larry/patientsurvey.git
git clone https://vachard@bitbucket.org/_larry/radiowaves.git
pwd=*16
git status
git add _RESSOURCES/compile_node ou git add .
git config --global user.email "vincent.achard@gmail.com"
git config --global user.name "vachard"
git commit -m "mon commentaire de folie"
git push https://vachard@bitbucket.org/_larry/geoloc.git master

