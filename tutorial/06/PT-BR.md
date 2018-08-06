# 1. Introdução

A gestão de recursos hídricos demanda informações obtidas normalmente por sistemas de informações geográficas(SIGs) que possam responder a perguntas como: qual a área de contribuição a montante de um determinado ponto em um rio? Ou qual o comprimento total dos cursos d'água de uma determinada bacia? Ou qual a disponibilidade hídrica em um determinado ponto? Para responder a estas e outras perguntas, a Agência Nacional de Águas(ANA) disponibiliza a base hidrográfica ottocodificada(BHO), composta por planos de informações geográficas como trechos de drenagem, áreas de contribuição hidrográfica e outras informações pré-processadas para ajudar na tomada de decisão em recursos hídricos.

Nas diversas propostas de codificação ou classificação de bacias hidrográficas, destaca-se a codificação desenvolvida pelo brasileiro Otto Pfafstetter. Dentre as vantagens dessa codificação pode-se citar: representação hierárquica, baseada na topografia da área drenada, economia de dígitos, informação topológica embutida nos dígitos, aplicabilidade global e integração com Sistemas de Informações Geográficas por meio da linguagem SQL.

O presente tutorial descreve passo a passo os procedimentos para construir a base hidrográfica ottocodificada(BHO). A construção dessa base é realizada por meio das funcionalidades do projeto open-source [pghydro](www.pghydro.org), que é uma extensão para o sistema gerenciador de banco de dados espaciais PostgreSQL/PostGIS.

Essas funcionalidades são acionadas por meio de uma interface desenvolvida na linguagem Python como plugin intitulado Pghydro Tools para o Sistema de Informação Geográfica QGIS (www.qgis.org).

São oferecidos aos componentes do SINGREH cursos de capacitação e apoio técnico para a construção de bases hidrográficas ottocodificadas(BHO). Para tanto é necessária solicitação formal da instituição interessada dirigida à Coordenação do Conjuntura e Gestão da Informação (CCOGI) da Superintendência de Planejamento de Recursos Hídricos (SPR) da Agência Nacional de Águas (ANA) pelo e-mail: bho@ana.gov.br

## 1.2.	Aspectos Gerais

O processo de construção de Bases Hidrográficas Ottocodificadas(BHO) para a gestão de Recursos Hídricos está condicionado pelas normas vigentes tais como a Constituição Federal, as Leis 9.433/1997 e 9.984/2000 e as resoluções pertinentes do CNRH e da própria ANA.

Uma das mais importantes é a Resolução nº 399/2004 que estabelece o critério técnico para a identificação do curso d’água principal de uma bacia hidrográfica e estabelece esse curso d’água assim determinado como a unidade à qual se atribui o domínio estadual ou da União, segundo o critério estabelecido na Constituição.

Desse modo, são passos fundamentais na elaboração de uma BHO, a determinação e codificação dos cursos d’água e a atribuição do domínio a esses cursos.

Observe-se que não é a unidade "Rio" que recebe o domínio, e sim o curso d'água. Segundo a Resolução nº 399/2004, o curso d’água principal de uma bacia é assim definido:

"Em cada confluência será considerado curso d’água principal aquele cuja bacia hidrográfica tiver a maior área de drenagem."

Dessa forma, um curso d’água pode ser composto de trechos de vários rios, tal como ocorre com o curso d’água principal da bacia do São Francisco, composto por parte do rio São Francisco mais o rio Samburá, ou o curso principal da bacia Amazônica, composto pelo rio Amazonas, mais o rio Solimões, mais o rio Marañón e trechos sem nome nas cabeceiras nos Andes. Outra consequência notável desse critério é que um rio pode ser parte da União e parte de um estado, como é o caso do rio São Francisco, cujas cabeceiras são de domínio do estado de Minas Gerais.

A Resolução nº 30/2002 do CNRH estabelece a codificação de Otto Pfafstetter como a codificação oficial de bacias do Brasil, a qual, pelas suas características técnicas, se adequa perfeitamente à lógica estabelecida pela Resolução nº 399/2004.

O processo descrito no presente documento se baliza nos textos normativos citados e os traduz em instruções para a elaboração de uma base de dados geográfica totalmente aderente às necessidades da gestão de recursos hídricos.

Algumas premissas resultam dessa necessária submissão aos textos legais e infralegais:

1. As bases são oriundas da cartografia oficial.
2. Preserva-se ao máximo o traçado e posicionamento constantes na cartografia oficial. Erros detectados são encaminhados para correção pelos órgãos responsáveis – IBGE e DSG;
3. Em caso de dúvidas, os órgãos responsáveis pela cartografia oficial são consultados;
4. A codificação das bacias é feita segundo o método de Otto Pfafstetter. Foram desenvolvidas codificações derivadas, como código de trecho, de curso d’água e de rio;
5. Os códigos convencionados como ponto de partida para a codificação são os estabelecidos pela Resolução nº 30/2002 do CNRH;
6. Os domínios são identificados usando os limites políticos publicados pelo IBGE e os critérios estipulados pela Constituição Federal de 1988 em seu Art. 20, inciso III.


## 1.3. Objetivo

O objetivo deste tutorial é descrever o processo que resulta na construção de uma Base Hidrográfica Ottocodificada(BHO), desde a preparação dos dados de entrada até a sistematização dos nomes de rios, que é a última etapa do processo.
 
## 1.4. Resumo do Processo

A construção da base hidrográfica em banco de dados espaciais proposta nesse trabalho compreende 7 etapas:

1. Criar a extensão espacial e a extensão pghydro no banco de dados espaciais;
2. Preparar o dado de entrada da camada de hidrografia composta por dados vetoriais do tipo linha;
3. Inserir e consistir a rede hidrográfica no banco de dados;
4. Gerar as áreas de contribuição hidrográfica a partir do Modelo Digital de Elevação Hidrologicamente Consistente(MDEHC);
5. Inserir e consistir as áreas de contribuição hidrográfica no banco de dados;
6. Verificar a consistência da rede de drenagem com as áreas de contribuição hidrográfica;
7. Gerar a codificação de bacias de Pfafstetter e as informações hidrográficas finais.

Opcional: Dominialidade de Cursos D’água, Agregação de Bacias e Sistematização da Hidronímia (nomes dos corpos d'água).

Figura 1 - Fluxo geral do processo de construção da base hidrográfica ottocodificada

Figura 2 - Fluxo do processo de consistência dos trechos de drenagem

Figura 3 - Fluxo do processo de consistência das áreas de contribuição hidrográfica

Figura 4 - Fluxo do processo de consistência dos trechos de drenagem com as áreas de contribuição hidrográfica

Figura 5 - Fluxo do processo de geração das informações hidrográficas finais