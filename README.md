
# PDPJ Skeleton
Estrutura de projeto a ser seguido como exemplo para todos os projetos FED do PDPJ
###
###

<br>

> Convenções de Codificação

:large_orange_diamond: Configuração
<ol>
  ⬥ Setup Jest

  ⬥ Arquivo package.json
</ol>

:large_orange_diamond: Arquitetura da solução 

:large_orange_diamond: Features

:large_orange_diamond: Shared

:large_orange_diamond: Components

<ol>
⬥ Enums

⬥ Guards
	
⬥ Services
</ol>

:large_orange_diamond: Assets

:large_orange_diamond: Environments

:large_orange_diamond: Arquivos de SCSS globais da aplicação

:large_orange_diamond: Typings

:large_orange_diamond: Test

:large_orange_diamond: Recursos TypeScript

:large_orange_diamond: Observações

<br>



## Convenções de codificação 

### :star: Configuração
#### <ol> Jest Setup </ol>
```ruby
// jest-setup.js
...
function test() {
  console.log("notice the blank line before this function?");
}
...
```

```ruby
// jest.config.js
...
const { projects } = require('./angular.json');
const { test } = projects[Object.keys(projects)[0]].architect;
const { pathsToModuleNameMapper: mapper } = require('ts-jest/utils');
const { globals } = require('jest-preset-angular/jest-preset');
const { compilerOptions } = require('./tsconfig.json');

module.exports = {
  globals: {
    'ts-jest': {
      ...globals['ts-jest'],
      tsConfig: '<rootDir>/tsconfig.spec.json',
    },
  },
  roots: ['<rootDir>/tests'],
  preset: 'jest-preset-angular',
  setupFilesAfterEnv: ['<rootDir>/setup-jest.ts'],
  moduleNameMapper: mapper(compilerOptions.paths, { prefix: '<rootDir>' }),
  collectCoverageFrom: test.options.collectCoverageFrom,
  testPathIgnorePatterns: ['/node_modules', '/dist'],
  testMatch: ['**/*.(spec|test).ts'],
  coverageDirectory: 'coverage',
  collectCoverage: true,
  clearMocks: true,
  bail: true,
};
...
```
<br>

#### <ol> Arquivo package.json </ol>
```ruby
"dependencies": {
    "@angular/animations": "~8.2.14",
    "@angular/cdk": "^8.2.3",
    "@angular/common": "~8.2.14",
    "@angular/compiler": "~8.2.14",
    "@angular/core": "~8.2.14",
    "@angular/forms": "~8.2.14",
    "@angular/platform-browser": "~8.2.14",
    "@angular/platform-browser-dynamic": "~8.2.14",
    "@angular/router": "~8.2.14",
    "ccpj-lib-componentes": "^1.56.0",
    "ccpj-lib-estilos": "^6.43.0",
    "pdpj-lib-components": "0.0.56",
    "rxjs": "^6.6.7",
    "tslib": "^2.3.1",
    "zone.js": "~0.9.1"
  },
```

```ruby
"devDependencies": {
    "@angular-builders/jest": "^9.0.0",
    "@angular-devkit/build-angular": "~0.803.29",
    "@angular/cli": "~8.3.29",
    "@angular/compiler-cli": "~8.2.14",
    "@angular/language-service": "~8.2.14",
    "@types/jest": "^25.2.3",
    "@types/node": "~8.9.4",
    "codelyzer": "^5.0.0",
    "jest": "^25.5.4",
    "jest-preset-angular": "^8.1.0",
    "protractor": "~5.4.4",
    "ts-jest": "^25.5.1",
    "ts-node": "~7.0.0",
    "tslint": "~5.15.0",
    "typescript": "~3.5.3"
  }
```
### :star: Arquitetura da solução

O projeto de frontend segue por convenção uma estrutura de pastas definida, durante o desenvolvimento, a criação de um componente ou utilização de um arquivo deve tentar seguir a estrutura. A pasta padrão para implementações é o src.

![image1](\Pictures\Picture1.png)

### :star: Features

