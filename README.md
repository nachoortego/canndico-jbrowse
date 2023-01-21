# JBrowse CLI

## Prerrequisitos 

- [Node.js 12+](https://nodejs.org/en/download/). No recomendamos usar `apt` para instalar Node.js, ya que suele instalar versiones antiguas. Otras alternativas son [NodeSource](https://github.com/nodesource) o [NVM](https://github.com/nvm-sh/nvm).

- [Samtools](http://www.htslib.org/) utilizando `sudo apt install samtools` o `brew install samtools`.

- [tabix](http://www.htslib.org/doc/tabix.html) utilizando `sudo apt install tabix` y `brew install htslib`.

- [genometools](http://genometools.org/) utlizando `sudo apt install genometools` o `brew install brewsci/bio/genometools`.

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
samtools faidx genome.fa
jbrowse add-assembly genome.fa --load copy --out /jbrowse2
```

Para agregar los GFF, ordenamos e indexamos con `tabix` y hacemos el resto de operaciones con `genometools`
```
gt gff3 -sortlines -tidy -retainids yourfile.gff > yourfile.sorted.gff
bgzip yourfile.sorted.gff
tabix yourfile.sorted.gff.gz
jbrowse add-track yourfile.sorted.gff.gz --load copy
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










