Validation :-
* It is a process of validating data in which the value entered are checked if the correct values are entered or not.
* It checks if the value is in correct Data type or not.
* There are four types of validation ->
    * Type Validation :- It is a type of validation in which the Data type is checked if the data is in format in which it is being asked. Ex -> Value asked = Integer and Value entered = String, so here the type validation check and throws error.
    * Semantic Validation -> In this type of validation the logic of the data is check like the age of a human being cannot be 145 yo because the maximum human life span is 120 yrs.
    * Syntatic Validation -> In this validation type the syntax of the value is checked if it is email or not.
    * Cross-Field Validation -> In this type fo validation the data is check if the same data is entered in the previous field which are interconnected to each other. Ex -> Suppose you create a new password and then you enter re-type password and the password is not matching so this is cross-field validation.


Where Validation Happens :-
* There is not a single predefined place where the validation will take place but it is distributed in many layers ->
    * Controller Layer -> In this layer the Type validation is takes place like string length, data type and basic formats. It is here to reject or fail fast if malformed data is entered and don’t waste time on the server side.
    * Service Layer -> In this layer most of the conditional statement, bussiness logic and checks if the email or item in stock exists or not.
    * Database Layer -> It is the final line of defence for your application’s data integrity and check if the final state of data is legal and unbroken. It checks the overall validations if an attcker somehow skipped the application layer.
