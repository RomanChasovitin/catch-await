# catch-await

[![NPM version][npm-image]][npm-url]
[![Downloads][download-badge]][npm-url]

> Async await wrapper for easy error handling

## Pre-requisites
You need to use Node 7.6 (or later) or an ES7 transpiler in order to use async/await functionality.
You can use babel or typescript for that.

## Install

```sh
yarn add catch-await --save
```

## Usage

```js
import { to } from 'catch-await';

async function asyncFunction(callback) {
  const { error: userError, data: user } = await to(UserModel.findById(1));
    if(!user) return callback('No user found');
      
    const { error: taskError, data: savedTask } = await to(TaskModel({userId: user.id, name: 'Demo Task'}));
    if(taskError) return callback('Error occurred while saving task');

    if(user.notificationsEnabled) {
       const { error: notificationError } = await to(NotificationService.sendNotification(user.id, 'Task Created'));
       if(notificationError) return callback('Error while sending notification');
    }

    if(savedTask.assignedUser.id !== user.id) {
       const { error: sendError , data: notification } = await to(NotificationService.sendNotification(savedTask.assignedUser.id, 'Task was created for you'));
       if(sendError) return callback('Error while sending notification');
    }

    callback(null, savedTask);
}

async function asyncFunctionWithThrow() {
  const { error, data: user } = await to(UserModel.findById(1));
  if (!user) throw new Error('User not found');
}
```

[npm-url]: https://npmjs.org/package/catch-await
[npm-image]: https://img.shields.io/npm/v/await-to-js.svg?style=flat-square

[download-badge]: http://img.shields.io/npm/dm/await-to-js.svg?style=flat-square
