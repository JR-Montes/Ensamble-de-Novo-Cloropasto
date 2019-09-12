## Ensamble de cloropasto *de novo* y con referencia

By Jose Rubén Montes & David S. Gernandt

September 2019

**Introducción**

En este tutorial aprenderemos a ensamblar las secuencias completas de pinos piñoneros (Sección Parrya), con base en lecturas Illumina de 100-150 pb provenientes de datos Hyb-Seq (target enrichnment y genome skimming). El tutorial cuenta con dos partes, A y B. Eso significa que algunas secciones del tutorial de llevarán a cabo en Geneious (Parte A) y otras en la terminal o servidor (Parte B).

### Elementos con los que debes contar:

1. Secuencias originales Illumina PE (Paired end)
2. Genoma de un cloroplasto de tu grupo o afin
3. Acceso a un servidor o una terminal de Mac o PC
	1. Si tienes acceso al servidor debes contar con un usuario y contraseña 
4. Conocimiento de líneas de comandos (básico)
5. Programa para descomprimir archivos (WinRar)
6. Programa par abrir y modificar archivos e texto plano (Notepad o TextWrangler).
7. Programa FTP para tranferir archivos de tu computadora al servidor si fuera el caso (FileZilla)


### PARTE I A:  Geneious R 11.0.5. 

**Nota** Primero, la mayoría de las rutas en Geneious se ejecutan a partir del menú de herramientas o con el botón secundario del ratón. Segundo, nunca selecciones varias archivos al mismo tiempo porque tendrán problemas. Se debe hacer por cada individuo o archivo. Finalmente, si el tutorial no indica cambiar, seleccionar o deseleccionar una casilla, has caso omiso y deja todo por default.


**1A. Importar lecturas R1 y R2 a Geneious**.

Selecciona las siguiente ruta:

`File > Import > From file > Descargas` 

**2A. Parear secuencias R1 y R2**.

Selecciona dos de las secuencias R1 y R2 por individuo: 

```
Sequence > Set Paired Reads > Pair By: Pairs of documents > Relative Orientation: Forward/Reverse (inward, e.g. Illumina paired end) > Insert Size: 250 > Read Technology: Illumina > Paired End

``` 

**3A. Filtrar secuencias**.

Selecciona el archivo pareado que generaste en el punto anterior:

```
Tools > Workflows > Trimm and Filter > Trimming: Remove new trimmed regions from sequences > Error Probability Limit: 0.01 > Trim 5' End, Trim 3' End > Filtering: Each sequence > Post-Trim > 50

```

 
**4A. Mapear al genoma de referencia**.

```
Tools > Align/Assemble > Map to Rerences > Data: SELECCIONA TU REFERENCIA > Method: Medium-Low Sensivity/Fast > Fine Tuning:Iterate up to 5 times > Trim Before Mapping:Do not trimm > Results:Save assembly:Report used reads:In sub-folder: Include mates: Contigs: consensus sequences

```

**5A. Exportar archivo mapeado**

Vas a seleccionar uno de los archivos resultantes del mapeo `"Used Reads"` 


```
File > Export > Selected Document > File of type: FastQ Paired Files Forward/Reverse Compressed (Sanger scores) (*.fastq.gz)

```


**NOTA**: Esta acción generará dos archivos `UsedReads_1.fastq.gz` y `UsedReads_2.fastq.gz` los cuales debes descomprimir.



**6A. Quitar índices internos *1* y *2* de los archivos**

Abre con el editor de textos planos los archivos `UsedReads_1.fastq.gz` y `UsedReads_2.fastq.gz`. A cada archivo debes remover el **_1** y **_2**. Busca y reemplaza por nada y guarda los archivos.


