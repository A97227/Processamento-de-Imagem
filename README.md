# Segmentação Automática de Vasos 

## Retinianos em Imagens de Fundo de Olho

## Alunos: Luís Gonçalves PG54011; Sandra Ferreira PG54221 Docente: Carlos Silva

## Abstrato
Neste projeto criou-se um método adaptado para a segmentação de vasos da retina, 
fornecendo apenas a imagem do fundo do olho a segmentar. Para tal, teve-se por 
base, o método descrito por S. Chaudhuri em [1], no qual alterou-se o tratamento de 
bordas adaptado para cada imagem a segmentar

## I. Introdução

A segmentação autónoma de vasos da retina permite a análise e deteção das variações 
apresentadas nessa zona, auxiliando assim o diagnostico de diferentes patologias que 
afetam a retina e os seus vasos. Para tal, ao longo deste projeto serão utilizadas diferentes 
técnicas de Processamento de Imagem, para retirar a informação necessária das imagens 
a estudar, baseando-se na técnica proposta em [1]. Faz-se uso da linguagem de 
programação Python e diferentes bibliotecas para a manipulação de imagens sendo as 
mais relevantes o package numpy, matplotlib e scipy.
Para testar o funcionamento do método criado, foi utilizado o Dataset, DRIVE,
composto por 40 conjuntos de imagens, que são compostos por imagens digitais da zona 
ocular, designada por fundus, onde está presente a retina, o disco ótico e a mácula, 
imagens das máscaras da zona fundus e imagens para os vasos detetados.

## II. Métodos

No presente trabalho, foi desenvolvido e implementado um algoritmo com o objetivo 
de segmentar e detetar vasos retinianos. Estes passos são importantes uma vez que levam 
a uma melhoria na qualidade da imagem dos vasos, que por sua vez auxilia no diagnóstico 
de patologias como retinopatia diabética, glaucomas ou hipertensão,[2]
nas quais os vasos 
sanguíneos podem apresentar anomalias mensuráveis no diâmetro e cor. Por exemplo, a 
hipertensão pode resultar em constrição focal das artérias retinianas, já os diabetes podem 
gerar novos vasos, neovascularização. Este processo de deteção de vasos também está 
presente no diagnóstico automatizado de doenças oculares, como por exemplo algoritmos 
de deteção de linha, usado para extração e descrição subsequente de padrões de vasos 
retinianos.
Contudo, a maioria dos algoritmos não leva em consideração as propriedades singulares 
dos objetos a serem detetados, o que nos casos dos vasos retinianos são chamados de 
‘antiparalelos’ e são representados por segmentos lineares que estão orientados a 180 
graus. Todavia estes pares podem ser aproximados por segmentos linear em partes, uma 
vez que os vasos têm pequenas curvaturas [1]
. 
Outro importante fator a destacar, é que os vasos retinianos apresentam uma menor 
reflecção, comparativamente a outras superfícies da retina o que leva a que os mesmo 
apresentem uma tonalidade mais escura quanto mais próxima do centro do vaso, como tal, 
embora o perfil de intensidade varie um pouco, pode ser aproximado por uma curva 
gaussiana,
𝑓(𝑥, 𝑦) = 𝐴 ⋅ {1 − 𝑘 ⋅ exp (−
𝑑
2
2⋅𝑎2
)} Eq. 1
 
Por fim um último aspeto a ter em consideração é a largura dos vasos diminui à 
medida que se afasta do disco ótico, embora seja gradual a faixa de variação ocorre entre 
2 a 10 pixéis (36 µm-180µm ).
Como foi supracitado a segmentação exata dos vasos é uma tarefa complexa, 
devido à presença de ruído, à variabilidade da largura, brilho e forma dos vasos e ao baixo 
contraste entre os vasos e o fundo. Além da presença de doenças patológicas. De forma a 
ultrapassar estes entraves, vários métodos têm sido propostos na literatura, como matched 
filter, este método é projetado para maximizar a relação sinal-ruído (SNR) na saída do 
sistema, de forma a ajustar a resposta do filtro seja o mais próximo possível com o sinal 
que se deseja detetar [3], neste caso com a imagem que se pretende obter. No âmbito 
unidirecional a podemos ter em conta a seguinte equação que representa um sinal passado 
um matched filter:
 𝑠0
