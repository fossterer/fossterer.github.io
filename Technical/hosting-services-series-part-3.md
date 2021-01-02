# Hosting Services Series: Securing UI and API Layers (Part-3)

How can I secure the infrastructure both at teh presentation as well as service layers?

An API-first strategy enables right from he start of the project, allowing multiple customer-specific presentation layers, [Backend-For-Frontend](https://docs.microsoft.com/en-us/azure/architecture/patterns/backends-for-frontends) patterns. Providing a UI first and opening up APIs to the public as an after thought is one way of architecting services but I take the rival approach.

## The problem

As discussed in [Part-1](hosting-services-series-part-1.md) of the series, a bad actor can craft a UI similar to my the one I offer and trick a user of my services to enter their valid credentials and initiate a session. Thought with the `same-site` and `http-only` restrictions on cookies and the nature of browsers not letting unrelated domains access a cookie, there is the problem of the actor replaying state-changing (for eg. `DELETE`) but valid requests to any of my services.

While `CSRF-Tokens` help over the forms in UI, as I keep the API (Service Layer) open, there seems to be a possibility that the cookies could be passed on by the browser to this API layer which can do actions unintended by the actual user.

## Readings

Several responses [here](https://stackoverflow.com/questions/10741339/do-csrf-attacks-apply-to-apis#10741650) express different viewpoints on how it is not relevant to certain applications, how it indeed is applicable and so on. What seems to be of use further is the **Double Submit Cookie** approach presented under a different name in this [response](https://stackoverflow.com/a/63132364).

## Way forward

### Immediate

Since we are going an *API-first* way anyway, the immediate need is to allow registrations via API indeed i.e

1. User registers via API and Password to obtain a JWT
   1. When JWT expires, a user has to log in again
2. User can conitnue to use it to authenticate for other purposes and may ultimately logout (or JWT expires)
3. User logs in with previously chosen *username/password* combination to obtain a new JWT *i.e* a new nonce for the purposes thereafter until logging out again
   1. When JWT expires, a user has to log in again

That way, an API user is the only one who has a way to access his resources.

#### Threat Model

- Whoever gets access to the JWT can access the user's resources until the token expires.
  - Once expired, the bad actor can neither re-login nor refresh the token
  - By this, we ensure that the user's password is not in transit for long
- Nothing else known at this time

### Future

As we implement *UI*, we start using Cookies to store these JWTs i.e

1. User registers/logs in via UI to obtain a JWT
   1. When JWT expires, a user has to log in again
2. User can conitnue to use it to authenticate for other purposes until JWT expires (or may ultimately logout(*How?*)
3. User can use the same JWT to access the API layer directly too

That way, the UI user is the only one who has a way to access his resources via UI.

#### To Do

Using another domain, demonstrate the possibility of (1) XSS atacks and (2) CSRF attacks over cookies and read the resources mentioned in Readings section](#Readings)

#### Threat Model

- Whoever gets access to the JWT can access the user's resources until the token expires.
  - The access can be via API which is unknown to the user yet is a valid route
  - Once expired, the bad actor can neither re-login nor refresh the token
- Nothing else known at this time
