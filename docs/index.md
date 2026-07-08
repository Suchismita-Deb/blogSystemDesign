# Welcome to MkDocs
## Project layout
HLD - Book, Jordan, Hello Interview, Any video. 
The point is it will be topic wise. Like any topic I got anywhere I will add it in single place.

Example - Most commonly asked and pattern.
LLD - Pattern Example. 
Most asked end to end.
Multithreading.

Java Project.
Spring project Interview Guide.

Microservice Architecture.


AI ML - Core to advanced role - Backend AI Engineer.

Run the project - python -m mkdocs serve and mkdocs serve


Scan in 10 seconds

Revise in 2 minutes

Remember key points  
To deploy - push the code in github and `mkdocs gh-deploy`.

Adding image.

Inside `docs - images - SystemDesign - Image.png` and the file are in say `docs - SystemDesign - file.md` the url will be go to the main folder then image.

The image url - `![NetworkLayer](../images/SystemDesign/NetworkLayer.png)`

The tag in the base doc branch.

Making the image small - `<img src="/images/SystemDesign/NetworkLayer.png" style="width:60%;">`

https://suchismita-deb.github.io/blogSystemDesign/

Practice UML diagram to get teh class relationship of the system and no need to write to the code. Design Pattern and UML diagram.

# System Design PDF

<iframe
class="pdf-viewer"
src="./pdfs/Graph.pdf">
</iframe>

<iframe
class="pdf-viewer"
src="./pdfs/SystemDesignTips.pdf">
</iframe>

```
git config user.name
git config user.email
git config http.sslVerify false
git push origin main
```


External monitor works - Extend display (There **Settings - Display** there will be 2 screens and moving the cursor to the left or right will point to the monitor or laptop screen) - Making any app visible in monitor then open the app and then **Win+Shift+Right/LeftKey**.  
Duplicate Screen will open all in the laptop and monitor.





distinctElementOptimized
arr[] = [1, 2, 1, 3, 4, 2, 3]
[3, 4, 4, 3]

int nums[] and int k.
Find the distinct nums in k size window in the array.

```java
import java.util.HashMap;
import java.util.Map;

int[] distinctElementOptimized(int nums[], int target) {
    Map<Integer, Integer> mp = new HashMap<>();
    
    int arr[] = new int[n-k+1];
    for(int i=0;i<k;i++){
        mp.put(nums[i],mp.getOrDefault(mp.get(nums[i]),0)+1);
    }
    int pointer=0;
    int pos=1;
    arr[0] = mp.size();
    for(int i=k;i<n;i++){
        mp.put(nums[pointer], mp.get(nums[pointer])-1);
        if(mp.get(nums[pointer])==0) {
            mp.remove(nums[pointer]);
        }
        mp.put(nums[i],mp.getOrDefault(mp.get(nums[i]),0)+1);
        arr[pos++] = mp.size();
    }
    return arr;
}
```

What is concurrentHashMap.
What is the HashMap internal.
Kafka Partition ordering.
What is the equals method.
