# BankLoan

# Loan
A Loan is a base class of any type of loan and it should not be able to be instantiated.
# Data
•	InterestRate – int
o	The amount a bank charges a borrower.
•	Amount - double
o	The amount of the loans offered by the bank.
# Constructor
A Loan should take the following values upon initialization: 
int interestRate, double amount

# Child Classes

There are two concrete types of Loan:
# StudentLoan
The student loan has an interest rate of 1 and an amount of 10 000.
Note: The Constructor should take no values upon initialization.
# MortgageLoan
The mortgage loan has an interest rate of 3 and an amount of 50 000.
Note: The Constructor should take no values upon initialization.

# Client
A Client is a base class of any type of client and it should not be able to be instantiated.
# Data
•	Name – string
o	If the Name is null or whitespace, throw a new ArgumentException with the message: 
"Client name cannot be null or empty."
•	Id - string
o	If the ID is null or whitespace, throw a new ArgumentException with the message: 
"Client’s ID cannot be null or empty."
•	Interest – int
o	The client’s interest. Be careful with the access modifier.
•	Income - double
o	The client’s income
o	If the income is below or equal to 0, throw an ArgumentException with the message:
"Income cannot be below or equal to 0."
# Behavior
void IncreaseInterest()
The IncreaseInterest() is an abstract method that increases the client’s interest. Keep in mind that the child classes of Client will implement the method differently.
# Constructor
A Client should take the following values upon initialization: 
string name, string id, int interest, double income

# Child Classes

There are several concrete types of Client:
# Student
Has an initial interest of 2 percent.
This class will be only accepted in combination with BranchBank. For more clarity, see the AddClient command in the business logic section of this document.
The constructor should take the following values upon initialization:
string name, string id, double income
# Behavior
void IncreaseInterest()
•	The method increases the client’s interest by 1 percent.
# Adult
Has an initial interest of 4 percent.
This class will be only accepted in combination with CentralBank. For more clarity, see the AddClient command in the business logic section of this document.
The constructor should take the following values upon initialization:
string name, string id, double income
# Behavior
void IncreaseInterest()
•	The method increases the client’s interest by 2 percent.


# Bank
Bank is a base class of any type of bank and it should not be able to be instantiated.
# Data
•	Name - string 
o	If the name is null or whitespace, throw an ArgumentException with the message: 
"Bank name cannot be null or empty."
o	All names are unique.
•	Capacity - int
o	The number of clients а Bank can have.
•	Loans - IReadOnlyCollection<ILoan>
•	Clients - IReadOnlyCollection<IClient>
# Behavior
double SumRates()
Returns the sum of the interest rates of each loan in the Bank.
void AddClient(IClient client)
Adds a Client in the Bank if there is a capacity for it.
If there is not enough capacity to add the Client to the Bank, throw an ArgumentException with the following message:
•	"Not enough capacity for this client."
void RemoveClient(IClient client)
Removes a Client from the Bank. It is important to note that you will always receive clients that have already been created within the application.
void AddLoan(ILoan loan)
Adds a Loan in the Bank.
String GetStatistics()
Returns a string with information about the Bank in the format below. 
"Name: {bankName}, Type: {bankTypeName}
Clients: {clientName1}, {clientName2} ... / Clients: none
Loans: {loansCount}, Sum of Rates: {sumOfInterestRates}"

# Constructor
A Bank should take the following values upon initialization: 
string name, int capacity

# Child Classes

There are 2 concrete types of Bank:
# BranchBank
Has 25 capacity.
The constructor should take the following values upon initialization:
string name 
# CentralBank
Has 50 capacity.
The constructor should take the following values upon initialization:
string name 


