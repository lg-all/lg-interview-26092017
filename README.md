# lg-interview-23062017

# lg-interview-23062017

Create simple service for keeping elearning course status data. It has to be able to accept
entries about user progress (in json format), store them (somehow) and allow to retrieve some data from it.

User status is sum of whole user interactions with course (sessions)
Session begins with init request.
In session there may be multiple save requests with score change.
At the end of the session there is finish request.
To simplify you should assume that there will always be single start and single end request.

Course score inside session is overridden with every new not empty score provided.

1. Functional requirements:
- application has to be able to accept under POST endpoint **/status/course** request with following json in body
```json
{
    "userId"    :   "some_string_with_user_id",
    "courseId"  :   "some_string_with",
    "type"      :   "type of request (init, save or finish)" ,
    "score"     :   "optional int parameter with user score",
    "timestamp" :   "ISO-8601 date time when request was issued"
}
```

- dates are and should be specified in format ISO-8601 calendar system,
 eg. `2017-12-03T10:15:30+01:00 Europe/Warsaw`

- application should allow getting current state of course session.
`GET /session/user/{userId}/course/{coursesId}/state` should return current state

```json
{
    "started_date"          : "ISO-8601 date when course was started first time",
    "total_session_time"    : "time (in seconds) of all users session",
    "end_date"              : "ISO-8601 date when course was finished last time",
    "best_score"            : "best of session end scores",
    "average_score"         : "average of session end scores"
}
```

- assure that at most 5 request can be stored at the same time (some "throttling" mechanism)

2. Nonfunctional requirements:
- application has to be written in any JVM language (Java 8 is preferable but scala,
kotlin or groovy are also allowed)
- we do not enforce any web framework (Spring Boot, Dropwizard, Spark, Ratpack - whatever will suit you)
- you can use any persistence method (in-memory, sqllite or something else) but it should not require
setting up separate database outside of project.
- code should be properly tested
- service dependencies and lifecycle should be managed by proper tool (maven, gradle, sbt)

