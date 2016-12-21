# Docu.Me - Documentation Portal Generator

Docu.Me generates clear,  searchable, easy-to-customize documentation portal for APIs from your Swagger specifications.
Built with Java and designed with Bootstrap and Jquery, it is based on the current version of the [Open API specification](https://github.com/OAI/OpenAPI-Specification) formerly known as Swagger.

This documentation generator tool helps in creating interactive API documentation by:
- Pre-populating your API documentation with your own API key needed to call your API services.(See the set API key installation section)
- Create an example section in your document based by executing an API call request while generating your documentation. This allows you to display the actual response example to the user.
	  
You can create your own swagger.yaml file with [Swagger Editor](http://editor.swagger.io/#/).

This repo contains the content and specifications for the [Amadeus Travel Innovation Sandbox](https://sandbox.amadeus.com).  

This is a Spring boot project built with [Spring Tool Suite](https://spring.io/tools/sts/all) (STS).
You can download/clone the repo and import it as an existing project into workspace in STS.
It is a Maven project so it has all required dependencies in the pom.xml.

### Prerequisites
You need the following installed and available in your $PATH:

* [Java 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Apache maven 3.0.3 or greater](http://maven.apache.org/install.html)

This is a simple [tutorial](https://www.mkyong.com/maven/how-to-install-maven-in-windows/) I followed to install them.

### Set API key as environment variable.
You can follow this [tutorial](https://www.java.com/en/download/help/path.xml).
Enter variable name as "APIKEY" and value with your apikey.

## Installation of Docu.me:
Clone the project into your local. You can follow one of the ways to generate documentation.

### From terminal

Go into the docuDemo folder :

	$ cd docu.me/docuDemo/
	$ mvn spring-boot:run -Drun.arguments="args1,args2"
	
 - args1 = Swagger yaml file location. For example "path/swagger.yml".
 - args2 = This parameter should be true if you want to generate examples on the fly by calling the API with default values or set it to false.

Your command will look like:

	$ mvn spring-boot:run -Drun.arguments="path/swagger.yml,false"

### From Spring Tool Suite

* Import _docuDemo_ folder as *Existing Maven Project*. Go to the DocuDemoApplication class and click Run configurations.
* Go to Arguments tab and pass "args1,args2" as explained above.
* Go to Environment > New . Enter variable name as "APIKEY" and value with your apikey.
* Click Apply > Run.

### From Eclipse

* Select the _docuDemo_ folder and import it as *Existing Maven Project*.
* Once the project is imported. Select Run > Run Configurations. Create a new Java Application.
* Set the Main class. Click on the Search button and select "DocuDemoApplication" from com.docume.main package.
* Go to Arguments tab and enter the following two arguments:
  - the full path file location of your swagger yaml file of your API. (ex C:/myproject/documentation/myswagger.yml)
  - a boolean value: true indicating that you want to generate examples on the fly by calling your API, false otherwise.
* Go to Environment > New . Enter variable name as "APIKEY" and value with your apikey.
* Click Apply > Run.

## Output

* Your API documentation will be generated under a new folder named _Portal_ located at the root level. You may need to refresh your project in order to see the changes.
* An index.html is your starting point to access your  different APIs and response models of the API documentation generated by Docu.Me.

### Customize it your way!

There are 3 pages to focus on:

### Index Page

Here we have list of APIs and list of response models in our APIs.

* The list of APIs come from the operationID present in the swagger.yml file.
* The list of models come from swagger.getDefinitions() which gives Map<String, Model>.

### API documentation page

We can see 10 sections on this page.

* Title : Title comes from the [info object](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#info-object) mentioned in the swagger file.
* Summary : Summary tag from yaml file,[Operation object](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#operationObject).
* Description : Description tag from the yaml file,[Operation object](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#operationObject).
* HttpMethod : We get HttpMethod from path.getOperationMap(),[path](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#paths-object) which gives us Map<HttpMethod, Operation>.
* Resource URL : This is build using  schemes/host/basePath/paths from the [swagger yaml file](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#swagger-object).
* Query Parameters : Paramters object from the Operations contains [parameters](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#parameterObject).
* Tags : Tags provided in the yaml file. This can be used for categorizing bunch of APIs together.
* Responses : Responses are provided by [Response](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#responsesObject) object.
* Example : Example section is displayed if you pass true for generating example data by calling the actual API,this is explained above while passing arguements to the application.
* Result : The response generated by 'Send request' is displayed here.
* Response Schema : This is created from the Model's definition provided by [Definition](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#definitionsObject) in the swagger yaml.

### Response model documentation page

All the response models are listed here with all of their elements.This information comes from [schemaObject](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#schemaObject) mentioned in definitions object. Elements can be of type Array, String or Reference to another model.

## Mustache templates:

When you want to make changes to the look and feel of the documentation portal mustache templates can be customized.
* You can follow [Mustache.js](https://mustache.github.io/mustache.5.html) guide for tweaking the mustache files.
* A list of mustache variables are present in Documentation.java class.
* Here, index.mustache creates index.html, model.mustache creates model.html and api.mustache creates {{api}}.html.
* In addition to changing the mustache files, one can customize the html files , just in case you want to add some static content that your swagger file could not accommodate !

### Limitations for generating examples:

* Supports only API key authorization, can be worked on for OAuth authorization mechanism using swagger security tools.
* In this case your documentation portal will be generated as expected but you cannot 'Get a live response' which needs to call your API.
