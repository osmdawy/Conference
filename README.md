App Engine application for the Udacity training course.

## Products
- [App Engine][1]

## Language
- [Python][2]

## APIs
- [Google Cloud Endpoints][3]

## Setup Instructions
1. Update the value of `application` in `app.yaml` to the app ID you
   have registered in the App Engine admin console and would like to use to host
   your instance of this sample.
1. Update the values at the top of `settings.py` to
   reflect the respective client IDs you have registered in the
   [Developer Console][4].
1. Update the value of CLIENT_ID in `static/js/app.js` to the Web client ID
1. (Optional) Mark the configuration files as unchanged as follows:
   `$ git update-index --assume-unchanged app.yaml settings.py static/js/app.js`
1. Run the app with the devserver using `dev_appserver.py DIR`, and ensure it's running by visiting your local server's address (by default [localhost:8080][5].)
1. (Optional) Generate your client library(ies) with [the endpoints tool][6].
1. Deploy your application.

## Run Instructions
1. Run the application using your_app_id.appspot.com/_ah/api/explorer

## Sessions and Speaker implementation
### Speaker
I implemented speaker as a string property in the session model this refers to the speaker name this is not the best approach because I have not unique id for the speaker and I assumed the name is unique which is not correct but this was for simplicity for user to not create a speaker entry for each new speaker.
### Session
Session is a set of properties as required.
- for sessionName, highlights, speaker I used the string property.
- for duration I used time property to refer to duration as hours and minutes.
- for typeOfSession I used string property repeated to refer that session can have 2 types for example if session is about recruiting for software engineer it has "Recruiting" and "Engineering" as a type.
- for date I used date property because it's clearly refer to a date.
- for start time I used timeProperty to refer to hours and minutes.
- for conferenceId it's Integer so I used integer.
### Session Form
- It's the same as session except for date and time property I used a string also which in _createSession is converted to its type using
```python
datetime.strptime().time()
```
and
```python
datetime.strptime().date()
```
### Additional query
```python
getSessionsByTypeAndTime()
```
This query check for session is the given type and startTime is less than or equal the given time
this query can be very helpful is user is interested in certain type of sessions but for  a time suitable.
```python
getAllSessions()
```
This query returns all sessions created in the database this can be very helpful if user wants to know all sessions then decide which conference he/she should attend.
### Problem with provided query
The problem with provided query is that google cloud app engine doesn't support two inequalities for different properties so to solve this I implemented this method
```python
_getSessionsByTime()
```
which can be triggered using the endpoint
```python
getSessionsByNotTypeAndTime()
```
This method query the database first for inequality of the type then using python code check for the start time property.
To increase efficiency it's better if we know(in most cases) which query will return the smaller result so we start by this query.
In my example I assumed the first query returns smaller result.




[1]: https://developers.google.com/appengine
[2]: http://python.org
[3]: https://developers.google.com/appengine/docs/python/endpoints/
[4]: https://console.developers.google.com/
[5]: https://localhost:8080/
[6]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool
