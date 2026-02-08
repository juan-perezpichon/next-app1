---
name: 🔧 apx_code_generator
description: Agente especializado en generar código para componentes BBVA APX usando Java, Springboot y patrones de BBVA APX Framework
tools: ['runCommands', 'edit', 'search', 'new', 'testFailure', 'github-mcp-server/get_file_contents', 'todos']
---

# Agente Generador de Código BBVA APX

Soy un especialista en desarrollo de **BBVA APX Framework** con experiencia avanzada en **Java**, **Springboot** y arquitecturas de microservicios. Mi expertise se centra en generar código de alta calidad y seguro siguiendo los estándares y patrones establecidos por BBVA.

## Análisis de Requisitos

**Analiza** si existe la instruccion '.github\instructions\apx_style_guide.instructions.md' en el repositorio con ayuda de la tool #tool:search . Si no existe, utiliza #tool:github-mcp-server/get_file_contents para obtener la información del fichero de instrucciones. Debes leer los ficheros Enteros para obtener la información necesaria para realizar la tarea. Las URLs son '<https://bbva.ghe.com/copilot-test/bbva-copilot-instructions/blob/main/technology/APX/github/instructions/apx_style_guide.instructions.md>' para la guía de estilo.
**Utiliza** la #tool:github-mcp-server/get_file_contents para obtener la información relativa a desarrollo de soluciones en arquitectura APX. Debes leer los ficheros Enteros para obtener la información necesaria para realizar la tarea. La URL al repositorio con toda la documentación de programación en APX es '<https://bbva.ghe.com/copilot-test/bbva-apx-documentation>'.

**Utiliza** la #tool:github-mcp-server/get_file_contents para obtener la guía de desarrollo seguro para la arquitectura APX. Debes leer la guia de seguridad Entera para obtener la información necesaria para realizar la tarea. La URL a la guía de desarrollo seguro para APX es 'https://bbva.ghe.com/copilot-test/bbva-copilot-instructions/blob/main/spaces/use-cases/POC-Security/documentos/APX%20-%20Gu%C3%ADa%20de%20desarrollo%20Seguro.md'.

# Pasos a seguir - ToDo

## Primer paso - Realiza el análisis de requisitos

 - Debes realizar el análisis de requisitos siguiendo las indicaciones del apartado "Análisis de Requisitos".

## Segundo paso - Lectura/Compresión de la documentacion de la arquitectura APX

  - Debes leer la documentación de la arquitectura APX para entender los estándares y patrones de desarrollo en APX que deben seguirse al generar código.

## Tercer paso - Lectura/Compresión de la guia de desarrollo seguro

   - Debes leer la guía de desarrollo seguro específica para la arquitectura APX para entender los requisitos y controles de seguridad que deben cumplirse en el desarrollo de componentes APX.
   - Analiza los patrones de seguridad, recomendaciones y requisitos obligatorios descritos en la guía.
   - Aplica los principios de desarrollo seguro en todas las tareas de generación y modificación de código, asegurando el cumplimiento de los controles de seguridad definidos para APX.


## Cuarto paso - Lectura/Compresión del repositorio

 - Debes leer el fichero README.md y todos los ficheros del repositorio para entender el funcionamiento del componente APX.

## Quinto paso - Creación de código

 - Debes generar el código necesario siguiendo los estándares de APX que se indican en las instrucciones y en la documentación de la arquitectura.
============================== INSTRUCTIONS APX ====================
---
description: "Guía de estilo y mejores prácticas para desarrollo en BBVA APX: dependencias, seguridad, migración y rendimiento"
applyTo: "**/*.java, pom.xml"
---

# Guía de Estilo BBVA APX - Development Good Practices

## Introducción

La arquitectura APX (Extended Java Backend Architecture) es una extensión de la arquitectura Backend que proporciona las mismas capacidades en el mundo distribuido. Esta guía establece las mejores prácticas, patrones de desarrollo y estándares de codificación para el desarrollo de componentes APX.

## Arquitectura APX

APX está basado en tecnologías open source y actúa como una extensión de la plataforma Mainframe, ofreciendo una alternativa confiable para el desarrollo de transacciones desacopladas del canal.

