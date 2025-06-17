# ABDI - App Business Data Infrastructure
# [MAIN WEBSITE](https://orestf.github.io/abdi_architecture)

#### Description

This is an overview of advanced rails architecture inspired by various patterns and frameworks like Domain Driven Design, Repository patterns, Trailblazer, Dry.rb and others.

#### Key concepts

Everything revolves around 4 nodes: App, Business, Data, Infrastructure. Each node has certain responsibilities that must be not used by other nodes.

![ABDI.png](https://github.com/user-attachments/assets/e5d9f4c7-33ce-47bf-b5cc-41a29f202fc9)


### App

![app.png](https://github.com/user-attachments/assets/5ae6db53-2a4a-4421-b8f5-301d00198125)


##### Responsibilities

1. Representation
    - datatables
    - decorators
    - facades
    - helpers
    - mailers
    - serializers
    - view

2. Request input/output
    - channels (web-sockets)
    - controllers
    - services for AMQP communication
    - any other kind of external communications

3. No business logic  

### Business

[![business.png](https://github.com/user-attachments/assets/092b353d-5804-444e-9a28-2f1d7e48c5e4)


##### Responsibilities

1. Use case validation  
1. Business rules execution  
1. Business rules callbacks
1. All business logic goes here

Business node is responsible for implementing client's requests (client is the one who pays for your code).
Business node concept is similar to Sagas, Use cases or  User stories, but is strict and simple.
It's structure should match business processes and real life domains. This is the only reason why application should be developed at all.  

Business node consist of:  
**Operations** - human-readable class that describes and performs business requirements. It's an entry point of business logic.  
**Actions** - are bricks for *Operations*. Could be called only within an *Operation*.  
**Forms** - data validation layer. First step of *Operation* call. *Operation* must not be executed if input data is invalid.
Business node could contain any custom components that are required for business requirements satisfaction.

The namespace/naming patter is: `BusinessDomainInPlural::Operations/Actions/Forms/::ClassNameAsVerb`

Structure:
```sh             
├── orders
│   ├── operations
│   │   ├── create.rb # -> calls Orders::Forms::Create, Orders::Actions::CalculatePrice, Orders::Actions::NotifyCustomer
│   │   └── cancel.rb
│   ├── forms
│   │   └── cancel.rb
│   └── actions
│       ├── calculate_price.rb
│       └── notify_customer.rb
└── Products
   ├── operations
   │   ├── create.rb
   │   └── delete.rb
   └── forms
       └── create.rb     
```

### Data

![data.png](https://github.com/user-attachments/assets/0193462e-601b-4368-a89d-ac573e86b799)



##### Responsibilities

1. Database read  
1. Database write
1. No business logic

This node is responsible for database communication. In other words - database read/write.  
*Data* is actually extracted and renamed rails models folder, BUT IT'S NOT behave like models.  

The key difference are:   
* zero business logic
* no callbacks except system required, like: cache clearing, data normalization, etc. 
* only persistence validation for correct DB usage, like: validate latitude if longitude present and vice-versa, validate email format and uniqueness, validate URL format, etc.  
no business validation like: customer region validation, age validation, etc. 

### Infrastructure

![infrastructure.png](https://github.com/user-attachments/assets/2ca7dfc5-a508-42eb-9254-b30b425f3493)



##### Responsibilities

1. Keep base classes: BaseOperation, BaseAction, etc.
1. Could keep clients for third-party API
1. Could keep any other custom non business constants and classes.
1. No business logic

[architecture_png]: ABDI.png
[app_png]: app.png
[business_png]: business.png
[data_png]: data.png
[infrastructure_png]: insfrastructure.png
