6. Validation and Transformation for Backend Engineers.

Validation :-
* It is a process of validating data in which the value entered are checked if the correct values are entered or not.
* It checks if the value is in correct Data type or not.
* There are four types of validation ->
    * Type Validation :- It is a type of validation in which the Data type is checked if the data is in format in which it is being asked. Ex -> Value asked = Integer and Value entered = String, so here the type validation check and throws error.
    * Semantic Validation -> In this type of validation the logic of the data is check like the age of a human being cannot be 145 yo because the maximum human life span is 120 yrs.
    * Syntatic Validation -> In this validation type the syntax of the value is checked if it is email or not.
    * Cross-Field Validation -> In this type fo validation the data is check if the same data is entered in the previous field which are interconnected to each other. Ex -> Suppose you create a new password and then you enter re-type password and the password is not matching so this is cross-field validation.

Transformation :-
* Transformation is the process in which the data entered in the field  is modified so that it can match the form of the data that the backend expects.
* For Example -> If you enter you age in query param are recieved as string by default and then modified in the Integer like {“23” -> 23}.

Where Validation Happens :-
* There is not a single predefined place where the validation will take place but it is distributed in many layers ->
    * Controller Layer -> In this layer the Type validation is takes place like string length, data type and basic formats. It is here to reject or fail fast if malformed data is entered and don’t waste time on the server side.
    * Service Layer -> In this layer most of the conditional statement, bussiness logic and checks if the email or item in stock exists or not.
    * Database Layer -> It is the final line of defence for your application’s data integrity and check if the final state of data is legal and unbroken. It checks the overall validations if an attcker somehow skipped the application layer.

What data can be Validated :-
* Almost any type of input data can be validated and ensures clean, correct and more efficient data handling.
* Common data that can be validated are :-
    * Intergers or String.
    * Numeric Value.
    * Date and time.
    * Contact information.
    * Unique id validation.

Validation Faliure (400 Bad Request) :-
* A 400 Bad Request (Validation faliure) error occurs when the server refused to process your request because the data sent failed to meet the requirement, syntax or constraints. it is a Client-Side Error.
* It occurs because malformed JSON, Invalid queries and syntactical error.

Validation and Transformation Piplines :-
* Validation and Transformation Piplines checks the correctness of the data and transforms it into a usable format for the Storage layer to proceed the response.
* It includes validation, data transformation and post-transformation validation etc.

Frontend and Backend Validation :-
* Both of these Frontend and backend validation is essential and they both serve entirelly different purpose. The absolute golden rule is to never trust frontend for security jobs because it can be exploited by many things that developers tools and it is running on the client’s browser so he can send malicious packets with curl and other.
* Frontend Validation -> 
    * Primary purpose is Excellent user experience UX.
    * Reduces loads by blocking bad request early.
    * Bypassable.
    * Speed is instantenous/ Immidiate Feedbacks.
    * Optional but highly recomended for better user experince.
* Backend Validation  -> 
    * Primary purpose is security data integrity and bussiness rules.
    * It has increased load.
    * Not bypassable.
    * Slower because requires internet for a round trip of servers.
    * Mandatory.