```
@D00351:395:CBRU5ANXX:7:1111:7111:2172_2(REMOVE):N:0:CAGCGTTA+ACAGTGGT
TTTCACGATGCTCATTTCTCCAATTATGAAGCATGGTTGGCTGATCCCACTCACATCAAACCTAATGCCCAAGTATTTTGGCCAATAGTAGGCCAAGAAATATTGAATGGGGATGTAGGTGGAGG
+
BBBBBFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFB<FFFFFFFFFFFFFB/<<FFFFBFFFF
@D00351:395:CBRU5ANXX:7:1111:3678:2717_2(REMOVE):N:0:CAGCGTTA+ACAGTGGT
AGGAGATTTAAGTAAATAACCAAATATAGCACTTGGGTTAAGAGTTGGGTTCGTTATTTTTCTTACATCTCCACCGCCGGGAGCCCAAGTATCATATACACCTCCAAAATACAAAGCTTTG
+
B<<BBFF</FFFBBFFF/FFFFBFFFFFFFFFFFFFB</B<B<B<FFFFFFFB/BFFB/<FBFFFFFFFFFB<F<//BFF</77/FFFF/FFB/BFB//F//7BBFBFFFFFFF<BBBFBB
@D00351:395:CBRU5ANXX:7:1111:12560:2860_2(REMOVE):N:0:CAGCGTTA+ACAGTGGT
GAAAATGTAAAAACTACCATTAAAGCAGCCCAAGTTATACCGACTATATCCATACGGATCATGTTCTCTAAAAAATAATGATTTCATCATCGAGATGATAAGGGAACTATTAACCATAGTGCAAA
```

### PARTE I B: En el servidor o terminal

**1B. Ensamble _de novo_ a la referencia**

