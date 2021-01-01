# Hosting Services Series: Laying out architecture (Part-2)

*A.k.a* the plan for hosting cloud services we all frequently depend upon.

![High-level architecture of cloud](https://cloud.fossterer.com/apps/files_sharing/publicpreview/jGcxa72kHR8P8pp?x=1848&y=600&a=true&file=IMG_20210101_114720.jpg&scalingup=0)

The independent microservices

- auth
- notes
- polls
- calendar
- mail
- contacts
- url-shortener

would be behind a reverse-proxying Nginx.

Further each microservice lives in its own Git repository in the following tentative form:

<dl>
<dt>Repo name:</dt> <dd>auth.fossterer.com</dd>
<dt>Directories:</dt> <dd>
<ol>
<li>ui</li>
<li> service</li>
<li> db</li>
</ol>
</dd>
<dt>Package names</dt> <dd> com.fossterer.auth, com.fossterer.calendar, com.fossterer.notes and so on. </dd>
</dl>

Soon enough, every such *service* gets to be stood up as a Docker container such that thereafter,

1. they can be *run* and *killed* on their own
2. a simple *Nginx* server block (or the equivalents in other reverse-proxying servers) can be setup to proxy dedicated domain names to these localhost URLs

![The implementation plan](https://cloud.fossterer.com/apps/files_sharing/publicpreview/DdQ56mjm9adZakq?x=1848&y=600&a=true&file=IMG_20210101_114811__01.jpg&scalingup=0)

The plan is to move my <cloud.fossterer.com> users away from existing Nextcloud apps one by one as I build secure and usable microservices that can live on their own.

## Benefits

- New lean services can be built and old ones can be killed at any time where users don't have to face any downtime
- Users get access to *Beta* services early so they can give them a try and provde early feedback during development itself
- All features come with the same Single-Sign On (SSO)

![Register/Login index.html](https://cloud.fossterer.com/apps/files_sharing/publicpreview/M6Y9Gwc5QetTQdg?x=1848&y=631&a=true&file=IMG_20210101_114915__01.jpg&scalingup=0)

The Register/Login page in the intial stages allows the following mechanisms:

- Username/Password

Soon, the following methods would be added

- WebAuthn
- OAuth2/OpenID Connect for Github, Google and StackExchange Logins