(𝑡) = 𝐹
−1
{𝐻 (𝑓(𝑆(𝑓) + 𝑁(𝑓)))} Eq. 2
onde S(f) é a transformada de Fourier de s(t) e N(f) é o espectro de ruído. Usando a 
desigualdade de Schwartz, pode ser provado que o filtro H(f) que maximiza a razão sinalruído de saída é dado por 𝐻𝑜𝑝𝑡(𝑓) = 𝑆(𝑓) Como o sinal de entrada(t) é real,𝐻𝑜𝑝𝑡(𝑓) =
𝑆(−𝑓) . Esse filtro ótimo com a resposta ao impulso h(t) é conhecido como Matched Filter 
para o sinal s(t). Em um sistema de comunicação típico, se houver n sinais diferentes 𝑠𝑖
(𝑡), 
i = (1, 2, …, n), o sinal recebido é passado por uma pilha de n filtros casados. Se a resposta 
devido ao filtro j for a máxima, conclui-se que o sinal𝑠𝑗
(𝑡) foi transmitido.[1]
No entanto no contexto de detecção dos vasos retinianos pode-se verificar que o 
perfil de intensidade é possível de ser assumido como simétrico em relação ao centro do 
vaso, por isso s(-t)=s(t) pelo que o filtro ótimo é dado por ℎ𝑜𝑝𝑡(𝑑) = − exp (−
𝑑
2
2⋅𝑎2
), 
comparativamente à situação unidimensional é importante referir que a componente n 
relativa aos diferente tipos de objeto, deixa de ser necessária e é apenas importante referir 
se um pixel específico pertence ou não ao vaso, no caso do mesmo pertencer, a magnitude 
de saída filtrada será superior a um determinado limiar definido anteriormente. 
No que diz respeito ao matched filter, no contexto bidirecional, é importante 
salientar que um vaso pode estar orientado num ângulo entre 0 e 180º, desta forma o 
matched filter s(t) apresenta o seu valor máximo quando o ângulo for 90º. Assim o filtro 
necessita de passar por todas as rotações possíveis entre o intervalo referido anteriormente, 
de forma que as respostas correspondentes sejam comparadas em cada pixel, onde apenas 
a resposta máxima é recolhida [1]
. Uma vez que a resposta deste filtro para um dado pixel, 
está inserido no fundo da retina, e considerando que o mesmo tem uma intensidade 
constante com ruído gaussiano branco aditivo de média zero, o valor de saída do filtro seria 
zero, por essa razão a convolução do Kernel resulta da subtração do valor médio da própria 
função, s(t).[1]
Para realizar uma melhoria do resultado, optou-se por combinar várias secções 
transversais ao longo do comprimento do vaso, o qual pode ser expresso pela equação 3
ℎ(𝑥,𝑦) = − exp (−
𝑥
2
2⋅𝑎2
) Eq. 3 
Onde |y| ≤ L/2, no qual L, que foi definido com valor 9, representa o comprimento do 
segmento assumido para os vasos com orientação fixa. 
III. Algoritmo
No sentido de realizar a implementação do algoritmo anteriormente referido, utilizou-se a 
aplicação JupyterNotebook na linguagem Python. Para tal, foi necessário realizar o import de 
algumas bibliotecas que foram fulcrais ao longo do código, nomeadamente Numpy, usada para o 
processamento de arrays e Math para a implementação das equações matemáticas do matched 
filter.
De seguida criou-se a função gaussiana, que pode ser consultada em anexo, que implementa 
o Matched Filter nas condições bidimensionais referidas acima, que tem como argumentos, o 
ângulo de rotação, o sigma com valor 2 e L com valor 9. É importante salientar que nesta função 
é calculado uma matriz de rotação 2x2 dada pela Figura 1, onde teta apresenta uma resolução 
angular de 15º o que retoma um Kernel para cada ângulo definido em teta, que são aplicados à 
imagem de fundo em cada pixel. Estes kernels são definidos para uma vizinhança N, tal que 𝑁 =
{(𝑢, 𝑣) | |𝑢| ≤ 3𝜎, |𝑣| ≤ 𝐿/2}, onde os valores que não pertencem a vizinhança são iguais a 0.
 
 Figura 1 – Matriz de rotação
