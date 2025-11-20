/*
```javascript
*/
const elements = ea.getViewSelectedElements().filter(el=>el.type==="image");
if(elements.length === 0) return;
ea.copyViewElementsToEAforEditing(elements);
ea.getElements().forEach(el=>{
  const l = Math.max(el.width, el.height);
  el.width = Math.round(el.width * 180/l);
  el.height = Math.round(el.height * 180/l);
});
ea.addElementsToView();