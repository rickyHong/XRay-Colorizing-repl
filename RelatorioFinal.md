
# Colorização de Imagens de Raio-X

**Tema:** Colorização de imagens

## Descrição

Imagens de raio-x são usadas para diversas aplicações. Dois de seus principais usos incluem a realização de exames médicos e a checagem do conteúdo de bagagens em aeroportos. Imagens em raio-x são, por sua própria natureza, imagens em escala de cinza, com um único canal gerado pela incidência dos raios-x nos detectores da aparelhagem.

No entanto, sabe-se que a percepção de tons de cinza por humanos é limitada quando comparada à sua capacidade de distinção entre o uso de cores. Sendo assim, a colorização pode ser utilizada para facilitar a distinção de características de interesse nessas imagens, facilitando o trabalho do profissional envolvido.

Nesse projeto, visamos trabalhar com imagens em escala de cinza provenientes de raios-x, mais especificamente, com imagens médicas (como radiografias) e checagem de segurança de bagagens. Nosso objetivo é a colorização dessas imagens.

Para isso, as imagens raio-x são primeiro tratadas a fim de melhorar sua qualidade e o contraste entre seus elementos significativos. Em seguida, é aplicada uma entre seis formas de colorização, obtendo a imagem final em pseudo cor. 
 
## Autores
  Chan Ken Chen (n.USP: 9436170)
  
  Elisa Saltori Trujillo (n.USP: 8551100)
 
## Imagens utilizadas

As imagens utilizadas no projeto, como indicado anteriormente, são de dois tipos distintos: temos imagens raio-x de bagagens e radiografias médicas. Os dois conjuntos de imagens, com sua origem, estão indicados a seguir.

### Imagens raio-X de bagagens 

