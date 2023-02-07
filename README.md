# JBrowse CLI

## Prerrequisitos 

- [Node.js 12+](https://nodejs.org/en/download/). No recomendamos usar `apt` para instalar Node.js, ya que suele instalar versiones antiguas. Otras alternativas son [NodeSource](https://github.com/nodesource) o [NVM](https://github.com/nvm-sh/nvm).

- [Samtools](http://www.htslib.org/) utilizando `sudo apt install samtools` o `brew install samtools`.

- [tabix](http://www.htslib.org/doc/tabix.html) utilizando `sudo apt install tabix` y `brew install htslib`.

- [genometools](http://genometools.org/) utlizando `sudo apt install genometools` o `brew install brewsci/bio/genometools`.

- [Descargar archivos](https://drive.google.com/drive/u/4/folders/1rYza8nhP4377urKF0E8Atl_DwukOCSJ8) desde Google Drive para poder visualizar todo correctamente.

## Instalar JBrowse CLI


Para instalar globalmente JBrowse CLI, ejecutar
```
npm install -g @jbrowse/cli
```
Luego de este comando se puede verificar la instalación con 
```
jbrowse --version
```
Esto nos dirá la version actual de JBrowse CLI


## Descargar JBrowse 2
En el directorio donde desees descargar Jbrowse 2, ejecutar 
```
jbrowse create jbrowse2
```
Para asegurarte de que todo funciona correctamente, el nuevo directorio debería verse así
```
jbrowse2/
├── asset-manifest.json
├── favicon.ico
├── index.html
├── manifest.json
├── robots.txt
├── static/
├── test_data/
└── version.txt
```

## Ejecutar JBrowse 2
JBrose 2 requiere de un servidor para funcionar. En nuestro caso, para ejecutarlo localmente de forma simple usaremos el siguiente comando en el directorio correspondiente 
```
cd jbrowse2/
npx serve .
```
Luego nos dirigiremos a la direccion especificada por el CLI (muy probablemente `http://localhost:3000`). Si la página se ve de la siguiente forma, podemos continuar a configurarlo con nuestra lista de genomas.

![image](https://user-images.githubusercontent.com/79536480/213889551-89370655-c01d-44d6-abfb-85b1f4b25914.png)


## Agregar los genomas
Lo primero que haremos, si todavia no está hecho, es clonar este repositorio
```
git clone https://github.com/nachoortego/canndico-jbrowse.git
```

Para agregar los FASTA, utilizaremos `samtools` para indexar y luego subiremos el archivo. Reemplazar la direccion destino por el directorio de instalacion de jbrowse2
```
samtools faidx Cannabis_sativa_Jamaican_Lion.fasta
jbrowse add-assembly Cannabis_sativa_Jamaican_Lion.fasta --load copy --out /jbrowse2

samtools faidx GCA_000230575.5_ASM23057v5_genomic.fna
jbrowse add-assembly GCA_000230575.5_ASM23057v5_genomic.fna --load copy --out /jbrowse2

samtools faidx GCA_003417725.2_ASM341772v2_genomic.fna
jbrowse add-assembly GCA_003417725.2_ASM341772v2_genomic.fna --load copy --out /jbrowse2

samtools faidx GCA_013030365.1_ASM1303036v1_genomic.fna
jbrowse add-assembly GCA_013030365.1_ASM1303036v1_genomic.fna --load copy --out /jbrowse2

samtools faidx GCF_900626175.2_cs10_genomic.fna
jbrowse add-assembly GCF_900626175.2_cs10_genomic.fna --load copy --out /jbrowse2

```

Para agregar los GFF, ordenamos e indexamos con `tabix` y hacemos el resto de operaciones con `genometools`
```
gt gff3 -sortlines -tidy -retainids CBDRx.gff > CBDRx.sorted.gff
bgzip CBDRx.sorted.gff
tabix CBDRx.sorted.gff.gz
jbrowse add-track CBDRx.sorted.gff.gz --load copy

gt gff3 -sortlines -tidy -retainids Finola.gene.gff3 > Finola.gene.sorted.gff3
bgzip Finola.gene.sorted.gff3
tabix Finola.gene.sorted.gff3.gz
jbrowse add-track Finola.gene.sorted.gff3.gz --load copy

gt gff3 -sortlines -tidy -retainids JamaicanLionDASH.gene.gff3 > JamaicanLionDASH.gene.sorted.gff3
bgzip JamaicanLionDASH.gene.sorted.gff3
tabix JamaicanLionDASH.gene.sorted.gff3.gz
jbrowse add-track JamaicanLionDASH.gene.sorted.gff3.gz --load copy

gt gff3 -sortlines -tidy -retainids PurpleKush.gene.gff3 > PurpleKush.gene.sorted.gff3
bgzip PurpleKush.gene.sorted.gff3
tabix PurpleKush.gene.sorted.gff3.gz
jbrowse add-track PurpleKush.gene.sorted.gff3.gz --load copy
```
## Visualización de genomas

Con todos los genomas agregados y el servidor apropiadamente iniciado, para visualizar nuestros archivos FASTA con sus GFF correspondientes seguiremos los siguientes pasos
- New Session
- LinearGenomeView (Lauch View)
- Elegimos nuestro genoma en la pestaña Assembly
- Open
- Open track selector
- Marcamos las casillas del genoma de referencia y su respectivo GFF

Ahora, con el zoom adecuado, deberíamos de poder ver todo correctamente como la siguiente imagen 

![image](https://user-images.githubusercontent.com/79536480/213890251-ff4b9693-6486-41b1-ab82-8c6378e0e3fc.png)

## Agregar nuevos genomas

En caso de querer agregar nuevas opciones de visualización, se debe descargar el `.fasta` y `.gff` correspondientes y agregarlos a JBrowse de la misma forma que se vió anteriormente. A veces los cromosomas del FASTA no coinciden con las referencias del GFF, por lo que será necesario hacer lo siguiente:

1. En el archivo `.fasta` identificar los encabezados (`>`).
2. Luego identificar los 10 cromosomas, que serán comunmente los primeros 10 encabezados del archivo.
3. Susituir el primner encabezado por `>Chr1`.
4. Repetir respectivamente para el resto de cromosomas

Con esto resuelto, todo debería mostrarse como es debido.










