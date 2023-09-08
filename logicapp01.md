The idea is to read the json payload, and for each client, process all the contacts.
A solution is to check the length of the contacts element. If it is an array, the number of elements will be returned otherwise it is an object. 

However, there is no simple way to implement this logic in logic app. The images below shows what happens if this process is implemented as is.

## Contact list is an array.
![alt text](images/array-success.png "Contact list is an array")


## Contact list is an object.
![alt text](images/array-failed.png "Contact list is an object")


**Step ** what now