Foi necessário ainda obter os valores da máscara convulucional 𝐾
′
𝑗
(𝑥, 𝑦) = 𝐾𝑗
(𝑥, 𝑦) − 𝑚𝑖
, 
onde 𝐾𝑗
(𝑥, 𝑦) seja igual aos valores obtidos pela equação 3, e 𝑚𝑖 = ∑(𝑥,𝑦)∈𝑁 𝐾𝑖(𝑥, 𝑦)/𝐴, sendo 
A o numero de pixéis da imagem. Foi necessário ainda multiplicar estes coeficientes por 10 e 
arredondá-los ao inteiro mais próximo. Garantiu-se também que a média destes kernels fosse 
negativa diminuindo o ruido criado.
Consequentemente foi criada uma função apli_filtro que aplica a filtragem formulada 
na função anterior, à imagem de entrada, chamando a função anterior para obter os 
diferentes Kernels. Ao ser aplicado deve-se escolher o maior valor obtido para cada pixel,
dentro dos 12 Kernels criados. Foi feita uma normalização e ajuste dos valores para 
produzir a imagem final. É ainda aplicado um filtro médio para diminuir o ruído da 
imagem, através na função apli_med.
Após estes processos foi necessário remover a borda das imagens da retina, contudo 
o mesmo não foi possível de ser realizado com as máscaras previamente fornecidas uma 
vez que as mesmas não se encontravam nas melhores condições. Por essa razão foi criado 
um algoritmo capaz de criar máscaras das próprias imagens fornecidas.
Sucessivamente foram criadas as funções, fit_elipse e binarizacao, de forma que na
primeira é retirada a máscara à imagem segmentada de forma a eliminar a borda e na 
segunda é feita a binarização da imagem pelo método de Otsu, de forma a isolar e destacar 
os vasos sanguíneos na imagem, de forma a não apresentarem vestígios da restante 
imagem.
Por fim de forma a determinar as métricas foi realizada uma função avaliação na qual 
é determinada a Accuracy, Sensibility, Specificity e Positive Predictive Value, as quais 
medem o desempenho do modelo de classificação, a capacidade de o modelo identificar 
corretamente as instâncias positivas, a capacidade de identificar corretamente as 
instâncias negativas e a precisão do modelo no que diz respeito a identificar como 
positivas as instâncias que são realmente positivas.
Relativamente ao algoritmo de criação das máscaras, começou-se por utilizar o filtro 
médio para diminuir o ruído da imagem inicial. De seguida é criada uma função para a 
construção de um filtro gaussiano com o fim de realizar operações de filtragem, através 
de um Kernel Gaussiano de dispersão sigma. Após implementar estas funções, é realizado 
um gradiente de magnitude, através de uma função gradiente, a qual faz uso de duas
derivadas parciais em relação ao eixo x e y, através do operador Sobel, com um tamanho 
equivalente a 3 em cada derivada. O gradiente de magnitude resulta da combinação das 
duas derivadas por meio da fórmula de Pitágoras, hipotenusa é igual à raiz quadrada da 
soma dos quadrados. Esta função tem como objetivo destacar as bordas na imagem 
original para mais tarde tratá-las a fim de realizar a construção da máscara.
Subsequentemente realiza-se a binarização, dilatação e subtração das bordas da 
imagem pela ordem indicada. Depois de realizar a subtração da imagem é aplicado uma 
função terceira2 com o objetivo de segmentar a imagem proveniente da subtração e isolar 
a camada mais interna da borda da imagem.
Por fim a imagem é preenchida dando origem à máscara através de duas funções, 
preencher_borda_com_branco e ultimo, onde a primeira retoma uma imagem preenchida 
com pixels brancos e a segunda retoma a máscara final após alguns processamento, 
nomeadamente erosão da imagem.

## IV. Resultados

