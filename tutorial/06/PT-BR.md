# 1. Introdu��o

A gest�o de recursos h�dricos demanda informa��es obtidas normalmente por sistemas de informa��es geogr�ficas(SIGs) que possam responder a perguntas como: qual a �rea de contribui��o a montante de um determinado ponto em um rio? Ou qual o comprimento total dos cursos d'�gua de uma determinada bacia? Ou qual a disponibilidade h�drica em um determinado ponto? Para responder a estas e outras perguntas, a Ag�ncia Nacional de �guas(ANA) disponibiliza a base hidrogr�fica ottocodificada(BHO), composta por planos de informa��es geogr�ficas como trechos de drenagem, �reas de contribui��o hidrogr�fica e outras informa��es pr�-processadas para ajudar na tomada de decis�o em recursos h�dricos.

Nas diversas propostas de codifica��o ou classifica��o de bacias hidrogr�ficas, destaca-se a codifica��o desenvolvida pelo brasileiro Otto Pfafstetter. Dentre as vantagens dessa codifica��o pode-se citar: representa��o hier�rquica, baseada na topografia da �rea drenada, economia de d�gitos, informa��o topol�gica embutida nos d�gitos, aplicabilidade global e integra��o com Sistemas de Informa��es Geogr�ficas por meio da linguagem SQL.

O presente tutorial descreve passo a passo os procedimentos para construir a base hidrogr�fica ottocodificada(BHO). A constru��o dessa base � realizada por meio das funcionalidades do projeto open-source [pghydro](www.pghydro.org), que � uma extens�o para o sistema gerenciador de banco de dados espaciais PostgreSQL/PostGIS.

Essas funcionalidades s�o acionadas por meio de uma interface desenvolvida na linguagem Python como plugin intitulado Pghydro Tools para o Sistema de Informa��o Geogr�fica QGIS (www.qgis.org).

S�o oferecidos aos componentes do SINGREH cursos de capacita��o e apoio t�cnico para a constru��o de bases hidrogr�ficas ottocodificadas(BHO). Para tanto � necess�ria solicita��o formal da institui��o interessada dirigida � Coordena��o do Conjuntura e Gest�o da Informa��o (CCOGI) da Superintend�ncia de Planejamento de Recursos H�dricos (SPR) da Ag�ncia Nacional de �guas (ANA) pelo e-mail: bho@ana.gov.br

## 1.2.	Aspectos Gerais

O processo de constru��o de Bases Hidrogr�ficas Ottocodificadas(BHO) para a gest�o de Recursos H�dricos est� condicionado pelas normas vigentes tais como a Constitui��o Federal, as Leis 9.433/1997 e 9.984/2000 e as resolu��es pertinentes do CNRH e da pr�pria ANA.

Uma das mais importantes � a Resolu��o n� 399/2004 que estabelece o crit�rio t�cnico para a identifica��o do curso d��gua principal de uma bacia hidrogr�fica e estabelece esse curso d��gua assim determinado como a unidade � qual se atribui o dom�nio estadual ou da Uni�o, segundo o crit�rio estabelecido na Constitui��o.

Desse modo, s�o passos fundamentais na elabora��o de uma BHO, a determina��o e codifica��o dos cursos d��gua e a atribui��o do dom�nio a esses cursos.

Observe-se que n�o � a unidade "Rio" que recebe o dom�nio, e sim o curso d'�gua. Segundo a Resolu��o n� 399/2004, o curso d��gua principal de uma bacia � assim definido:

"Em cada conflu�ncia ser� considerado curso d��gua principal aquele cuja bacia hidrogr�fica tiver a maior �rea de drenagem."

Dessa forma, um curso d��gua pode ser composto de trechos de v�rios rios, tal como ocorre com o curso d��gua principal da bacia do S�o Francisco, composto por parte do rio S�o Francisco mais o rio Sambur�, ou o curso principal da bacia Amaz�nica, composto pelo rio Amazonas, mais o rio Solim�es, mais o rio Mara��n e trechos sem nome nas cabeceiras nos Andes. Outra consequ�ncia not�vel desse crit�rio � que um rio pode ser parte da Uni�o e parte de um estado, como � o caso do rio S�o Francisco, cujas cabeceiras s�o de dom�nio do estado de Minas Gerais.

A Resolu��o n� 30/2002 do CNRH estabelece a codifica��o de Otto Pfafstetter como a codifica��o oficial de bacias do Brasil, a qual, pelas suas caracter�sticas t�cnicas, se adequa perfeitamente � l�gica estabelecida pela Resolu��o n� 399/2004.

O processo descrito no presente documento se baliza nos textos normativos citados e os traduz em instru��es para a elabora��o de uma base de dados geogr�fica totalmente aderente �s necessidades da gest�o de recursos h�dricos.

Algumas premissas resultam dessa necess�ria submiss�o aos textos legais e infralegais:

1. As bases s�o oriundas da cartografia oficial.
2. Preserva-se ao m�ximo o tra�ado e posicionamento constantes na cartografia oficial. Erros detectados s�o encaminhados para corre��o pelos �rg�os respons�veis � IBGE e DSG;
3. Em caso de d�vidas, os �rg�os respons�veis pela cartografia oficial s�o consultados;
4. A codifica��o das bacias � feita segundo o m�todo de Otto Pfafstetter. Foram desenvolvidas codifica��es derivadas, como c�digo de trecho, de curso d��gua e de rio;
5. Os c�digos convencionados como ponto de partida para a codifica��o s�o os estabelecidos pela Resolu��o n� 30/2002 do CNRH;
6. Os dom�nios s�o identificados usando os limites pol�ticos publicados pelo IBGE e os crit�rios estipulados pela Constitui��o Federal de 1988 em seu Art. 20, inciso III.


## 1.3. Objetivo

O objetivo deste tutorial � descrever o processo que resulta na constru��o de uma Base Hidrogr�fica Ottocodificada(BHO), desde a prepara��o dos dados de entrada at� a sistematiza��o dos nomes de rios, que � a �ltima etapa do processo.
 
## 1.4. Resumo do Processo

A constru��o da base hidrogr�fica em banco de dados espaciais proposta nesse trabalho compreende 7 etapas:

1. Criar a extens�o espacial e a extens�o pghydro no banco de dados espaciais;
2. Preparar o dado de entrada da camada de hidrografia composta por dados vetoriais do tipo linha;
3. Inserir e consistir a rede hidrogr�fica no banco de dados;
4. Gerar as �reas de contribui��o hidrogr�fica a partir do Modelo Digital de Eleva��o Hidrologicamente Consistente(MDEHC);
5. Inserir e consistir as �reas de contribui��o hidrogr�fica no banco de dados;
6. Verificar a consist�ncia da rede de drenagem com as �reas de contribui��o hidrogr�fica;
7. Gerar a codifica��o de bacias de Pfafstetter e as informa��es hidrogr�ficas finais.

Opcional: Dominialidade de Cursos D��gua, Agrega��o de Bacias e Sistematiza��o da Hidron�mia (nomes dos corpos d'�gua).

Figura 1 - Fluxo geral do processo de constru��o da base hidrogr�fica ottocodificada

Figura 2 - Fluxo do processo de consist�ncia dos trechos de drenagem

Figura 3 - Fluxo do processo de consist�ncia das �reas de contribui��o hidrogr�fica

Figura 4 - Fluxo do processo de consist�ncia dos trechos de drenagem com as �reas de contribui��o hidrogr�fica

Figura 5 - Fluxo do processo de gera��o das informa��es hidrogr�ficas finais