### Componentes Principales

**APX Transaction**: Unidad de aplicación ejecutada en APX Online
**APX Library**: Encapsula la lógica de negocio y acceso a datos
**DTO**: Representación de entidades de negocio como Bean
**APX JOB**: Arquitectura de ejecución batch basada en Spring Batch

## 1. Mejores Prácticas de Desarrollo

### 1.1 Estandarización del Código

**OBLIGATORIO**: Todo desarrollo en APX debe estar definido en inglés:

Clases, métodos, variables y comentarios en inglés
Facilita la reutilización en todo el Grupo BBVA
Mejora la colaboración internacional

java
// ❌ INCORRECTO
public class ClienteServicio {
    public String obtenerNombre() { ... }
}

// ✅ CORRECTO
public class CustomerService {
    public String getName() { ... }
}

### 1.2 Reutilización de Código

El modelo de reutilización se basa en el empaquetado de código reutilizable en librerías APX
Toda lógica reutilizable o acceso a datos debe empaquetarse como librería APX
Evitar duplicación de código entre componentes

### 1.3 Invariabilidad del Código Generado

**PROHIBIDO** modificar componentes generados automáticamente por la arquitectura:

Clases abstractas de transacciones
Configuraciones generadas por el IDE APX
Solo modificar archivos con sufijo -app y clases de implementación

java
// ✅ SOLO MODIFICAR
public class GSAARD09Impl extends GSAARD09Abstract {
    @Override
    public void execute() {
        // Tu implementación aquí
    }
}

## 2. Gestión de Dependencias

### 2.1 Dependencias de Compilación

**REGLAS OBLIGATORIAS**:

1. **Dependencias padre prohibidas**: El pom.xml padre NO debe declarar dependencias

xml
<!-- ❌ INCORRECTO en pom padre -->
<dependencies>
    <dependency>
        <groupId>com.bbva.elara</groupId>
        <artifactId>elara-library</artifactId>
    </dependency>
</dependencies>

<!-- ✅ CORRECTO - Solo módulos -->
<modules>
    <module>GSAARD09</module>
    <module>GSAARD09IMPL</module>
</modules>

2. **Dependencias locales**: Declarar dependencias en el componente que las usa
3. **Prohibido opcional**: No usar <optional>true</optional> en dependencias
4. **Solo DTOs en interfaces**: Librerías de interfaz solo pueden usar dependencias DTO

### 2.2 Dependencias de Ejecución (Runtime)

**Configuración OSGi**: Usar Import-Package para dependencias runtime

xml
<Import-Package>
    org.osgi.framework;version="${osgi.version.manifest}",
    com.bbva.elara.aspect;version="${osgi.version.manifest}",
    spring;version="${osgi.version.manifest}",
    <!-- DTOs de terceros solo si no se referencian directamente -->
    com.bbva.external.dto;version="${osgi.version.manifest}",
    *;version="${osgi.version.manifest}"
</Import-Package>

**REGLAS IMPORTANTES**:

Mantener orden correcto en la lista de paquetes
PROHIBIDO usar resolution:='optional'
PROHIBIDO editar referencias de paquetes de arquitectura
Siempre incluir el paquete por defecto * al final

### 2.3 Gestión de Versiones APX

**Versiones por defecto**:

APX Online: com.bbva.elara:elara-project:8.0.11
APX Batch: com.bbva.elara:elara-batch:8.0.8

xml
<parent>
    <groupId>com.bbva.elara</groupId>
    <artifactId>elara-project</artifactId>
    <version>${apx.core.online.version}</version>
</parent>

### 2.4 Eliminación de Dependencias No Utilizadas

**OBLIGATORIO**: Usar APX CLI para eliminar dependencias

bash
apx del dependency -g com.bbva.example -a example-dto -y

**NO** eliminar manualmente del pom.xml para evitar inconsistencias OSGi.
## 3. Seguridad y Mejores Prácticas

### 3.1 Acceso a Datos

**Base de Datos**:

java
// ✅ CORRECTO - Usar JDBC Utility
this.jdbcUtils.queryForMap("customer.select.by.id", customerId);

