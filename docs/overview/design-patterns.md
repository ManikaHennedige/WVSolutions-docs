## Design Patterns

### Introduction

 This document primarily outlines the design patterns and preferences adopted during the development of WVSolutions, including relevant justification for choosing these preferences. This document should provide the reader with an outline of the developer's thought process and logic flow in approaching development. Ideally, future modifications and improvements to this application should similarly follow this paradigm.

### Client-Server Architecture

WVSolutions adopts a variation of the popular Client-Server architecture, which complements component-based development. Several features in this application, including the various Accounting and Inventory Management features, rely on synchronous operations that require databases providing consistency. Adopting a client-server based application facilitates this functionality.

An Onion or use case-based architecture was not adopted for the main structure for this application as the number of distinct, non-overlapping use cases remain fairly small, and trying to mold this application into an Onion architecture would result in rather heavily duplication of code. However, our database services do adopt a modified Onion architecture, relying primarily on Interfaces in calling database functions. 

### 'Server' Operations

Most database-critical operations requiring synchronization and consistency are performed directly from the database itself, through the use of database triggers. For instance, the buying and selling of inventory units (through the generation of relevant purchase orders/invoices) would result in the StockQty and HoldQty of the respective items being appropriately changed through a trigger. Similarly, the confirmation of an invoice (i.e. changing an Order status from 'Active' to 'Invoiced') would enforce the immutability of invoices - the order (and associated order item) details would not be able to be changed.

For detailed explanations on Trigger functions, please visit the 'features/triggers' section of the documentation.

### Component-driven Development

Development is done through components, which facilitates reusability and maintainability of the code. This is preferred as several components do end up being reused, such as the OwnerDetails and PetDetails components, with the only difference being whether or not the component is rendering a new pet/owner (to be filled in/added) or an existing one (to be updated).

Components in the application are generally organized by the focus. For example, components relating to information about the pets (including displaying a list of all pets and displaying a particular pet's details) are grouped together under 'Pets'. Similarly, components relating to Payments and processing (such as receiving payments from customers) are grouped together under 'Payments'

### Algorithms & Data Processing

Heavy processing of data are generally abstracted away from the individual components and are located in the 'Engine' folder. This contains the Controllers that perform calculations and run operations for specific components. For example, the PaymentController determines the invoices payable given a certain sum paid and a list of invoices, and returns the indexes of the invoices back to the component requiring it, allowing it to display the payable invoices to the user.

### Model-View-Controller Paradigm

Some elements of MVC (or MVVM) were adopted into development, because displaying razor pages innately incorporate elements of MVC - the 'Models' folder contain the objects that correspond with database tables, with their attributes similarly corresponding with the database columns. Some model files are appended with 'Partial' (for example InvoicePaymentPartial), and these are partial definitions of the original model, usually with custom methods appended.

Inside the 'Models' folder there is another 'ViewModels' subfolder, which contain adaptations of the abovementioned objects, but processed for more convenient viewing on their respective pages/components. These processed objects also may include data annotations for the purpose of data validation, which would be overwritten (upon database scaffolding) if added to the original model file. 