# LoanRepository
The LoanRepository is a repository for all the loans that are in the banks.
Data
•	Models - IReadOnlyCollection<ILoan>
Behavior
void AddModel(ILoan loan)
•	Adds a loan to the collection.
boolean RemoveModel(ILoan loan)
•	Removes a loan from the collection. Returns true if the deletion was successful, otherwise - false.
ILoan FirstModel(string name)
•	Returns the first loan of the given type, if there is any. Otherwise, returns null. Within the method implementation, you can access the name of the loan's type using "GetType().Name". This allows you to identify the loan type and perform the necessary logic to retrieve the first loan of that type.
# BankRepository
The BankRepository is a repository for all the banks that are created in the application.
Data
•	Models - IReadOnlyCollection<IBank>
Behavior
void AddModel(IBank bank)
•	Adds a bank to the collection.
boolean RemoveModel(IBank bank)
•	Removes a bank from the collection. Returns true if the deletion was successful, otherwise - false.
IBank FirstModel(string name)
•	Returns the first bank with the given name, if there is such bank. Otherwise, returns null.

# The Controller Class
The program’s business logic should be concentrated around several commands, which you have to implement in the correct class.
The interface is IController. You must create a Controller class, which implements the interface and implements all of its methods. The constructor of the Controller does not take any arguments. The given methods should have the logic described for each in the Commands section. When you create the Controller class, go into the Engine class constructor and uncomment the "this.controller = new Controller();" line.

# Data
You will need some private fields in your controller class:
•	loans - LoanRepository 
•	banks - BankRepository

# Commands
There are several commands, which control the business logic of the application. They are stated below.

# AddBank Command
# Parameters
•	bankTypeName - string
•	name - string
# Functionality
Creates a bank from the appropriate type and adds it to the BankRepository.
If the bankTypeName is an invalid type in the application, throw an ArgumentException with the following message:
•	"Invalid bank type."
If the Bank is added successfully, the method should return the following string:
•	"{bankTypeName} is successfully added."

# AddLoan Command
Parameters
•	loanTypeName - string
# Functionality
Creates a loan from the appropriate type and adds it to the LoanRepository. 
If the loanTypeName is an invalid type in the application, throw an ArgumentException with the following message:
•	"Invalid loan type."
If the Loan is added successfully, the method should return the following string:
•	"{loanTypeName} is successfully added."

# ReturnLoan Command
# Parameters
•	bankName - string
•	loanTypeName - string
# Functionality
Adds the appropriate ILoan, returned by a client, to the Bank with the given name. You have to remove the Loan from the LoanRepository if the insert is successful.
It is important to note that the bank referenced by the bankName parameter will always exist in the BankRepository. Therefore, you can assume that the specified bank is valid and present.
If there is no such loan, you have to throw an ArgumentException with the following message:
•	"Loan of type {loanTypeName} is missing."
If no exceptions are thrown, return the String:
•	"{loanTypeName} successfully added to {bankName}."

# AddClient Command
# Parameters
•	bankName - string
•	clientTypeName - string
•	clientName - string
•	id - string
•	income - double
# Functionality
•	If the given clientTypeName is not recognized as a valid type in the application, the method should throw an ArgumentException with the following message:
"Invalid client type."
•	Select from the BankRepository the bank with the given bankName.
o	If the given clientTypeName is NOT a valid client type for the selected bank, the following message is returned: 
"Unsuitable bank."
o	Otherwise creates and adds client from the appropriate type to the Bank with the given name. The following message should be returned: 
"{clientTypeName} successfully added to {bankName}."

# FinalCalculation Command
# Parameters
•	bankName - string
# Functionality
Calculates all funds that have passed through the Bank with the given name. It is calculated from the sum of all income from clients and amount from loans in the Bank.
Return a string in the following format:
•	"The funds of bank {bankName} are {funds}."
o	The funds should be formatted to the 2nd decimal place!

# Statistics Command
# Functionality
Returns information about each bank. You can use Bank's GetStatistics() method to implement the current functionality.
"Name: {bankName}, Type: {bankType}
Clients: {clientName1}, {clientName2} ... / Clients: none
Loans: {loansCount}, Sum of Rates: {sumOfInterestRates}
Name: {bankName}, Type: {bankType}
Clients: {clientName1}, {clientName2} ... / Clients: none
Loans: {loansCount}, Sum of Rates: {sumOfInterestRates}
..."

# End Command
Ends the program.
