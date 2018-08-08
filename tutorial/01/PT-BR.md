# 1. Comece Aqui 

## 1.1 Sobre Este Tutorial

Esse Tutorial apresenta uma solução completa de construção de uma **Base Hidrográfica Ottocodificada** por meio de Sistema de Banco de Dados Espaciais (PostgreSQL/PostGIS) e está dividido em x partes:

Parte I:

1. **Item 1**

Parte II:

1. **Item 2**

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

## 1.3 Requisitos Tecnológicos

Postgresql 9.5+

PostGIS 2.3+

QGIS 2.14+
