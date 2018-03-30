# Criação de ícones personalizados para ionic com python


### Introdução

Esse tutorial ajuda a criação de icones como se fossem nativos do próprio Ionic, assim facilitando muito o desenvolvimento mobile.

### Instruções de Build 

### Preparando Ambiente
Necessário instalação de algumas coisas antes de começar a programação. Todo tutorial foi baseado em instalação para Ubuntu 16.04
	
	ruby, python, fontforge, ttfautohint, sass, svgo, woff-tools, brotli, woff2

### Ruby

     sudo apt-get install ruby-full

### fontforge

    sudo apt-get install software-properties-common (caso o add-apt não esteja instalado)
    sudo add-apt-repository ppa:fontforge/fontforge
    sudo apt-get update
    sudo apt-get install fontforge

### ttfautohint

	sudo apt-get update
	sudo apt-get install ttfautohint

### sass

	npm install -g sass

### svgo

	npm install -g svgo


### woff-tools

	sudo apt-get update
	sudo apt-get install woff-tools

### brotli

	sudo apt-get install git (caso não tiver instalado)
	git clone https://github.com/google/brotli.git
	cd brotli/
	mkdir out && cd out
	../configure-cmake
	make
	make test
	make install

	CMake
	cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=./installed ..
	cmake --build . --config Release --target install

	Pip
	sudo apt-get install python-pip (caso não tiver instalado)

### woff2

	git clone https://github.com/google/woff2.git
	cd woff2
	mkdir out && cd out
	cmake ..
	make
	make install

### Iniciando um projeto Ionic

Algumas dependências básicas são necessárias para o funcionamento do Ionic, montar o ambiente antes de seguir os passos abaixo.

Com os requesitos para o funcionamento do Ionic instalados, siga os passos abaixo:

    ionic start <nome do projeto> tabs
    wget https://github.com/ionic-team/ionicons/archive/3.0.zip
    unzip 3.0.zip 
    rm 3.0.zip 
    npm uninstall --save ionicons

### Incluíndo os ícones

Incluir os ícones na pasta `./ionicons-3.0/src`. Lembrando que deverá ser incluso os ícones na versão android (material design), com prefixo `md-` e iOS, com prefixo `ios-`. A versão iOS também pode ser incluído os ícones na versão outline, com o sufixo `-outline`. Há também os ícones do tipo logo, que possuem o prefixo `logo-`.

### Alterando as configurações

Para usar os novos ícones gerados é preciso sobreescrever duas configurações do `@ionic/app-scripts`. Um deles será no `ionic_sass` e o outro no `ionic_copy`. Será preciso criar uma pasta na raiz do projeto `config/` com dois arquivos, `./config/ionic_sass.js ./config/ionic_copy.js.`

O arquivo de configuração `ionic_sass` precisa indicar para o sass onde importar o ionicons. Já o arquivo `ionic_copy` precisa indicar para o webpack copiar os arquivos de fontes para a pasta de destino quando o projeto for compilado.

Segue abaixo a configuração dos arquivos.

#### Ionic Copy

```var copyDefaultConfig = require('@ionic/app-scripts/config/copy.config.js');

copyDefaultConfig.copyFonts.src = ['{{ROOT}}/node_modules/ionic-angular/fonts/**/*', '{{ROOT}}/ionicons-3.0/dist/fonts/**/*'];

module.exports = function () {
  return copyDefaultConfig;
};```

#### Ionic Sass

	var copyDefaultConfig = require('@ionic/app-scripts/config/sass.config.js');
	copyDefaultConfig.includePaths.push('ionicons-3.0/dist/scss');
	module.exports = function () {
		return copyDefaultConfig;
	};

### Setando as configurações para os arquivos criados

Criado os arquivos é preciso dizer ao @ionic/app-scripts onde ele encontrará os novos arquivos de configuração. Para fazer isso basta adicionar as seguintes instruções no arquivo `package.json` em qualquer linha na raiz do objeto.

    "config": {
    	"ionic_copy": "./config/ionic_copy.js",
    	"ionic_sass": "./config/ionic_sass.js"
    },

### Gerando os ícones
Após incluí-los na pasta indicada, basta executar, na pasta `./ionicons-3.0` o seguinte comando:

	python ./builder/generate.py

### Utilizando os ícones
Feito isso, basta incluir os novos ícones utilizando os padrões que já conhecemos:

	<ion-icon name="NOME_DO_NOVO_ÍCONE"></ion-icon>

