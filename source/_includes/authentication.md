# Authentication

## /get-token

_`POST https://api.vivacitylabs.com/get-token`_

> Replace `USERNAME` and `PASSWORD` with the username and password given to you by your Vivacity Project Manager.

```shell
# Please note this is not a specific api v3 request
curl -X POST "https://api.vivacitylabs.com/get-token"
    -H "Content-Type: application/json"
    -d '{"username":"USERNAME","password":"PASSWORD"}'
```

> This returns your access and refresh token in the following json format:

```json
{
    "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJxOXc2c3E1emViZVktaFdBQ2FEVkNGRjNnTl9LNWZQblNlbEFJemUwU3IwIn0.eyJqdGkiOiIyYjAxODUzZC01MDhjLTQ1OGQtYjlhMi0zYTBmNjE3ZjRmZmUiLCJleHAiOjE2Mjk0NjgyNTQsIm5iZiI6MCwiaWF0IjoxNjI5NDY3NjU0LCJpc3MiOiJodHRwOi8va2V5Y2xvYWsuY2hpaHVhaHVhOjgwODAvYXV0aC9yZWFsbXMvc2Vuc29yLWRhdGEiLCJzdWIiOiJmZTEwMjQwZC01MTIzLTRjYzUtYmMxZi1iNjMyOWI2YWQ4ZDMiLCJ0eXAiOiJCZWFyZXIiLCJhenAiOiJzZW5zb3ItZGF0YS1jbGllbnQiLCJhdXRoX3RpbWUiOjAsInNlc3Npb25fc3RhdGUiOiIwMzdmMmJkYy1mYzdmLTQ3MzgtYmM2MC04MzZkMDQ5ZDgyMGIiLCJhY3IiOiIxIiwic2NvcGUiOiJwcm9maWxlIiwicHJlZmVycmVkX3VzZXJuYW1lIjoiZGVtby11c2VyIn0.gWBaqQBtJuXFeVJ_2EQTQHcZ1Uq5afG1QWTeMDq6WK7xwNCNeFcoBOMPJ7aw9zSE2xsK9H9p0vNlWxZsT1i8tEpb0LCan-dy6Ag19szUlJ8UOpH4Hv-Q09sIBOXhpVX9hWObkVYhXM9Q1DmlwuZXpNmbUs_20Eq8BIgxjVHJq489EaHh8EDm26tkU8DbLTzbR4PLG5f1vHZAutoH8g1q8mXmCiiW9A1bLL_nTvl0sGqrH1g0bkJBEu-bLncb0tjlDadtmMEbtfP3cXBXqhhfTzFFoJn-SWkAEDhV0KX05rs9FPRuwwvc5klyJLiBNNFVDBweAdZGIrqoePSvVl2oFA",
    "expires_in": 600,
    "refresh_expires_in": 1800,
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIzZTgxMGM2ZC00YmJjLTQ0MzQtYTM0MC0zYmVlZDYyNmFjNTgifQ.eyJqdGkiOiIxZWE2NGVjMS01ZThiLTQxYmItYjM2OC0yZGEwMmExMzJmYzMiLCJleHAiOjE2Mjk0Njk0NTQsIm5iZiI6MCwiaWF0IjoxNjI5NDY3NjU0LCJpc3MiOiJodHRwOi8va2V5Y2xvYWsuY2hpaHVhaHVhOjgwODAvYXV0aC9yZWFsbXMvc2Vuc29yLWRhdGEiLCJhdWQiOiJodHRwOi8va2V5Y2xvYWsuY2hpaHVhaHVhOjgwODAvYXV0aC9yZWFsbXMvc2Vuc29yLWRhdGEiLCJzdWIiOiJmZTEwMjQwZC01MTIzLTRjYzUtYmMxZi1iNjMyOWI2YWQ4ZDMiLCJ0eXAiOiJSZWZyZXNoIiwiYXpwIjoic2Vuc29yLWRhdGEtY2xpZW50IiwiYXV0aF90aW1lIjowLCJzZXNzaW9uX3N0YXRlIjoiMDM3ZjJiZGMtZmM3Zi00NzM4LWJjNjAtODM2ZDA0OWQ4MjBiIiwic2NvcGUiOiJwcm9maWxlIn0.vr9KD8O9MPgxkh-AAkwbOsCMlfMytmhsioEb6WJN2RU",
    "token_type": "bearer",
    "not-before-policy": 0,
    "session_state": "037f2bdc-fc7f-4738-bc60-836d049d820b",
    "scope": "profile"
}
```

> The example below shows how to use the `access_token` to authenticate an api request:

```shell
curl -X GET "https://api.vivacitylabs.com/v3/hardware/metadata"
  -H "Authorization: Bearer ACCESS_TOKEN"
```

To authorize, first you must query our `/get-token` endpoint for an access token. This will require your account username and password which should be added to the body of the request in the format displayed on the right. If you do not know you username or password please contact your Vivacity project manager.

<aside class="notice">
Note this endpoint is not a version specific endpoint so does not contain <code>v3</code> in the path. <code>https://api.vivacitylabs.com/get-token</code>
</aside>

The `/get-token` endpoint returns both an access token and a refresh token along with details about their expiration times. As you can see from the `token_type` these token are Bearer tokens. To use these to authorize your `v3` api requests you must attach it to the request via the `Authorization Header` in the format `Bearer ACCESS_TOKEN`.

If you are using postman this may look like this:

<video controls>
  <source src="slate/img/movies/postman-authentication.mp4" type="video/mp4">
</video>

<aside class="notice">
If your api requests begin being denied with the returned error of <code>forbidden</code> it is likely that your access token has expired. You will either need to request a new one using <code>/get-token</code> or refresh your expired one using <code>/refresh-token</code>.
</aside>

## /refresh-token

_`POST https://api.vivacitylabs.com/refresh-token`_

TODO - description and code examples
