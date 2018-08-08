# 1. Comece Aqui 

## 1.1 Sobre Este Tutorial

Esse tutorial apresenta uma solução completa de construção de uma **Base Hidrográfica Ottocodificada** por meio de Sistema de Banco de Dados Espaciais (PostgreSQL/PostGIS) e está dividido em 7 etapas obrigatórias e 3 etapas opcionais:

1. Preparar o dado de entrada da camada de hidrografia composta por dados vetoriais do tipo linha;

2. Criar a extensão espacial e a extensão pghydro no banco de dados espaciais;

3. Inserir e consistir a rede hidrográfica no banco de dados;

4. Gerar as áreas de contribuição hidrográfica a partir do Modelo Digital de Elevação Hidrologicamente Consistente(MDEHC);

5. Inserir e consistir as áreas de contribuição hidrográfica no banco de dados;

6. Verificar a consistência da rede de drenagem com as áreas de contribuição hidrográfica;

7. Gerar a codificação de bacias de Pfafstetter e as informações hidrográficas finais.

Opcional:

8. Dominialidade de Cursos D’água;

9. Sistematização da Hidronímia (nomes dos corpos d'água);

10. Agregação de Bacias.

## 1.2 Como usar esse tutorial 

### 1.2.1. Instruções

As instruções são indicadas pelo tipo de notação abaixo:

Exemplo:

>Clique no botão _Next_ para continuar.

### 1.2.2. Código

Instruções em SQL ou PLPGSQL são apresentadas em uma caixa como no formato abaixo:

    SELECT postgis_full_version();

Este exemplo pode ser executado a partir de uma janela de consulta ou por meio de linhas de comando.

### 1.2.3. Notações

Notações são utilizadas para fornecer informações que são úteis, mas não são críticas para o entendimento do tópico.

Exemplo:  

***
**Nota:**
 aqui está o exemplo de uma notação que é útil, porém não é essencial ao entendimento do processo.
***

### 1.2.4. Funções

As funções são definidas a partir de fonte de texto em negrito.

Exemplo:

>**ST_Touches(geometry A, geometry B)** returns TRUE if either of the geometries’ boundaries intersect

### 1.2.5. Arquivos, Esquemas, Tabelas e Colunas

Nomes de Arquivos, Esquemas, Tabelas e Colunas são representados por uma caixa envoltória ao nome como no exemplo abaixo:

>Selecione o atributo `nome` na tabela `municipio`.

### 1.2.6. Menus, Submenus, Botões e Outros Elementos

Menus/submenus, botões e outros elementos de interface gráfica como campos ou check box são apresentados na formatação de texto itálico.

Exemplo:

>Clique em _Arquivo > Novo_.

>Marque a opção com a palavra _Confirma_
