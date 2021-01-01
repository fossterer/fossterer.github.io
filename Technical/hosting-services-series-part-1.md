# Hosting Services Series: Choosing an Authentication/Authorization approach for my services (Part-1)

*A.k.a* why not DIY authentication for my services.

As I am gearing up to roll my own services to learn the pain points of managing such services for my users before I release them, I started wondering how authentication and authorization are to be handled.

## MITM attacks

As a simple case, I wondered why can't I just maintain a username/password database on the server? Of course, all my services use **HTTPS* so that the password transfer over the network is secure already. That covers against [Main in The Middle (MITM) attacks](https://en.wikipedia.org/wiki/Man-in-the-middle_attack). Am I missing anything else?

## CSRF attacks

First line of thought was already discussed in [this question](https://security.stackexchange.com/questions/55966/whats-wrong-with-my-own-authentication-scheme) by the OP. Observe that the OP (1) never chose an answer as 'accepted' and (2) kept dilligently responding to concerns in comments with inputs on how the mentioned concerns are addressed. While it is clear that none of the responses promptly answered the question, it brought up the concern of **Croos-site Request Forgery attacks* where any bad actor can trick a logged-in user of my services into performing a request without their express intent to do so.

To counter this, the service should supply a anti-CSRF token for every POST/PUT/DELETE (*i.e.* state-changing) requests. Note however that the concern of CSRF is still orthogonal to the password storage issue.

One more point brought up is *salting*. My original idea ignored it but I can pretty much leverage [Bcrypt](https://en.wikipedia.org/wiki/Bcrypt#Criticisms).

## Password salting

Supposedly [BCrypt](https://en.wikipedia.org/wiki/Bcrypt) is good enough, once I take care of the [maximum length limitation](https://en.wikipedia.org/wiki/Bcrypt#Password_hash_truncation). I only need to find a good library that handles these limitations already.

Wait?! Did I just commit to using a library while the whole point is to do my own authentication system instead of using an existing implementation?

## Naive users

Ok. So if I salt the password appropriately, it should be safe to store. Right? This simple [xkcd](https://xkcd.com/792/) offers a weak point in this approach. If users of my service tend to use the same password on multiple (unrelated to my) services, if the database (mine or othe others') gets leaked, then it is bad. [This response](https://security.stackexchange.com/a/52065/56124) nails in explaining why "just encrypting" is not enough.

A resourceful bad actor can attempt multiple common password and expose my hashing technique!

Earlier discussed BCrypt in fact does multiple rounds of hashing just to tire out the bad actor and/or their resources. Why reinvent the wheel myself with the added concern that users of my service could not be benefetting from industry-standard best practices?

At this point, implementing a random salt for every password seems to be making me move far from my original "service development" task to "authentication implementation" sub-task.

## Session Maintenance

Of course an after-problem to the problem of building my own authentication system, I need my logged-in users to continue to stay logged in until they expressly choose to logout. How to do that? I know of the flow `Supply username/password (client) --> authentication (server) --> return a session token (server)` whereafter the onus is on the client to provide the token with every subsequent request. Of course, now I am more aware of the need to include an additional CSRF token where applicable but I digress. **JSON Web Tokens (JWT)** are the popular choice for such token mechanism.

## So?

The resolution is:

- that during *Registration*, my services leverage **BCrypt** to store passwords as offered by [Spring Security](https://stackoverflow.com/questions/57523178/bcrypt-in-my-spring-security-project-spring-security) and
- that during subsequent *Login*, my **auth-service** provides a **JWT** to the user so they can acccess other services offered via *Claims*

But, but.. I still want to develop my own auth system!
![Batman Slaps Robin](../images/hosting-services-series/batman-slaps-robin.jpeg)

## Further Reading

1. <https://security.stackexchange.com/questions/18197/why-shouldnt-we-roll-our-own>
2. <https://security.stackexchange.com/questions/2202/lessons-learned-and-misconceptions-regarding-encryption-and-cryptology>
