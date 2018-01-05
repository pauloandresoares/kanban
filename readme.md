# Sistema Kanban da FireLabs.
## Projeto utilizado para teste das tecnologias Redux, React JS e Gulp

## LADDING PAGE

**1 - Iniciar projeto**

Criar diretório /kanban/landing, e executar o comando:
```sh
npm init
```

Criar os diretórios dentro de landing /src/images, /src/javascripts, /src/styles e criar o arquivo /src/index.html

**2 - Instalar o gulp**

Instar o gulpexecutando os seguintes comando no diretório /src

```sh
npm install gulp-cli -g
npm install gulp@3.9.1  --save-dev

```

Criar o arquivo: src/gulpfile.babel.js dentro do diretório landing

**3 - Configurar o ES7**

```sh
npm install babel-core babel-preset-es2015 --save-dev
```

Criar um arquivo /src/.babelrc no diretório landing com o seguinte conteúdo:

```javascript
{
    "presets": ["es2015"]
}
```
**3 - Escrevendo o Gulpfile com o ES6**

Instalar os pacotes do Gulp.js
```sh
npm install gulp-connect gulp-nodemon gulp-notify gulp-livereload gulp-changed del gulp-util gulp-concat gulp-plumber gulp-imagemin gulp-minify-css gulp-minify-html gulp-rev gulp-rev-collector gulp-uglify gulp-sass gulp-babel --save-dev
```

Editar o arquivo 'gulpfile.babel.js'

```javascript
import gulp from 'gulp'
import connect from 'gulp-connect'
import notify from 'gulp-notify'
import livereload from 'gulp-livereload'
import changed from 'gulp-changed'
import del from 'del'
import util from 'gulp-util'
import concat from 'gulp-concat'
import plumber from 'gulp-plumber'
import imagemin from 'gulp-imagemin'
import minifyCss from 'gulp-minify-css'
import minifyHtml from 'gulp-minify-html'
import rev from 'gulp-rev'
import revCollector from 'gulp-rev-collector'
import uglify from 'gulp-uglify'
import sass from 'gulp-sass'
import babel from 'gulp-babel'

const PATH = {
	htmlSrc: 'src/',
	sassSrc: 'src/styles/',
	imgSrc: 'src/images/',
	jsSrc: 'src/javascripts/',

	buildDir: 'build/',
	revDir: 'rev/',
	distDir: 'dist/'
}

const onError = (error) => {
	util.beep()
	util.log(util.colors.red(error))
}

const igniteServer = () => {
	return connect.server({
		root: 'build',
		livereload: true
	})
}

gulp.task('b-html', () => {
	return gulp
			.src(PATH.htmlSrc.concat('**/*.html'))
			.pipe(gulp.dest(PATH.buildDir.concat('/')))
			.pipe(livereload())
})

gulp.task('b-css', () => {
	return gulp
			.src(PATH.sassSrc.concat('**/*.scss'))
			.pipe(sass({
				includePaths: require('node-neat').includePaths,
				style: 'nested',
				onError: () => {
					console.log('Opps!! Something happens when compile SASS')
				}
			}))
			.pipe(plumber({ errorHandler: onError }))
			.pipe(gulp.dest(PATH.buildDir.concat('/css')))
			.pipe(livereload())
})

gulp.task('b-js', () => {
	return gulp
			.src(PATH.jsSrc.concat('*.js'))
			.pipe(babel({
				presets: ['es2015']
			}))
			.pipe(plumber({ errorHandler : onError }))
			.pipe(changed(PATH.buildDir.concat('/js')))
			.pipe(gulp.dest(PATH.buildDir.concat('/js')))
			.pipe(livereload())
})

gulp.task('b-img', () => {
	return gulp
			.src(PATH.imgSrc.concat('**/*.+(png|jpg|jpeg|gif|svg)'))
			.pipe(changed(PATH.buildDir.concat('/images')))
			.pipe(gulp.dest(PATH.buildDir.concat('/images')))
			.pipe(livereload())
})

gulp.task('watch', () => {
	gulp.watch('src/*.html', ['b-html'])
	gulp.watch('src/styles/**', ['b-css'])
	gulp.watch(PATH.jsSrc.concat('**/*.js'), ['b-js'])
	gulp.watch(PATH.imgSrc.concat('**/*.+(png|jpg|jpeg|gif|svg)'), ['b-img'])
})

gulp.task('build', ['b-html', 'b-css', 'b-js', 'b-img'], () => {
	return igniteServer()
})

const ENV = process.env.SERVER_ENV || 'development'

if (ENV === 'development') {
	gulp.task('default', ['build', 'watch'])
}
```

