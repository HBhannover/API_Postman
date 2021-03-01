# API_Postman

## Scenario 1: Response of Request_11 is input for Request_21 ##
```
Request_11: GET: https://jsonplaceholder.typicode.com/posts
Response_11: 
  [
    {
        "userId": 1,
        "id": 1,
        "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
        "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto"
    },
    {
        "userId": 1,
        "id": 2,
        "title": "qui est esse",
        "body": "est rerum tempore vitae\nsequi sint nihil reprehenderit dolor beatae ea dolores neque\nfugiat blanditiis voluptate porro vel nihil molestiae ut reiciendis\nqui aperiam non debitis possimus qui neque nisi nulla"
    },
    { ...
 ]   
```
```
Request_21: GET: https://jsonplaceholder.typicode.com/comments?postId={{testID}}
```
## ==> Solution: ##

Step 1. By Request_11 in section "Tests" we have to add this code:
 ```  
    var responseBody = pm.response.json();
    var antwortArr = responseBody.map(function(item){
        return item.userId;
    });
   pm.environment.set("envVarArray", antwortArr);
   console.log("envVarArray: ",pm.environment.get("envVarArray"));
   ```
   
 Step 2. By Request_21 in section "Pre-request Script" we will add this code:
 ```
   let envVarArr = pm.environment.get("envVarArray");
   pm.environment.set("testID",envVarArr.pop());
   pm.environment.set("envVarArray", envVarArr);
   (envVarArr.length)? postman.setNextRequest(pm.info.requestName) : postman.setNextRequest(null);
   ```
   
   `pm.info.requestName` gives the name of "itself" so we are saying to run this again if the `envVarArr` is not empty meaning it havenot finished iterating through the `userIds`
   (We can use `pm.globals.set(...)` instead of `pm.enviroment.set(...)`)
 
 
