---
tags:
  - type guards
  - checking variable as undefined
  - typescript
---


Link : https://rangle.io/blog/how-to-use-typescript-type-guards/#:~:text=TypeScript%20allows%20you%20to%20create,the%20type%20in%20some%20scope.&text=When%20a%20function%20returns%20this,passed%20in%20will%20be%20narrowed.
``` 
const isCar = (variableToCheck: any): variableToCheck is Car =>
  (variableToCheck as Car).turnSteeringWheel !== undefined;
```