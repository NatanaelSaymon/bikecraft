# Gulp - Utilizando gulp para sass

Gulp.js é uma ferramenta de automação de tarefas em JavaScript. Tarefas como minificar, otimizar e compilar arquivos, tão repetitivas e necessárias ao desenvolvimento, podem ser automatizadas com o Gulp.

**Node.js** — um ambiente que permite a execução JavaScript no lado do servidor, para a web.

**Npm** — um gerenciador de pacotes para Node.js. Uma ferramenta que permite configurar rápida e facilmente ambientes e plugins Node localmente.

**Local vs. Global** — Node.js é instalado globalmente, mas Gulp e todos os seus plugins serão instalados localmente, por projeto.

**Executor de Tarefa** — Um executor de tarefa como Gulp automatiza todos os seus processos para que você não tenha que pensar sobre eles. O Gulp requer um pacote.json e gulpfile.js.

## Passos
- 1 - Instalar Node.js
- 2 - Instalar Gulp Cli globalmente
- 3 - Instalar Node e Gulp localmente
- 4 - Instalar plugins do Gulp
- 5 - Criando diretorios do projeto
- 6 - Configurar o tarefas

<br><br>

## Passo 1 - Instalar Node.js

Acesse <a href="https://nodejs.org/en/" target="_blank">https://nodejs.org/en/</a> e procure a melhor forma de instalar o Node para você.

<br>

Para checar se o Node está instalado, abra a linha de comando e digite:

````js
node -v
````

Se estiver tudo correto, esse comando mostrará a versão do Node instalada.


## Passo 2 - Instalar Gulp CLI globalmente

````js
npm install gulp-cli --global
````

<br>

## Passo 3 - Instalar Node e Gulp Locamente

Crie a pasta do projeto e rode o comando:

````js
npm init
````

Este comando irá guiá-lo através da criação de um package.json genérico. É bastante direto, mas simplesmente pressione enter se você não tiver certeza ou não quiser preencher algo.

````js
{
  "name": "site",
  "version": "1.0.0",
  "description": "Iniciando com Gulp",
  "main": "index.js",
  "scripts": {
     "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [
    "gulp",
    "sass"
  ],
  "author": "Natanael Saymon",
  "license": ""
}
````

Agora vamos executar um comando para instalar o Gup localmente.

````js
npm install --save-dev gulp
````

(--save-dev: tem objetivo de adicionar o gulp como uma dependência de desenvolvimento (dev dependency) Todas as dependencias do projeto estao no arquivo package.json)

<br>
 

Verifique se esta tudo funcionado corretamente, para isso: gulp --version

deve aparecer as versoes da CLI e Local

Ex:

CLI version 2.0.1 <br>
Local version 4.0.0

<br><br>

## Instalando Plugins do Gulp

- **Gulp Sass** - para compilar SCSS para CSS.
- **node-sass** - necessario para funcionar gulp-sass.
- **Gulp Autoprefixer** - para adicionar automaticamente os prefixos dos browsers nas regras CSS.
- **Gulp cssnano** - para minificar e otimizar CSS.

Isso já fará um fluxo de trabalho interessante — você pode escrever SCSS sem se preocupar com a adição de prefixos ou minificar manualmente o código.

<br><br>

Instale os 4 de uma vez com: 

````js
npm install --save-dev gulp-sass node-sass gulp-autoprefixer gulp-cssnano 
````

Se você verificar o seu package.json, você notará que uma nova seção foi adicionada.

````js
"devDependencies": {
    "gulp": "^3.9.1",
    "node-sass": "^4.14.1",
    "gulp-autoprefixer": "^4.0.0",
    "gulp-cssnano": "^2.1.2",
    "gulp-sass": "^3.1.0"
  }
````

<br><br>


## Passo 5 - Estrutura de Pasta do Projeto

Agora vamos criar 2 diretorios: 

**dist**: arquivos de destino

**src**: arquivos de origem

<img src="https://miro.medium.com/max/194/1*LNQWz9TWeqcbpPH7YWZAzA.jpeg">

<br><br>

## Passo 6 - Configurar tarefas

Dentro da pasta do projeto crie um arquivo de configuracao do Gulp: **gulpfile.js**. Nele iremos definir uma variavel para cada plugin.

````js
'use strict';

/*line 01*/const gulp = require('gulp');
/*line 02*/const rename = require('gulp-rename');
/*line 03*/const sass = require('gulp-sass'); 
/*line 04*/sass.compiler = require("node-sass");
/*line 05*/const cssnano = require('gulp-cssnano');
/*line 06*/const autoprefixer = require('gulp-autoprefixer');
/*line 07*/
/*line 08*/gulp.task('default', watch);
/*line 09*/
/*line 10*/gulp.task('sass', compilaSass);
/*line 11*/
/*line 12*/function compilaSass(){
/*line 13*/    return gulp
/*line 14*/        .src("src/scss/**/*.scss")
/*line 15*/        .pipe(sass({outputStyle:'compressed'}).on('error', sass.logError)) 
/*line 16*/        .pipe(autoprefixer())
/*line 17*/        .pipe(cssnano())
/*line 18*/        .pipe(rename({extname:'.min.css'}))
/*line 19*/        .pipe(gulp.dest("src/css/"));
/*line 20*/}
/*line 21*/
/*line 22*/function watch(){
/*line 23*/    gulp.watch("src/scss/**/*.scss", compilaSass)
/*line 24*/}
````

<br><br>

## Explicando: 

**Line 1** - a instrução diz ao node.js para procurar na pasta node_modules por um pacote chamado gulp. Quando o pacote é encontrado, atribuímos seu conteúdo à constante gulp. Raciocínio igual pode ser aplicado as linha 2 a 6.
<br><br>
**Line 8** - atribuimos a função **watch()** à tarefa **default** que pode ser chamada simplesmente digitando **gulp** na linha do seu terminal.
<br><br>
**Line 10** - atribuimos a **função compilaSass** à tarefa nomeada como **sass**.
<br><br>
**Line 14** - a instrução .src(“scr/scss/**/*.scss") diz à tarefa para procurar por todos os arquivos .scss que estão localizados na pasta src/scss bem como em suas sub-pastas, por isto a inclusão do **.
<br><br>
**Line 15** - executamos o plug-in gulp-sass para efetuar a compilação dos arquivos .scss em .css.
<br><br>
**Line 18** - estamos renomeando o arquivo gerando pelo scss e inserindo a extensao .ccc
<br><br>
**Line 19** - a instrução gulp.dest("src/css") diz à tarefa para onde enviar os arquivos já compilados para .css.
<br><br>
**Line 22** - informamos ao gulp que ele deve observar qualquer alteração que aconteça no diretório e execute a função compilaSass() .
<br><br>
Agora basta executar via terminal o comando **gulp** as mudancas estao sendo verificadas com **gulp.watch**.