// ❌ PROHIBIDO - Manejo directo de DataSource
DataSource ds = ...; // NUNCA hacer esto
Connection conn = ds.getConnection(); // PROHIBIDO

**REGLAS OBLIGATORIAS**:

NUNCA manejar DataSource directamente
SIEMPRE usar variables BIND en consultas SQL
Incluir esquema de BD en las consultas
PROHIBIDO usar sentencias específicas de BD (ej: ROWNUM de Oracle)

sql
-- ✅ CORRECTO - Compatible multi-BD
SELECT * FROM SCHEMA.TABLE WHERE ID = ?

-- ❌ PROHIBIDO - Específico Oracle
SELECT * FROM (SELECT *, ROWNUM rnum FROM TABLE) WHERE rnum >= 1

**MongoDB**:

Usar DocumentWrapper y clases del conector Datio
PROHIBIDO usar BSON, Document directamente

### 3.2 Gestión de Errores

**Usar modelo de arquitectura**:

java
// ✅ CORRECTO
if (validation.fails()) {
    this.addAdvice("GSAA00001", "Invalid parameter");
}

// ❌ PROHIBIDO - Lanzar excepciones desde execute()
throw new RuntimeException("Error");

**Excepciones permitidas**:

com.bbva.apx.exception.business.BusinessException
com.bbva.apx.exception.db.DuplicateKeyException
com.bbva.apx.exception.db.NoResultException
com.bbva.apx.exception.db.TimeoutException


### 3.3 Manejo de Excepciones

Las aplicaciones APX no deben  manejar excepciones lanzados por conectadores suministrados por  APX, por productos invocados por terceros, ni excepciones heredadas de RuntimeException.
Por lo tanto  try... catch(Exception e)  no está permitido en las aplicaciones APX.
Si ocurre una excepción, la arquitectura la capturará y activará el manejo de errores. Será responsable de deshacer los accesos donde exista transaccionalidad (base de datos, mensajes, etc.).
-Las unicas excepciones que pueden ser lanzadas desde la aplicación son las siguientes:
    - Excepciiones Arquitectura APX
        - com.bbva.apx.exception.business.BusinessException
        - com.bbva.apx.exception.io.network.TimeoutException
        - com.bbva.apx.exception.db.DuplicateKeyException
        - com.bbva.apx.exception.db.DataIntegrityViolationException
        - com.bbva.apx.exception.db.IncorrectResultSizeException
        - com.bbva.apx.exception.db.NoResultException
        - com.bbva.apx.exception.db.TimeoutException
        - com.bbva.elara.utility.interbackend.cics.exception.BusinessException
        - com.bbva.titan.client.model.TitanException
        - com.bbva.apx.exception.io.network.CircuitBreakerException
        - com.bbva.apx.exception.grpc.APXgRPCException
    - Excepciones de Java:
        - java.lang.NumberFormatException
    - Excepciones Spring:
        - org.springframework.web.client.RestClientException
        - org.springframework.web.client.HttpStatusCodeException

### 3.4 Logging

**Niveles apropriados**:

java
// INFO - Solo información de monitoreo
LOGGER.info("Transaction GSAAT09 started");

// DEBUG - Datos funcionales y detalles
LOGGER.debug("Processing customer: {}", customerId);

// ERROR - Errores que impiden ejecución
LOGGER.error("Database connection failed", exception);

// WARN - Situaciones anómalas pero controladas
LOGGER.warn("Using default value for missing parameter");

**PROHIBIDO**:

System.out.println()
Logs INFO con datos funcionales (usar DEBUG)
Capturar excepciones genéricas: catch(Exception e)
Cualquier función de System: System.currentTimeMillis(),  System.out.println

### 3.5 Transaccionalidad

**REGLAS CRÍTICAS**:

La arquitectura gestiona commit/rollback automáticamente
PROHIBIDO invocar commit/persist manualmente
Usar recursos XA para garantizar transaccionalidad
PROHIBIDO manejo manual de transacciones

java
// ❌ PROHIBIDO
connection.commit();
entityManager.persist(entity);

// ✅ La arquitectura gestiona automáticamente
this.jdbcUtils.update("customer.insert", parameters);

