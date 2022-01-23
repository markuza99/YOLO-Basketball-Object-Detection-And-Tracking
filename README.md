# YOLO-Basketball-Object-Detection-And-Tracking
## Neophodni resursi i datoteke
  - Osnovu našeg projekta predstavlja darknet framework autora AlexeyAB: https://github.com/AlexeyAB/darknet
  - Naši konfiguracioni fajlovi se mogu naći u repozitoriju i na linku: https://drive.google.com/file/d/1Gt-Gczp2lnkoutlO_CYHCVBvU7W7T2x0/view?usp=sharing
  - Naši weights fajlovi se mogu naći na linku: https://drive.google.com/file/d/1x6PZA8onL0GMsSgOhE-oNalUvskiYJMu/view?usp=sharing
  - Svi neophodni fajlovi koje smo koristili za treniranje i testa se nalaze na linku: https://drive.google.com/file/d/1x6PZA8onL0GMsSgOhE-oNalUvskiYJMu/view?usp=sharing
  - Za testiranje video snimaka koristili smo videe koji se nalaze na linku: https://drive.google.com/file/d/1fYc5D-eBD99kd6wRt-tSZ4Vw64uYLEwW/view?usp=sharing

## Kako kompajlirati projekat
Detaljna uputstva za kompajliranje projekta se mogu naći na narednik linkovima:
  - Windows: https://github.com/AlexeyAB/darknet#how-to-compile-on-windows-using-vcpkg
  - MacOS/Linux: https://github.com/AlexeyAB/darknet#how-to-compile-on-linuxmacos-using-cmake
## Kako smo trenirali naš model
1. Prvo što smo uradili jeste prikupljanje i labeliranje naseg skupa podataka (upotrebom labelImg alata)
        <img src="https://github.com/markuza99/YOLO-Basketball-Object-Detection-And-Tracking/blob/main/screenshot.png">
2. Nakon toga smo uzeli odgovarajući config file koji se generisao nakon kompajlirala i izvršili sledeće promene:
- Promenili liniju batch u batch=64
- Promenili liniju subdivisions u subdivisions=16 (ili u neki drugi veći broj koji je deljiv sa 64 u zavisnosti od naše RAM memorije)
- Promenili liniju max_batches u 6000 (formula za max_batches je broj_klasa*20000, pritom taj broj ne bi trebao da bude manji od ukupnog broja slika ili od 6000)
- U svakom ` [yolo] ` sloju smo izmenili liniju classes u classes=2 (Uzimajući u obzir da imamo samo 2 klase: osoba i lopta)
- U svakom `[convolutional]` sloju koji se nalazi iznad `[yolo]` sloja smo izmenili filter liniju po sledećoj formuli: filter=(num_classes+5)*3. Pošto smo imali samo 2 klase, filter je bio 21
3. Zatim smo kreirali neophodne names i data fajlove koje smo sačuvali u darknet/data direktorijumu.S
- names fajl je sadržao imena naših klasa:
```
ball
person
```
- data fajl je sadržao podatke o test skupu, trening skupu, broju klasa, direktorijumu za čuvanje kreiranih weight fajlova i referencu na names fajl.
```
classes =2
train = data/train.txt
valid = data/test.txt
backup = backup/
names = data/obj.names
```
4. Dodali smo folder obj (Gde su se nalazile slike za trening kao i odgovarajući txt fajl labeliranih klasa) i training.txt datoteku koja je sadržala putanje svih slika za trening (data/*.jpg)
5. pokretanjem komande `darknet detector train .\data\obj.data <putanja do konfiguracionog fajla> <putanja do pre-trained weights fajla>` započelo bi se testiranje.

## Kako smo testirali model

Kako bismo dobili odgovarajuću evaluaciju pokrenuli smo komandu `darknet detector map data/obj.data <putanja do konfiguracionog fajla> <putanja do weights fajla> -dont_show -ext_output`. 

## Korisne komande

- YOLO detekcija - slike: `darknet detector test <putanja do data fajla> <putanja do config fajla> <putanja do weights fajla> <putanja do slike>`
- YOLO detekcija - video: `darknet detector demo  <putanja do data fajla> <putanja do config fajla> <putanja do weights fajla> <putanja do videa>`

