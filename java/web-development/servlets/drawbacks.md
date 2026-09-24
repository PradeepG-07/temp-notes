## Conclusion

If we observe carefully some of the **drawbacks** we can observe are:
1. External tomcat has to be configured properly and deployment requires **copying and pasting jar files** from build directories to tomcat.
2. No dependency injection leading to **tight coupling**.
3. **Route handling is difficult** when different routes are required and ends up having multiple servlets which handles only some HTTP methods.
	1. Example if I want APIs like `/users/create`, `/users/delete`, `/users/get-user`etc. These needs to be handled separately in each class. 
4. **JSON parsing and mapping** it to java classes is to be handled manually.
5. There is **no central place** to handle exceptions.
6. **Repetitive logic** like creating response, performing validations on requests, mapping JSON to java objects, etc. would increase size of controllers.
7. **Database integration** requires handling the db connections, **transactions manually**.