Instalar o node neat

```sh
npm install node-neat --save-dev
```


**3 - Estilizando a página inicial**


Criar o arquivo src/javascript/app.js com o seguinte conteudo:
```javascript
'use strict';

$(document).ready(function () {
		$(document).on('click', 'a[href^="#"]', function (e) {
				var id = $(this).attr('href');

				var target = $(id);

				if (target.length === 0) {
						return;
				}

				e.preventDefault();

				$('body, html').animate({ scrollTop: target.offset().top });
		});
});
```

Criar o arquivo src/styles/app.scss com o seguinte conteudo:
```css
body {
  font-family: 'Open Sans', sans-serif; }

h1, h2, h3, h4, h5, h6 {
  font-family: 'Open Sans', sans-serif; }

.navbar.navbar-default {
  background: linear-gradient(to right, #02d4a7, #01b4a0);
  border-color: transparent; }

.navbar .navbar-brand img {
  height: 2em; }

.navbar .navbar-nav li a {
  text-transform: uppercase;
  opacity: 0.7;
  font-weight: 600;
  color: #fff;
  font-size: 1.1em; }
  .navbar .navbar-nav li a:hover {
    color: #fff;
    opacity: 1;
    transition: all 0.3s; }

.header {
  position: relative;
  width: 100%;
  min-height: auto;
  background: linear-gradient(to right, #02d4a7, #01b4a0);
  color: white;
  padding: 11em 0;
  height: 600px; }
  .header h1 {
    font-size: 4.3em; }
  .header h2 {
    font-weight: 300; }
  .header .btn-header-custom {
    color: #0bb297;
    background: #fff;
    border: 1px solid #fff;
    margin: 1.5em 0;
    transition: all 0.3s; }
    .header .btn-header-custom:hover, .header .btn-header-custom:focus {
      background: transparent;
      color: #fff;
      border: 1px solid #fff; }

.header-svg {
  position: relative;
  border-bottom: 1px solid #0bb297; }
  .header-svg .sep {
    top: -99px;
    bottom: auto;
    position: absolute;
    left: 0;
    z-index: 1000;
    width: 100%;
    height: 7.2em; }
    .header-svg .sep path {
      fill: #fff; }
  .header-svg svg:not(:root) {
    overflow: hidden; }

#about {
  padding: 7em 0; }
  #about h1 {
    color: #212121;
    font-weight: 600;
    font-size: 5em; }
  #about h3 {
    color: #727272;
    font-weight: 300;
    font-size: 2.2em;
    text-transform: uppercase; }
  #about p {
    font-weight: 300;
    font-size: 1.4em;
    color: #727272;
    padding: 1.4em 0;
    text-align: justify; }
  #about a {
    background: #04c89f;
    border-color: #04c89f;
    text-transform: uppercase;
    font-weight: 600;
    border-radius: 12%;
    color: #fff; }
  #about img {
    padding: 9.5em 0; }

#features {
  padding: 7.5em 0; }
  #features h2 {
    color: #212121;
    font-weight: 600;
    font-size: 5em; }
  #features ul {
    list-style: none;
    padding: 0; }
    #features ul li {
      font-size: 1.5em;
      color: #727272;
      font-weight: 300;
      margin-bottom: 1em; }
      #features ul li i {
        color: #25E2BB; }
  #features img {
    padding: 7em 0; }

#features-info .info-1 {
  background: linear-gradient(to left, #02d4a7, #01b4a0);
  text-align: center;
  padding: 5em 0; }
  #features-info .info-1 h3 {
    color: #ffffff;
    font-weight: 600;
    font-size: 3.5em; }
  #features-info .info-1 p {
    color: #ffffff;
    font-weight: 300;
    font-size: 1.3em; }

#features-info .info-2 {
  background: #ededed;
  text-align: center;
  padding: 5em 0; }
  #features-info .info-2 h3 {
    color: #212121;
    font-weight: 600;
    font-size: 3.5em; }
  #features-info .info-2 p {
    color: #212121;
    font-weight: 300;
    font-size: 1.3em; }

#contact {
  padding: 7.5em 0; }
  #contact h2 {
    font-size: 5em;
    font-weight: 600;
    color: #212121; }
  #contact h3 {
    font-weight: 300;
    font-size: 2.5em;
    color: #727272;
    padding: 1.2em 0; }
  #contact .form-control {
    border-radius: 0;
    border: 1px solid #727272;
    padding: 1.5em; }
  #contact button {
    background: #04c89f;
    border-color: #04c89f; }

.separator {
  background: #04c89f;
  height: 8em;
  width: 100%; }

.footer {
  background: #ededed; }
  .footer img {
    padding: 5em 0;
    height: 15.5em; }
  .footer .footer-nav {
    list-style: none;
    margin: 0 auto 3em auto; }
    .footer .footer-nav li {
      display: inline-block;
      margin-right: 1.2em;
      color: #737373;
      font-weight: 600;
      font-size: 1.1em; }
      .footer .footer-nav li a {
        color: #737373;
        font-weight: 600;
        font-size: 1.1em; }
  .footer .social a {
    margin-right: 0.6em;
    font-size: 2.7em;
    color: #04c89f; }
    .footer .social a:first-child {
      margin-left: 1em; }
    .footer .social a:hover {
      text-decoration: none; }

.center-block {
  display: block;
  margin-right: auto;
  margin-left: auto; }

```

