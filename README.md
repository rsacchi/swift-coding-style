# tenantapp-ios

QuintoAndar Inquilino App (iOS) code base

# Quinto Andar Swift Coding Style

##Index

* [GitFlow](#gitflow)
* [Arquitetura](#arquitetura)
* [Correção](#correção)
* [Nomenclatura](#nomenclatura)
	* [Atributos](#atributos)
	* [Métodos](#métodos)
* [Organização do Código](#organização-do-código)
	* [Espaçamento](#espaçamento)
	* [Pragma Marks](#pragma-marks)
	* [Protocolos](#protocolos)
	* [Código não usado](#código-não-usado)
	* [Comentários](#comentários)
* [Tipos](#tipos)
* [Classes e Estruturas](#classes-e-estruturas)
	* [Uso de Self](#uso-de-self)
* [Optionals](#optionals)
* [Caminho Feliz](#caminho-feliz)
* [Agradecimentos](#agradecimentos)

##GitFlow

Usamos GitFlow para organizar nossso fluxo de trabalho. Não usamos a branch de HotFixes, pois os fixes são tratados como novas versões. Sendo assim, colocamos os fixes como features. Contamos com as seguintes branches:

### 1) Features

Pasta contendo todas as features em que vc está trabalhando. Atente que termina com S no final, feature**s**. Cada nova feature deve ser modular e ser rapidamente integrada no develop para que os merges não fiquem muito defasados.

### 2) Fixes

Pasta para bug fixes que serão integrados na develop.

#### Pull Request

Ao terminar sua feature ou fix, siga o fluxo:

* Crie um Pull Request
* Espere até 4 horas alguém dar assign
* Se ninguém der assign após 4 horas, escolha alguém e comunique no Slack
* O developer assigned tem até **24 horas** para fazer o review
* É responsabilidade de quem abriu o Pull Request de cobrar o review
* Faça as correções necessárias no seu código
* Faça rebase da Develop na sua Feature ou Fix
* Faça as correções necessárias no seu código
* Faça o Merge final na Develop

Depois de integrar no Develop, apagar a branch. 
ATENÇÃO: ao fazer merge da branch, escolha a opção "Squash and merge". Resuma tudo que foi feito na branch no comentário do commit de merge.

### 2) Develop

Branch principal de desenvolvimento do projeto. Somente código TESTADO, FUNCIONAL e REVISADO pode entrar nessa branch.

### 3) Releases 

Quando acabar o desenvolvimento de uma determinada versão, devemos criar uma branch nova dentro da pasta de branches Releases (**também com S no final**).
 
O formato do nome da Branch deve ser: v1.2.3, onde 1.2.3 é o número da versão. P.e., a versão 2.3.0 seria o branch **v2.3.0**.

**NÃO** devemos colocar código novo nessa branch. A branch de Release é exclusivamente para **BUGFIXES**. O processo de correção de bugfixes é o seguinte:

* Crie uma branch de bugfix a partir da release
* Após terminar a correção, abra um Pull Request para a branch de Release
* Após revisão do código, faça o merge na branch de Release

### 4) Master

Somente após a aprovação na App Store, é feita a integração da branch de Release na Master (e também na Develop).

Não esquecer de criar a tag no commit da Master. O formato deve ser:
**v2.3.0** (major, minor, bugfix)

### Referência

Atenção: ignore o branch hotfixes

[A successful Git branching model
](<http://nvie.com/posts/a-successful-git-branching-model/>)

![GitFlow](http://nvie.com/img/git-model@2x.png)

##Arquitetura

Usamos o pattern MVVM nos novos projetos. Alguns projetos estão em MVC.

Para maior facilidade, instalar o template de criação de arquivos, para criar automaticamente:

* YourViewControlller.swift
* YourViewModel.swift
* YourFeedback.swift

Utilizar uma classe de Manager para lidar com acesso ao Realm e aos Request de Network. Usar um [Router](https://littlebitesofcocoa.com/93-creating-a-router-for-alamofire) para centralizar as chamadas aos Requests dos endpoints de um domínio específico. Por exemplo, para o Login devemos ter:

* LoginModelManager.swift
* LoginRouter.swift
* LoginRequests.swift

##Correção

Não devem haver *warnings* nem *errors* no código.

##Nomenclatura

Todo o código e comentários devem estar em **INGLÊS**. Permitidas exceções para mapeamento de respostas de endpoints.

Usar nomes relevantes e descritivos em *camelCase* para variáveis, métodos, classes, enums, etc. Nomes de tipos devem ser capitalizados.

**Sim:**

```swift
private let addressNumber = 353

class FifthFloor {
	var meaningfulVariable: String
}
```

**Não:**

```swift
private let number = 353

class FifthFl {
	var mVar: String
}
```

Para maior referência de nomenclatura, seguir os [guidelines de design de API](https://swift.org/documentation/api-design-guidelines/) em Swift.

### Atributos

Atributos devem terminar com a indicação de seu tipo. Por exemplo

**Sim:**

```swift
private var addressNumberLabel: UILabel
```

**Não:**

```swift
private var addressNumber: UILabel
```

Exceção para Strings

Listas e coleções devem estar no plural. Não usar List no nome da variável.

**Sim:**

```swift
private var estates: [Estate]
```

**Não:**

```swift
private var estate: [Estate]
private var estatesList: [Estate]
private var estateList: [Estate]
```

### Métodos

As declarações de métodos devem ser concisas e caso possuam uma longa assinatura, utilizar quebra de linha

**Sim:**

```swift
func playGame(on platform: String) -> Bool {
  	// can we play tonight?
}
```

**Não:**

```swift
func playGame(platform: String, title: String, 
	person: Person, duration: Int) -> Bool {
  	// can we play forever?
}
```

## Organização do Código

### Espaçamento

Usar **4 espaços** para identação e alinhamento.

Não deixar espaços em branco no fim de uma linha.

* No Xcode *Automatically trim whitespace* e *Including whitespace-only lines* marcados em Text Editing arruma isso automaticamente.

**Sim:**

```swift
func add(_ arg1: Int, _ arg2: Int) -> Int {
	a = b + c * 2
	// stuff
}

func divide(_ arg1: Int, _ arg2: Int) -> Int {
    // more stuff
}
```

**Não:**

```swift
func add ( _ arg1: Int, _ arg2:Int) -> Int {
	a=b + c*2
	// wrong stuff
}
func divide( _ arg1 : Int, _ arg2 : Int ) ->Int{
    // more wrong stuff
}
```

Usar linhas em branco para separar blocos lógicos de código. Nunca deixar mais do que uma linha em branco seguida.

O ideal na verdade é que se tem um bloco lógico muito grande, você deveria extraí-lo em um método e chamar esse método no lugar.

**Sim:**

```swift
func buildWatch(hour: Int, minute: Int)
	var material: Material = createMaterial()
	applyMaterial(material)
	
	moveHour(hour)
	moveMinute(minute)

	assembly()
}
```

**Não:**

```swift
func buildWatch(hour: Int, minute: Int)
	var material: Material = createMaterial()
	applyMaterial(material)	
	moveHour(hour)
	moveMinute(minute)
	assembly()
}
```

### Pragma Marks

Usar pragma marks para dividir o contexto do código. A orientação que existam os seguintes MARKs, nessa ordem, para subclasses de `UIViewController`:

1. `//MARK: - Attributes`
2. `//MARK: - IBOutlets`
3. `//MARK: - View Lifecycle`
4. `//MARK: - IBActions`
5. `//MARK: - Instance Methods`

### Protocolos

A implementação dos métodos de um protocolo deve estar dentro de um extension da própria classe. Colocar um pragma mark indicando qual o protocolo que está sendo implementado.

**Sim:**

```swift
class MyViewcontroller: UIViewController {
	// class stuff here
}

// MARK: - UITableViewDataSource
extension MyViewcontroller: UITableViewDataSource {
	// table view data source methods
}

// MARK: - UIScrollViewDelegate
extension MyViewcontroller: UIScrollViewDelegate {
	// scroll view delegate methods
}
```

**Não:**

```swift
class MyViewcontroller: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
  	// all methods
}
```

### Código não usado

Remover qualquer código que esteja comentado ou não esteja sendo usado (dead code). Código gerado automaticamente que não é usado também deve ser removido.

### Comentários

Não usar comentários. A nomenclatura utilizada deve ser suficiente para explicar o código. Caso contrário, refactor.

Exceção é permitida nos casos raros onde é necessário uma explicação fora do contexto.


##Tipos

Utilizar a inferência de tipos do Swift.

**Sim:**

```swift
let message = "Hello Swift!"
let currentBounds = computeBounds()
var names: [String] = []
var lookup: [String : Int] = [:]
```

**Não:**

```swift
let message: String = "Hello Swift!"
let currentBounds: CGRect = computeBounds()
var names = [String]()
var lookup: [String: Int] = [:]
```

## Classes e Estruturas

Utilizar `struct` para coisas que **não possuam** uma identidade. Por exemplo, uma resolução de monitor.

Utilizar `class` para coisas que **possuam** identidade. Por exemplo, uma pessoa.

### Uso de Self

Não use `self` no código Swift, a não ser que seja necessário, como nos casos de acesso à propriedades dentro de *closures*.


## Optionals

Utilize Optional quando o valor retornado pode ser nulo.

Não utilize *force unwrap* `!`. Use variáveis *implicitly unwrapped* apenas para `IBOutlets`.

Utilize *optional chaining* para percorrer Optionals aninhados.

```swift
self.textContainer?.textLabel?.setNeedsDisplay()
```

Utilize *optional binding* quando pretender realizar várias operações com a variável que for *unwrapped*.

```swift
if let textContainer = self.textContainer {
  // do many things with textContainer
}
```

Utilize o mesmo nome da variável quando estiver fazendo o *unwrap*. Também sempre faça o *unwrap* na mesma cláusula de `if let` quando for acessar um Optional aninhado. Nos casos possíveis, utilize `where` para especificar a condição do *unwrap*.

**Sim:**

```swift
var subview: UIView?
var volume: Double?

// later on...
if let subview = subview, let volume = volume, volume == 10 {
  	// do something with subview and volume 10
}
```

**Não:**

```swift
var optionalSubview: UIView?
var volume: Double?

if let usubview = optionalSubview {
  	if let v = volume {
  		if v == 10 {
    		// do something with usubview and v 10 :(
    	}
  	}
}
```

## Caminho Feliz

Sempre priorize o caminho feliz, utilizando quando necessário `guard` para limpar o fluxo principal.

**Sim:**

```swift
func makeTrip(from: String?, to: String?) -> Bool {
	guard let from = from else { return false }
	guard let to = to else { return false }	
	return takeOff(from, to)
}
```

**Não:**

```swift
func makeTrip(from: String?, to: String?) -> Bool {
	if let from = from {
		if let to = to {
			return takeOff(from, to)
		}	
		return false
	}
	return false
}
```


## Agradecimentos

Esse guia utilizou como referência o coding style de:

* [Mikail Freitas](https://gist.github.com/mikailcf/8a0b9fa12bf8493411f4b373a4db7735)
 
* [raywenderlich.com](https://github.com/raywenderlich/swift-style-guide/blob/master/README.markdown)