(disponível em:  http://dmery.ing.puc.cl/index.php/material/gdxray/)

As imagens raio-x de bagagens foram obtidas da GRIMA X-ray Database. Elas consistem em imagens no formato PNG em escala de cinza com resolução variável. Elas apresentam imagens raio-x de bolsas ou malas com objetos em seu interior, com destaque para objetos perigosos, como armas, pregos ou lâminas.  
Segundo a base de dados, essas imagens foram obtidas com a utilização de um detector digital de raio-X (Canon, modelo CXDI-50G), um tubo emissor de raio-x (Poskom, modelo PXM-20BT) e um protetor de chumbo de segurança para isolamento do ambiente de inspeção. 

 **Exemplos:**
 
 <p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/elisa/Sample_Images/Baggages/B0009_0001.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Sample_Images/Baggages/B0044_0001.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/elisa/Sample_Images/Baggages/B0023_0001.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Sample_Images/Baggages/B0011_0001.png" height="300">
 </p>
 
[1] Mery, D.; Riffo, V.; Zscherpel, U.; Mondragón, G.; Lillo, I.; Zuccar, I.; Lobel, H.; Carrasco, M. (2015): GDXray: The database of X-ray images for nondestructive testing. Journal of Nondestructive Evaluation, 34.4:1-12. [ PDF ]

### Radiografias 

(disponível em:  https://www.kaggle.com/nih-chest-xrays/data)

As radiografias foram obtidas do National Institutes of Health Chest X-Ray Dataset e consistem em radiografias da região do tórax. Elas possuem um tamanho padrão de 1024x1024 pixels e estão no formato PNG. As imagens utilizadas no projeto serão retiradas do conjunto amostra disponibilizado do dataset, consistindo em um total 5606 imagens. 

 **Exemplos:**

  <p float="left" align="middle">
  <img src="https://github.com/elisasaltori/XRayColorizing/raw/elisa/Sample_Images/Chest_Xrays/00000013_026.png" height="300">
  <img src="https://github.com/elisasaltori/XRayColorizing/raw/elisa/Sample_Images/Chest_Xrays/00000132_002.png" height="300">
  <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Sample_Images/Chest_Xrays/00003896_005.png" height="300">
  <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Sample_Images/Chest_Xrays/00003989_024.png" height="300">
  </p>
  
[2] Wang X, Peng Y, Lu L, Lu Z, Bagheri M, Summers RM. ChestX-ray8: Hospital-scale Chest X-ray Database and Benchmarks on Weakly-Supervised Classification and Localization of Common Thorax Diseases. IEEE CVPR 2017, ChestX-ray8_Hospital-Scale_Chest_CVPR_2017_paper.pdf

## Etapas de solução do problema

 Cada imagem, para ser colorizada, passa pelas seguintes etapas:
 - Suavização
 - Realce de bordas
 - Normalização ou equalização
 - Colorização
 
 Cada um desses passos possui, ao menos, dois métodos distintos que podem ser aplicados sobre a imagem. Cada uma dessas etapas será descrita nas seções a seguir.
 
 ### Suavização
 
A suavização da imagem é a primeira etapa no tratamento da imagem de entrada. Seu objetivo é a remoção de ruído, tornando a imagem de saída mais nítida. 

Para esse passo, três métodos foram implementados e testados:
- Filtro de média
- Filtro de mediana
- Filtro gaussiano

#### Filtro de média

Esse filtro consiste simplesmente na média dos valores na vizinhança de um pixel. Ele foi aplicado por ser um método simples e comum de suavização.

No entanto, notamos logo de início os problemas típicos desse filtro: a perda de bordas, com "borramento" da imagem. Logo abaixo, podemos ver um exemplo da aplicação do filtro (aplicado com uma vizinha de tamanho 3x3). À esquerda, temos a imagem original; à direita, a imagem com o filtro aplicado.

 <p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Sample_Images/Baggages/B0009_0001.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Smoothing_Filters/average.png" height="300">
 </p>

Esse filtro, logo, não possui um comportamento adequado para a aplicação. A perda de detalhes é excessiva e poderia causar dificuldade na análise da imagem.

#### Filtro de mediana

Para esse filtro, o valor de um pixel é definido pela mediana dos valores encontrados em uma janela de vizinhança determinada em torno do pixel.

Ele apresenta um comportamento mais adequado que o filtro de média, ocasionando menor "borramento" da imagem. Logo abaixo, podemos ver dois exemplos de aplicação do filtro (aplicado com uma vizinha de tamanho 3x3). Em ambos os casos, temos, a esquerda, temos a imagem original; à direita, a imagem com o filtro aplicado.

- **Imagem de bagagem:**
 <p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Sample_Images/Baggages/B0009_0001.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Smoothing_Filters/median.png" height="300">
 </p>

- **Radiografia:**
 <p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Sample_Images/Chest_Xrays/00000013_026.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Smoothing_Filters/chest_median.png" height="300">
 </p>

#### Filtro gaussiano

O filtro gaussiano foi implementado por possuir um comportamento mais uniforme que o filtro de mediana.

Logo abaixo, podemos ver dois exemplos de aplicação do filtro (aplicado com uma vizinha de tamanho 3x3 e desvio padrão de valor 1). Em ambos os casos, temos, a esquerda, temos a imagem original; à direita, a imagem com o filtro aplicado.


- **Imagem de bagagem:**
 <p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Sample_Images/Baggages/B0009_0001.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Smoothing_Filters/gaussian_3_1.png" height="300">
 </p>

- **Radiografia:**
 <p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Sample_Images/Chest_Xrays/00000013_026.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Smoothing_Filters/chest_gaussian.png" height="300">
 </p>
 
#### Comparação entre métodos

Temos abaixo as três imagens lado a lado: com filtro de média, com mediana e com filtro gaussiano.

 <p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Smoothing_Filters/average.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Smoothing_Filters/median.png" height="300">
  <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Smoothing_Filters/gaussian_3_1.png" height="300">
 </p>
 
 Como já observado antes, o uso do filtro da média causa uma distorção excessiva na imagem e, portanto, ele não será utilizado nas imagens finais. Já entre os filtros de mediana e gaussiano, o filtro mediana apresenta nos casos-teste uma maior redução de ruído na imagem, com uma distorção suficientemente contida. Por conta disso, ele será utilizado na colorização final da imagem. 

 ### Realce de bordas
A segunda etapa pela qual a imagem passa é o realce de bordas. Essa etapa é opcional, podendo não ser aplicada na obtenção da imagem final. Os filtros de realce de bordas são utilizados para recuperar detalhes que possam ter sido perdidos com a suavização e destacar elementos importantes na imagem.

Dois métodos foram testados nessa etapa:
- Operador Sobel
- Filtro laplaciano

#### Operador Sobel

O operador sobel foi implementado por possuir um bom resultado na identificação de contornos. Em sua utilização, o operador é aplicado sobre a imagem de entrada e o resultado dessa operação é somado à imagem original.
Abaixo, estão os resultados da aplicação do filtro, com uma vizinhança de 3x3. Temos à esquerda a imagem original, e à direita, o resultado da aplicação do filtro. Ambos os conjuntos de imagens são apresentados já equalizados a fim de facilitar a visualização dos efeitos do operador.

- **Imagem de bagagem:**
 <p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Histogram/gun_histogram.png" height="300">
 <img src="https://raw.githubusercontent.com/elisasaltori/XRayColorizing/master/Test_Images/Sharpening_Filters/sobel_histogram.png" height="300">
 </p>

 - **Radiografia:**
  <p float="left" align="middle">
  <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Histogram/chest_histogram.png" height="300">
  <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Sharpening_Filters/sobel_chest.png" height="300">
  </p>
  
  Como visto acima, o operador sobel apresentou um comportamento interessante para a distinção de elementos no raio-x da bagagem, destacando os diversos objetos na mala, que poderá ser útil na composição final da imagem.
  
  Já na imagem da radiografia, percebe-se também um aumento significativo no destaque das bordas. No entanto, é necessário tomar cuidado para que sua aplicação não gere artefatos que atrapalhem o diagnóstico por profissionais médicos.

#### Filtro Laplaciano
O filtro implementado é o laplaciano. Diferentemente do filtro Sobel, que se trata de um filtro de derivada de primeira ordem, o filtro laplaciano aproxima uma derivada de segunda ordem e, portanto, é mais sensível a ruído. Esse método foi aplicado para comparar sua atuação com o operador anterior.
Foi utilizada uma aproximação 3x3 do filtro para a implementação.
Abaixo, estão os resultados da aplicação do filtro. Temos à esquerda a imagem original, e à direita, o resultado da aplicação do filtro. Ambos os conjuntos de imagens são apresentados já equalizados a fim de facilitar a visualização dos efeitos do operador.

- **Imagem de bagagem:**
 <p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Histogram/gun_histogram.png" height="300">
 <img src="hhttps://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Sharpening_Filters/new_laplacian.png" height="300">
 </p>
 
 Como pode ser visto, não conseguimos obter um bom resultado com o filtro laplaciano. A imagem de saída possui um excesso de ruído, o que não é observado com a aplicação do operador sobel.

 ### Normalização ou equalização
Como terceira etapa, a imagem passa necessariamente por um método de aumento de contraste. Essa etapa é importante para facilitar a distinção dos elementos da figura.

Há duas opções nessa etapa:
 - Apenas a normalização da imagem
 - Normalização seguida de equalização por histograma
 
 Segundo as características de uma imagem, uma ou a outra abordagem pode ser mais apropriada.

#### Normalização da imagem

Nesse método, temos um simples "alargamento" do contraste, mapeando os valores da imagem de forma que sua menor intensidade seja 0 e sua maior seja 255. A normalização não há nenhum efeito, portanto, se o intervalo original das imagem já for de 0 a 255.

#### Equalização por histograma

A equalização por histograma aumenta o contraste global da imagem, melhor distribuindo suas intensidades. 

A equalização por histograma apresentou um comportamento adequado na maioria dos casos, aumentando significativamente o contraste entre os tons intermediários da imagem quando comparada à normalização comum. Abaixo, podemos verificar esse efeito em sua aplicação sobre duas imagens:

- **Normalização da imagem (esquerda) vs equalização por histograma (direita):**
 <p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Histogram/compare_chest_hist/00000013_026.png_1121_grey.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Histogram/compare_chest_hist/00000013_026.png_1122_grey.png" height="300">
 </p>
 
 - **Normalização da imagem (esquerda) vs equalização por histograma (direita):**
 <p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Histogram/gun_normal.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Histogram/gun_histogram.png" height="300">
 </p>
 
 No entanto, verificamos também que a equalização por histograma não funciona bem para todos os casos. Em particular, verificamos que, quando a imagem possui uma grande área de fundo branco, o contraste obtido pela equalização por histograma é distintamente pior que a obtida pela normalização. Isso observado no exemplo abaixo:
 
  - **Normalização da imagem (esquerda) vs equalização por histograma (direita):**
 <p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Histogram/hist_bad/normal_good.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Histogram/hist_bad/histbad.png" height="300">
 </p>

O resultado pode ser dito, inclusive, pior que o da imagem original. 

Portanto, embora o método do histograma tenha bons resultados para a maioria das imagens, devemos ter cuidado ao aplicá-la indiscriminadamente sobre todas. 
 
 ### Colorização
 
 Como última etapa do processo, temos a colorização da imagem. Nesse passo, obtemos uma imagem de 3 camadas, correspendentes cada uma a um dos canais RGB.

Nessa etapa, procuramos aplicar uma variedade de métodos a fim de comparar os resultados obtidos.

Os três primeiros métodos descritos foram feitos exclusivamente para a aplicação em raios-x de bagagens. Os três últimos são colormaps genéricos, obtidos da biblioteca matplotlib.
 
 #### Metódo 1: Colorização de bagagem com thresholds fixos
 
 Esse mapeamento é inspirado nos mecanismos utilizados por aeroportos no scan de imagens, como exemplificado em [1]. Um mapeamento semelhante é feito no método 2, mas com thresholds variáveis.
 
 A função de mapeamento foi feita utilizando o sistema de cores HSV.

Essa colorização utiliza três cores principais de acordo com o material encontrado:
- **Laranja**: materiais orgânicos, de menor densidade.
- **Verde**: materiais plásticos, de densidade média.
- **Azul**: materiais duros, como metal.

Na definição da matiz de um pixel, então, foi considerada que a intensidade de um pixel é inversamente proporcional à densidade do material: um pixel de menor intensidade (mais escuro) representa uma densidade maior e vice versa.

Foram então definidos limites simples inferiores e superiores sobre a intensidade dos pixels para a definição de sua matiz (laranja, verde ou azul):
- **Azul:** 0 a 40 (Matiz = 236/360)
- **Verde:** 41 a 105 (Matiz = 100/360)
- **Laranja:** 106 a 255 (Matiz = 47/360)

Esses valores foram obtidos por observação manual das intensidades de pixel em imagens de bagagem.

Já os canais de saturação e valor foram definidos da seguinte forma:
- **Saturação:** min(2*(255-img)/255.0, 1)
- **Valor:** img/255.0
    
Os canais foram assim definidos para manter pixels brancos (intensidade 255) em seu estado original (com saturação 0 e valor 1) e a fim de dar mais destaque às intensidades médias e baixas na imagem (densidade de material média e alta). 

Abaixo temos três exemplos da aplicação do mapamento (os dois últimos com a aplicação do operador sobel):

<p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/test.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/sobel_color1.png" height="300">
  <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/B0011_0001.png_1212.png" height="300">
 </p>

Os resultados dessa colorização serão discutidos na seção de resultados.

 #### Metódo 2: Colorização de bagagem com thresholds variáveis
 
 Esse método de colorização aplica as mesmas formas de mapeamento que o método 1, descrito na seção acima. Sua única diferença está na definição dos thresholds, que, neste método, são obtidos por uma técnica de segmentação de imagem: o método multi otsu.
 
 O método de otsu é um método de segmentação de imagem que busca maximizar a variância entre classes, baseando-se no histograma da imagem. O método de otsu foi criado inicialmente para a segmentação da imagem em apenas duas partes, o que não seria adequado para a colorização visada, já que, como indicado no método 1, buscamos distinguir entre três níveis de densidade de objetos. Utilizamos, então, uma expansão do método, multi otsu's method [3], para o tratamento de três níveis na imagem.
 
 Decidimos implementar essa variação do método 1, com thresholds variáveis, com o objetivo de tornar a colorização de bagagens mais adaptável às características de uma determinada imagem, para qual a aplicação de um threshold fixo pode ser inadequada.
 
 Abaixo, temos alguns exemplos da aplicação do método. Seus resultados serão discutidos posteriormente, comparando-os aos dos demais métodos de colorização.
 
 <p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/Method_1/B0009_0001.png_2011.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/Method_1/B0011_0001.png_2011.png" height="300">
  <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/Method_1/B0023_0001.png_1212.png" height="300">
 </p>
 
 As duas primeiras imagens passaram apenas pela suavização pelo filtro de mediana e pela normalização. A última imagem passou, ademais dos métodos indicados para as duas primeiras imagens, pelo operador sobel e a equalização por histograma.
 
 #### Metódo 3: Colorização de bagagem com funções seno
 
 Esse método de colorização foi desenvolvido por Kase[2]. Esse método, diferentemente dos dois anteriores, não busca segmentar a imagem em três níveis distintos para a colorização, utilizando funções trignométricas simples para mapeamento das intensidades de cinza nos canais RGB.
 
 Ele é apresentado aqui como uma técnica alternativa de mapeamento, buscando contrastar com a abordagem das duas primeiras.
 
 Suas funções de mapeamento são dadas a seguir:
 
 ```
    canal R = np.fabs((np.sin(0.666667*np.pi*img_in/255-0.02*np.pi)))
    canal G = np.fabs((np.sin(0.666667*np.pi*img_in/255-1.2*np.pi)))
    canal B = np.fabs((np.sin(0.666667*np.pi*img_in/255-1.3333*np.pi)))
 ```
 Exemplos de aplicação:
 <p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/Method_2/B0009_0001.png_2020.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/Method_2/B0011_0001.png_1222.png" height="300">
  <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/Method_2/B0023_0001.png_2222.png" height="300">
 </p>
 
 As duas últimas imagens tiveram o operador sobel e a equalização por histogramas aplicada, enquanto que a primeira teve somente a normalização aplicada.
 
 
 #### Método 4: Hot colormap
 
Esse é o primeiro método genérico apresentado aqui, obtido da biblioteca matplotlib. Os três métodos dessa biblioteca aqui apresentados possuem características distintas na sua composição de cores e foram selecionados para que os resultados obtidos com elas pudessem ser melhor contrastados.

Esses colormaps genéricos pré-implementados serão utilizados nos resultados para a colorização de radiografias.

O hot colormap é um mapa de cores sequencial, utilizando, como seu nome indica, cores quentes, como amarelo e vermelho.

Podemos ver abaixo alguns exemplos de sua aplicação.

-**Bagagem:**
<p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/B0011_0001.png_1222.png" height="300">
 </p>
 
 A imagem acima utiliza o operador sobel e a equalização por histograma.
 
 -**Radiografias:**
<p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/color_chest/00000013_026.png_1122.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/color_chest/00000013_026.png_1222.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/color_chest/00000132_002.png_1121.png" height="300">
 </p>
 
 As três imagens utilizam o filtro de mediana e a equalização por histograma. A primeira e a terceira utilizam o filtro laplaciano, enquanto que a segunda utiliza o operador sobel.

#### Método 5: Inferno colormap

O inferno colormap é o segundo método genérico apresentado. Trata-se de um mapa perceptualmente uniforme, facilitando a distinção de elementos em qualquer ponto de sua escala.

Podemos ver abaixo alguns exemplos de sua aplicação. 

-**Bagagens:**
<p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/B0011_0001.png_1232.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/B0023_0001.png_2232.png" height="300">
 </p>
 
 Ambas as imagens utilizam o filtro de mediana, o operador sobel e a equalização por histograma.
 
 -**Radiografias:**
<p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/color_chest/00000013_026.png_1132.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/color_chest/00000013_026.png_1232.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/color_chest/00000132_002.png_1131.png" height="300">
 </p>

As três imagens utilizam o filtro de mediana e a equalização por histograma. A primeira e a terceira utilizam o filtro laplaciano, enquanto que a segunda utiliza o operador sobel.

Pode-se observar, tanto no caso das bagagens quanto da radiografia, que o inferno colormap fornece à imagem uma boa distinção entre seus elementos. 

 #### Método 6: Spectral colormap
 
 O último dos mapeamentos genéricos obtidos da biblioteca matplotlib, o spectral colormap é um mapeamento divergente.
 
 Podemos ver abaixo alguns exemplos de sua aplicação. 

<p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/color_chest/spectral/00000013_026.png_2052.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/color_chest/spectral/00000099_003.png_2252.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/color_chest/spectral/00000239_003.png_2252.png" height="300">
 </p>

As duas últimas imagens tiveram o operador sobel aplicado. As três imagens passaram pela equalização por histograma.
 
 ## Resultados
 
 ### Imagens de bagagens
 
 Discutiremos, primeiro, os resultados obtidos para as imagens de bagagens, correspondentes à aplicação dos três primeiros métodos de colorização. Faremos primeiro uma comparação geral entre os três métodos e, depois, compararemos mais a fundo os métodos de segmentação da imagem com thresholds.
 
 #### Comparação entre métodos
 
 Temos abaixo alguns exemplos da aplicação dos métodos 1 (thresholds fixos), 2 (thresholds variáveis) e 3 (senos). 
 
 **Exemplo 1: Colorização de bagagens: métodos 1, 2 e 3**
<p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/Method_0/B0009_0001.png_2001.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/Method_1/B0009_0001.png_2011.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/Method_2/B0009_0001.png_2020.png" height="300">
 </p>

  **Exemplo 2: Colorização de bagagens: métodos 1, 2 e 3**
<p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/Method_0/B0011_0001.png_2001.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/Method_1/B0011_0001.png_2011.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/Method_2/B0011_0001.png_2021.png" height="300">
 </p>
 
 A partir das figuras acima, observamos nos três métodos uma boa identificação de objetos de alta densidade. No entanto, como a colorização com o método 2 (senos) fornece um mapeamento com funções contínuas, não temos uma fragmentação de objetos como ocorre no exemplo 2 nos métodos de thresholds (visível na delimitação da arma).
 
 Como ponto negativo do uso do método dos senos frente aos dos thresholds, temos uma menor diferenciação das camadas intermediárias da imagem, como os zippers no canto esquerdo do exemplo 2, que foram corretamente delimitados pelo método de thresholds variáveis.
 
 A cor mais brilhante do método dos senos também pode possivelmente distrair o leitor de ameaças que não sejam de tão alta densidade, enquanto que os métodos de thresholds formam uma imagem um pouco mais uniforme em seu contraste. O uso da colorização por senos pode ser, entretanto, interessante para casos em que os objetos de interesse sejam justamente objetos de alta densidade. 
 
 A seguir, faremos uma comparação mais específica entre os dois métodos de thresholds implementados.

 
 #### Comparação entre métodos de threshold fixo e de threshold variável
 
 O uso de thresholds fixos ou variáveis apresenta suas vantagens e desvantagens. Apresentamos abaixo quatro exemplos de imagens lado ao lado utilizando os dois métodos. À esquerda, temos as imagens colorizadas com o método 1 (de thresholds fixos) e, à direita, temos as colorizadas com o método 2 (de thresholds variáveis). Em seguida, analisaremos os casos em que os métodos têm melhor ou pior desempenho.
 
 **Exemplo 1**
 <p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/Method_0/B0044_0001.png_1001.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/Method_1/B0044_0001.png_1011.png" height="300">
 </p>
 
 **Exemplo 2**
  <p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/Method_0/B0016_0001.png_2001.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/Method_1/B0016_0001.png_2011.png" height="300">
 </p>
 
 **Exemplo 3**
<p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/Method_0/B0026_0001.png_2002.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/Method_1/B0026_0001.png_2012.png" height="300">
 </p>

**Exemplo 4**
<p float="left" align="middle">
<img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/Method_0/B0029_0001.png_2002.png" height="300">
<img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/Method_1/B0029_0001.png_2012.png" height="300">
 </p>

 
 O threshold variável, definido com o método de segmentação, apresenta de fato um comportamento mais adaptável que o método de colorização com thresholds fixos. Ele possui maior sucesso na detecção de objetos de maior densidade, principalmente quando se utiliza uma imagem de menor qualidade final, em que esses objetos possuem intensidades de cinza mais claras que o esperado. 
 
 Isso pode ser observado principalmente nos exemplos 3 e 4. No exemplo 3, o método de thresholds consegue delimitar completamente a arma e o celular presentes na arma, enquanto que o de thresholds fixos os fragmenta inadequadamente. Já no exemplo 4, embora o método de thresholds variáveis falhe de forma similar ao método de thresholds fixo ao fragmentar a arma, ele obtém sucesso ao identificar corretamente os limites da placa e dos zippers. 
 
 No entanto, o método de thresholds variáveis não funciona muito bem na maioria dos casos para a identificação da camada de densidade intermediária, pintada de verda nas imagens. Nos exemplos 2 e 3, quase todo o conteúdo da mala é colorido de verde, mesmo não havendo nenhum objeto naquela região. Uma possível causa desse comportamento é a falta de uma divisão clara para objetos de densidade intermediária nas imagens utilizadas: quase todas possuem apenas objetos metálicos, de alta densidade, sem objetos plásticos ou orgânicos.
 
 Nesses casos, o método de thresholds fixos possui algumas vezes um funcionamento em comparação melhor, como no exemplo 1, em que a camada verde limita-se a cabos e a dobras da mochila com maior densidade.


 #### Uso do operador sobel
 
 O operador sobel tem um comportamento variável quando aplicado às imagens de bagagens. Temos casos em que o realce de bordas fornecido por ele destaca bem os itens da figura, tornando ainda mais distinta a presença de objetos perigosos. Exemplos dessas imagens são dados logo abaixo: 
 
<p float="left" align="middle">
<img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/Method_0/B0023_0001.png_2202.png" height="300">
<img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/Method_1/B0026_0001.png_2212.png" height="300">
 </p>
 
 A primeira imagem utiliza o método 1 de colorização (thresholds fixos), enquanto a segunda imagem utiliza o método 2 (thresholds variáveis).

No entanto, como consequência do realce de bordas, o operador sobel em alguns casos intensifica demasiadamente ruídos já presentes na imagem. Isso pode ser observado na imagem abaixo:

<p float="left" align="middle">
<img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/Method_1/B0011_0001.png_2212.png" height="300">
 </p>

O operador sobel também tende a não funcionar bem com imagens que obtêm um mal resultado com a equalização por histograma. Como exemplo, temos as imagens a seguir:

<p float="left" align="middle">
<img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/Method_1/B0009_0001.png_2212.png" height="300">
<img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/Color_Bags/Method_2/B0009_0001.png_2222.png" height="300">
 </p>
 
 Ambas as imagens têm a mesma imagem original,tendo a primeira sido colorizada com o método 1 (thresholds variáveis) e a segunda colorizada com o método 2 (senos). Como pode ser visto, como a equalização não consegue aumentar de forma eficaz o contraste da imagem, obtemos com o operador sobel um resultado final escuro, que não é bem colorizado pelos métodos que possuímos.
 
 ### Radiografias
 
 #### Comparação entre métodos
 
 A seguir, temos três exemplos de imagens colorizadas segundo os três métodos genéricos: hot, inferno e spectral. Dentre os exemplos, o exemplo 3 apresenta uma imagem com o operador sobel aplicado.
 
 **Exemplo 1: colormaps hot, inferno e spectral**
 <p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/color_chest/hot/00000099_003.png_2032.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/color_chest/inferno/00000099_003.png_2042.png" height="300">
  <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/color_chest/spectral/00000099_003.png_2052.png" height="300">
 </p>
 
  __Exemplo 2:__
 <p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/color_chest/hot/00003899_002.png_2032.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/color_chest/inferno/00003899_002.png_2042.png" height="300">
  <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/color_chest/spectral/00003899_002.png_2052.png" height="300">
 </p>
 
   __Exemplo 3:__
  <p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/color_chest/hot/00000132_002.png_2232.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/color_chest/inferno/00000132_002.png_2242.png" height="300">
  <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/color_chest/spectral/00000132_002.png_2252.png" height="300">
 </p>
 
 A partir das imagens acima, podemos identificar algumas vantagens e desvantagens para os diferentes mapeamentos de cores. De início, temos que o colormap Spectral, por ser um mapeamento divergente, possui um grande contraste entre o fundo e os ossos mais claros(vermelho) e a parte mostrada em azul. Isso pode ser um aspecto negativo para a imagem, já que pode distrair o leitor de detalhes apresentados em vermelho e criar um falsa dicotomia entre ossos apresentados em azul ou em vermelho.
 
 Já os colormaps hot e inferno, por serem sequenciais, não apresentam esse problema. As cores variam de maneira uniforme na imagem, sem dar destaque excessivo a uma ou a outra parte. No entanto, o colormap hot apresenta uma desvantagem em relação ao inferno: as partes mais brilhantes da imagem, em amarelo claro, dificultam a visualização de detalhes. Isso não ocorre no inferno, por ser um mapa perceptualmente uniforme aos olhos humanos, o que possibilita que as características sejam distinguidas da mesma forma em qualquer valor de intensidade.
 
 Sendo assim, o mapeamento inferno apresenta o melhor resultado com as radiografias, possuindo um aspecto mais agradável aos olhos humanos e fornecendo uma maior distinção dos elementos da imagem, o que é o objetivo da colorização.
 
 #### Uso do operador sobel
 
Por fim, avaliamos o uso do operador sobel sobre imagens de radiografia. A seguir, temos dois exemplos de imagens, sem e com o operador sobel aplicado. O colormap inferno foi utilizado por ter sido considerado o de melhor resultado.

   **Exemplo 1: sem aplicação do operador sobel (esquerda) e com o operador sobel (direita)**
 <p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/color_chest/inferno/00003896_005.png_2042.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/color_chest/inferno/00003896_005.png_2242.png" height="300">
 </p>
 
   **Exemplo 2: sem aplicação do operador sobel (esquerda) e com o operador sobel (direita)**
 <p float="left" align="middle">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/color_chest/inferno/00003989_024.png_2042.png" height="300">
 <img src="https://github.com/elisasaltori/XRayColorizing/raw/master/Test_Images/Color_Images/color_chest/inferno/00003989_024.png_2242.png" height="300">
 </p>
 
 Em primeiro lugar, podemos perceber que o operador sobel possui um comportamento mais uniforme em radiografias em comparação aos resultados obtidos em sua aplicação sobre bagagens. Isso torna seu uso mais interessante, já que ele poderia então ser aplicado sobre múltiplas imagens radiografias sem uma verificação mais detalhada das características individuais de cada imagem.
 
 Temos que o operador sobel realça os contornos da imagem de forma consistente, com adição limitada de ruído. Contornos de costelas e vértebras se tornam imediatamente visíveis e regiões de intensidades similares têm também suas características mais evidenciadas. 
 
 No entanto, a aplicação do operador tem também suas desvantagens. Alguns artefatos são criados ou destacados com o realce de bordas, o que poderia prejudicar o diagnótisco de acordo com a informação que um professional desejasse extrair da imagem. A mudança do aspecto original da radiografia também poderia, inicialmente, causar estranhaze ao médico, acostumado a radiografias usuais em preto e branco, sem realce de bordas.
 
 Portanto, o uso do operador sobel deveria ser discutido junto a profissionais médicos, com o objetivo de definir em que casos sua aplicação seria benéfica. Podemos dizer, no entanto, que sua aplicação sobre radiografias tem um efeito consistente, realçando de forma efetiva os elementos da imagem, e que, assim, seu uso não deveria ser rapidamente descartado.
 

 
 ## Referências

[1] How To Read An Airport Security X-Ray Image. Disponível em: < http://snallabolaget.com/how-to-read-an-airport-security-x-ray-image/ >. Acesso em: 12 jun. 2018.

[2] KASE, Kannan. Effective Use of Color in X-ray Image Enhancement for Luggage Inspection. 2002. 30f. Dissertação de Mestrado - University of Tennessee, Knoxville, 2002.

[3] Deng-Yuan Huang, Ta-Wei Lin, Wu-Chih Hu, Automatic Multilevel Thresholding Based on Two-Stage Otsu's Method with Cluster Determination by Valley Estimation, Int. Journal of Innovative Computing, 2011.