Modificar o conteudo do arquivo src/index.html para o seguinte conteúdo:
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<meta http-equiv="X-UA-Compatible" content="ie=edge">
	<title>Document</title>
	<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/3.3.7/css/bootstrap.min.css">
	<link href="https://fonts.googleapis.com/css?family=Open+Sans:300,400,600,700" rel="stylesheet">
	<link href="http://code.ionicframework.com/ionicons/2.0.1/css/ionicons.min.css" rel="stylesheet">
	<link rel="stylesheet" href="/css/app.css">
</head>
<body>
	<nav class="navbar navbar-default navbar-fixed-top affix-top"> 
		<div class="container-fluid">
			<div class="navbar-header">
				<button class="navbar-toggle collapsed" data-toggle="collapse" data-target="nav">
					<span class="sr-only">Toggle</span>
					Menu
				</button>
				<a href="" class="navbar-brand">
					<img src="/images/tilst.png" class="img-responsive" alt="Tilst">
				</a>
			</div>
			<div class="collapse navbar-collapse" id="nav">
				<ul class="nav navbar-nav navbar-right">
					<li>
						<a href="/">Home</a>
					</li>
					<li>
						<a href="#about">About</a>
					</li>
					<li>
						<a href="#features">Features</a>
					</li>
					<li>
						<a href="#contact">Contact</a>
					</li>
				</ul>
			</div>
		</div>
	</nav>

	<header class="header">
		<div class="container">
			<div class="row text-center">
				<div class="col-xs-12 col-md-12">
					<h1>
						Kanban board Trello like
					</h1>
					<h2>
						By School of net
					</h2>
					<a href="" class="btn btn-primary btn-lg btn-header-custom">
						<i class="ion-paper-airplane"></i>
						 Sign up Now
					</a>
				</div>
			</div>
		</div>		
	</header>

	<section class="header-svg">
		<svg xmlns="http://www.w3.org/2000/svg" version="1.1" class="sep" width="100%" height="100" viewBox="0 0 100 101" preserveAspectRatio="none">
    		<path d="M0 100 L100 0 L100 100 Z"></path>
  		</svg>		
	</section>

	<section id="about">
		<div class="container">
			<div class="row">
				<div class="col-xs-12 col-md-6">
					<h1>Tilst About</h1>
					<h3>Minimist Kanban board</h3>
					<p>
						Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.
					</p>
					<a href="" class="btn btn-primary btn-lg">
						<i class="ion-paper-airplane"></i>
						See more
					</a>
				</div>
				<div class="col-xs-12 col-md-6">
					<img src="/images/mac.png" class="img-responsive" alt="">
				</div>
			</div>
		</div>
	</section>

	<div class="separator"></div>

	<section id="features">
		<div class="container">
			<div class="row">
				<div class="col-xs-12 col-md-6">
					<img src="/images/mac.png" class="img-responsive" alt="">
				</div>
				<div class="col-xs-12 col-md-6">
					<h2>Features:</h2>
					<ul>
						<li>
							<i class="ion-checkmark-circled"></i>
							Lorem Ipsum is simply dummy text of the printing
						</li>
						<li>
							<i class="ion-checkmark-circled"></i>
							Lorem Ipsum is simply dummy text of the printing
						</li>
						<li>
							<i class="ion-checkmark-circled"></i>
							Lorem Ipsum is simply dummy text of the printing
						</li>
						<li>
							<i class="ion-checkmark-circled"></i>
							Lorem Ipsum is simply dummy text of the printing
						</li>
					</ul>					
				</div>
			</div>
		</div>
	</section>
	<section id="features-info">
		<div class="container-fluid">
			<div class="row">
				<div class="col-xs-12 col-md-6 info-1">
					<h3>Lorem Ipsum</h3>
					<p>
						Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book.
					</p>
				</div>
				<div class="col-xs-12 col-md-6 info-2">
					<h3>Lorem Ipsum</h3>
					<p>
						Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book.
					</p>
				</div>
			</div>
		</div>
	</section>

	<section id="contact">
		<div class="container">
			<div class="row text-center">
				<h2>Contact</h2>
				<h3>Message us for more info</h3>
				<form>
					<div class="form-group">
						<input type="text" name="name" class="form-control" placeholder="Insert your name">						
					</div>
					<div class="form-group">
						<input type="email" name="email" class="form-control" placeholder="Insert your email">
					</div>
					<div class="form-group">
						<textarea rows="" cols="" class="form-control" placeholder="Insert your message"></textarea>						
					</div>
					<div class="form-group">
						<button type="submit" class="btn btn-primary btn-lg">Send</button>
					</div>
				</form>
			</div>
		</div>
	</section>

	<footer class="footer">
		<div class="container">
			<div class="row">
				<div class="col-xs-12 text-center">
					<img src="/images/tilst_green.png" class="img-responsive center-block" alt="Tislt green">
					<ul class="footer-nav">
						<li>Tilst &copy; 2017</li>
						<li>
							<a href="">Home</a>
						</li>
						<li>
							<a href="">About</a>
						</li>
						<li>
							<a href="">Features</a>
						</li>
						<li>
							<a href="">Contact</a>
						</li>
					</ul>
					<div class="social">
						<a href="">
							<i class="ion-social-facebook"></i>
						</a>
						<a href="">
							<i class="ion-social-instagram"></i>
						</a>
						<a href="">
							<i class="ion-social-whatsapp"></i>
						</a>
					</div>
				</div>
			</div>
		</div>
	</footer>

	<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
	<script src="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/3.3.7/js/bootstrap.min.js"></script>
	<script src="/js/app.js"></script>
