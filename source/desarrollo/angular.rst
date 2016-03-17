Guia de estilo para  AngularJS
===============================

La presente guia de estilo presenta un conjunto de convenciones tomadas por el equipo
de Posma Group para facilitar el desarrollo de aplicaciones AngularJS.

Esta guia sera revisada constantemente y se recomienda repasarla cada cierto tiempo
para aprender e implementar nuevas convenciones.


- Un componente por archivo; Mejor mantenimiento del proyecto, esto aplica a directivas,
  controladores, factorys, servicios, etc...

- Funciones pequenas - 75 Lineas de codigo; Mejora las pruebas y la mantenibilidad del codigo

- Closures en los componentes de Angular; Evita colision de variables globales: 

.. code-block:: javascript 

    (function(){
         // codigo aqui
     })();

- Nombrar con identificadores unicos a los modulos evitan colisiones entre ellos:
  (app.users, app.maps)

- Setters: Declarar los modulos sin usar 'var'; usar un setter para instanciar el modulo. 

.. code-block:: javascript 

    angular
        .module('app', [
            'ngAnimate',
            'ngRoute',
            'app.dashboard'
         ]);

- Getter: Al usar un modulo, se evita usar var; hace mas legible el codigo y evita colisiones. 

.. code-block:: javascript

    angular
        .module('app')
        .controller('SomeController', SomeController);

    function SomeController() { }

- Setter y Getter: un modulo se instancia (set) una sola vez y se invoca (get) tantas
  veces sea necesario.

.. code-block:: javascript

    angular.module('app', []); // Set, se instancia una sola vez.
    
    angular.module('app'); // Get, se invoca N veces

- Usar funciones con nombre como retorno mejora la lectura del codigo, o hace mas facil para
  debuggear y minimiza los anidamientos de funciones de retorno

.. code-block:: javascript

    // dashboard.js
    angular
        .module('app')
        .controller('DashboardController', DashboardController);

    function DashboardController() { }
    // logger.js
    angular
        .module('app')
        .factory('logger', logger);

	function logger() { }

- Usar la sintaxis de *"ControllerAs"* a la hora de inyectar un controlador crea una forma mas
  legible en la vista de instanciar objetos asociados a dicho controlador de la forma
  "controlador.objeto", esto hace el codigo mas legible y evita problemas de referneciado.

.. code-block:: html

    <div ng-controller="CustomerController as customer">
        {{ customer.name }}
    </div>

- Al usar la sintaxis de *"ControllerAs"* el **$scope** esta atado dentro del controller
  con "**this**", de manera que this puede ser usado para la referencia de los valores de la vista.

**Aclaratoria**: Usar esta sintaxis no evita el uso de $scope para referenciar objetos dentro del
entorno, aun cuando this puede ser usado para el manejo de variables dentro de la vista, el uso
de $scope es mas poderoso y complejo.

La manera mas simple para el uso de 'this' es capturarla en una variable definida en el controlador,
por ejemplo vm, que representa ViewModel. Esta variable puede cambiar de nombre de controlador a
controlador aumentando su legibilidad en vistas que utilizan dos o mas controladores.

.. code-block:: javascript

    function CustomerController() {
        var vm = this;
        vm.name = {};
        vm.sendMessage = function() { };
    }

- Al crear objetos vinculabes, es ideal crearlos al inicio del controlador en orden alfabetico,
  esto aumenta su legibilidad, esto aplica para objetos que van a ser usados mas adelante en el
  bloque del controlador dentro de funciones, de manera que al ser usados ya esten vinculados

.. code-block:: javascript

	function AvengersController(avengersService, logger) {
	    var vm = this;
	    vm.avengers = [];
	    vm.getAvengers = getAvengers;
	    vm.title = 'Avengers';

	    activate();

	    function activate() {
	        return getAvengers().then(function() {
	            logger.info('Activated Avengers View');
	        });
	    }

	    function getAvengers() { 

	    //getAvengers se vinculo a vm.getAvengers al inicio del controlador
	    
	        return avengersService.getAvengers().then(function(data) {
	            vm.avengers = data;
	            return vm.avengers;
	        });
	    }
	}


- Delegar la logica de los controladores a los factorys o los services, esto permite reutilizacion
  de codigo, dado que puede ser inyectado en otro controlador una vez expuesto el servicio,
  es mas facil de aislar para hacer pruebas unitarias, remueve la dependendica del controlador
  y ofusca la los detalles de implemntacion en el controlador y por ultimo, mantiene el codigo
  limpio y compacto.
  
