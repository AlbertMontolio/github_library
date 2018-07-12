# Section9: Using Services & Dependency Injection

what are services?

application
	appComponent
		AboutComponent
		-> log data to console

		UserComponent
			->
			UserDetailComponent
			->log data to console

we dont want to repeat methods


			
services are like repositories

UserService to manage the user storage

# 97. Creating a Logging Service

creating a service

in the app folder,  file name, logging.service.ts

@Service? no, a service is just a normal typescript class

```js
export class LoggingService {
	logStatusChange(status: string) {
		console.log('A server status changed, new status: ' + status);
	}
}
```

now we put this code where we want to use it

new-account.component.ts

```js
export class NewAccountComponent {
  @Output() accountAdded = new EventEmitter<{name: string, status: string}>();

  onCreateAccount(accountName: string, accountStatus: string) {
    this.accountAdded.emit({
      name: accountName,
      status: accountStatus
    });
    console.log('A server status changed, new status: ' + accountStatus);
  }
}
```

```js
import { LoggingService } from '../logging.service';
```

```js
const service = new LoggingService();
service.logStatusChange(accountStatus);
// console.log('A server status changed, new status: ' + accountStatus);
```

this works, but its wrong. thats not how u use a service in angular
generally, angular provides a much better way to use the services
you shouldn create the instances manually

which tools angular offers us to get access to our serices?

# 98. Injecting the logging service into components



Hierarchical Injector

access to our services, the tool is, dependency injector

a dependency, is something a class of ours will depend on, the new account component, depends on the loggin service, becuase we want to call a method on that serice

the dependcy injector, injects this dependency, injects an instance of the service class, in our component

we need to inform angular that we require such an instnace

how do we do that?

in our component, we add a consturctor

where we want to us ethe service



i can bind it to a property, by using the shortcut. instantly create a property with the same name and assign the value

```js
consturctor(private loggingService: LoggingService) {}
```

the type has to be the class you want to be injected


this simple task, informs angular, that we will need an instance of this service

who intantiates the class new-account component

who is responsive for creating our components? angular. we are placing selectors in our app, when angular finds a selector, it creates an instnace

angular need to construct them correctly. if we put the arguments that we need inour constructor, angular will give us the argument

it knows, we want an instance of the Logging class

angular knows what we want, but he does not know how

additional step

we need to provide a service. provide means, we tell angular how to create it

just add one extra property, to the component decorator

providers: [LoggingServ8ce]. we specify the type of we want to get prpovided

```js
@Component({
  selector: 'app-new-account',
  templateUrl: './new-account.component.html',
  styleUrls: ['./new-account.component.css'],
  providers: [LoggingService]
})
```


```js
@Component({
  selector: 'app-new-account',
  templateUrl: './new-account.component.html',
  styleUrls: ['./new-account.component.css'],
  providers: [LoggingService]
})
export class NewAccountComponent {
  @Output() accountAdded = new EventEmitter<{name: string, status: string}>();

  constructor(private loggingService: LoggingService) {}

  onCreateAccount(accountName: string, accountStatus: string) {
    this.accountAdded.emit({
      name: accountName,
      status: accountStatus
    });
    console.log(this.loggingService);
    this.loggingService.logStatusChange(accountStatus);
    // console.log('A server status changed, new status: ' + accountStatus);
  }
}
```


angular creates the instances for us. this bleibst in the angular ecosystem

same in the account comopnent

now we have the logging functionality centralized in a service

our code is cleaner

to get more dry

# 99. Creating a Data Service

store and manage our data

app.component.ts

```js
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  accounts = [
    {
      name: 'Master Account',
      status: 'active'
    },
    {
      name: 'Testaccount',
      status: 'inactive'
    },
    {
      name: 'Hidden Account',
      status: 'unknown'
    }
  ];

  onAccountAdded(newAccount: {name: string, status: string}) {
    this.accounts.push(newAccount);
  }

  onStatusChanged(updateInfo: {id: number, newStatus: string}) {
    this.accounts[updateInfo.id].status = updateInfo.newStatus;
  }
}
```


we store the accounts thre

lets create accounts.service.ts

move your accounts array there

accounts.service.ts

