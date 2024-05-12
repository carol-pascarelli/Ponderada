# Banco de dados

&nbsp;&nbsp;&nbsp;&nbsp; Este repositório contém o banco de dados individual desenvolvido para a aplicação web para a faculdade de zuyd em relação ao Cesim game.


<div align="center">
<sub>Figura 1 - Banco de dados completo</sub>
<img src="/assets/DB.png" width="100%" >
<sup>Fonte: Material produzido pelos autores (2024)</sup>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;Aqui está o modelo relacional completo do banco de daods, com tabelas de login tanto do tutor quanto do jogador, questionários, a fazeres, entre outros que serão melhor explicados a seguir.


**Tutor**
<div align="center">
<sub>Figura 2 - Tutor</sub>
<img src="assets\Tut.png" width="100%" >
<sup>Fonte: Material produzido pelos autores (2024)</sup>
</div>

&nbsp;&nbsp;&nbsp;&nbsp; A tabela do tutor contém atributos nome, nacionalidade e id login que são informações que poderão ser adicionadas após o login simples do usuário apenas com e-mail e senha. Separando estas duas tabelas o processo de cadastramento na plataforma fica um pouco menos extenso. Há também uma tabela Tutor_Time para que um tutor não tenha que criar contas diferentes para times diferentes ou até mesmo utilize a mesma conta para anos diferentes participando do jogo, otimizando ainda mais o uso da plataforma.


**Jogador**
<div align="center">
<sub>Figura 3 - Jogador</sub>
<img src="assets\Pend.png" width="100%" >
<sup>Fonte: Material produzido pelos autores (2024)</sup>
</div>

&nbsp;&nbsp;&nbsp;&nbsp; Para o perfil do jogador alguns atributos a mais do que no perfio do tutor são necessários após o login, já que parte do intuito do Cesim game é ocnhecer pessoas de outras nacionalidades. Assim, conta com atributos como nome, nacionalidade, idade, id_login, universidade, gênero e conta tamb~em com sua porcentagem de felicidade que estará em display em seu perfil. Há também duas tabelas relacionadas a tabela de jogador: Estudante-Time e Jogado_A_Fazeres. Dessa forma o jogador, assim como o tutor, não precisará de contas diferentes se for jogar o cesim game novamente, e a tabela de a fazeres armazena todas as tarefas que deverão ser feitas por ele, assim como sua data de entrega.


**Medidor de felicidade**
<div align="center">
<sub>Figura 4 - Medidor de felicidade</sub>
<img src="assets\Med.png" width="100%" >
<sup>Fonte: Material produzido pelos autores (2024)</sup>
</div>

&nbsp;&nbsp;&nbsp;&nbsp; Outra requisição do nosso parceiro foi o medidor de felicidade, que ficará em display no perfil do jogador para que seus colegas de time consigam acompanhar e ajudar com possíveis conflitos que talvez não estejam sendo vistos pelos outros membros do grupo, contendo os atributos id_questionário, que irá avaliar o nível de felicidade do jogador e o nível de felicidade que estará em display.


**Questionário**
<div align="center">
<sub>Figura 5 - Questionários</sub>
<img src="assets\Quest.png" width="100%" >
<sup>Fonte: Material produzido pelos autores (2024)</sup>
</div>

&nbsp;&nbsp;&nbsp;&nbsp; Por fim, este banco de dados conta com uma tabela de questionários, que armazenará todos os questionários do jogo, relacionado também a uma tabela de respostas que contará com os atributos id_jogador, já que nem todos os jogadores obrigatoriamente receberão o mesmo questionário, a resposta dada pelo jogador e seu id. 

&nbsp;&nbsp;&nbsp;&nbsp; A seguir está o código completo do banco de dados em MyPostgre

