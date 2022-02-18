# Provenance 

A common architecture used for data collection.

Provenance collects and stores articles from [infoq.com](https://www.infoq.com/) and follows
a Netflix [Conductor-esque](https://netflix.github.io/conductor/) architecture.

### History

In 2016 we began a project to address some of the concerns around fake news. 
While most were analyzing articles, we decided to take another approach; showing consumers
where their content comes from. Our goal was to bring source [journalist] context to
the foreground.

### The Exercise

Get the tests to pass!

- query the articles gateway
- map rss results to a collection
- start the background worker
- and deploy your application to Heroku

Look for todo items in the codebase for where to get started.

### Heroku

Sign up for a [Heroku account](https://signup.heroku.com/login).

## Quick start

### Source

Download the codebase.

Create a jar file without running tests.

```bash
./gradlew assemble
```

### Articles

Run the articles component tests to see what's failing.

```bash
./gradlew :components:articles:test
```

Review the *TODO* comments in the `ArticlesController` class and get the tests to pass.
Along the way it will be helpful to use the `writeJsonBody` method to convert articles to json.

```java
writeJsonBody(servletResponse, articles);
```

### Endpoints

Run the endpoints component tests to see what's failing. 

```bash
./gradlew :components:endpoints:test  
```

Review the *TODO* comments in the EndpointWorker class and get the tests to pass.
Along the way it will be helpful to use `XmlMapper` to convert RSS feeds to Java objects.

```java
RSS rss = new XmlMapper().readValue(response, RSS.class);
```

### Test suite

Ensure all the tests pass.

```bash
./gradlew build
```

### Schedule work

Review *TODO* comments in the `App` class within the provenance-server component.
Create and start a `WorkScheduler`.

```java
WorkScheduler<EndpointTask> scheduler = new WorkScheduler<>(finder, workers, 300);
``` 

_Pro tip:_ review the `testScheduler` test in the `WorkSchedulerTest` class.

### Run locally

Build the application again then run it locally to ensure that the endpoint worker is collecting articles.

```bash
./gradlew build
java -jar applications/provenance-server/build/libs/provenance-server-1.0-SNAPSHOT.jar 
```

Make a request for all articles in another terminal window.
 
```bash
curl -H "Accept: application/json" http://localhost:8881/articles
```

### Deploy

[Create a new Heroku app](https://dashboard.heroku.com/new-app) and
deploy your application to Heroku.

Hope you enjoy the exercise!