```js
export class AccountsService {
  accounts = [
    {
      name: 'Master Account',
      status: 'active'
    },
    {
      name: 'Testaccount',
      status: 'inactive'
    },
    {
      name: 'Hidden Account',
      status: 'unknown'
    }
	];
	
	addAccount(name: string, statuts: string) {
		this.accounts.push({name: name, status: status});
	}

	updateStatus(id: number, status: string) {
		this.accounts[id].status = status;
	}
}
````



we dont need the add and update methods in the appcomponent

```js
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  
}
```

app.compoment.html

```html
<div class="container">
  <div class="row">
    <div class="col-xs-12 col-md-8 col-md-offset-2">
      <app-new-account (accountAdded)="onAccountAdded($event)"></app-new-account>
      <hr>
      <app-account
        *ngFor="let acc of accounts; let i = index"
        [account]="acc"
        [id]="i"
        (statusChanged)="onStatusChanged($event)"></app-account>
    </div>
  </div>
</div>
```

we need the accounts!

app.component.ts

```js
  accounts: {name: string, status: string}[] = [];
```

we should inject the service

```js
import { Component } from '@angular/core';
import { AccountsService } from './accounts.service';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
  providers: [AccountsService]
})
export class AppComponent {
  accounts: {name: string, status: string}[] = [];

  constructor(private accountsService: AccountsService) {}

}
```


initializations should be done in oninit, not in the constructor

```js
export class AppComponent implements OnInit {
```

```js
  ngOnInit() {
    this.accounts = this.accountsService.accounts
  }
```

in our service,

since accounts is an array, it is a reference type

by setting it equal, we are getting access to the exact same array


new-account.component.ts

```js
import { Component, EventEmitter, Output } from '@angular/core';
import { LoggingService } from '../logging.service';

@Component({
  selector: 'app-new-account',
  templateUrl: './new-account.component.html',
  styleUrls: ['./new-account.component.css'],
  providers: [LoggingService]
})
export class NewAccountComponent {
  @Output() accountAdded = new EventEmitter<{name: string, status: string}>();

  constructor(private loggingService: LoggingService) {}

  onCreateAccount(accountName: string, accountStatus: string) {
    this.accountAdded.emit({
      name: accountName,
      status: accountStatus
    });
    console.log(this.loggingService);
    this.loggingService.logStatusChange(accountStatus);
    // console.log('A server status changed, new status: ' + accountStatus);
  }
}
```



we dont need to emit the event anymore, we use the service

```js
import { Component } from '@angular/core';
import { LoggingService } from '../logging.service';
import { AccountsService } from '../accounts.service';

@Component({
  selector: 'app-new-account',
  templateUrl: './new-account.component.html',
  styleUrls: ['./new-account.component.css'],
  providers: [LoggingService, AccountsService]
})
export class NewAccountComponent {
  constructor(private loggingService: LoggingService, private accountsService: AccountsService) {}

  onCreateAccount(accountName: string, accountStatus: string) {
    
    this.loggingService.logStatusChange(accountStatus);
  }
}
```


```html
  onCreateAccount(accountName: string, accountStatus: string) {
    this.accountsService.addAccount(accountName, accountStatus);
    this.loggingService.logStatusChange(accountStatus);
  }
```

account.component.ts

```js
import { Component, Input } from '@angular/core';
import { LoggingService } from '../logging.service';
import { AccountsService } from '../accounts.service';

@Component({
  selector: 'app-account',
  templateUrl: './account.component.html',
  styleUrls: ['./account.component.css'],
  providers: [LoggingService, AccountsService]
})
export class AccountComponent {
  @Input() account: {name: string, status: string};
  @Input() id: number;

  constructor(private loggingService: LoggingService,
              private accountsService: AccountsService) {}

