# Pasos para realizar la práctica

## 1.- Creación fichero Jenkinsfile con stage de build

1.- Crear un fichero con nombre "Jenkinsfile" sin extensión.

2.- Añadir el siguiente contenido:

	node {

		stage ('Build App')
		{
			echo 'Running Build App'
			
			bat 'mvn clean package'
		}
				
	}


## 2.- Actualización de fichero Jenkinsfile para añadir nuevo stage de pruebas automáticas
	
	stage ('Run unit test')
	{
		echo 'Running Unit Tests'
	
		bat 'mvn clean test -Dtest=HolaControllerTest'
					
		step([$class: 'JUnitResultArchiver', testResults: "**/surefire-reports/*.xml"]) 
			
	}
	
	stage ('Run Integration test')
	{
		echo 'Running Integration Tests'
	
		bat 'mvn clean verify'
									
		step([$class: 'JUnitResultArchiver', testResults: '**/failsafe-reports/TEST-*.xml'])

	}

## 3.- Instalación de Jenkins

1.- Descargar desde https://www.jenkins.io/download/thank-you-downloading-windows-installer-stable/

2.- Iniciar instalación
	
	2.1.- Service Logon Credentials: Seleccionamos "Run service as LocalSystem"
	
	2.2.- Port Selection: Seleccionamos 8080
	
	2.3.- Java home directory: Seleccionamos ruta donde tenemos instalada la jdk (e.j.: C:\dev\java\java-11-openjdk-11.0.12-1\)
	

## 4.- Configuración de Jenkins

1.- Una ver terminada la instalación abrimos la siguiente dirección en un navegador web: http://localhost:8080/

2.- Unlock Jenkins
		
		Buscar contraseña en el fichero "initialAdminPassword" y la ruta que se nos indica 
		(e.j.: C:\ProgramData\Jenkins\.jenkins\secrets\initialAdminPassword)

3.- Install suggested plugins

4.- Create First Admin User: Creamos nuevo usuario administrador

5.- Instance configuration. Nos guardamos la URL de acceso a jenkins.

## 5.- Creación y ejecución de pipeline

0.- Para poder descargarnos el código de nuestro proyecto en github desde Jenkins, debemos crear un token de autenticación en Github, para ello:
	
	Accedemos a nuestra cuenta de Github
	
	Vamos a la pantalla de Herramientas/Configuración de desarrollo: https://github.com/settings/tokens
	
	Seleccionamos en el menú lateral izquierdo la opción: Personal Access Tokens
	
	Pulsamos el botón de "Generar nuevo token"
	
		Introducimos un nombre al token

		Seleccionamos todas las opciones de "repo", "admin:repo_hook" y "delete_repo"
			

1.- Crear "Nueva tarea"

	1.1.- Añadir nombre: demo y seleccionar "Multibranch Pipeline"

2.- Branch Sources
	
	Seleccionamos Github
	
	Añadimos credenciales
	
		Seleccionamos el nombre del proyecto en el desplegable (en nuestro caso "Demo")

		Campo "Kind": Seleccionamos "Username & password"
		
		Username: <github_user>
		
		Password: <github_token>
		
		Campo ID: springbootdemo
		
		Campo Description: Spring Boot webapp demo
	
	Seleccionamos las credenciales creadas en el desplegable
	
	Pulsamos "Validate" para confirmar que la conexión a Github es correcta

3.- Esperamos a que el job se lance automáticamente

4.- Refrescamos pantalla para ver el pipeline creado

5.- Se lanzarán las fases de "Build App" y "Run unit test"

6.- Si obtenemos el siguiente error en los logs debemos instalar el plugin de "HTML publisher"

	java.lang.NoSuchMethodError: No such DSL method 'publishHTML'



    