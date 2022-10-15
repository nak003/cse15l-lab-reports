# Lab report 2
## Part 1 - SearchEngine.java
Below is the code that I wrote for SearchEngine.java 
```
class SearchHandler implements URLHandler {
    // The one bit of state on the server: string can be added and searched by
    // various requests.
    ArrayList <String> list = new ArrayList<String>();

    public String handleRequest(URI url) {
        String[] parameters = url.getQuery().split("=");
        if (parameters[0].equals("s")) {
            if (url.getPath().contains("/add")) {
                list.add(parameters[1]);
                return parameters[1] + " is now added to search engine";
            }
            if (url.getPath().contains("search")){
                ArrayList <String> resultList = new ArrayList<String>();
                for (int i = 0; i < list.size(); i++){
                    if (list.get(i).contains(parameters[1])){
                        resultList.add(list.get(i));
                    }
                }

                if (resultList.size()!=0){
                    String result;
                    result = returnArrayList(resultList);
                    return result; 
                }
                
                else {
                    return "No words in search engine with " + parameters[1];
                }
            }
        }
        return "404 Not Found!";
    }

    public String returnArrayList(ArrayList<String> list){
        String result = "";
        for (int i=0; i<list.size();i++){
            result = result + list.get(i) + " ";
        }
        return result; 
    }
```
After starting the server, I added anewstringtoadd to the search engine by /add?s=anewstringtoadd. 
![image](addanew.png)
- handleRequest method is called 
- parameters[0] is equal to 's' that is between ? and =
    - If this value was changed, it would return "404 Not Found!" 
- parameters[1] is equal to 'anewstringtoadd' which is added to ArrayList list that represents the search engine. 
    - If this value was changed, it changes the string that will be added into the search engine. 

For next screenshot, I searched string with app after adding anewstringtoadd, apple, and pineapple. 
![image](searchapp.png)
- handleRequest method and returnArrayList method that I added to use as a helper method was called.
- parameters[0] equal to 's' 
    - If this value was changed, it would return "404 Not Found!"
- parameter[1] is equal to 'app' and the code finds the string with 'app' and returns those strings. Those words that contiains the 'app' are now added to the result arraylist and store that into string value using returnArrayList method. 
    - If this value was changed, it would find the strings with newly changed value. 

For last screenshot, I wrote wrong url and it showed "404 Not Found!" 
![image](wrong.png)
- handleRequest mehtod is called. 
- the path contains "/remove" and it will result "404 Not Found!" because the search engine only takes "/add" or "/search" 
    - If this is changed to "/add" or "/search" it would either add or search but if it is different, it will still show "404 Not Found!"
- parameter[0] is equal to "string" so it also causes error because it either add or search the string when the parameter[0] is equal to "s". 
    - If this is changed to 's' it would work but if it is different, it will still show "404 Not Found!"

## Part 2 - Bugs in ArrayExample and ListExample 
**ArrayExample Bug - reversedInPlace** 

 for reversedInPlace Method, **failure inducing input** is {1,2,3,4,5}. 
 ![image](test.png)
 This should return {5,4,3,2,1}. However, it failed the test.
![image](symptom.png)
It was expected <2> but it was <4> for the **symptom**. 
![image](symptom1.png)
The **bug**, problem of the code, was that the last half of the indexes are getting the already changed list. For example, the test failed because the array was {5,4,3,4,5}. 

To fix this bug, changed the code so that the for loop only runs to the half of the list. 

New Code: 
```  
static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length/ 2; i += 1) {
      int previous = arr[i];
      arr[i] = arr[arr.length - i - 1];
      arr[arr.length-i-1] = previous;
    }
  }
```  

**ListExample Bug - merge** 

for merge Method, **failure inducing input** is {x,y} for list1 and {a,z} for list2
![image](test1.png)
This should return {a,b,x,y}. However, it failed the test.
![image](testfail.png)
It returned out of memory error for the **symptom**. 
![image](testwitherror.png)
The **bug**, problem of the code, was that the last while loop increments the index1 instead of index2. Because index2 is not incremented, it was in the infinte loop since index2 will be always smaller than the list2.size. 

To fix the bug, I changed index1 to index2 inside of while(index2 < list2.size()). 

New Code:
```  
while(index2 < list2.size()) {
      result.add(list2.get(index2));
      index2 += 1;
}
```  