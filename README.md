
# State design pattern

- According to Gang of Four Definitions, a State Design Pattern allows an object to alter its behavior when its internal state changes. In simple words, we can say that the State Design Pattern allows an object to change its behavior depending on its current internal state completely.
  
- The State Design Pattern allows an object to change its behavior when its internal state changes. This pattern is particularly useful when an object must go through several states, and its behavior differs for each state. Instead of having conditional statements scattered throughout a class to handle state-specific behaviors, the State Design Pattern delegates this responsibility to individual state classes.
  
  
## Let's implement the real world example in C#

- This real world code demonstrates the State pattern which allows an Account to behave differently depending on its balance. The difference in behavior is delegated to State objects called RedState, SilverState and GoldState. These states represent overdrawn accounts, starter accounts, and accounts in good standing.


- Create abstract class State that define Account properties and methods.

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/2586712c-d99e-4a69-8af1-426c79433ebd)

- Create State for our Account if balance is less than 0 then account is in RedState, if account balance is up to 1000 then it is in SilverState and balance is greater than 1000 then it is in GoldenState. So for that we crreate this three state and implement with State abstract class.

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/49cebf58-5c37-448e-a90a-aa581a5cd37c)

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/cf69cf84-19f6-4110-b886-f23a412aed16)

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/b9380fe3-ba7a-4ff3-9486-7c3b76c400c7)


- Create Account class to perform transaction

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/08902736-4f64-4313-9e7f-6f641200da29)

- Client Side

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/0f95496a-0527-4e77-8291-72b1e712b7e1)

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/80dc8013-0740-4953-b338-8e2bf7c7f3a1)