```sql

-- Drop das tabelas se existirem
DROP TABLE IF EXISTS "Respostas";
DROP TABLE IF EXISTS "Questionario_felicidade";
DROP TABLE IF EXISTS "Medidor_felicidade";
DROP TABLE IF EXISTS "Questionarios";
DROP TABLE IF EXISTS "Login";
DROP TABLE IF EXISTS "Jogador_A_Fazeres";
DROP TABLE IF EXISTS "Tutor_Time";
DROP TABLE IF EXISTS "Estudante_Time";
DROP TABLE IF EXISTS "A fazeres";
DROP TABLE IF EXISTS "Jogador";
DROP TABLE IF EXISTS "Time";
DROP TABLE IF EXISTS "Tutor";

-- Tabela 'Tutor'
CREATE TABLE "Tutor" (
  "id" SERIAL PRIMARY KEY,
  "Nome" VARCHAR(255), -- Corrigido para VARCHAR
  "Nacionalidade" VARCHAR(255), -- Corrigido para VARCHAR
  "id_Login" INTEGER
);

-- Tabela 'Time'
CREATE TABLE "Time" (
  "id" SERIAL PRIMARY KEY,
  "Nome" VARCHAR(255), -- Corrigido para VARCHAR
  "Universo" VARCHAR(255), -- Corrigido para VARCHAR
  "Medidor_felicidade" INTEGER,
  "Rodada" INTEGER
);

-- Tabela 'Jogador'
CREATE TABLE "Jogador" (
  "id" SERIAL PRIMARY KEY,
  "Nome" VARCHAR(255), -- Corrigido para VARCHAR
  "Nacionalidade" VARCHAR(255), -- Corrigido para VARCHAR
  "Idade" DATE,
  "Perfil" INTEGER,
  "id_Login" INTEGER,
  "Universidade" INTEGER,
  "id_Medidor_felicidade" INTEGER,
  "Gênero" INTEGER
);

-- Tabela 'A fazeres'
CREATE TABLE "A fazeres" (
  "id" SERIAL PRIMARY KEY,
  "Tarefa" VARCHAR(255), -- Corrigido para VARCHAR
  "Data_entrega" DATE -- Corrigido para DATE
);

-- Tabela 'Estudante_Time'
CREATE TABLE "Estudante_Time" (
  "id" SERIAL PRIMARY KEY,
  "id_Time" INTEGER,
  "id_Jogador" INTEGER
);

-- Tabela 'Tutor_Time'
CREATE TABLE "Tutor_Time" (
  "id" SERIAL PRIMARY KEY,
  "id_Tutor" INTEGER,
  "id_Time" INTEGER
);

-- Tabela 'Jogador_A_Fazeres'
CREATE TABLE "Jogador_A_Fazeres" (
  "id" SERIAL PRIMARY KEY,
  "id_A_fazeres" INTEGER, -- Corrigido para id_A_fazeres
  "id_Jogador" INTEGER
);

-- Tabela 'Login'
CREATE TABLE "Login" (
  "id" SERIAL PRIMARY KEY,
  "email" VARCHAR(255), -- Corrigido para VARCHAR
  "Senha" INTEGER
);

-- Tabela 'Questionarios'
CREATE TABLE "Questionarios" (
  "id" SERIAL PRIMARY KEY
);

-- Tabela 'Medidor_felicidade'
CREATE TABLE "Medidor_felicidade" (
  "id" SERIAL PRIMARY KEY,
  "Nivel" INTEGER,
  "id_Questionario_felicidade" INTEGER
);

-- Tabela 'Questionario_felicidade'
CREATE TABLE "Questionario_felicidade" (
  "id" SERIAL PRIMARY KEY
);

-- Tabela 'Respostas'
CREATE TABLE "Respostas" (
  "id" SERIAL PRIMARY KEY,
  "id_Jogador" INTEGER,
  "Resposta" INTEGER,
  "id_Questionarios" INTEGER
);

-- Chaves Estrangeiras
ALTER TABLE "Tutor" ADD FOREIGN KEY ("id_Login") REFERENCES "Login" ("id");
ALTER TABLE "Jogador" ADD FOREIGN KEY ("id_Login") REFERENCES "Login" ("id");
ALTER TABLE "Jogador" ADD FOREIGN KEY ("id_Medidor_felicidade") REFERENCES "Medidor_felicidade" ("id");
ALTER TABLE "Estudante_Time" ADD FOREIGN KEY ("id_Time") REFERENCES "Time" ("id");
ALTER TABLE "Estudante_Time" ADD FOREIGN KEY ("id_Jogador") REFERENCES "Jogador" ("id");
ALTER TABLE "Tutor_Time" ADD FOREIGN KEY ("id_Tutor") REFERENCES "Tutor" ("id");
ALTER TABLE "Tutor_Time" ADD FOREIGN KEY ("id_Time") REFERENCES "Time" ("id");
ALTER TABLE "Jogador_A_Fazeres" ADD FOREIGN KEY ("id_A_fazeres") REFERENCES "A fazeres" ("id");
ALTER TABLE "Jogador_A_Fazeres" ADD FOREIGN KEY ("id_Jogador") REFERENCES "Jogador" ("id");
ALTER TABLE "Medidor_felicidade" ADD FOREIGN KEY ("id_Questionario_felicidade") REFERENCES "Questionario_felicidade" ("id");
ALTER TABLE "Respostas" ADD FOREIGN KEY ("id_Jogador") REFERENCES "Jogador" ("id");
ALTER TABLE "Respostas" ADD FOREIGN KEY ("id_Questionarios") REFERENCES "Questionarios" ("id");
```

