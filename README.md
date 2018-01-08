# LarangPaginator

This is a Laravel Angular Paginator for tables. For other backend languaage to use this library. Please make sure your response conforms with this response: 
  
      
    {
       "total": 50,
       "per_page": 15,
       "current_page": 1,
       "last_page": 4,
       "next_page_url": "http://laravel.app?page=2",
       "prev_page_url": null,
       "path": "http://laravel.app",
       "from": 1,
       "to": 15,
       "data":[
            {
                // Result Object
            },
            {
                // Result Object
            }
       ]
    }
 
 ## Dependencies
 
 `npm install bootstrap@4.0.0-beta.2`
 
 `npm install --save @ng-bootstrap/ng-bootstrap`
 
 ## Installation
 
 `npm install --save larang-paginator`

   
## Usage in Application

Follow the instruction below to use LarangPaginator.

Add `LarangPaginatorModule.forRoot()` in AppModule or Other Modules using `LarangPaginatorModule`
  
   ## *.component.ts
   Add/refactor the following code to the appropriate places in your component.
   # Notice: 
  ```` 
  path: full path of the api url to call for data.
  from: the key the eventService will use in mapping when data has responded from paginator. (from key must be unique to every component using pagination)
  data: (paginated response), this must be the first data rendered from the component which information are picked to generate the pagination.
  limit: paginated data per page, default is 50.
  ````
  
  A sample larangPaginator built url for paginating will be `http://localhost:8088/api/consumer/transactions?page=2&paginate=50`
  
````
 public paginator = { 
    path: 'http://localhost:8088/api/consumer/transactions',
    limit: 30,
    data: {},
    from: 'consumer_transactions'
  
 };
 
 // EventService is a service created to push data to fro between classes that can inject a service, you can inject the eventservice from the paginator.
   
   constructor(
     private eventsService: EventsService
   ) {
     this.eventsService.on(this.paginator.from, (res) => {
       // pass response to the property rendering the data in view
       
       this.paginator.data = res.data; // update paginated data in view
     });
     
     private getTransactions() {
      this.http.get(this.paginator.path).subscribe(
      (res) => {
          this.paginator.data = res.data
      },
      (err) => {
      
      }
      )
     }
     
     ngOninit() {
      this.getTransactions();
     }
      
  ````
  
  ## *.component.html
  Add this below the table you want it to paginate data from backend.
  
  ````
  <table>
    <tr>
      <td>#</td>
      <td>Name</td>
    </tr>
  
    <tr *ngFor="let page of paginator.data; let i = index;">
      <td>{{((paginator.data['current_page'] - 1) * paginator.limit + i + 1) || (i + 1)}}</td>
      <td>{{page?.name}}</td>
    </tr>
  
  </table>
  
  <app-paginator [from]="paginator.from" [data]="paginator.data" [path]="paginator.path"
                 [limit]="paginator.limit"></app-paginator>
 ````

 
## Build as a package

`npm run pack-build`

