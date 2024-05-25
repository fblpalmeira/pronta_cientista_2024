# [Pronta pra ser Cientista](https://www.instagram.com/prontapracientista/), 4a. edição - 2024

## Dia 4, Analisando a Biodiveridade, 25 de maio de 2024 

### [Francesca B. L. Palmeira](https://fblpalmeira.github.io/), francesca@alumni.usp.br 

-----
 
## Prática 

O objetivo desta aula prática é visualizar a riqueza e a abundância de espécies vegetais utilizando os dados coletados durante o nosso trabalho de campo na Floresta da USP em Ribeirão Preto, SP em 11/05/2024.

Durante o trabalho de campo, cada grupo de trabalho amostrou uma parcela de aproximadamente 20 x 10 metros na borda da Floresta, com uma distância de aproximadamente 30 metros entre as parcelas. A amostragem durou cerca de 30 a 40 minutos em cada parcela.

<img src="https://github.com/fblpalmeira/pronta_cientista_2024/blob/main/docs/mapa_floresta_pronta_2024.jpg">

Figura 2.  Localização das áreas amostradas na Floresta da USP em Ribeirão Preto.

-----

## Links dos exercícios no Posit Cloud

Vamos fazer os exercícios utilizando a [linguagem R](https://www.r-project.org/about.html) em uma versão do [RStudio](https://posit.co/download/rstudio-desktop/) que está disponível na nuvem, sem precisar instalar de nenhum programa ou software no computador. Primeiro, você deverá criar uma conta pessoal no [Posit Cloud](https://posit.cloud/). O site é seguro e não precisa pagar nada, pois vamos optar pelo plano gratuito. O tutorial abaixo tem o passo a passo para abrir a conta. 

- [Tutorial para abrir uma conta no Posit Cloud `.pdf`](https://github.com/fblpalmeira/pronta_cientista_2024/blob/main/docs/Tutorial_abrir_conta_PositCloud_pronta_2024.pdf)

A seguir, precisaremos acessar os exercícios clicando em qualquer um dos links abaixo. Dentro de cada link, tem o mesmo material. Como estamos utilizando o plano gratuito, apenas cinco pessoas podem acessar simultaneamente cada link. Caso algum link não funcione, pode ser que tenha mais de cinco pessoas tentando acessar ao mesmo tempo. Quando isso acontecer, tente acesso pelos outros links. Lembre-se que para fazer os exercícios que estão online, você vai precisar ter acesso à internet, abrir uma conta no [Posit Cloud](https://posit.cloud/) e fazer login com o seu email particular. 

- [Link1 `.R`](https://posit.cloud/content/5718175) 

- [Link2 `.R`](https://posit.cloud/content/6029576)

- [Link3 `.R`](https://posit.cloud/content/6029582)

- [Link4 `.R`](https://posit.cloud/content/6029592)

-----

## Preparando os dados

Carregar e manipular os dados no `R`:

``` r

###############
# Exercício 1 #
###############

# Construir um gráfico de Distribuição de Abundância de Espécies 
# das parcelas amostradas em campo

# Instalar e abrir o pacote "readxl" que lê arquivos do Excel (extensões .xls ou .xlsx)
install.packages("readxl") # Instalar 
library(readxl) # Abrir 

``` 

Ler e visualizar a planilha

``` r

# Criar um objeto (ex.: y) para armazenar a planilha do Excel
# O nome do objeto é editável e você pode mudar se necessário
# É importante que o nome do objeto seja curto e não tenha acento
y <- read_excel("planilha_pronta_2024.xlsx", na = " ")
y # Vizualizar a planilha parcialmente

# A tibble: 6 × 8
  Grupo            Espécie         Família    Porte  Estado Local       N Características
  <chr>            <chr>           <chr>      <chr>  <chr>  <chr>   <dbl> <chr>          
1 6 Personalidades Guaricica       NA         Árvore Fruto  Direita     1 Parece uma noz 
2 6 Personalidades Embaúba         NA         Árvore NA     Direita     1 Parece um plát…
3 6 Personalidades Asa de Fada     NA         Árvore Fruto  Direita     1 Parece uma asa…
4 6 Personalidades Cogumelo branco NA         Fungo  NA     Direita     1 Pequeno e bran…
5 Raposa           1R              NA         Árvore NA     NA          4 Folha redonda,…
6 Raposa           Guapuruvu       Leguminosa Árvore Fruto  NA          8 Tronco com cic…

``` 

## Vizualizando a planilha inteira

Utilize o comando 'View' para visualizar a planilha inteira. Cada linha representa uma observação e cada coluna representa uma variável.

``` r

View(y) # Vizualizar a planilha inteira

``` 

<img src="https://github.com/fblpalmeira/pronta_cientista_2024/blob/main/docs/Planilha_View.jpeg"/>

## Vamos começar a explorar os dados coletados? 

Utilizando a função 'str' poderemos visualizar a estrutura interna dos dados (variável numérica, categórica, etc.), entre outras informações. 

``` r

str(y) # Verificar a estrutura interna de um objeto

tibble [24 × 8] (S3: tbl_df/tbl/data.frame)
 $ Grupo          : chr [1:24] "6 Personalidades" "6 Personalidades" "6 Personalidades" "6 Personalidades" ...
 $ Espécie        : chr [1:24] "Guaricica" "Embaúba" "Asa de Fada" "Cogumelo branco" ...
 $ Família        : chr [1:24] NA NA NA NA ...
 $ Porte          : chr [1:24] "Árvore" "Árvore" "Árvore" "Fungo" ...
 $ Estado         : chr [1:24] "Fruto" NA "Fruto" NA ...
 $ Local          : chr [1:24] "Direita" "Direita" "Direita" "Direita" ...
 $ N              : num [1:24] 1 1 1 1 4 8 2 10 7 4 ...
 $ Características: chr [1:24] "Parece uma noz" "Parece um plátano" "Parece uma asa de fada" "Pequeno e branco" ...

```

Agora, vamos precisar manipular alguns dados da planilha para fazer o gráfico. Primeiro, iremos filtrar a coluna "Especies" e "N" (Número de indivíduos) e depois iremos contar quantos indivíduos por espécie registramos nas parcelas amostradas.

``` r

# Abrir os pacotes "dplyr" e "forcats" para manipular os dados da planilha
install.packages("dplyr") 
install.packages("forcats") 
library(dplyr) 
library(forcats) 

# Primeiro, vamos tirar os acentos do cabeçalho de algumas colunas
# Neste caso, os acentos apenas são permitidos apenas dentro das colunas, 
# mas não nos cabeçalhos
y <- y %>% rename(Especie = "Espécie",
                   Familia = "Família",
                   Caracteristicas = "Características")

# Agora, vamos agrupar as espécies pelo nome (sp1, sp2, etc.) e contar o número de 
# indivíduos (N)
y1 <- y %>% group_by(Especie) %>%
             summarize(N = sum(N))
y1

``` 

## Após a filtragem dos dados teremos o objeto (y1) a seguir:

``` r

# A tibble: 17 × 2
   Especie             N
   <chr>           <dbl>
 1 1R                  4
 2 2B                  4
 3 2R                 10
 4 4B                 10
 5 5B                  3
 6 6B                  6
 7 8B                  1
 8 Angico             14
 9 Asa de Fada         1
10 Cogumelo branco     1
11 Embaúba             4
12 Espécie 3          30
13 Guapuruvu          29
14 Guaricica           1
15 Manguba             1
16 Palmeira            1
17 Pitanga             1

```

## Gráfico de barras mostrando a Distribuição de Abundância de Espécies 

Após contar o número de espécies por indivíduos, iremos visualizar esses dados em um simples gráfico de barras. Aproveite para verificar quais espécies foram mais abundante e quais foram raras. 

``` r

# Abrir o pacote 'ggplot2' para construir o gráfico
# Vamos construir um gráfico de barras para visualizar o número de indivíduos 
# por espécie
install.packages("ggplot2")
library(ggplot2)
ggplot(y1, aes(Especie, N)) + 
  geom_col()

```

<img src="https://github.com/fblpalmeira/pronta_cientista/blob/main/data/Figura1a_DAE.png"/>

## Organizando o gráfico em ordem crescente

Finalmente, temos um gráfico! Agora vamos deixá-lo mais intuitivo e utilizar a função "reorder" para ordenar as espécies conforme o número de indivíduos registrados.

``` r

# Ordenar o número de indivíduos por espécie usando o comando 'reorder'  
ggplot(y1, aes(reorder(Especie, N), N)) +
  geom_col()
  
```

<img src="https://github.com/fblpalmeira/pronta_cientista/blob/main/data/Figura1b_DAE.png"/>

## Organizando o gráfico em ordem decrescente

``` r

# Ordenar o número de indivíduos por espécie em ordem decrescente utilizando o 
# sinal de menos na frente da varíavel N (-N), desta forma, poderemos identificar 
# facilmente quais são as espécies mais abundantes e quais são as mais raras
ggplot(y1, aes(reorder(Especie, -N, sum), N)) +
  geom_col()

```

<img src="https://github.com/fblpalmeira/pronta_cientista/blob/main/data/Figura1c_DAE.png"/>

## Colorindo o gráfico

``` r

# Colorir as barras do gráfico editando o argumento "fill" dentro da função "geom_col" 
ggplot(y1, aes(reorder(Especie, -N, sum), N)) +
  geom_col(fill = "darkgreen") # Editar a cor do gráfico

```

<img src="https://github.com/fblpalmeira/pronta_cientista/blob/main/data/Figura1d_DAE.png"/>

## Renomeando as etiquetas dos eixos x e y 

``` r

# Renomear as etiquetas dos eixos (x, y) do gráfico utilizando a função "labs"
# Colocar acentos e descrever sucintamente para que o leitor possa saber o que significa cada eixo
ggplot(y1, aes(reorder(Especie, -N, sum), N))+
  geom_col(fill = "darkgreen")+
  labs(x = "Nome das espécies", y = "Número de indivíduos (n)") # Editar as etiquetas

```

<img src="https://github.com/fblpalmeira/pronta_cientista/blob/main/data/Figura1e_DAE.png"/>

## Limpando o plano de fundo

``` r

# Limpar o fundo do gráfico e a grade de linhas (x, y) dos painéis 
# utilizando a função 'theme'
ggplot(y1, aes(reorder(Especie, -N, sum), N)) +
  geom_col(fill = "darkgreen") +
  labs(x = "Nome das espécies", y = "Número de indivíduos (n)")+
  theme_bw() + # Limpar painel
  theme (panel.grid.major.y = element_blank(), # Limpar grades de linhas
         panel.grid.minor.y = element_blank())+ 
  theme (panel.grid.major.x = element_blank(), 
         panel.grid.minor.x = element_blank())
         
```

<img src="https://github.com/fblpalmeira/pronta_cientista/blob/main/data/Figura1f_DAE.png"/>

## Aumentando o tamanho das letras 

``` r

# Aumentar o tamanho da letra da etiqueta em cada eixo (x e y) editando os 
# argumentos (axis.title.x=element_text(size=12) e (axis.title.y=element_text(size=12)) 
# Caso necessário utilize uma letra maior ou menor que 12
ggplot(y1, aes(reorder(Especie, -N, sum), N)) +
  geom_col(fill = "darkgreen") +
  labs(x = "Nome das espécies", y = "Número de indivíduos (n)")+
  theme_bw() +
  theme (panel.grid.major.y = element_blank(), 
         panel.grid.minor.y = element_blank()) + 
  theme (panel.grid.major.x = element_blank(), 
         panel.grid.minor.x = element_blank()) +
  theme (axis.text.x=element_text(size=12)) + # Aumentar a letra do eixo x
  theme (axis.text.y=element_text(size=12)) # Aumentar a letra do eixo y

```

<img src="https://github.com/fblpalmeira/pronta_cientista/blob/main/data/Figura1h_DAE.png"/>

-----

## Construir uma curva de rarefação das espécies amostradas nas duas parcelas

``` r

###############
# Exercício 2 #
###############

# Fazer uma curva de rarefação utilizando a riqueza de espécies 
# e o número de indivíduos contados

# Instalar e abrir o pacote 'reshape2' para transpor a tabela de dados (y)
install.packages("reshape2")
library(reshape2)
y2 <- dcast(y, Especie ~ Grupo, value.var="N")
y2

            Especie 6 Personalidades Borboleta Chico Dollares Flor USP Raposa
1               1R               NA        NA             NA       NA      4
2               2B               NA        NA             NA        4     NA
3               2R               NA        NA             NA       NA     10
4               4B               NA        NA             NA       10     NA
5               5B               NA        NA             NA        3     NA
6               6B               NA        NA             NA        6     NA
7               8B               NA        NA             NA        1     NA
8           Angico               NA         4              6        4     NA
9      Asa de Fada                1        NA             NA       NA     NA
10 Cogumelo branco                1        NA             NA       NA     NA
11         Embaúba                1        NA             NA        1      2
12       Espécie 3               NA        30             NA       NA     NA
13       Guapuruvu               NA         9              5        7      8
14       Guaricica                1        NA             NA       NA     NA
15         Manguba               NA        NA              1       NA     NA
16        Palmeira               NA         1             NA       NA     NA
17         Pitanga               NA         1             NA       NA     NA

```

## Substituir os NA's por zeros

``` r

# Substituir os NA's por zeros
# NA´s (Not Applicable) são as células vazias que não contém nenhuma informação,
# geralmente são informações que foram perdidas ou não puderam ser coletadas
y2 <- y2 %>% replace(is.na(.), 0) 
y2

           Especie 6 Personalidades Borboleta Chico Dollares Flor USP Raposa
1               1R                0         0              0        0      4
2               2B                0         0              0        4      0
3               2R                0         0              0        0     10
4               4B                0         0              0       10      0
5               5B                0         0              0        3      0
6               6B                0         0              0        6      0
7               8B                0         0              0        1      0
8           Angico                0         4              6        4      0
9      Asa de Fada                1         0              0        0      0
10 Cogumelo branco                1         0              0        0      0
11         Embaúba                1         0              0        1      2
12       Espécie 3                0        30              0        0      0
13       Guapuruvu                0         9              5        7      8
14       Guaricica                1         0              0        0      0
15         Manguba                0         0              1        0      0
16        Palmeira                0         1              0        0      0
17         Pitanga                0         1              0        0      0

``` 

## Transformar o data.frame y2 em uma matriz "integer"

``` r

# Transformar o data.frame (y2) em uma matriz (y3)
y3 <- as.matrix(apply(y2[,-1],2,as.integer))

      6 Personalidades Borboleta Chico Dollares Flor USP Raposa
 [1,]                0         0              0        0      4
 [2,]                0         0              0        4      0
 [3,]                0         0              0        0     10
 [4,]                0         0              0       10      0
 [5,]                0         0              0        3      0
 [6,]                0         0              0        6      0
 [7,]                0         0              0        1      0
 [8,]                0         4              6        4      0
 [9,]                1         0              0        0      0
[10,]                1         0              0        0      0
[11,]                1         0              0        1      2
[12,]                0        30              0        0      0
[13,]                0         9              5        7      8
[14,]                1         0              0        0      0
[15,]                0         0              1        0      0
[16,]                0         1              0        0      0
[17,]                0         1              0        0      0

``` 

## Informar os nomes das espécies 

``` r

# Ao transformar o data frame em matriz, ele suprime o nome das espécies na primeira
# coluna. Então, precisamos retornar os nomes das espécies na primeira coluna
row.names(y3) <- y2[,1]
y3

                6 Personalidades Borboleta Chico Dollares Flor USP Raposa
1R                             0         0              0        0      4
2B                             0         0              0        4      0
2R                             0         0              0        0     10
4B                             0         0              0       10      0
5B                             0         0              0        3      0
6B                             0         0              0        6      0
8B                             0         0              0        1      0
Angico                         0         4              6        4      0
Asa de Fada                    1         0              0        0      0
Cogumelo branco                1         0              0        0      0
Embaúba                        1         0              0        1      2
Espécie 3                      0        30              0        0      0
Guapuruvu                      0         9              5        7      8
Guaricica                      1         0              0        0      0
Manguba                        0         0              1        0      0
Palmeira                       0         1              0        0      0
Pitanga                        0         1              0        0      0

``` 

## Contar o número de indivíduos por parcela

``` r

# Contar o número de indivíduos por parcela
colSums(y3)

6 Personalidades        Borboleta   Chico Dollares         Flor USP           Raposa 
               4               45               12               36               24 

``` 

## Comparando as parcelas amostradas: Curva de rarefação de espécies 

A comparação da riqueza de espécies entre as comunidades deve ser feita com base na riqueza de espécies rarefeita, que é calculada com base no número de indivíduos da parcela de amostragem com menor abundância [(Da Silva et al. 2022 - Análises ecológicas no R)](https://analises-ecologicas.netlify.app/cap10.html).

``` r

# Instalar e abrir o pacote 'iNEXT' para fazer a interpolação e extrapolação dos dados
# Comparar a amostragem entre os grupos 
install.packages("iNEXT")
library(iNEXT)
estimates <- iNEXT(y3, datatype="abundance", endpoint=100)
ggiNEXT(estimates) +
  scale_linetype_discrete(labels = c("Interpolado", "Extrapolado")) +  
  scale_colour_manual(values = c("orange", "cyan","red", "blue","green")) +
  labs(x = "Número de indivíduos", y = " Riqueza de espécies")+
  theme_bw() +
  theme (panel.grid.major.y = element_blank(), 
         panel.grid.minor.y = element_blank()) + 
  theme (panel.grid.major.x = element_blank(), 
         panel.grid.minor.x = element_blank()) +
  theme (axis.text.x=element_text(size=12)) +
  theme (axis.text.y=element_text(size=12))
  
```   

<img src="https://github.com/fblpalmeira/pronta_cientista/blob/main/data/Figura2_Rarefacao_Parcelas_1_e_2_a.png" align="center" width = "800px"/>

## Inserir uma linha vertical tracejada e salvar a figura 

A comparação da riqueza de espécies entre as comunidades deve ser feita com base na riqueza de espécies rarefeita, que é calculada com base no número de indivíduos do grupo ou parcela com menor abundância (166 indivíduos - linha preta tracejada).

``` r

# Inserir uma linha vertical (linha preta tracejada) para indicar a menor abundância
# A comparação da riqueza de espécies entre as comunidades deve ser feita com base 
# na riqueza de espécies rarefeita, que é calculada com base no número de indivíduos 
# da comunidade com menor abundância (4 indivíduos registrado pelo Grupo 6 Personalidades). 
# Fonte: Da Silva et al. 2022 - Análises ecológicas no R
ggiNEXT(estimates) +
  geom_vline(xintercept = 4, lty = 2) +
  scale_linetype_discrete(labels = c("Interpolado", "Extrapolado")) +  
  scale_colour_manual(values = c("orange", "cyan","red", "blue","green")) +
  labs(x = "Número de indivíduos", y = " Riqueza de espécies")+
  theme_bw() +
  theme (panel.grid.major.y = element_blank(), 
         panel.grid.minor.y = element_blank()) + 
  theme (panel.grid.major.x = element_blank(), 
         panel.grid.minor.x = element_blank()) +
  theme (axis.text.x=element_text(size=12)) +
  theme (axis.text.y=element_text(size=12))

``` 

<img src="https://github.com/fblpalmeira/pronta_cientista/blob/main/data/Figura2_Rarefacao_Parcelas_1_e_2_c.png" align="center" width = "800px"/>

----

## Interpretação dos resultados

Existiu diferença estatística entre a riqueza e o número de espécies nas parcelas amostradas pelo grupos?

-----

## Vamos exercitar um pouco de programação fazendo um gráfico de waffle? 

O gráfico de waffle é utilizado para representar proporções. Neste exercício, você poderá editar os argumentos da função e fazer um gráfico que conta a história da sua vida. Veja o exemplo abaixo e inspire-se. 

O código do exercício está em um dos quatro [links](https://github.com/fblpalmeira/pronta_cientista/blob/main/README.md#links-dos-exerc%C3%ADcios-no-posit-cloud) disponibilizados no início desta página. Caso tenha alguma dúvida sobre o exercício é só seguir o tutorial abaixo. No tutorial, tem todo o passo a passo de como construir o gráfico da sua biografia e, ao final, exportá-lo para o computador.

- [Tutorial para fazer um gráfico de waffle `.pdf`](https://github.com/fblpalmeira/pronta_cientista/blob/main/data/Tutotial_RStudio_Cloud_Waffle_Pronta_Cientista_2023_05_27.pdf)

## A vida da cientista em um gráfico de waffle

Edite o código abaixo utilizando os dados da sua biografia.

``` r

# Faça o seu gráfico aqui - Modelo 1
png(file="Figura2_waffle.png", width = 1000, height = 600)
parts <- c('Maternal'=1, 'Jardim'=3, 'Ensino Fundamental' = 4, 'Ensino Médio' = 5)
waffle(parts, rows=3, title = "A minha vida em um gráfico de waffle", 
       xlab = "1 quadrado = 12 meses")+
       theme (legend.text =element_text (size = 15), axis.title.x=element_text(size = 20))+
       labs(caption = "@Seu nome")
dev.off()

``` 

<img src="https://github.com/fblpalmeira/pronta_cientista/blob/main/data/Figura1_waffle.png" align="center" width = "800px"/>

-----

## Bibliografia básica

- [Capítulo 10 - Rarefação: Análises Ecológicas no R `.html`](https://analises-ecologicas.netlify.app/cap10.html)

- -----

## Leitura adicional

- [Capítulo 2 - Voltando ao básico: como dominar a arte de fazer perguntas cientificamente relevantes `.html`](https://analises-ecologicas.netlify.app/cap2.html)
