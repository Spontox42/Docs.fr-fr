## <a name="multiple-authentication-providers"></a><span data-ttu-id="78f36-101">Plusieurs fournisseurs d’authentification</span><span class="sxs-lookup"><span data-stu-id="78f36-101">Multiple authentication providers</span></span>

<span data-ttu-id="78f36-102">Lorsque l’application nécessite plusieurs fournisseurs, chaînez les méthodes d’extension de fournisseur derrière [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication) :</span><span class="sxs-lookup"><span data-stu-id="78f36-102">When the app requires multiple providers, chain the provider extension methods behind [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication):</span></span>

```csharp
services.AddAuthentication()
    .AddMicrosoftAccount(microsoftOptions => { ... })
    .AddGoogle(googleOptions => { ... })
    .AddTwitter(twitterOptions => { ... })
    .AddFacebook(facebookOptions => { ... });
```