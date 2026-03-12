Keycloak est une plateforme IAM (Identity and Access Management).
Son rôle est de gérer : authentification + autorisation + gestion des utilisateurs
Donc au lieu que l'application gère : login + mot de passe + tokens + sécurité . Keycloak s’en occupe 

Authentification
Gestion :login password MFA SSO

Autorisation
Gestion :roles permissions access policies Single Sign-On (SSO)

Exemple :login une seule fois
accès à plusieurs applications
Identity Federation

Connexion avec :Google Facebook LDAP Active Directory

Les concepts importants de Keycloak

**Realm : 
Un realm est un espace de sécurité.
Chaque realm possède : utilisateurs rôles clients

**Client : 
Un client représente une application.
Exemple :frontend app backend API mobile app

**User : 
Les utilisateurs authentifiés.

**Roles : 
Permissions associées aux utilisateurs.
Exemple : admin user manager

Protocoles supportés :
**OAuth2	authorization
**OpenID Connect	authentication
**SAML	enterprise SSO