## 4. Rendimiento y Optimización

### 4.1 Zonas de Ejecución

Clasificación de transacciones por rendimiento:

**00**: Críticas de alto rendimiento (ej: Granting Ticket)
**10**: Dependencias bajas (≤10 dependencias)
**20**: Transacciones de proceso (orquestación)
**30**: En observación (nuevas transacciones)
**40**: Dependencias altas (>10 dependencias)

### 4.2 Threading

**PROHIBIDO ABSOLUTAMENTE**:

Crear threads manualmente
Gestión de hilos por aplicaciones
Modificar variables de entorno JVM

java
// ❌ PROHIBIDO
new Thread(() -> {
    // Código de aplicación
}).start();

// ❌ PROHIBIDO
System.setProperty("property", "value");

### 4.3 Conexiones Externas

**APX Online**:

Usar Generic API Connector para servicios externos
Validación obligatoria del Arquitecto de Soluciones
Certificados gestionados por Seguridad Lógica

**APX Batch**:

PROHIBIDO invocar APX Online o Host
Consulta previa a equipo de Arquitectura para APIs externas

### 4.4 Base de Datos

**Optimizaciones**:

Usar paginación con pagingQueryForList()
Evitar consultas dinámicas (validación requerida)
Usar bind variables siempre
Implementar pool de conexiones apropiado

java
// ✅ Paginación eficiente
List<Map<String, Object>> results = jdbcUtils.pagingQueryForList(
    "customer.select.all", firstRow, pageSize, parameters
);

## 5. Testing y Calidad

### 5.1 Unit Testing (Nuevo Modelo sin Spring)

**Configuración Moderna**:

java
@RunWith(MockitoJUnitRunner.class)
public class GSAARD09ImplTest {
    
    @InjectMocks
    private GSAARD09Impl library;
    
    @Mock
    private JdbcUtils jdbcUtils;
    
    @Mock
    private ApplicationConfigurationService configService;
    
    @Before
    public void setUp() {
        MockitoAnnotations.initMocks(this);
        ThreadContext.set(new Context());
    }
    
    @Test
    public void executeSuccessTest() {
        // Arrange
        when(jdbcUtils.queryForMap(anyString(), anyMap()))
            .thenReturn(expectedResult);
        
        // Act
        library.execute();
        
        // Assert
        assertEquals(0, library.getAdviceList().size());
    }
}

### 5.2 Migración de Tests

**APX CLI para actualización**:

bash
# Verificar estructura de tests
apx check --test

# Reparar automáticamente
apx check --test --repair

**Cambios requeridos**:

Eliminar @RunWith(SpringJUnit4ClassRunner.class)
Eliminar @ContextConfiguration
Reemplazar @Resource por @Mock
Usar MockitoAnnotations.initMocks(this)

### 5.3 Cobertura de Código

**Mínimo obligatorio**: 80% de cobertura

xml
<plugin>
    <groupId>org.jacoco</groupId>
    <artifactId>jacoco-maven-plugin</artifactId>
    <configuration>
        <rules>
            <rule>
                <element>BUNDLE</element>
                <limits>
                    <limit>
                        <counter>LINE</counter>
                        <value>COVEREDRATIO</value>
                        <minimum>0.80</minimum>
                    </limit>
                </limits>
            </rule>
        </rules>
    </configuration>
</plugin>

## 6. Migración y Versionado

### 6.1 Versionado de Componentes

**Librerías**:

Solo una versión activa simultáneamente
Cambios deben ser retrocompatibles
Usar sobrecarga de métodos para nuevas signatures

**Transacciones**:

Múltiples versiones simultáneas permitidas
Versionado obligatorio para cambios no retrocompatibles

**Cambios retrocompatibles**:

Agregar campo opcional de entrada
Campo obligatorio → opcional (entrada)
Campo opcional → obligatorio (salida)
Lógica interna

**Cambios NO retrocompatibles**:

Agregar campo obligatorio de entrada
Campo obligatorio → opcional (salida)
Agregar cualquier campo de salida

### 6.2 Migración de Versiones APX

**Proceso recomendado**:

1. Verificar compatibilidad con apx check --pom
2. Actualizar versión de arquitectura
3. Ejecutar tests completos
4. Validar funcionalidad en entorno local

bash
# Verificar y reparar POM
apx check --pom --repair

# Actualizar dependencias
apx update

## 7. Utilidades APX

### 7.1 Utilidades Disponibles

**Mediante APX CLI**:

JDBC: apx add util -n jdbc
API Connector: apx add util -n intapiconnector
MongoDB: apx add util -n mongo
Interbackend: apx add util -n intback (Solo España)
Document Generator: apx add util -n docgen
Rules Engine: apx add util -n drools
CICS Connect: apx add util -n cics -y (Solo LATAM)
### 7.2 Uso de Utilidades

**JDBC Utility**:

java
// Consulta simple - campo
String name = this.jdbcUtils.queryForString("customer.name", customerId);

//Consulta de registro 
Map<String, Object> response = this.jdbcUtils.queryForMap("customer.get", parameters);


// Consulta con paginación
List<Map<String, Object>> customers = this.jdbcUtils.pagingQueryForList(
    "customer.list.all", firstRow, pageSize, filters
);

// Actualización
int rows = this.jdbcUtils.update("customer.update", parameters);

**API Connector - Configuración**:

 Configuración **External** API Connector :

 Cuando se está gestionand una conexión con apis Externas se deben seguir los siguientes pasos. 

1. Identificar el archivo UUAARXXX-arc.xml dentro de la ruta src\main\resources\META-INF\spring


2. Se debe adicionar la propiedad (property) externalApiConnector (IMPORTANTE: Conservar este nombre) en el bean abstracto de la librería UUAARXXXAbstract  y adicionar un bean refiriendo al conector externo de la siguiente manera:

Imporante: NO OLVIDAR ADICIONAR AL BEAN LOS ARGUMENTOS En el ejemplo: 

xml
    <bean id="uuaaRXXXAbstract" abstract="true" class="com.bbva.uuaa.lib.rxxx.impl.UUAARXXXAbstract">
        ...
        <property name="internalApiConnector" ref="internalApiConnector"/>
        <property name="externalApiConnector" ref="externalApiConnector"/>
    </bean>


    <bean id="externalApiConnector" factory-bean="apiConnectorFactoryImpl" factory-method="getAPIConnector">
        <constructor-arg index="0" type="org.osgi.framework.BundleContext" ref="bundleContext"/>
        <constructor-arg index="1" type="boolean" value="false"/>
    </bean>

3. Validar que se ha agregado la variable de externalApiConnector, sino adicionarlo. 

java
    protected APIConnector internalApiConnector;

    protected APIConnector externalApiConnector;

    /**
    * @param internalApiConnector the this.internalApiConnector to set
    */
    public void setInternalApiConnector(APIConnector internalApiConnector) {
        this.internalApiConnector = internalApiConnector;
    }

    /**
    * @param externalApiConnector the this.externalApiConnector to set
    */
    public void setExternalApiConnector(APIConnector externalApiConnector) {
        this.externalApiConnector = externalApiConnector;
    }


 **API Connector - Impersonation**:


Si se desea activar un evento mediante apiConnector es necesario generar una conexión con impersonation

Configuración Impersonation API Connector :

1. Identificar el archivo UUAARXXX-arc.xml dentrode la ruta src\main\resources\META-INF\spring


2. Se debe adicionar la propiedad (property) externalApiConnector (IMPORTANTE: Conservar este nombre) en el bean abstracto de la librería UUAARXXXAbstract  y adicionar un bean refiriendo al conector externo de la siguiente manera:

Imporante: NO OLVIDAR ADICIONAR AL BEAN LOS ARGUMENTOS En el ejemplo: 

xml
    <bean id="uuaaRXXXAbstract" abstract="true" class="com.bbva.uuaa.lib.rxxx.impl.UUAARXXXAbstract">
        ...
        <property name="internalApiConnector" ref="internalApiConnector"/>
        <property name="externalApiConnector" ref="externalApiConnector"/>
         <property name="internalApiConnectorImpersonation" ref="internalApiConnectorImpersonation"/>
    </bean>


  <bean id="internalApiConnectorImpersonation" factory-bean="apiConnectorFactoryImpl" factory-method="getAPIConnector">
  <constructor-arg index="0" type="org.osgi.framework.BundleContext" ref="bundleContext" />
  <constructor-arg index="1" type="boolean" value="true" />
  <constructor-arg index="2" type="boolean" value="true" />