O papel dessa camada na aplicação é de estruturar e agrupar as funcionalidades por rotas e módulos, de forma que as dependências singulares a estes fiquem encapsuladas, facilitando assim a manutenção e entendimento do funcionamento da aplicação. Estes agrupadores podem abranger uma única rota ou um conjunto de rotas, desde que façam sentido no âmbito da regra de negócio, exemplo: rota “/user” e rota “/user/:id” podem pertencer a um mesmo módulo.

### :star: Shared 
<ol>
	
> Components
	
Componentes que serão compartilhados pela aplicação, que possuem lógica. 
	
</ol>

<ol>
	
> Enums
	
Enumerators podem ser utilizados desde que facilitem a compreensão do código de forma sucinta.
	
</ol>

<ol>

> Guards
	
Recurso do framework utilizado para realizar a verificação de acesso do Client a determinada rota ou até mesmo um redirecionamento.
	
</ol>

<ol>

> Services
	
Comunicação externa deve ser separado por entidade (por ex. user.service.ts, auth.service.ts, customer.service.ts). Cada arquivo de service deve representar um domínio utilizado pelo front-end. As services também podem ser utilizadas para compartilhar o estado entre componentes ou módulos. 
Com intuito de não desenvolver códigos duplicados é altamente recomendado que seja feito o uso de super classes para abstrair comportamentos ou lógica que pertença a mais de um serviço. Ex.: Tratamento de erros; CRUD. 
	
</ol>

### :star: Assets

É um diretório utilizado para armazenamento de dependências estáticas, tais como, imagens, fontes ou estilos. 

### :star: Environments

Configuração do ambiente da aplicação, pode ser importada por meio do <em>alias</em> ***@environment*** 

### :star: Arquivos de SCSS globais da aplicação

O SCSS é uma sintaxe do SASS que é um processador de CSS que proporciona ao desenvolvedor inserir lógica ou até programar a camada de estilização da aplicação por meio do framework. Aumenta consideravelmente a escalabilidade, uma vez que tem o potencial de reduzir a zero a redundância ao desenvolver a camada de estilos (CSS).

### :star: Typings

O uso de arquivos de definição de tipos de forma global é uma prática de mercado cujo o intuito é de evitar importações desnecessárias dentro do projeto e facilitar a manutenção, visto que os arquivos relativos a tipos são removidos em tempo de build do <em>TypeScript</em>. Entretanto enums e classes ou objetos auxiliares que agreguem  alguma regra de negócio ao projeto devem ser declarados de forma tradicional e importados. Os arquivos dentro dessa pasta possuem a extensão ***.d.ts*** pois se tratam de arquivos de definição de tipo. E como eles sao importados de forma global não precisam ser exportados.

### :star: Test 

***Cenário de testes de Components:***
<ol>
	
•	Importante adicionar o componente a ser testado nas declarations;
	
</ol>
<ol>
	
•	Adicionar o ***sharedmodule***, e os demais módulos no imports.

</ol>
	
***Cenário de testes de Services:*** ADICIONAR CODEBLOCK COM EXEMPLO
<ol>
	
•	Adicionar ***SharedModule***, ***RouterTestingModule***, ***HttpClientTestingModule*** e os demais módulos no import; 

</ol>
<ol>

•	Atribuir router (***Router***), service (service a ser testado) e HTTP (***HttpTestingController***) à variáveis dentro do <em>beforeEach</em>.

</ol>

### :star: Recursos TypeScript

O uso dos ***paths*** no <em>TypeScript</em> é algo que está consolidado na comunidade e facilita que os diretórios da aplicação sejam alterados em decorrência de alguma mudança estrutural. Essa funcionalidade pode ser utilizada por meio da propriedade ***compilerOptions.paths*** dentro do arquivo ***tsconfig.json***.

### :star: Observações

A pasta <strong><em>utils</em></strong> deve gradualmente ser substituída por escopo em ***app/shared***. 
Ex.: ***app/shared/pipes***, ***app/shared/validators***, ***app/shared/operators***.