</body>
</html>
```

## APP React

**1 - Instalar o React**

```sh
npm install create-react-app -g
```

Entrar no diretorio raiz do projeto, kanban/ e executar o seguinte comando:
```sh
create-react-app app
```

Testar a aplicação React:
```sh
cd app
npm start
```

Ejetar os arquivos de configuração da app React
```sh
npm run eject
```


**2 - Instalar o React**

Instalando o node-sass
```sh
npm install node-sass --save-dev
```


Instalando o sass loader
```sh
npm install sass-loader --save-dev
```

**3 - Configurar o Webpack** 

Abrir o arquivo app/config/webpack.config.dev.js, e adicionar a seguinte linha:

```javascript
  {
	test: /\.scss$/,
	include: paths.appSrc,
	loaders:[
	  require.resolve('style-loader'),
	  require.resolve('css-loader'),
	  require.resolve('sass-loader'),
	]
  },
```

Colocar o códdigo acima a abaixo do seguinte código já existente:
```javascript
	  // Process JS with Babel.
	  {
		test: /\.(js|jsx|mjs)$/,
		include: paths.appSrc,
		loader: require.resolve('babel-loader'),
		options: {
		  
		  // This is a feature of `babel-loader` for webpack (not Babel itself).
		  // It enables caching results in ./node_modules/.cache/babel-loader/
		  // directory for faster rebuilds.
		  cacheDirectory: true,
		},
	  },
	  
	  // .....IMPLEMENTAR AQUI......
	  
