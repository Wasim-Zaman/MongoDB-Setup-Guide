
# MongoDB Setup Guide

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Installing MongoDB](#installing-mongodb)
3. [Accessing MongoDB with mongosh](#accessing-mongodb-with-mongosh)
4. [Creating a User](#creating-a-user)
5. [Updating a User](#updating-a-user)
6. [Other User Management Operations](#other-user-management-operations)
7. [Connecting to MongoDB](#connecting-to-mongodb)
8. [Conclusion](#conclusion)

## Prerequisites
- A VPS or local server with Ubuntu (or any other OS).
- MongoDB installed on the server.
- `mongosh` (MongoDB Shell) installed.

## Installing MongoDB
Follow the official [MongoDB installation guide](https://docs.mongodb.com/manual/installation/) to install MongoDB on your server.

## Accessing MongoDB with mongosh
To connect to your MongoDB server using `mongosh`, run the following command:

```bash
mongosh -u <username> -p <password> --authenticationDatabase <authDatabase>
```

Replace `<username>`, `<password>`, and `<authDatabase>` with your MongoDB credentials.

### Example:
```bash
mongosh -u root -p Wasim@1323** --authenticationDatabase admin
```

## Creating a User
To create a new user with specific roles, follow these steps:

1. Switch to the database where you want to create the user (use `admin` for global access):

   ```javascript
   use admin
   ```

2. Create the user using the `createUser()` method:

   ```javascript
   db.createUser({
     user: "myuser",
     pwd: "mypassword",
     roles: [
       { role: "readWriteAnyDatabase", db: "admin" },
       { role: "dbAdminAnyDatabase", db: "admin" }
     ]
   });
   ```

### Example:
```javascript
use admin
db.createUser({
  user: "myuser",
  pwd: "mypassword",
  roles: [
    { role: "readWriteAnyDatabase", db: "admin" },
    { role: "dbAdminAnyDatabase", db: "admin" }
  ]
});
```

## Updating a User
If you need to update an existing user’s roles, use the `updateUser()` method:

```javascript
db.updateUser("myuser", {
  roles: [
    { role: "readWriteAnyDatabase", db: "admin" },
    { role: "dbAdminAnyDatabase", db: "admin" }
  ]
});
```

### Example:
```javascript
db.updateUser("myuser", {
  roles: [
    { role: "readWriteAnyDatabase", db: "admin" },
    { role: "dbAdminAnyDatabase", db: "admin" }
  ]
});
```

## Other User Management Operations

### Getting User Information
To view the details of a user, including their roles:

```javascript
db.getUser("myuser");
```

### Removing a User
To delete a user from the database:

```javascript
db.dropUser("myuser");
```

## Connecting to MongoDB
You can connect to different databases using the same user. Specify the desired database in your connection string when using Mongoose or any other MongoDB driver.

### Mongoose Example:
```javascript
const mongoose = require('mongoose');

const uri = 'mongodb://myuser:mypassword@<your-vps-ip>:27017/myDatabase';

mongoose.connect(uri, { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log('MongoDB connected successfully'))
  .catch(err => console.error('MongoDB connection error:', err));
```

## Conclusion
This guide provides essential steps for setting up and managing MongoDB users using `mongosh`. Feel free to modify it as needed for your projects!
