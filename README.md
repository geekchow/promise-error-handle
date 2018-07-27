# promise-error-handle

## local error handle for chained promises.
### sample code.
```
function getData() {
  console.log('start getData');
  return new Promise((resolve, reject) => {
//      setTimeout(() => resolve('getData')); 
    setTimeout(() => reject('getData')); 
  });
}

function getMoreData(data) {
  console.log('getMoreData: previous ' + data);
  return new Promise((resolve, reject) => {
     setTimeout(() => resolve('getMoreData')); 
//     setTimeout(() => reject('getMoreData')); 
  });
}

function getEvenMoreData(data) {
  console.log('getEvenMoreData: previous ' + data);
  return new Promise((resolve, reject) => {
     console.log('getEvenMoreData promise is evaludated');
     setTimeout(() => resolve('getEvenMoreData')); 
  });
}

getData()
.then(getMoreData, err => { console.log('getData hit error'); return false; })
// .then(getMoreData)
// .then(getEvenMoreData, err => console.log('getMoreData hit error'))
.then(getEvenMoreData)
.then(m => console.log('data from getEvenMoreData: ', + m))
.catch(e => console.log('final catch error: ' + e))

/* output
"start getData"
"getData hit error"
"getEvenMoreData: previous false"
"getEvenMoreData promise is evaludated"
"data from getEvenMoreData: "
NaN
*/
```
As sample above, getData got error and its then method provided a local error handler function. 
At this case, the final catch won't be executed, and the chain is not been stopped, the getMoreData is skip obvisously as exepected, 
but however, the getEvenMoreData is executed with data from getData's error handle function. In my opinion it's weird, just skip all the following then
should be more intuitive.