```
No mesmo arquivo app/config/webpack.config.dev.js, e adicionar as seguintes linhas na configuração do exclude:

```javascript
		exclude: [
		  /\.html$/,
		  /\.(js|jsx)$/,
		  /\.css$/,
		  /\.json$/,
		  /\.bmp$/,
		  /\.gif$/,
		  /\.jpe?g$/,
		  /\.png$/,
		  /\.sass$/,
		  /\.scss$/
		],
```

Entrar no diretorio 'app/src' e renomear o arquivo 'App.css' para 'App.scss'

No diretório app/src, criar as pastas actions, components, constants, conteiners e reducers

**4 - Instalando dependencias do redux**

Executar o comando no diretório /app
npm install react-redux redux prop-types react-dnd react-dnd-html5-backend --save 

**5 - Configuração da arquitetura**

Entrar no diretório app/src/ e excluir os arquivos: App.scss, App.js e App.test.js

Executar o comando no diretório /app para instalar o react router redux

```sh
npm install react-router-redux@next history --save
```

Editar o arquivo app/src/index.js :
```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux'
import { Route } from 'react-router'
import { ConnectedRouter } from 'react-router-redux'

import Home from './containers/Home'

import cfg from './store'

const store = cfg.configStore()

ReactDOM.render(
    <main>
        <Provider store={ store }>
            <ConnectedRouter history = { cfg.history }>
                <Route exact path="/" component = { Home } />
            </ConnectedRouter>
        </Provider>
    </main>,
    document.getElementById('root')
)
```

Criar o arquivo app/src/store.js com o seguinte conteudo:

```javascript
import { createStore, applyMiddleware } from 'redux'
import { routerMiddleware } from 'react-router-redux'
import createHistory from 'history/createBrowserHistory'
import reducers from './reducers'

const history = createHistory()
const middlewares = routerMiddleware()

const configStore = () => {
    return createStore(
        reducers,
        applyMiddleware(middlewares)
    )
}

export default { configStore, history } 
```

Criar o arquivo app/src/reducers/index.js com o seguinte conteúdo:

```javascript
import { combineReducers } from 'redux'
import { routerReducer } from 'react-router-redux'

export default combineReducers({
    router: routerReducer
})
```

Criar o arquivo app/src/components/Home.js com o seguinte conteúdo:

```javascript
import React, { Component } from 'react'

class Home extends Component {
	constructor(props) {
		super(props)
	}
    render() {
        return (
            <h1>Kanban Board</h1>
        )
    }

}

export default Home
```