  onSetTo(status: string) {
    this.accountsService.updateStatus(this.id, status);
    this.loggingService.logStatusChange(status);
  }
}
```

we dont see the changes in our page

# 100. Understanding the hierarchical injector

if we propvide a service, in one component, the angular framwerok, knows how to create an instnce of this service for this component, and, ALL ITS CHILDS COMPONTS

this component, and all the childs, will recieve the same instance of the service

the highest possible level is the appmodule
AppModule - same instance of service is available application-wide

the same instance of the class, of the service, is available in all the app, in all components, in all directives, in other services

the next level

the appcomponent: same isntance of service is available for all components (but not for other service)

the instances dont propagate app, they only go down

any other component, no child components: same instance of service is available for the component and all its child components

it even overwrites, if we are reciving a service from a compontent from above.

that was our problem






# 101. how many instances of services should it be?

we want to have the same instance

our accounts service has three instances

first one, in our app.component.ts

new account and account have also accountsService instnaces

therefore, if we add a new service, we push it in the array of accounts, its a new instance

different of the instance that we have in app.component, the one where we loop

fix?

in new-account.component.ts

we remove it from the provideres!!!

we get it from oven!!!

from the app.component.ts

# 102. Injecting Services into Services

the highest possible level, is not the app component

```js
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
  providers: [AccountsService]
})
```

so we delete it there

the highest possible level is in the app module

we can provide the accountsservice there

```js
providers: [AccountsService],
```

we are making sure, that our whole application, recieves the same isntance of the service, unless we overwrite it, as we did before

now we can inject a service into a service. just if the service is in the app.module.ts

if its in a component we cant

lets say we want to log sth, if we call an action in the accountservice

we provide a loggingservice in my app.moduel

```js
  providers: [AccountsService, LoggingService],
```

we comment out 

```js
providers: [LoggingService]
```

in new-account.component.ts and account.component.ts

in new-account.component.ts

```js
  onCreateAccount(accountName: string, accountStatus: string) {
    this.accountsService.addAccount(accountName, accountStatus);
    // this.loggingService.logStatusChange(accountStatus);
  }
```

and comment it out also in account.component.ts

we want to use this login service in the accounts service



accounts.service.ts

```js
	constructor(private loggingService: LoggingService) {}
```

now we could do

```js
	addAccount(name: string, statuts: string) {
    this.accounts.push({name: name, status: status});
    this.loggingService.logStatusChange(status);
	}
```

error

```js
Can't resolve all parameters for AccountsService: (?)
```

if you inject a service into sth

this sth, needs to have some metada data attachaed to it

components, directives, they have metadata, in the @

a service, has not

we need to attach some metadata

@Injectable()

this tells angular, that in this service services can be injected

we put it, only if we expect to inject things in the current service.


# 103. Using services for cross-component communication

provide an event, that we can trigger in a compoent and listent to it in another

in accounts.service.ts

```js
  statusUpdated = new EventEmitter<string>();
```

account.component.ts

```js
  onSetTo(status: string) {
    this.accountsService.updateStatus(this.id, status);
    // this.loggingService.logStatusChange(status);
    this.accountsService.statusUpdated.emit(status);
  }
```

i am emiting an event, that i set up in the service

in the new account, which is parallael, i want to listen to it

```js
export class NewAccountComponent {
  constructor(private loggingService: LoggingService, private accountsService: AccountsService) {
    this.accountsService.statusUpdated.subscribe();
  }
```

i want to subscribe to the event statusUpdated. event emitter it kind of wraps an observable


```js
export class NewAccountComponent {
  constructor(private loggingService: LoggingService, private accountsService: AccountsService) {
    this.accountsService.statusUpdated.subscribe(
      (status: string) => alert('new status: ' + status)
    );
  }
```

we recieve a new status, i know its a string

i will trhow an alert, saying, bla blabla

i have cross cocmponenet communication through a service with the event emitter


-----------------

# Lecture 104

If you're using Angular 6+ (check your package.json  to find out), you can provide application-wide services in a different way.

Instead of adding a service class to the providers[]  array in AppModule , you can set the following config in @Injectable() :

@Injectable({providedIn: 'root'})
export class MyService { ... }
This is exactly the same as:

export class MyService { ... }
and

import { MyService } from './path/to/my.service';
 
@NgModule({
    ...
    providers: [MyService]
})
export class MyService { ... }
Using this new syntax is completely optional, the traditional syntax (using providers[] ) will still work. The "new syntax" does offer one advantage though: Services can be loaded lazily by Angular (behind the scenes) and redundant code can be removed automatically. This can lead to a better performance and loading speed - though this really only kicks in for bigger services and apps in general.
















