.. code-block:: javascript


	function OrderController(creditService) {
   		var vm = this;
	    vm.checkCredit = checkCredit;
	    vm.isCreditOk;
	    vm.total = 0;

	    function checkCredit() {
	       return creditService.isOrderTotalOk(vm.total)
	          .then(function(isOk) { vm.isCreditOk = isOk; })
	          .catch(showError);
	    };
	}

- Un controlador por vista, asegura un enfoque y robustez a cada vista, y permite que las pruebas
  unitarias sobre cada vista sean confiables. 

- Al emparejar un controlador con una vista, lo ideal es hacer en el momento de crear las rutas de
  las vistas, de esta manera se evita tocar el *html* para la inyeccion de controladores, 
  se mantiene un standard de inyeccion de controladores, facilita el debuggeado.

.. code-block:: javascript 

	// route-config.js
	angular
	    .module('app')
	    .config(config);

	function config($routeProvider) {
	    $routeProvider
	        .when('/avengers', {
	            templateUrl: 'avengers.html',
	            controller: 'Avengers',       	//<-- Inyeccion del Controlador
	            controllerAs: 'vm'      		//<-- Alias del Controlador
	        });
	}


- Services y Factories: Los services y factories son a grandes rasgos similares en su funcionamiento
  (ambos son Singletons), aun cuando los factories abarcan mas funcionalidades que los services;
  sin embargo, para simplicidad de la explicacion este sera el enfoque que se les dara para esta guia
  de estilo. Por lo tanto, para mantener un standard se utilizaran factories en cualquier caso donde
  un service seria aceptable. Esto implica que todos los services son factories, algo importante de
  notar es que un factory devuelve un objeto, entre los cuales este objeto puede ser un recurso,
  un url, un string e inclusive un servicio (Javascript permite devolver inclusive funciones).

.. code-block:: javascript


	//Esto es un servicio
		angular
	    .module('app')
	    .factory('logger', logger);

	function logger() {
	    return {
	        logError: function(msg) {
	          // Codigo de la funcion.
	        }
	   };
	}


- Los Factories deben tener una responsabilidad **unica**, que esta definida por el contexto con el
  que se crea, una vez que el factory excede esa unica resposabilidad, otro factory debe ser creado
  para llevar acabo esa nueva responsabilidad.

- Un factory siempre deberia tener sus miebmros accesables en el tope de la interfaz del servicio.
  Esto quiere decir que cuando se tienen variables a las que el servicio debe acceder, deberian
  estar definidas al inicio, esto ayuda a identificar que miembros son llamados y deben ser
  "*mockeados*" y/o probados, ademas de que evita tener que navegar dentro del codigo para
  verificar el codigo cuando las funciones son mas largas.

.. code-block:: javascript 

	function dataService() {
	    var someValue = '';
	    var service = {
	        save: save,
	        someValue: someValue,
	        validate: validate
	    };
	    return service;

	    ////////////

	    function save() {
	        // Cuerpo de la funcion
	    };

	    function validate() {
	        // Cuerpo de la funcion
	    };
	}

- Al igual que con los controladores es ideal que todas las *declaraciones de funciones* en los servicios
  se utilicen para ofuscar los detalles de implementacion. Esto sirve para quitar toda la complejidad del
  codigo del principio y permitir tener una idea clara y concisa del funcionamiento del servicio, ademas 

  que esto evita problemas de dependencia (por ejemplo la *var a* depende de *var b*, al declararse en el
  tope evita conflictos).

.. code-block:: javascript 

	function dataservice($http, $location, $q, exception, logger) {
	    var isPrimed = false;
	    var primePromise;

	    var service = {
	        getAvengersCast: getAvengersCast,
	        getAvengerCount: getAvengerCount,
	        getAvengers: getAvengers,
	        ready: ready
	    };

	    return service;

	    function getAvengers() {
	        // implementation details go here
	    }

	    function getAvengerCount() {
	        // implementation details go here
	    }

	    function getAvengersCast() {
	        // implementation details go here
	    }

	    function prime() {
	        // implementation details go here
	    }

	    function ready(nextPromises) {
	        // implementation details go here
	    }
	}	


- Refactorizar la logica de manera que el tratamiento, obtencion e interaccion de
  datos sea manejado por un factory aparte del controlador que los utiliza. Esto
  significa que la responsabilidad del controlador es de presentacion y recoleccion
  de informacion para la vista. No le deberia importar como o de donde salen los datos,
  siempre y cuando sepa a quien debe preguntar. Separar la obtencion de datos del controlador
  simplifica la logica del mismo. Facilita las pruebas unitarias y el mock.

