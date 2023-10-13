<img src="https://github.com/jil1710/readmedemo/assets/125335932/123ae210-4eb3-466b-8395-348f41546036" width="250px" height="150px"/>

# Authentication And Authorization

- Authentication is the process of determining a user's identity.
- Authorization is the process of determining whether a user has access to a resource.
- In ASP.NET Core, authentication is handled by the authentication service, IAuthenticationService, which is used by authentication middleware. The authentication service uses registered authentication handlers to complete authentication-related actions. Examples of authentication-related actions include:
    - Authenticating a user.
    - Responding when an unauthenticated user tries to access a restricted resource.
 
## Cookie Authentication

- Cookie authentication in ASP.NET Core web application is the popular choice for developers to implement authentication in most customer-facing web applications and is also easy to implement in ASP.NET Core as it is provided out of the box without the need to reference any additional NuGet packages.
- ASP.NET Core provides a cookie authentication mechanism which on login serializes the user details in form of claims into an encrypted cookie and then sends this cookie back to the server on subsequent requests which gets validated to recreate the user object from claims and sets this user object in the HttpContext so that it is available & is valid only for that request.

- **How does cookie authentication in ASP.NET Core work?**

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/b219f13f-472b-4998-9b34-f726faeede42)

  - User requests URL from the server
  - Based on the access set server will either return the page if it’s a public page or will return the login page if its a secured page i.e. asking the user to login to access the secured page
  - User will perform login action & credentials will be sent to the server for validation
  - If provided credentials are valid then the server will set an authentication cookie in response.
  - Now all further requests from the user will carry this cookie in the header so that on each request server knows that the user is already authenticated & also user details can be read from cookie to identity request if from which user.

- **Let's Implement Cookie Authentication:**

  - First Create ASP.Net Core Project And Add LoginModel and User model for role base authorization:

    ![image](https://github.com/jil1710/readmedemo/assets/125335932/ea52c3c1-02a8-40d9-b6ed-ae6c0073dadd)

    ![image](https://github.com/jil1710/readmedemo/assets/125335932/c52c93b8-70e0-414d-8811-1c60cddd7956)

  - Now seed some user into List<User> or you can use database or identity framework to validate or storing of user. Here for i am using list of User static data as storage.

    ![image](https://github.com/jil1710/readmedemo/assets/125335932/a4a85237-21ab-40f6-920d-6f1f2579237d)

  - After that Cookie Authentication scheme and Authorization policy into program.cs file

    ![image](https://github.com/jil1710/readmedemo/assets/125335932/98fef953-8926-49d2-8a9a-c50a968dc116)

  - Add This two middleware in same order as below :

    ![image](https://github.com/jil1710/readmedemo/assets/125335932/991e8ad1-1881-4a46-81b4-a52af98c016a)

  - Add login api that validate the user and authenticate the user then create the session by storing the cookie.

    ![image](https://github.com/jil1710/readmedemo/assets/125335932/0dc20e57-bed6-46e2-bcae-15c627b4de7a)

    ![image](https://github.com/jil1710/readmedemo/assets/125335932/7bb81bd9-39df-4454-baf1-432102f682a5)


  - Then Create secret route that is only access by `Administrition` role and authenticated user.

    ![image](https://github.com/jil1710/readmedemo/assets/125335932/823ea762-c6f5-4a8d-8aba-0a3d1938bd5b)

# JWT and Multiple Scheme Authentication

- JSON Web Token is an open standard (RFC 7519) that defines a safe, compact, and self-contained, secured way for transmission of information between a sender and a receiver through a URL, a POST parameter, or inside the HTTP Header. It should be noted that the information to be transmitted securely between two parties is represented in JSON format and it is cryptographically signed to verify its authenticity. JWT is typically used for implementing authentication and authorization in Web applications. Because JWT is a standard, all JWTs are tokens but the reverse is not true. You can work with JSON Web Tokens in .NET, Python, Node.js, Java, PHP, Ruby, Go, JavaScript, etc.

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/830f1ed7-c025-48b6-b5fe-760709c2dc50)









  


