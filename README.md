# Config-Babel-Webpack


- Configurando Babel
    - [x]  Primeiro crie seu projeto com (yarn init -y)
    - [x]  Instale a dependência (yarn add @babel/cli -D)
    - [x]  Instale a dependência (yarn add @babel/preset-env -D)
    - [x]  Instale a dependência (yarn add @babel/core -D)
    - [x]  Se for usar o git pra controle de versão crie o arquivo (.gitignore)
    - [x]  Crie um arquivo de configuração para o babel (.babelrc)

    ```jsx
    //dentro do arquivo .babelrc
    {
    "presets": ["@babel/preset-env"]
    }
    ```

    - [x]  Feito isso já podemos criar o index.html e um arquivo main.js
    - [ ]  Agora é hora de criar o script pra gerar nosso bundle.js

    ```jsx
    //dentro do package.json crie um script
    "scripts": {"dev": "babel ./main.js -o ./bundle.js -w}

    ```

    - [x]  Agora é so linkar nosso bundle.js no nosso html
    - Obs: para se usar os operadores res/spread no babel temos que instalar um pluguin
        - [ ]  (yarn add @babel/plugin-proposal-object-rest-spread -D)

        ```jsx
        //dentro do babelrc criamos um novo array de plugins
        {
        "presets": ["@babel/preset-env"],
        "plugins": ["@babel/plugin-proposal-object-rest-spread"]
        }
        ```

    - Para usar functions async/await no babel precisamos instalar um outro plugin
        - [x]  Instale o plugin (yarn add @babel/plugin-transform-async-to-generator -D)

        ```jsx
        //Adicione o novo plugin no babelrc
        {
        "presets": ["@babel/preset-env"],
        "plugins": [
        "@babel/plugin-proposal-object-rest-spread",
        "@babel/plugin-transform-async-to-generator"
        ]
        }
        ```

        - [x]  Instale a dependencia (yarn add @babel/polyfill -D)

        ```jsx
        //Adicione nas configurações do webpack o @babel/polyfill
        module.exports = {
          entry: ['@babel/polyfill', './src/main.js'], //transforma em um array e add
          output: {
            path: __dirname + "/public",
        		filename: 'bundle.js'
          },
        	devServer: {
        	    contentBase: __dirname + 'public'
        	  },
        	module: {
             rules: [
               {
                 test: /\.js$/,
                 exclude: /node_modules/,
        				 use: {
        					loader: "babel-loader"
        				}
               }
             ],
           },
        };
        ```

- Configurando WebPack
    - [x]  Instale a dependência (yarn add webpack webpack-cli -D)
    - [x]  Instale a dependência (yarn add babel-loader -D)
    - [x]  Instale a dependência (yarn add webpack-dev-server -D) ⇒ cria o servidor
    - [x]  Crie um arquivo de configuração para o webPack (webpack.config.js)

    ```jsx
    module.exports = {
      entry: './src/main.js',
      output: {
        path: __dirname + "/public",
    		filename: 'bundle.js'
      },
    	devServer: {
    	    contentBase: __dirname + 'public'
    	  },
    	module: {
         rules: [
           {
             test: /\.js$/,
             exclude: /node_modules/,
    				 use: {
    					loader: "babel-loader"
    				}
           }
         ],
       },
    };
    ```

    ```jsx
    //dentro do package.json substitua o script
    "scripts": {"dev": "babel ./main.js -o ./bundle.js -w"}

    //Por
    "scripts": {"dev": "webpack-dev-server --mode=development"}
    ```

    - OBS: para gerar o arquivo que está oculto do bundle.js precisa rodar o comando no package.json

        ```jsx
        //Por
        "scripts": {
        "dev": "webpack-dev-server --mode=development",
        "build": "webpack --mode=production"
        }
        ```