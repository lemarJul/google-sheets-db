# Google Sheets DB

## Table of Contents

- [Google Sheets DB](#google-sheets-db)
  - [Table of Contents](#table-of-contents)
  - [Description](#description)
  - [Getting Started](#getting-started)
    - [Prerequisites](#prerequisites)
    - [Initialization](#initialization)
  - [Usage example](#usage-example)
    - [Instantiation](#instantiation)
    - [Access tables](#access-tables)
    - [.search(_criteria_)](#searchcriteria)
    - [.create(_data_)](#createdata)
    - [.read(_id_)](#readid)
    - [.update(_newData_, _id_)](#updatenewdata-id)
    - [.delete(_id_)](#deleteid)
  - [Flaws](#flaws)
  - [License](#license)

## Description

Simple database Abstraction using a Google SpreadSheet as the underlying storage. <br>
Provides a SCRUD implementation to handle your data.

You can access the project [here](https://script.google.com/d/1aimWaOqHCGgHHFkCv18-LjLXKCRZd_YrQRrrSbNYceH1--BPr2uCwaYu/edit?usp=sharing) as reader.

## Getting Started

### Prerequisites

- A google account;
- A GAS script;
- A google spreadsheets;

### Initialization

You can ether make a copy of the source file and starting building your project over
<br>**OR**<br>
You can add it as a library : scriptID => _1aimWaOqHCGgHHFkCv18-LjLXKCRZd_YrQRrrSbNYceH1--BPr2uCwaYu_

## Usage example

### Instantiation

```javascript

    const ss = SpreadsheetApp.openById("xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx");

    // As part of the script
    const myDB = new GSDB(ss);

    // As Library
    const myDB = new LEM_GSDB.connect(ss)

```

### Access tables

```javascript

    // let's assume your spreadsheet has sheets "users", "user_roles","customers","logs", ...

    const users = myDB.users;
    const userRoles = myDB.user_roles;
    const userCustomers = myDB.customers;
    const userLogs = myDB.logs;
    ...

```

### .search(_criteria_)

```js

    myDB.users.search({ lastName: "doe" });
    /* returns 
        [
            { 
                id: 1,
                name: "john",
                lastName: "doe",
                email: "john@doe.com" 
            },
            {
                id: 2, 
                name: "jane", 
                lastName: "doe",
                email: "jane@doe.com" 
            }
        ];
    */

    myDB.users.search();
    // returns an array of all the records in the table.

```

### .create(_data_)

```js

    myDB.users.create(
        { 
            name: "julien",
            lastName: "lemarchand" 
        }
    );
    /* returns 
        {
            id: n+1,
            name: "julien",
            lastName: "lemarchand",
            email: "" 
        },
    */

    myDB.users.create(
        {
            name: "julien",
            lastName: "Lemarchand",
            eyesColor: "green",
        }
    );
    /* returns
        {
            id: n+1,
            name: "julien",
            lastName: "lemarchand",
            email: "" 
        }
        
    "eyeColor" is ignored here because it is not part of the table fields
    */

```

### .read(_id_)

```js

    myDB.users.read(2);
    /* returns
        { 
            id: 2,
            name: "jane",
            lastName: "Doe",
            email: "john@doe.com"
        }
    */

```

### .update(_newData_, _id_)

```js

    const idToUpdate = 1;

    //the following two result the same:
    myDB.users.update(
        { 
            id: idToUpdate, 
            lastName: "bonham",
            email: "bonzo@drummer.rip" 
        }
    );

    myDB.users.update(
        { 
            id: 4,
            lastName: "bonham",
            email: "bonzo@drummer.rip" 
        },
        idToUpdate
    );


    // returns { id: 1, name: "john", lastName: "bonham", email: "bonzo@drummer.rip" }

```

### .delete(_id_)

```js

myDB.users.delete(3);

```

## Flaws

- GAS only provides auto-completion of top-level objects and functions **for libraries**. Due to the object approach of this project, there will therefore be no autocompletion available once the DB instance has been created. Adding it as a library to your project could be a bit frustrating to use, although the methods are quite simple.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