**2B**. Copia el archivo `UsedReads.fastq` a tu directorio con FileZilla y ensambla las lecturas pareadas _de novo_ con [SPAdes](http://spades.bioinf.spbau.ru/release3.5.0/manual.html).

**3B**. Abre tu sesión en el servidor y crea un directorio llamado `SPAdes`. En este directorio deben estar las lecturas `UsedReads_1.fastq.gz` y `UsedReads_2.fastq.gz`.

`mkdir SPAdes` 

Entra al directorio `SPAdes`

**4B**. Exporta SPAdes a tu PATH:

`PATH=$PATH:/usr/local/bin/SPAdes-3.6.2`

**5B**. Comprueba que se exportó SPAdes a tu PATH:

`echo $PATH`

**6B**. Debes de observar al final de la ruta el nombre de la versión de SPAdes

`/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/lib/cd-hit/bin/SPAdes-3.6.2`

**7B**. Corre el programa con la siguiente línea de comandos:

`spades.py --careful -1 UsedReads_1.fastq -2 UsedReads_2.fastq -o spadesAris_out`

**8B**. Cuando el programa termina de correr, se genera una carpeta `"spades_out"`. Dentro de esa carpeta existe un archivo `"scaffolds.fasta"`, el cual debes transferir a tu computadora e importarlo a Geneious. Cambia el nombre del archivo `"scaffolds.fasta"` para identificarlo desde al servidor antes transferirlo a tu computadora a traves de FileZilla.

`mv scaffolds.fasta scaffolds_SPADES_Aris04.fasta`


### PARTE II A: En Geneious


**1A. Mapear los scaffolds a la referencia y generar un plastoma quimérico.**

Importar el archivo `scaffolds_SPADES_Aris04.fasta` de SPAdes a Geneious. Al momento de importarlo se imprime una pestaña con la siguiente información. Tu debes elegir la opción `Create sequences list`

```
Grouping Sequences

 How do you want to store the sequences from scaffolds_SPADES_Aris04.fasta in Geneious?
 
```

2. Selecciona el archivo `scaffolds_SPADES_Aris04.fasta` que se recuperó de SPAdes
3. Conserva sólo los scaffolds >200 pb 
4. Generar una copia de la lista de scaffolds (botón derecho sobre el documento: `copy document` y `paste`. 
5. En la copia, seleccionar en la lista directamente todas las secuencias por debajo de 200 pb y borrarlas usando el botón derecho “Delete selected bases”.
6. Mapear los scaffolds >200 pb a la referencia. Sigue la ruta:
 
```
 Tools > Align/Assemble > Map to Referecne > Data: Reference sequence "SELECCIONA TU REFERENCIA" > Method: High Sensitivity/Medium, Fine Tuning: Iterate up to 5 times > Trim Before Mapping: Do not trimm > Results: Save assembly report, used reads, in sub-folder, Include mates, contigs, consensus sequences
 
```


- Nótese que si hubiéramos mapeado todos los scaffolds de SPADES hay muchos más segmentos pequeños en las áreas de sobrecobertura debidas al enriquecimiento de los dos loci de plastoma (izquierda). Al mapear sólo los scaffolds >200 pb conservamos los más importantes y reducimos el ruido en el área de sobrecobertura (derecha).

   
- En el nuevo archivo `"Contig"` en el sub-folder de scaffolds, anota las regiones sin cobertura (numbero de secuencias <1). Sigue la ruta:

`Menu > AnnotatePredict > Find Low/High coverage`

Las anotaciones _Low 0_ aparecerán en rojo. El `0` se refiere al valor de cobertura en esa zona, <1. 

- Selecciona:
`Find regions with coverage below` Elige: `Number of sequences` a `1` En la caja de abajo, deselecciona:`Find regions with coverage above`.
 

- Copiar todas las anotaciones a la secuencia consenso. Ubicarse sobre alguna de las anotaciones _Low 0_ y con el botón derecho:

`Annotation > Copy all Low to > Consensus`

No uses la opción default `"merge"` cuando copies las anotaciones.

**2A. Generar secuencia consenso**

1.Generar la secuencia consenso, utilizando la referencia para llenar las áreas sin cobertura:

`Tools > Generate Consensus Sequence > If no coverage call Ref > Trim to reference sequence`


- Hemos obtenido la secuencia quimérica en la que los scaffolds producidos de novo con SPADES cubren la mayoría del plastoma, y las regiones sin cobertura han sido llenadas con la secuencia de P. strobus, pero con anotaciones que nos permiten distinguir las áreas con secuencia de P. aristata (sin anotaciones) de las que contienen secuencia de P. strobus (con anotaciones Low 0 en color rojo). Cambiar el nombre de la secuencia consenso (el plastoma quimérico) de “Contig2” a algo más significativo, como “ArisChimeric1”. 

**3A. Mapear secuencia quimera a las secuencias originales "Used Reads".**

**1.** Ahora mapearemos las lecturas de P. aristata sobre la secuencia quimérica `"ArisChimeric1”` para tratar de ir rellenando las áreas que estaban sin cobertura y que actualmente tienen secuencia de P. strobus (con anotaciones Low 0). Es necesario regresar al archivo original de lecturas pareadas de P. aristata (filtradas, trimeadas y sin duplicados). Sigue la ruta:

```
Tools > Align/Assemble > Map to reference”, Data: "ArisChimeric1” > Method: Medium-Low Sensivity/Fast > Fine Tuning: Iterate up to 5 times > Trim Before Mapping: Do not trimm > Results: Save assembly report, used reads, in sub-folder, Include mates, contigs, consensus sequences

```


- Una vez generado el ensamble es necesario revisar cada una de las áreas con anotaciones _Low 0_ en color rojo, que señalaban las zonas donde no se mapeó ningún scaffold _de novo_. Se observa que algunas de las áreas previamente anotadas como “sin cobertura” han sido resueltas (imagen izquierda). En este caso en particular se observa que existen lecturas en esta zona que generarán ambigüedades (posiciones marcadas en color). 

- Sin embargo, estas zonas fueron previamente cubiertas por el scaffold _de novo_ (recuerda que la anotación _Low_ en rojo señala el área sin cobertura, por lo que el resto de la secuencia sí estaba cubierta por el scaffold), por lo que se resolverán usando el nucleótido mostrado por dicho scaffold (es decir, el nucleótido mostrado en la referencia.

-  Aquellas áreas que siguen sin resolverse (imagen derecha) se anotarán como `gaps`. Para ello debemos anotar las áreas con baja cobertura y aquellas en las que se observen problemas de mapeo. 

**2**. Anotar las regiones con baja cobertura, utilizaremos un mínimo de 30X:

`Menu > Annotate & Predict > Find Low/High Coverage > option Number of sequences "30"`.

Observa que se generaron nuevas anotaciones Low en color rojo, cuyo valor es 0->29, lo cual indica que marcan las áreas con cobertura <30X.


Dado que existen dos tipos de anotaciones _Low_ en nuestro ensamble, es importante no confundirlas. 

**3**. Copiaremos a la secuencia consenso sólo las anotaciones de las zonas que no tenían cobertura de scaffolds de novo y que tampoco alcanzan una cobertura >30X en este ensamble (es decir, las zonas problemáticas). Con el botón derecho sobre la anotación Low 0->29:

`Annotation > Copy to > Consensus`
 
Es importante mencionar que en el ensamble se anotaron también otras áreas con cobertura <30X que estaban satisfactoriamente cubiertas con los scaffolds de novo (dado que no tenían anotación Low 0). Éstas no se modificarán.


**4**.	Finalmente, revisaremos y corregiremos los nucleótidos ambiguos en la secuencia consenso. Estas ambigüedades se corregirán ya sea utilizando la secuencia de referencia, que es aquella del scaffold _de novo_ que mapeó en esa posición, o ya sea anotando el área como problemática para considerarla un gap que habrá de llenarse posteriormente. 

En las pestañas del lado derecho seleccionar la pestaña `“Display”` (la que tiene la pantallita azul) y seleccionar `“Highlighting”` `“Ambiguities”`.

En este ejemplo vemos cerca de 50,000 pb en la pantalla y es suficiente para ver la secuencia consenso en gris y poder reconocer los nucleótidos ambiguos marcados en negro. Existen ambigüedades cerca de la posición 40,000, así que nos acercaremos para corregirlas.

**5**. En este caso, se generó una `N` debido a los problemas de alineamiento de un microsatélite `(polyA)`. Debido a que no es posible modificar directamente la secuencia consenso de un ensamble, agregaremos una anotación para reemplazar posteriormente la `N`. Selecciona la `N` con el mouse, y usa el botón `“Add Annotation”`. Le pondremos por nombre `“cambiar N por A”`, `Type “repeat region”, “undirected”`. Observa que quedó la anotación en color rosa.

**6**. La siguiente zona con ambigüedades es un conjunto de nucleótidos ambiguos. Agregaremos una sola anotación para todos, señalando la secuencia original del scaffold _de novo_ que cubrió esas posiciones (mostrado en la referencia). Debido a que este paso es manual, debe hacerse con mucho cuidado.

**7**. La siguiente área corresponde a otro grupo de nucleótidos ambiguos pero aquí observamos que la referencia también tiene ambiguedades. Esto es porque existen más de un scaffold que mapeó esta región Observamos que esta zona cubre las posiciones alrededor de 81,600 pb y si regresamos al archivo en el que mapeamos los scaffolds efectivamente observamos dos scaffolds distintos cubriendo esa posición (imagen 1). Lo que haremos será anotar la zona problemática donde aún persisten ambigüedades para considerarla un gap. Manualmente seleccionamos las ambiguedades en la secuencia y con el botón derecho seleccionamos `Add Annotation` crar la anotación > Name `“NNNN”`, Tyoe `”Low”`, `”undirected”`, imagen 3).(right image).

**8**. Ahora que todas las nucleótidos ambiguos antados como _Low 0_ han sido atendidod, podemos generar la secuencia consenso, est vez seleccionamos la opción 

`"If no coverage call N"`.

**9**. Cambiaremos el nombre de la secuencias de `"Contig2"` a `"ArisPlastome1"`. Antes de modificar esta secuencia generaremos una copia. Editaremos la copia para resolver las ambigüedades y las zonas problema. Substituye `"N"` for all nucleotides annotated.

**10**. En la copia de trabajo empezaremos por las anotaciones rosas `“repeat region”`. En el ejemplo, hay una `R` que debe reemplazarse por G. Selecciona directamente la `R` y con el teclado escribe una `G` en su lugar. Aparecerá un mensaje solicitando autorices la edición. Luego aparecerá la G y una anotación amarilla, que si pones el mouse encima verás que señala que allí hubo una edición y de qué tipo. Repite este paso con todas las anotaciones rosas. En el caso de grupos de varios nucleótidos primero inserta la secuencia correcta y después borra la secuencia incorrecta.

**11**. Ahora editaremos las zonas problema y las convertiremos en gaps. Selecciona una anotación de `“zona problema”` o `“Low 0->29”`, y reemplaza toda esa secuencia por `“NNNN”`. Al suprimir el área aparecerá un mensaje para saber si deseas eliminar sólo la anotación o también las bases. Selecciona `“Delete bases too”`, y después manualmente agrega `“NNNN”`

Puedes colocar el número que quieras de Ns, dado que el programa Sealer no toma en cuenta el número de `Ns`. Pero es preferible poner varias, porque si pones sólo una puede tratarse a veces de algún nucleótido ambiguo que se te haya escapado, así que con 4 o 5 estás seguro que son las que tú pusiste. Verás que aparece la anotación de edición en color amarillo. Repite este paso con todas las anotaciones rojas de zonas problema o de baja cobertura.

**12**. Una vez que todos los gaps han sido anotados, exportaremos la secuencia a la carpeta del servidor `Abyss`:
`Menú > File > Export > Selected Documents`

Formato `*.fasta.` Le llamaremos 
`“ArisPlastome1_withgaps”`


**13**. También exportamos a la misma carpeta de `Abyss` las lecturas limpias (filtradas, trimeadas, sin duplicados) desde Geneious:

`Menú > Tools > Export > Selected Documents`

Formato 
`“fasta paired files”` y Le llamaremos `“Aris04AllReads”`.

### PARTE II B: En el servidor o terminal


**1B. Resolve the gaps on the server with abyss–sealer** 

Go to the server and create a new folder "abyssARIS" where you will place the files "ArisPlastome1_withGaps.fasta", "Aris04AllReads1.fasta" and "Aris04AllReads2.fasta".
21)	If the files were generated in Windows, you will have to convert the end of line format to Unix:

```
tr -d '\r' <ArisPlastome1_withGaps.fasta >ArisPlastome1_withGaps.fa
tr -d '\r' <Aris04AllReads1.fasta >Aris04AllReads1.fa
tr -d '\r' <Aris04AllReads2.fasta >Aris04AllReads2.fa
```

If abyss-sealer is not installed, review the instructions in the appendix. Add abyss-sealer to the PATH:

`PATH=$PATH:~/Desktop/Downloads/abyss/bin`

Run abyss-sealer:

`abyss-sealer -b20G -P10 -k90 -k80 -k70 -k60 -k50 -k40 -k30 -o ArisGaps -S ArisPlastome1_withGaps.fa Aris04AllReads1.fa Aris04AllReads2.fa`

Once abyss-sealer has finished running, you will find three files in the output folder: 

`*_log.txt, *_merged.fa, *_scaffold.fa` 
(where * is the output name that you specified, in our case ArisGaps).

Download them from the server to your computer. Evaluate the file `ArisGaps_log.txt`, which contains the report and indicates how many gaps closed satisfactorily and with what value of k.

The file `ArisGaps_scaffold.fa` contains the plastome sequence with the gaps closed. Import it to Geneious. 

This sequence still has the same name `ArisPlastome1_Copy`. Change the name and its description to indicate that it now has the gaps resolved: `ArisPlastome1_Copy_sealed`, `Sealer 6 of 6`.

We will copy the original sequence `ArisPlastome1_Copy` with the 6 initial gaps to the same folder to align both versions. 

`Menu > Align/Assemble > Pairwise Align > MAFFT` with automatic selection of the algorithm and the defaults (gap open 1.53 and offset 0.123).

In the alignment we can observe each of the regions that we had annotated and verify that the Ns have been substituted for sequence in some of them.








