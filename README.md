# swagger-finatra
Add Swagger support for Finatra (1.6 and 2.1.1) web framework.

# Getting started
## Gradle
#### Add repository

	repositories {
	  maven { url "https://oss.sonatype.org/content/repositories/releases/" }
	}

#### Add Dependency

##### Scala 2.10, Finatra 2.1.1

	compile "com.github.xiaodongw:swagger-finatra2_2.10:0.4.2"

##### Scala 2.11, Finatra 2.1.1

	compile "com.github.xiaodongw:swagger-finatra2_2.11:0.4.2"

## SBT
	resolvers += "Sonatype OSS Snapshots" at "https://oss.sonatype.org/content/repositories/releases/"

#### Add Dependency

##### Finatra 2.1.1

    libraryDependencies += "com.github.xiaodongw" %% "swagger-finatra2" % "0.4.2"

## Add document information for you controller
    object SampleSwagger extends FinatraSwagger

    class SampleController extends Controller with SwaggerSupport {
      override val finatraSwagger: FinatraSwagger = SampleSwagger

      get("/students/:id",
        swagger { o =>
          o.summary("Read the detail information about the student")
          o.tags("Student")
          o.routeParam[String]("id", "the student id")
          o.response[Student](200, "the student details")
          o.response(404, "the student is not found")
        }) { request =>
        ...
      }

## Add document controller

##### Finatra 2.1.1
    object SampleApp extends HttpServer {
      SampleSwagger.registerInfo(
        description = "The Student / Course management API, this is a sample for swagger document generation",
        version = "1.0.1",
        title = "Student / Course Management API")

      override def configureHttp(router: HttpRouter) {
        router
          .add(new SwaggerController(finatraSwagger = SampleSwagger))
          ...
      }
    }
Swagger API document: ```http://localhost:8888/api-docs```

Swagger UI: ```http://localhost:8888/api-docs/ui```

# Finatra 1.6
Previous version of Finatra (1.6) is also supported, Check [here](finatra1.md) for the guide.