</bean>

3. Validar que se ha agregado la variable de externalApiConnector, sino adicionarlo. 

java
    protected APIConnector internalApiConnector;
    protected APIConnector internalApiConnectorImpersonation;

    /**
    * @param internalApiConnector the this.internalApiConnector to set
    */
    public void setInternalApiConnector(APIConnector internalApiConnector) {
        this.internalApiConnector = internalApiConnector;
    }

    public void setInternalApiConnectorImpersonation(APIConnector internalApiConnectorImpersonation) {
        this.internalApiConnectorImpersonation = internalApiConnectorImpersonation;
    }

**Comprobación Bean InternalAPIConnector**

Validar que el bean de  internalApiConnector se encuentra bien generado, de la siguiente forma:

xml

    <bean id="internalApiConnector" factory-bean="apiConnectorFactoryImpl" factory-method="getAPIConnector">
        <constructor-arg index="0" type="org.osgi.framework.BundleContext" ref="bundleContext"/>
    </bean>

**API Connector - Implementación**:

Si no se está llevando a cabo un interacción con APIS Externas o con Eventos que requieren impesonation se **NO** se debe modificar el archivo UUAARXXX-arc.xml ya que al adicionar la utilidad se crea por defecto el Bean de **internalApiConnector**

java

String service = "service.id"  //identificador del servicio en la consola de operaciones

String httpMethod = "POST" //Cualquiera de los verbos http POST-GET-PATCH

String body = "{'json':'body'}" // body de la solicitud, si es un get tendremos espacios vacios ""

HttpHeaders headers = new HttpHeaders(); //Objeto de headers para la aplicación

headers.set("Content-type","application/json;charset=utf-8"); //adición de nuevos headers

HttpEntity<Object> httpEntity = new HttpEntity<>(body, headers); //la creacion de http entity se realiza usando el body con los respectivos headers



// Realizar llamada
/** 
 * En caso de ser externalApiconnector o internalApiConnectorImpersonation se debe quien invoque el exchange 
 * 
 * Por ejemplo: 
 * externalApiconnector.exchange(...)
 * internalApiConnectorImpersonation.exchanange(...)
 * **/

ResponseEntity<CustomerDTO> response = internalApiConnector.exchange(
                            service,
                            HttpMethod.valueOf(httpMethod), httpEntity, String.class,params
                    );



**CICS Connect**

java
// Importar clases necesarias - desde el package com.bbva.elara.utility.interbackend.cics.dto
//IMPORTANTE: Asegurarse de importar las clases correctas del paquete indicado
//IMPORTANTE: No importar clases de otros paquetes o librerías. por ejemplo; com.bbva.elara.utility.cics.wrapper
//IMPORTANTE: Se debe usar la excepción com.bbva.elara.utility.interbackend.cics.exception.BusinessException

import com.bbva.elara.utility.interbackend.cics.dto.OutputHeader; 
import com.bbva.crpy.dto.cics.HostResponseDTO;
import com.bbva.elara.utility.interbackend.cics.dto.HostAdvice;
import com.bbva.elara.utility.interbackend.cics.dto.SendMessageResponse;
import com.bbva.elara.utility.interbackend.cics.dto.Status;


// Parámetros requeridos para invocación CICS
String connectionName = "CICS_CONNECTION_NAME";  // Nombre de la conexión CICS  - IMPORTANTE: Este dato debe ser un input, no debe estar hardcodeado
String copyName = "COPY_NAME";                    // Nombre del COPY/programa CICS  - IMPORTANTE: Este dato debe ser un input, no debe estar hardcodeado
Map<String, Object> elements = new HashMap<>();   // Mapa con los elementos de entrada/salida

// Ejemplo de uso
elements.put("field1", "value1");
elements.put("field2", 12345);

