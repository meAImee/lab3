# Lab Report2 -- Aimee He
# Part 1 - Search Engine

The code for the **Search Engine** is shown as the following block:
```
import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;

class Handler implements URLHandler {
    // The one bit of state on the server: a number that will be manipulated by
    // various requests.
    int num = 0;
    ArrayList<String> stringStore=new ArrayList<>();
    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            return String.format("Number: %d", num);
        } else if (url.getPath().equals("/increment")) {
            num += 1;
            return String.format("Number incremented!");
        } else if(url.getPath().contains("/add")){
            String[] parameters = url.getQuery().split("=");
            stringStore.add(parameters[1]);
            return parameters[1]+" is added";
        }else if(url.getPath().contains("/search")){
            String[] parameters = url.getQuery().split("=");
            int s=stringStore.size();
            String stringPrintOnClient="";
            for(int i=0;i<s;i++){
                if(stringStore.get(i).contains(parameters[1])){
                    System.out.println(stringStore.get(i));
                    stringPrintOnClient=stringPrintOnClient+stringStore.get(i)+" is found"+"\n";
                }
            }
            return stringPrintOnClient ;
        }
        
        
        
        
        
        else {
            System.out.println("Path: " + url.getPath());
            if (url.getPath().contains("/add")) {
                String[] parameters = url.getQuery().split("=");
                if (parameters[0].equals("count")) {
                    num += Integer.parseInt(parameters[1]);
                    return String.format("Number increased by %s! It's now %d", parameters[1], num);
                }
            }
            return "404 Not Found!";
        }
    }
}

class SearchEngine {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");

```

## Screenshot 1 (Calling **handleRequest** method:)
![SearchEngine1](https://github.com/meAImee/lab3/raw/main/SearchEngine1.png)



Arguments: 
```
/add?s=pineapple
```
Value of **stringStore** changed, added **"pineapple"**;



## Screenshot 2 (Calling **handleRequest** method:)
![SearchEngine2](https://github.com/meAImee/lab3/raw/main/SearchEngine2.png)

Arguments:
```
/add?s=apple
```
Value of **stringStore** changed, added "apple";

## Screenshot 3 (Calling **handleRequest** mathod)
![SearchEngine3](https://github.com/meAImee/lab3/raw/main/SearchEngine3.png)

Argument **/search?s=app

String **stringPrintOnClinent** changed, first added **"pineapple is found"**, then added **"apple is found"**.

# Part 2
## ArrayExample.java

**Bug 1**

Faliure-inducing input:
```
@Test 
  public void testReverseInPlace() {
    int[] input1 = { 3,4 };
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{ 4,3 }, input1);
  }
```

The symptom:
 ```
 arrays first differed at element [1]; expected:[3] but was:[4]
 at ArrayTests.testReverseInPlace(ArrayTests.java:9)
 ```

The bug fixed:
```
static void reverseInPLace(int[] arr){
    int changeInt; 
    for(int 1 = 0; i < arr.length/2; i+=1; ){
        changeInt = arr[arr.length-i-1];
        arr[arr.length-i-1] = arr[i];
        arr[i] = changeInt;
    }
}
```
Explanation:
> The method **reverseInPlace** does not work because it makes the second half written reversely while losing the first half (not as expected). Thu the symtom shows that the test failed.

**Bug 2**

Faliure inducing input:
```
@Test
  public void testAverageWithoutLowest(){
    double[] input1={3.0,3.0,4.0};
    assertEquals((int)4.0, (int)ArrayExamples.averageWithoutLowest(input1));
  }
```

The symptom:
```
java.lang.AssertionError: expected:[4] but was:[2]
 at ArrayTests.testAverageWithoutLowest(ArrayTests.java:22)
```

The bug fixed:
```
 static double averageWithoutLowest(double[] arr) {
    if(arr.length < 2) { return 0.0; }
    double lowest = arr[0];
    for(double num: arr) {
      if(num < lowest) { lowest = num; }
    }
    double sum = 0;
    int count = 0;
    for(double num: arr) {
      if(num != lowest) { sum += num; }
      if(num ==  lowest) {count += 1; }
    }
    return sum / (arr.length - count);
  }
```
Explanation: 
> Removed all lowest values, but only removed one while counting total ints, thus inducing failure output.

## ListExamples.java
Faliure-inducing input:
```
@Test
    public void testMerge(){
        List<String> rawList1 = new ArrayList<>();
        List<String> rawList2 = new ArrayList<>();
        List<String> ansList = new ArrayList<>();
        rawList1.add("a");
        rawList2.add("b");
        rawList2.add("c");
        ansList.add("a");
        ansList.add("b");
        ansList.add("c");
        assertEquals(ansList, ListExamples.merge(rawList1, rawList2));
    }
```

The symptom:
```
java.lang.OutOfMemoryError: Java heap space
 at java.base/java.util.Arrays.copyOf(Arrays.java:3512)
 at java.base/java.util.Arrays.copyOf(Arrays.java:3481)
 at java.base/java.util.ArrayList.grow(ArrayList.java:237)
 at java.base/java.util.ArrayList.grow(ArrayList.java:244)
 at java.base/java.util.ArrayList.add(ArrayList.java:454)
 at java.base/java.util.ArrayList.add(ArrayList.java:467)
 at ListExamples.merge(ListExamples.java:42)
 at ListTests.testMerge(ListTests.java:36)
```

The bug fixed:
```
  // Takes two sorted list of strings (so "a" appears before "b" and so on),
  // and return a new list that has all the strings in both list in sorted order.
  static List<String> merge(List<String> list1, List<String> list2) {
    List<String> result = new ArrayList<>();
    int index1 = 0, index2 = 0;
    while(index1 < list1.size() && index2 < list2.size()) {
      if(list1.get(index1).compareTo(list2.get(index2)) < 0) {
        result.add(list1.get(index1));
        index1 += 1;
      }
      else {
        result.add(list2.get(index2));
        index2 += 1;
      }
    }
    while(index1 < list1.size()) {
      result.add(list1.get(index1));
      index1 += 1;
    }
    while(index2 < list2.size()) {
      result.add(list2.get(index2));
      index2 += 1;
    }
    return result;
  }
```
Explanation:
> The **merge** method can't be duplicated when there's elements left in it.










