= Samples

[[messages-sample]]
== Messages Sample

The messages sample integrates `spring-security-oauth2-client` and `spring-security-oauth2-resource-server` with *Spring
Authorization Server*.

The username is `user1` and the password is `password`.

[[run-messages-sample]]
=== Run the Sample

* Run Authorization
  Server -> `./gradlew -b samples/default-authorizationserver/samples-default-authorizationserver.gradle bootRun`
* Run Resource Server -> `./gradlew -b samples/messages-resource/samples-messages-resource.gradle bootRun`
* Run Client -> `./gradlew -b samples/messages-client/samples-messages-client.gradle bootRun`
* Go to `http://127.0.0.1:8080`

[[federated-identity-sample]]
== Federated Identity Sample

The federated identity sample builds on the messages sample above, adding social login and federated identity features
to *Spring Authorization Server* using custom configuration.

[[google-login]]
=== Login with Google

This section shows how to configure Spring Security using Google as an Authentication Provider.

[[google-initial-setup]]
==== Initial setup

To use Google's OAuth 2.0 authentication system for login, you must set up a project in the Google API Console to obtain
OAuth 2.0 credentials.

NOTE: https://developers.google.com/identity/protocols/OpenIDConnect[Google's OAuth 2.0 implementation] for
authentication conforms to the
https://openid.net/connect/[OpenID Connect 1.0] specification and is https://openid.net/certification/[OpenID
Certified].

Follow the instructions on the https://developers.google.com/identity/protocols/OpenIDConnect[OpenID Connect] page,
starting in the section, "Setting up OAuth 2.0".

After completing the "Obtain OAuth 2.0 credentials" instructions, you should have a new OAuth Client with credentials
consisting of a Client ID and a Client Secret.

[[google-redirect-uri]]
==== Setting the redirect URI

The redirect URI is the path in the application that the end-user's user-agent is redirected back to after they have
authenticated with Google
and have granted access to the OAuth Client _(created in the previous step)_ on the Consent page.

In the "Set a redirect URI" sub-section, ensure that the *Authorized redirect URIs* field is set
to `http://localhost:9000/login/oauth2/code/google-idp`.

TIP: The default redirect URI template is `{baseUrl}/login/oauth2/code/{registrationId}`.
The *_registrationId_* is a unique identifier for the `ClientRegistration`.

[[google-application-config]]
==== Configure application.yml

Now that you have a new OAuth Client with Google, you need to configure the application to use the OAuth Client for the
_authentication flow_. To do so:

. Go to `application.yml` and set the following configuration:

+

[source,yaml]
----
spring:
security:
oauth2:
client:
registration:    <1>
google-idp:    <2>
provider: google
client-id: google-client-id
client-secret: google-client-secret
----

+

.OAuth Client properties
====
<1> `spring.security.oauth2.client.registration` is the base property prefix for OAuth Client properties.
<2> Following the base property prefix is the ID for the `ClientRegistration`, such as google-idp.
====

. Replace the values in the `client-id` and `client-secret` property with the OAuth 2.0 credentials you created earlier.
Alternatively, you can set the following environment variables in the Spring Boot application:

* `GOOGLE_CLIENT_ID`
* `GOOGLE_CLIENT_SECRET`

[[github-login]]
=== Login with GitHub

This section shows how to configure Spring Security using Github as an Authentication Provider.

[[github-register-application]]
==== Register OAuth application

To use GitHub's OAuth 2.0 authentication system for login, you
must https://github.com/settings/applications/new[Register a new OAuth application].

When registering the OAuth application, ensure the *Authorization callback URL* is set
to `http://localhost:9000/login/oauth2/code/github-idp`.

The Authorization callback URL (redirect URI) is the path in the application that the end-user's user-agent is
redirected back to after they have authenticated with GitHub
and have granted access to the OAuth application on the _Authorize application_ page.

TIP: The default redirect URI template is `{baseUrl}/login/oauth2/code/{registrationId}`.
The *_registrationId_* is a unique identifier for the `ClientRegistration`.

[[github-application-config]]
==== Configure application.yml

Now that you have a new OAuth application with GitHub, you need to configure the application to use the OAuth
application for the _authentication flow_. To do so:

. Go to `application.yml` and set the following configuration:

+

[source,yaml]
----
spring:
security:
oauth2:
client:
registration:    <1>
github-idp:    <2>
provider: github
client-id: github-client-id
client-secret: github-client-secret
----

+

.OAuth Client properties
====
<1> `spring.security.oauth2.client.registration` is the base property prefix for OAuth Client properties.
<2> Following the base property prefix is the ID for the `ClientRegistration`, such as github-idp.
====

. Replace the values in the `client-id` and `client-secret` property with the OAuth 2.0 credentials you created earlier.
Alternatively, you can set the following environment variables in the Spring Boot application:

* `GITHUB_CLIENT_ID`
* `GITHUB_CLIENT_SECRET`

[[run-federated-identity-sample]]
=== Run the Sample

* Run Authorization
  Server -> `./gradlew -b samples/federated-identity-authorizationserver/samples-federated-identity-authorizationserver.gradle bootRun`
* Run Resource Server -> `./gradlew -b samples/messages-resource/samples-messages-resource.gradle bootRun`
* Run Client -> `./gradlew -b samples/messages-client/samples-messages-client.gradle bootRun`
* Go to `http://127.0.0.1:8080`