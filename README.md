# ETLC_SecureDB

## Synopsis

This Apex class will help you ensure the Lightning components applications/components that you write to access data from Salesforce are always enforcing the user's CRUD, FLS and Record Access (Sharing model) security.

## Motivation

As indicated in the official [Lightning Security](https://developer.salesforce.com/page/Lightning_Security) article:

> CRUD/FLS permissions are not automatically enforced in Lightning components or controllers, nor can you rely on Lightning components to enforce security.

Therefore you must explicitly check for CRUD and FLS when performing any database operations on your records.

## Installation

Copy the Apex classes (*ETLC_SecuredDB*, *ETLC_SecuredDB_Test*, *ETLC_Exception*) to your ORG, which can be done automatically with this button:

<a href="https://githubsfdeploy.herokuapp.com?owner=ElToroIT&repo=ETLC_SecureDB">
  <img alt="Deploy to Salesforce" src="https://raw.githubusercontent.com/afawcett/githubsfdeploy/master/deploy.png">
</a>

## API Reference

### ETLC_SecuredDB
This Apex class helps you make secured queries and DML operations enforcing the user's CRUD, FLS and Record Access security. It has these public methods:

**public static List&lt;sObject&gt; query(String SOQL)**
This method allows you to pass a SOQL string, which will be executed following the user’s  CRUD, FLS and Record Access security. This method uses Dynamic SOQL to fetch the data. If the security is violated, it will throw an exception.

**public static void validateQuery(sObject dbRecord)**
This method checks if the current user’s CRUD, FLS and Record Access security allows them to see this record and fields. You can use this check if you have already fetched the record or if you do not want to use Dynamic SOQL. If the security is violated, it will throw an exception.

**public static void validateQuery(List<sObject> dbRecords)**
This method is similar to the previous method, but works with a list of records. If the security is violated, it will throw an exception.

**public static void performDML(Operation op, sObject dbRecord)**
This method performs the desired DML operation after it has checked that the user’s CRUD, FLS and Record Access security will allow him/her to perform such operation. If the security is violated, it will throw an exception.

**public static void performDML(Operation op, List&lt;sObject&gt; dbRecords)**
This method is similar to the previous method, but works with a list of records. If the security is violated, it will throw an exception.

**public static String getFieldsForPlainValidator(sObject dbRecord)**
Returns a string representing the object/fields structure for this record. It's used to make the code more efficiently by not having to perform the whole check every time. 

**public static String getFieldsForPlainValidator(List&lt;sObject&gt; dbRecords)**
This method is similar to the previous method, but works with a list of records. 

**public static void plainValidator(Operation op, String sObjName, Set&lt;String&gt; fieldNames)**
This method performs a security check on the object and set of field names passed, but does not perform a full scan of the data. 

**public static void plainValidator(Operation op, String fields)**
This method performs a security check on the string passed, but does not perform a full scan of the data. The string can be built by calling the *getFieldsForPlainValidator()* method

#Flags
There are 2 static flags that can help configure the way the code works.

**public static Boolean showDebugMessages = false**
This controls the output of debug log messages to the debug log. The default value is not to log anything.

**public static Boolean findFieldsUsingJSON = false**
View [Issue #1](https://github.com/eltoroit/ETLC_SecureDB/issues/1) where @afawcett suggests to use the Apex native method SObject.getPopulatedFieldsAsMap() rather than the JSON methods to find the different objects/fields. I have done some tests but I am not sure which one is better, so I have left both of them with this flag that can toggle the behaviour. the default log is not using JSON.

#### Additional Notes:

- The code throws exceptions when there are misuses of the code (invalid parameters for requests for example) or if it finds the user’s CRUD, FLS and Record Access security are violated. When the code is running under test mode, it throws an **ETLC_Exception** explaining what security was actually violated. For security purposes when the code is not running in test mode, the exception thrown is the generic NoAccessException.

- The type of DML operations are defined with an enum called **ETLC_SecuredDB.Operation** with these possible values:  *Creating*, *Reading*, *Updating*, *Deleting*, and *Upserting*

### ETLC_SecuredDB_Test
This Apex class helps you get the code coverage for the previous class. I have defined some basic test operations to ensure that misuses of the code (invalid parameters for requests for example) are handled properly.

I have also included one method (**private static void QueryFailSecurity_1() **) that will fail the user’s CRUD, FLS and Record Access security, but I have assumed that the user found via the *CommunityNickname* does not have FLS access to the *description*. If you do not have a user with this *CommunityNickname* or the user does have access to *description* field, the test will obviously fail.

## Contributing

1. Fork it!
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request :D

## History

| Version | Description |
| --- | --- |
| 2.0 | Added performance improvements |
| 1.0 | First code release |

## License

This repository uses the MIT library, which basically means it’s free… Enjoy!

## About Me

ElToroIT [Twitter](https://twitter.com/ElToroIT) [LinkedIn](https://www.linkedin.com/in/eltoroit) loves helping developers understand Salesforce and how easy is to work with this great platform. He also teaches the Salesforce [developer courses](http://www.salesforce.com/services-training/training_certification/training-by-role.jsp) in English and Spanish.


Don't forget to visit my blog: [http://eltoro.it](http://eltoro.it) 
