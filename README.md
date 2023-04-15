# Backdrop Engineering Challenge

## Overview

#### As the newest member of Backdrop Engineering team, the task is to build the backend component for a new application. The application requires users to add their bank accounts by providing their bank account number, selecting a bank name, and writing their name as registered with their banks. The backend should validate the account name provided by the user against the account name provided by the Paystack API, using the Levenshtein Distance algorithm. The backend should also provide a GraphQL query that takes in a bank code and account number and returns an account name. If the user has provided an account name, it should be returned, otherwise the Paystack API account name will be returned. Tests must be written for the solution. In the readme, it should be explained why the pure Levenshtein Distance algorithm is more effective than the broader Damerau-Levenshtein Distance algorithm in this specific scenario if assumptions are made, they should be stated in the readme.

## GraphQL Mutation and Query

#### The backend component of this task includes a GraphQL mutation that accepts three arguments: user_account_number, user_bank_code, and user_account_name. The mutation validates the user's account information using the Paystack API, and sets the user's is_verified attribute to true if the names match. We also created a GraphQL query that takes in a bank code and account number, and returns the corresponding account name.

## Types

#### 1. UserType

Provides a defintion for a user in the application.

```
 type UserType {
        id: ID
        email: String
        phone: String
    }
```

#### 2. BankAccountType

provides a definition for a user's bank account.
It holds a reference to the user who the account belongs to

```
 type BankAccountType {
        id: ID
        user_account_name: String
        user_account_number: String
        user_bank_code: String
        user_bank_id: String
        user: UserType
    }
```

#### 3. BankType

It provides a definition for bank account entity that is retrieved from Paystack API

```
 type BankAccountType {
        id: ID
        user_account_name: String
        user_account_number: String
        user_bank_code: String
        user_bank_id: String
        user: UserType
    }
```

#### 4. LoginResponseType

Provides a defintion for the response,provided by the login mutation. Return a reference to the **UserType** instance and a jwt token.

```
 type LoginResponseType {
        token: String!
        user: UserType
    }
```

## Mutation

#### 1.addUser

Provide a definition for registering a user, returns a reference to the **UserType** instance.

```
addUser(email: String! password: String! phone: String): UserType

```
**Parameters**
- email: the unique email for the user registering
- password: the password for the user
- phone (optional): the phone number for the user

#### 2.deleteUser

Provide a definition for deleting a user, returns a reference to the **UserType** instance.

**Authentication** authorization header scheame "Bearer token"

```
deleteUser(id: ID!): UserType
```
**Parameters**
- ID the unique ID of the user assigned by the database

#### 3.addAccount

Provide a definition for adding an account to a user, returns a reference to the **BankAccountType** instance

**Authentication** authorization header scheame "Bearer token"

```
addAccount(
            user_account_name: String!
            user_account_number: String!
            user_bank_code: String!
            user_id: String!
        ): BankAccountType
```
**Parameters**
- user_account_name: The name on the account to be added, it will be compared to the name on the account returned by PaystackAPI
- user_account_number: The number on the account to be added
- user_bank_code: The bank code on the account to be added, will be verified by PaystackAPI
- user_id: the unique ID of the user assigned by the database

#### 4.loginUser

Provide a definition for loging in a user, returns a reference to the **UserType** instance.

```
 loginUser(email: String! password: String!): LoginResponseType

```

**Parameters**

- email: the user's email
- password: the user's password

## Query

#### 1. getUser

Provides a definition for getting a user by id, returns a reference to the **UserType** instance

**Authentication** authorization header scheame "Bearer token"

```
 getUser(ID: ID!): UserType
```

**Parameters**

- ID the unique ID of the user assigned by the database

#### 2. getUsers

Provides a definition for getting a users by returns a collection of **UserType** instances

**Authentication** authorization header scheame "Bearer token"

```
getUsers: [UserType]
```

#### 3. getAccount

Provides a definition for getting a user's account by user id, returns a reference to an instance of **BankAccountType**

```
        getAccount(user_id: ID!): BankAccountType
```

#### 4. getBanks

Provides a definition for getting the list of bank accounts from the paystack API and returns a collection of **BankType** instances

```
        getBanks: [BankType]
```

## Answer to Question

#### The pure Levenshtein Distance algorithm might be a more effective solution than the broader Damerau–Levenshtein Distance algorithm in this specific scenario because the latter algorithm allows for additional operations such as transpositions, insertions, and deletions. However, these additional operations are not necessary in this scenario, as we only need to account for slight spelling errors. Therefore, using the pure Levenshtein Distance algorithm is a more efficient and accurate solution.