.. code-block:: javascript


		//Controlador que retorna respuestas
		verRespuestasBaseCtrl.$inject = ['$rootScope', '$location', 'baseAnswers', 'baseQuestionInfo'];
	    function verRespuestasBaseCtrl($rootScope, $location, baseAnswers, baseQuestionInfo){
	        var vm = this;
	        vm.baseAnswers = baseAnswers;
	        vm.baseQuestionInfo = baseQuestionInfo;
	        //llamado a un factory que devuelve los datos necesarios para la vista
	        baseAnswers.query({id: vm.baseQuestionInfo.PkBaseQuestion},
	            function (successResult) {
	                baseAnswers = successResult;
	            },
	            function () {
	                console.log("Error al obtener las preguntas.");
	            });
	        return vm;        
	    };

- En caso de llamar un servicio de datos que devuelve una promesa (como en el caso de *$http*),
  retorna una promesa en la llamada a la funcion tambien.


.. code-block:: javascript 

	activate();

	function activate() {
	    ///
	     // Step 1
	     // Ask the getAvengers function for the
	     // avenger data and wait for the promise
	     ///
	    return getAvengers().then(function() {
	        ///
	         // Step 4
	         // Perform an action on resolve of final promise
	         //
	        logger.info('Activated Avengers View');
	    });
	}

	function getAvengers() {
	      //
	      // Step 2
	      // Ask the data service for the data and wait
	      // for the promise
	      //
	      return dataservice.getAvengers()
	          .then(function(data) {
	                //
	               // Step 3
	               // set the data and resolve the promise
	               //
	              vm.avengers = data;
	              return vm.avengers;
	      });
	}

- Al igual que los controladores y los servicios, las directivas se deben a limitar a una
  sola por archivo, adicional a esto, las directivas deberian de "limpiar" cuando terminan,
  usando *element.on('$destroy', ...)* o *scope.$on('$destroy', ...)* cuando se remueve la directiva.


.. code-block:: javascript

	// calendar-range.directive.js 

	//
	// @desc order directive that is specific to the order module at a company named Acme
	// @example <div acme-order-calendar-range></div>
	//
	angular
	    .module('sales.order')
	    .directive('acmeOrderCalendarRange', orderCalendarRange);

	function orderCalendarRange() {
	    // implementation details
	}

- Al manipular directamente el DOM, es ideal usar una directiva, siempre y cuando no existan metodos
  alternos para hacerlo, por ejemplo, usar CSS para poner estilos, o usar *ngShow* o *ngHide*, entre otros.

- Al crear la directiva, se debe proveer un prefijo unico para ella, por ejemplo, al crear una directiva 
  llamada "*posmaTieneMiMejorDirectiva*" en el HTML deberia declararse como *posma-tiene-mi-mejor-directiva*,
  el prefijo 'posma' puede usarse para diferenciarlo de "*venezuelaTieneMiMejorDirectiva*", es importante notar
  que usar prefijos como '*ng-*' o '*ion-*' genera conflictos con directivas de Angular o Ionic, es importante
  tomar en cuenta eso.

- Cuando se crea una directiva que funcione como un elemento stand-alone, se debe permitir elementos y atributos
  configurables (allow restrict EA).


.. code-block:: html


    // HTML
	<my-calendar-range></my-calendar-range>
	<div my-calendar-range></div>
	