// Invocar CICS y obtener respuesta
SendMessageResponse sendMessageResponse = this.interBackendCicsUtils.invokeCics(
    connectionName, 
    copyName, 
    elements
);

 String responseStatus = Optional.of(sendMessageResponse)
                    .map(HostResponseDTO::getSendMessageResponse)
                    .map(SendMessageResponse::getOutputHeader)
                        .map(OutputHeader::getReturnCode)
                        .map(Status::getCode)
                        .orElse("DefaultStatus");

 if (!("00".equals(responseStatus) || "04".equals(responseStatus))) {
                    addAdviceError("UUUAA00001", "CICS invocation failed with status: " + responseStatus);
                }

// Procesar elementos de respuesta
return sendMessageResponse;




## 8. Patrones Anti-Pattern

### 8.1 Antipatrones a Evitar

**Blob Pattern**: Evitar clases muy grandes con múltiples responsabilidades
**Magic Container**: No usar campos de entrada para múltiples funciones

java
// ❌ ANTIPATRÓN - Magic Container
public void execute() {
    String action = getParameter("action");
    if ("A".equals(action)) {
        // Lógica de alta
    } else if ("B".equals(action)) {
        // Lógica de baja
    } else if ("M".equals(action)) {
        // Lógica de modificación
    }
}

// ✅ PATRÓN CORRECTO - Métodos específicos
public void executeCreate() { ... }
public void executeDelete() { ... }
public void executeUpdate() { ... }

## 9. Configuración de Proyecto

### 9.1 Estructura de Directorios

artifact/
├── dtos/
│   └── GSAACD09/
├── libraries/
│   ├── GSAARD09/           # Interface
│   └── GSAARD09IMPL/       # Implementación
└── transactions/
    └── GSAATD09-01-ES/

### 9.2 Configuración Maven

**pom.xml principal**:

xml
<properties>
    <apx.core.online.version>[8.0.0,9.0.0)</apx.core.online.version>
    <apx.core.batch.version>[8.0.0,9.0.0)</apx.core.batch.version>
    <maven.compiler.source>11</maven.compiler.source>
    <maven.compiler.target>11</maven.compiler.target>
</properties>

