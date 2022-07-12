Lambda:
 - leno izvajanje: zadeve odlasamo do takrat ko jih je sele treba nardit
 - opusti imeana vmesinkov in metod 
 - podobno **Shadow classes?**
```java
String[] names = {"evan", "nik", "jurij", "kristijan", "bore bore"}

Arrays.sort(imena, 
			new Comparator<String>() {
			@Override
			public int compare(String a, String b) {
	
				return a.length() - b.length();
			}});
//Lambda			
Arrays.sort(imena, (String a, String b) -> {
	a.length() - b.length()});

//Clean lambda
Arrays.sort(imena, (a, b) -> a.length() - b.length());

Izziv:
	Seznami:
	-  pq-add -> OK and -> 
	                