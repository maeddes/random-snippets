Run mvn command for hello-world project

```
mvn io.quarkus:quarkus-maven-plugin:0.26.1:create \
    -DprojectGroupId=de.maeddes \
    -DprojectArtifactId=knative-hello \
    -DclassName="de.maeddes.GreetingResource" \
    -Dpath="/"
```

edit the source code to have 2 endpoints

```java
@Path("/")
public class GreetingResource {

    @GET
    @Path("hello")
    @Produces(MediaType.TEXT_PLAIN)
    public String hello() {
        return "Hello World!";
    }

    @GET
    @Path("fail")
    @Produces(MediaType.TEXT_PLAIN)
    public String fail() {
        System.exit(1);
        return "Hello World!";
    }
}
```

Run the code with `mvn compile quarkus:dev`

Edit the test class

```java
    @Test
    public void testHelloEndpoint() {
        given()
          .when().get("/hello")
          .then()
             .statusCode(200)
             .body(is("Hello World!"));
    }
```

* Docker

Pre-build
`mvn package -Pnative -Dnative-image.docker-build=true`