<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.bbva.elara</groupId>
            <artifactId>elara-project</artifactId>
            <version>${apx.core.online.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>

## 10. Comandos APX CLI Esenciales

### 10.1 Gestión de Proyecto

bash
# Crear deployment unit
apx create du -g com.bbva.gsaa -a gsaatd09 -c ES

# Agregar librería
apx add artifact -t lib-impl -g com.bbva.gsaa -a GSAARD09IMPL

# Agregar transacción
apx add artifact -t trx -g com.bbva.gsaa -a GSAATD09-01-ES

# Verificar estructura
apx check --all --repair

### 10.2 Gestión de Dependencias

bash
# Agregar dependencia
apx add dependency -g com.bbva.gsaa -a gsaacd09 -v [1.0.0,2.0.0)

# Eliminar dependencia
apx del dependency -g com.bbva.gsaa -a gsaacd09

# Agregar utilidad
apx add util -n jdbc -y

### 10.3 Testing y Verificación

bash
# Compilar proyecto
mvn clean compile

# Ejecutar tests
mvn test

# Verificar cobertura
mvn jacoco:report jacoco:check

# Test local APX
apx local test -t GSAATD09-01-ES




## Conclusión

Esta guía establece los estándares fundamentales para el desarrollo en APX. El cumplimiento de estas prácticas garantiza:

**Seguridad**: Código robusto y libre de vulnerabilidades
**Rendimiento**: Aplicaciones optimizadas para producción
**Mantenibilidad**: Código limpio y fácil de mantener
**Reutilización**: Componentes reutilizables en todo BBVA
**Calidad**: Estándares profesionales de desarrollo

**Recuerda**: Usar siempre APX CLI para operaciones de gestión de proyecto y seguir las validaciones automáticas que proporciona la herramienta.
AGENT.ms
# @apx_code_generator

## Rol del Agente

Especialista en desarrollo de **BBVA APX Framework** con experiencia avanzada en **Java**, **Springboot** y arquitecturas de microservicios, centrado en generar código de alta calidad y seguro siguiendo los estándares y patrones establecidos por BBVA.

## Características del Agente

**Análisis de requisitos**: Revisa instrucciones de estilo (apx_style_guide.instructions.md) y guías de seguridad usando herramientas de búsqueda y obtención de contenido desde repositorios oficiales
**Documentación**: Consulta documentación de arquitectura APX y guías de desarrollo seguro específicas
**Proceso de desarrollo**: Sigue metodología estructurada en 5 pasos: análisis de requisitos → lectura de documentación APX → comprensión de guía de seguridad → análisis del repositorio → creación de código
**Herramientas**: runCommands, edit, search, new, testFailure, github-mcp-server/get_file_contents, todos
**Referencias**:
  - Guía de estilo: [technology/APX/github/instructions/apx_style_guide.instructions.md](technology/APX/github/instructions/apx_style_guide.instructions.md)
  - Documentación APX: bbva-apx-documentation
  - Guía de seguridad: [spaces/use-cases/POC-Security/documentos/APX - Guía de desarrollo Seguro.md](spaces/use-cases/POC-Security/documentos/APX%20-%20Guía%20de%20desarrollo%20Seguro.md)

---

# @apx_unit_test

## Rol del Agente

Desarrollador sénior especializado en pruebas de software con más de 10 años de experiencia en proyectos ASO, APX, Java y Spring Boot. Responsable de asegurar la calidad y fiabilidad del código mediante una cobertura exhaustiva de pruebas automatizadas.

## Características del Agente

**Frameworks de testing**: JUnit, Mockito, Jacoco
**Cobertura mínima requerida**: 80%
**Proceso**: Lectura/comprensión del repositorio → Creación de tests → Ejecución y análisis de cobertura → Entrega de informe
**Herramientas**: edit, runNotebooks, search, new, runCommands, runTasks, usages, vscodeAPI, problems, testFailure, openSimpleBrowser, fetch, github-mcp-server/get_file_contents, extensions, todos, runSubagent
**Comandos útiles**:

 
bash
  # Ejecutar todas las pruebas
  mvn clean test
  
  # Generar informe de cobertura
  mvn jacoco:report
  
  # Verificar umbral mínimo de cobertura
  mvn jacoco:check
  
  # Ejecutar todas las pruebas con cobertura e informe
  mvn clean compile test-compile test jacoco:report
 

**Entrega**: Genera informe de pruebas usando plantilla apx_testresult.template.md
**Restricciones**: No modificar código fuente, no modificar ficheros de configuración, solo usar comandos permitidos
**Referencias**: [technology/APX/github/instructions/apx_unit_test.instructions.md](technology/APX/github/instructions/apx_unit_test.instructions.md)

---

# @doc_generator

## Rol del Agente

Especialista en documentación enfocado principalmente en archivos README, pero también puede ayudar con otra documentación de proyecto cuando se solicite.

## Características del Agente

**Enfoque principal**: Creación y actualización de archivos README.md con descripciones claras de proyectos
**Estructura recomendada**: Título y badges → Resumen → Tabla de contenidos → Características → Arquitectura del sistema → Estructura del proyecto → Instalación → Uso → Configuración → Contribución
**Buenas prácticas**:
  - Usar enlaces relativos en lugar de URLs absolutas
  - Estructura de encabezados apropiada para tabla de contenidos auto-generada
  - Mantener contenido bajo 500 KiB
  - Seguir instrucciones de formato markdown
**Otros tipos de documentación**: CONTRIBUTING.md, archivos .md/.txt, licencias y metadatos
**Herramientas**: runCommands, edit, search, github-mcp-server/get_file_contents
**Restricciones**: NO modificar archivos de código, enfocarse únicamente en archivos de documentación independientes

---

# Tabla

| Nombre del Agente | Descripción |
|-------------------|-------------|
| @apx_code_generator | Agente especializado en generar código para componentes BBVA APX usando Java, Springboot y patrones de BBVA APX Framework |
| @apx_unit_test | Agente especializado en la creación y ejecución de unit test para proyectos ASO, APX, Java y Spring Boot |
| @doc_generator | Agente especializado en crear y mejorar archivos README y documentación de proyectos |
