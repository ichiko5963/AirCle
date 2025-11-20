
/* 
```js*/ 
const api = ea.getExcalidrawAPI(); 
const {openMenu} = api.getAppState(); 
if(openMenu) { 
	api.updateScene({appState:{openMenu: null}}); 
} else { 
	const isElementSelected = Boolean(ea.getViewSelectedElement()); 
	api.updateScene({appState:{openMenu: isElementSelected?"shape":"canvas"}}); 
}