.. code-block:: javascript

	// js
	angular
	    .module('app.widgets')
	    .directive('myCalendarRange', myCalendarRange);

	function myCalendarRange() {
	    var directive = {
	        link: link,
	        templateUrl: '/template/is/located/here.html',
	        restrict: 'EA'
	    };
	    return directive;

	    function link(scope, element, attrs) {
	      // implementacion
	    }

- Cuando se inyecta la directiva se deberia usar la sintaxis de "*controllerAs:*" para mantener la uniformidad
  del codigo. Adicional a esto se debe usar "*bindToController = true*" cuando se usa la sintaxis anterior, 
  de manera de poder asociar el scope exterior con el scope del controlador.


    
.. code-block:: html

    //HTML
	<div my-example max="77"></div>


.. code-block:: javascript

	//js
	angular
	    .module('app')
	    .directive('myExample', myExample);

	function myExample() {
	    var directive = {
	        restrict: 'EA',
	        templateUrl: 'app/feature/example.directive.html',
	        scope: {
	            max: '='
	        },
	        link: linkFunc,
	        controller: ExampleController,
	        // note: This would be 'ExampleController' (the exported controller name, as string)
	        // if referring to a defined controller in its separate file.
	        controllerAs: 'vm',
	        bindToController: true // because the scope is isolated
	    };

	    return directive;

	    function linkFunc(scope, el, attr, ctrl) {
	        console.log('LINK: scope.min = %s *** should be undefined', scope.min);
	        console.log('LINK: scope.max = %s *** should be undefined', scope.max);
	        console.log('LINK: scope.vm.min = %s', scope.vm.min);
	        console.log('LINK: scope.vm.max = %s', scope.vm.max);
	    }
	}

	ExampleController.$inject = ['$scope'];

	function ExampleController($scope) {
	    // Injecting $scope just for comparison
	    var vm = this;

	    vm.min = 3;

	    console.log('CTRL: $scope.vm.min = %s', $scope.vm.min);
	    console.log('CTRL: $scope.vm.max = %s', $scope.vm.max);
	    console.log('CTRL: vm.min = %s', vm.min);
	    console.log('CTRL: vm.max = %s', vm.max);
	}


.. code-block:: html

	// example.directive.html
	<div>hello world</div>
	<div>max={{vm.max}}<input ng-model="vm.max"/></div>
	<div>min={{vm.min}}<input ng-model="vm.min"/></div>



Nombrado
--------

Cada componente lleva en su nombre de archivo el tipo de funcion que cumple, por ejemplo los
controladores son '*nombre.controller.js*', los servicios son '*nombre.service.js*' y asi
sucesivamente para los casos de directivas, modulos, filtros, configuracion etc.

Se usan nombres consistentes para todos los componentes, empezando de una descripcion de
la funcion del componente, seguido de tipo '*feature.type.js*' por ejemplo, *formulario.controller.js*
el cual tendria como nombre en su modulo *FormularioController*, *login.service.js* seria *LoginService*.

De igual forma los archivos de prueba de cada componente deberian contar con el sufijo de 'spec',
ejemplo *formulario.controller.spec.js* o *login.service.spec.js*.

El nombrado de los controladores deberia de estar asociado a su funcionamiento, ademas de usar
**UpperCamelCase** para su nombrado, por ejemplo "*EsteEsElnombreDelControlador*", "*EsteEsOtroControlador*".


- La estructura de proyecto deberia ser de la siguiente manera:


| app/
| app.module.js
| app.config.js
| components/
|     calendar.directive.js
|     calendar.directive.html
|     user-profile.directive.js
|     user-profile.directive.html
| layout/
|     shell.html
|     shell.controller.js
|     topnav.html
|     topnav.controller.js
| people/
|     attendees.html
|     attendees.controller.js
|     people.routes.js
|     speakers.html
|     speakers.controller.js
|     speaker-detail.html
|     speaker-detail.controller.js
| services/
|     data.service.js
|     localstorage.service.js
|     logger.service.js
|     spinner.service.js
| sessions/
|     sessions.html
|     sessions.controller.js
|     sessions.routes.js
|     session-detail.html
|     session-detail.controller.js


Modularidad
-----------

- Deben crearse modulos pequenos que encapsulen solo una responsabilidad.

- Se debe crear un modulo del app, el cual se encarga de agrupar y asociar todos los
  otros modulos de la aplicacion entre ellos.

- Solo se debe usar la logica para asociar los modulos entre ellos en el *app.module*,
  cualquier otra logica debe ser relegada a los otros modulos 

- Se deberian crear modulos reusables que permitan usarse en otras aplicaciones como por 
  ejemplo manejo de excepciones, logging, diagnosticos, seguridad, entre otros.

- Si existen dependencias de modulos, estas deben estar definidas dentro de los modulos
  propios, no en el modulo de la aplicacion.


.. code-block:: javascript

	angular.module('app', [

		//Shared Modules
		'app.core', //app.core necesita ngAnimate, ngSanitize
		'app.widgets',

		// Feature Areas
		'app.customers',
		'app.dashboard'// app.dashboard utiliza app.core y app.widget como modulos compartidos
	]);

	angular
		.module ('app.core',[
			// Angular modules
			'ngAnimate',
			'ngSanitize',
			// Cross-app modules
			'blocks.exception',
			'blocks.logger',
			'blocks.router',
			// 3rd-party modules
			'ui-router',
			'ngplus'
		]);

	angular.	
		module('app.dashboard', [
			//Shared Modules
			'app.core', 
			'app.widgets'
		]);
