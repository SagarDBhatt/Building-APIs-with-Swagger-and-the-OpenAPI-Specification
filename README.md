# Building-APIs-with-Swagger-and-the-OpenAPI-Specification
The Swagger platform offers a rich ecosystem of tools that developers can use to create well-crafted APIs that boast higher adoption rates. Curious about how to leverage Swagger in your workflow? In this course, learn how to build and document high-quality APIs with Swagger and the OpenAPI Specification. After going over the basics of the Swagger ecosystem, instructor Kevin Bowersox shows how to build API definitions that speed up the delivery of APIs using the OpenAPI Specification. He also shows how to create and publish APIs with SwaggerHub, an integrated API development platform. To wrap up, Kevin steps through a hands-on project that shows you how to plan API development for applications.

# Open Source Swgger Tools - Swagger Editor and Swagger UI
1. Clone OR Download zip file for Swagger editor and Swagger UI from swagger github repository. 

-> I follow OAS 3.0 (Open API Specification) to document APIs. Go to Open API Specification Github repo and follow read.md file to follow the specifications.

## Specify the version of OAS.
openapi : 3.0.0    

## info tag specify the meta=data of API
info :        
  title: Sentinel API
  version: 1.0.0
  
## paths to specify the resources. This shows exact functionality of our API: 
-> we are follwing yaml specification of writing / documenting. Thus, we need 2 spaces before ' get: '

    paths:
      /getAllSentinelData:
        get: 
          responses:
            200: 
             description: Get all the sentinel data. 
             content:
               application/json: 
                  schema: 
                    type: array
                    items: 
                      properties: 
                        id:
                          type: integer
                          example: 400
                        name:
                          type: string
                          example: sample string

## Group bunch of APIs within the same group: 

-> To achieve this, we need to define the 'tags:' between info: and paths: 

info :        
  title: Sentinel API
  version: 1.0.0

tags:
  - name: Sentinel Project.
  
paths:
  /getAllSentinelData:
    get:   
      tags: 
        - Sentinel Project.
  
=> All the APIs with tags "Sentinel Project" will be grouped in single namespace "Sentinel Project".

## Documenting passed query param in Open API Specs: 

-> in : query :: defines the datatype of the parameter.In our case, it is query parameter. Eg: http://localhost:5000/getSentinelById?id=10

### Imp: Any paramaeter started with '-' symbol represnets array. For eg:  parameters: -  in : query

paths:
  /getAllSentinelData:
    get:   
      parameters:
        -  in : query
           name: id
           description: prequel Id
           schema: 
              type: integer
              example: 10
              
## Documenting path parameter in Open API Specs:  http://localhost:5000/getSentinelById/10

    paths:
      /getSentinelDataById/{prequelD}
        get:
          parameters:
            - in : path
              name: PrequelId
              description: Prequel Id from Sentinel Data
              required: true
              schema:
                type: integer
                exapple: 1234
            
      responses:
        200: 
          description: This os a JSON payload of Sentinel product. 
          content:
            application/json:
              schema: 
                type: Object
                properties:
                  prequelId:
                    type: object
                    example: 12
                  name: 
                    type: string
                    example: Lemon Water
                    
## Code Reusability in API Documentation: 

-> Some block of code are common for all the APIs. It is better to define once and reuse the same block like calling function. 
-> This can be achieved by using 'Components' block. 

Eg: In each api we need to define 'schema'. This is repitation of the code work. Rather, define schema in components and call the schema wherever is needed. 

  responses:
        200: 
          description: This os a JSON payload of Sentinel product. 
          content:
            application/json:
              schema: 
                $ref: '#/components/schemas/sentinel_schema'
                
                
    components:
      schemas:
        sentinel_schema:
          type: Object
          properties:
             prequelId:
               type: object
               example: 12
             name: 
               type: string
               example: Lemon Water
            
            
## Another component that can be added is responses: 
-> Some error code are common to provide in a response list. Those error can be defined in the component and reuse in the documentation. 

    Components:
      responses:
        500ErrorAPI:
          description: Unexpected error