Na implantação do algoritmo utilizou-se como foi supracitado, o dataset fornecido, 
para exemplo recorreu-se à primeira imagem fundus, como pode ser observado no Anexo 
C1, no qual se aplicou o matched filter referido anteriormente, com os valores de σ=2 e 
de L=9, obtendo-se o resultado observado na segunda imagem do Anexo C1. Utilizou-se
previamente um filtro médio 5 x 5 a fim de remover ruido não desejado nas imagens 
originais. Após isto aplicou-se então o método descrito e concluindo a deteção dos vasos 
usou-se as máscaras fornecidas pelo dataset para a remoção das bordas destas imagens.
Como foi referido anteriormente, reparou-se que as máscaras não se ajustavam 
corretamente a todas as imagens de maneira igual, sendo que em algumas as bordas eram
removidas e noutras a ainda existia parte destas, fazendo com que no passo seguinte, isto 
é, na binarização da imagem através do método de binarização de Otsu, obtivesse-se um 
valor superior ao esperado, o que leva a que não sejam detetados parte dos vasos, que se 
encontrem abaixo do threshold.
Posto isto analiso-se a qualidade do método ao compará-lo com a máscara dos vasos 
fornecida no Dataset. Assim obteve-se os valores na Tabela 1 com os dados das métricas 
calculadas, sendo apenas consideradas as mais relevantes a Accuracy, Sensitivity, 
Specificity e Positive Predictive Value.
Tabela 1 – Métricas obtidas seguindo o método em [1].
ACC SEN SPEC PPV
62,1±1,30% 32,3±15,3% 98,6±0,4% 71,8±18,1%
Tendo por base os valores obtidos nas métricas, considerou-se que as bordas seriam 
algo a melhorar, por isso, para cada imagem foram criadas bordas adaptadas as imagens, 
fazendo com que fossem removidas por completo, podendo ser comparado entre as
Imagens do Anexo C6, onde representam respetivamente a imagem original, o método 
inicial, e o proposto. Com este método proposto queremos então homogeneizar a 
qualidade da imagem, fazendo com deixe de haver imagens com apenas as bordas, e que 
passe a ser segmentado também os vasos.
Assim com este tratamento de bordas, estas são removidas completamente, fazendo 
então com que estas já não tenham influência nos valores de threshold calculados por 
Otsu. 
Calculou-se novamente as métricas, desta vez para o método proposto, sendo 
observado na Tabela 2.
Tabela 2 – Métricas obtidas seguindo o método proposto.
ACC SEN SPEC PPV
62,2±0,9% 47,5±7,8% 98,8±1,1% 86,0±7,6%
Comparou-se as imagens obtidas e como foi referido anteriormente no método 
proposto observa-se uma maior quantidade de vasos nas imagens afetadas anteriormente 
pela borda. Notou-se ainda que em ambas, foram segmentadas zonas referentes aos discos
óticos, bem como algumas possíveis lesões observáveis na retina, fazendo com que haja 
uma segmentação de zonas erradas, diminuindo assim a qualidade dos valores métricos 
calculados.
Quanto às métricas houve um aumento do valor médio, embora que a exatidão e 
especificidade o aumento seja bem ligeiro, de cerca de, 0.1% e 0.2% respetivamente. 
Houve ainda uma diminuição do desvio padrão mostrando que a qualidade das imagens 
está mais homogénea, tendo elas uma qualidade mais semelhante.
No Anexo C é possível observar os resultados para todas as 40 imagens, sendo que a 
primeira é a imagem original, a segunda foi usada o método descrito em [1] e a terceira o 
método proposto, isto para cada imagem.
Outro tópico importante de fazer referência é o facto de que tal como a máscara do 
fundus da retina, a máscara dos vasos retinais podem também não ter uma alta qualidade, 
uma vez que pertencem ao mesmo dataset.

## V.Conclusão
Concluindo, neste projeto foi criado um sistema autónomo capaz de segmentar vasos 
através de uma imagem da retina fornecida.
Atendendo aos objetivos propostos para este trabalho, este foram cumpridos, uma vez 
que foi desenvolvido kernels complexos para extração de informação na imagem e 
desenvolvidos uma cadeia de processamento de imagem.
Em termos de trabalhos futuros uma possibilidade seria a tentativa de remoção de 
zonas indesejadas como o disco ótico e lesões, bem como uma melhor deteção de vasos 
mais finos.
Em suma, este projeto possibilitou processamento de uma imagem retiniana e a 
segmentação dos seus vasos.

## VI. Bibliografia
[1] S. Chaudhuri, S. Chatterjee, N. Katz, M. Nelson and M. Goldbaum, "Detection of 
blood vessels in retinal images using two-dimensional matched filters," in IEEE 
Transactions on Medical Imaging, vol. 8, no. 3, pp. 263-269, Sep 1989
[2] Carlos Silva, Guia Do Projeto – “Segmentação Automática de Vasos Retinianos em 
Imagens de Fundo de Olho”
[3] Microsoft PowerPoint - chp 3 lecture notes – Acessado em : 
https://ee.eng.usm.my/eeacad/mandeep/EEE436/chp%203.pdf 01/02/2024