&nbsp;&nbsp;&nbsp;&nbsp; A seguir está o código completo do banco de dados em xml. Este código também pode ser enontrado em seu próprio arquivo neste mesmo repositório.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<!-- SQL XML created by WWW SQL Designer, https://github.com/ondras/wwwsqldesigner/ -->
<!-- Active URL: https://sql.toad.cz/ -->
<sql>
<datatypes db="mysql">
	<group label="Numeric" color="rgb(238,238,170)">
		<type label="Integer" length="0" sql="INTEGER" quote=""/>
	 	<type label="TINYINT" length="0" sql="TINYINT" quote=""/>
	 	<type label="SMALLINT" length="0" sql="SMALLINT" quote=""/>
	 	<type label="MEDIUMINT" length="0" sql="MEDIUMINT" quote=""/>
	 	<type label="INT" length="0" sql="INT" quote=""/>
		<type label="BIGINT" length="0" sql="BIGINT" quote=""/>
		<type label="Decimal" length="1" sql="DECIMAL" re="DEC" quote=""/>
		<type label="Single precision" length="0" sql="FLOAT" quote=""/>
		<type label="Double precision" length="0" sql="DOUBLE" re="DOUBLE" quote=""/>
	</group>

	<group label="Character" color="rgb(255,200,200)">
		<type label="Char" length="1" sql="CHAR" quote="'"/>
		<type label="Varchar" length="1" sql="VARCHAR" quote="'"/>
		<type label="Text" length="0" sql="MEDIUMTEXT" re="TEXT" quote="'"/>
		<type label="Binary" length="1" sql="BINARY" quote="'"/>
		<type label="Varbinary" length="1" sql="VARBINARY" quote="'"/>
		<type label="BLOB" length="0" sql="BLOB" re="BLOB" quote="'"/>
	</group>

	<group label="Date &amp; Time" color="rgb(200,255,200)">
		<type label="Date" length="0" sql="DATE" quote="'"/>
		<type label="Time" length="0" sql="TIME" quote="'"/>
		<type label="Datetime" length="0" sql="DATETIME" quote="'"/>
		<type label="Year" length="0" sql="YEAR" quote=""/>
		<type label="Timestamp" length="0" sql="TIMESTAMP" quote="'"/>
	</group>
	
	<group label="Miscellaneous" color="rgb(200,200,255)">
		<type label="ENUM" length="1" sql="ENUM" quote=""/>
		<type label="SET" length="1" sql="SET" quote=""/>
		<type label="Bit" length="0" sql="bit" quote=""/>
	</group>
</datatypes><table x="1087" y="291" name="Tutor">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="Nome" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="Nacionalidade" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="id_Login" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Login" row="id" />
</row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
<table x="1574" y="368" name="Time">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="Nome" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="Universo" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="Medidor_felicidade" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="Rodada" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
<table x="1298" y="862" name="Jogador">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="Nome" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="Nacionalidade" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="Idade" null="1" autoincrement="0">
<datatype>DATE</datatype>
<default>NULL</default></row>
<row name="Perfil" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="id_Login" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Login" row="id" />
</row>
<row name="Universidade" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="id_Medidor_felicidade" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Medidor_felicidade" row="id" />
</row>
<row name="Gênero" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
<table x="1768" y="778" name="A fazeres">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="Tarefa" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="Data_entrega" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
<table x="1637" y="615" name="Estudante_Time">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="id_Time" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Time" row="id" />
</row>
<row name="id_Jogador" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Jogador" row="id" />
</row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
<table x="1329" y="322" name="Tutor_Time">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="id_Tutor" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Tutor" row="id" />
</row>
<row name="id_Time" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Time" row="id" />
</row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
<table x="1558" y="905" name="Jogador_A_Fazeres">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="id_A fazeres" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="A fazeres" row="id" />
</row>
<row name="id_Jogador" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Jogador" row="id" />
</row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
<table x="837" y="464" name="Login">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="e-mail" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="Senha" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
<table x="1169" y="533" name="Questionarios">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
<table x="955" y="929" name="Medidor_felicidade">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="Nivel" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="id_Questionario_felicidade" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Questionario_felicidade" row="id" />
</row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
<table x="646" y="940" name="Questionario_felicidade">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
<table x="1256" y="681" name="Respostas">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="id_Jogador" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Jogador" row="id" />
</row>
<row name="Resposta" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="id_Questionarios" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Questionarios" row="id" />
</row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
</sql>
```