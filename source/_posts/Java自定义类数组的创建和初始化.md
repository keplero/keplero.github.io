---
title: Java自定义类数组的创建和初始化
date: 2019-03-23 15:42:04
tags: java
categories: 技术  
---

&emsp;&emsp;刚刚在慕课学习Java的集合类List过程中，向集合中添加元素时，遇到一个问题：

 - 定义了一个Course类;
 

```Java
public class Course {

	private String id;    
	private String name;  //课程名称
	
	//get  set方法
	public String getId() {
		return id;
	}
	public void setId(String id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
```

 - 在测试类中有一个Course类的List集合allCourses，一个向该List集合添加元素的方法addToAllCourses() ;
 

```
public class ListTest {  

	private List allCourses;  //用于存放备选课程的List
	
	//构造函数，初始化allCourses
	public ListTest() {  		
		this.allCourses = new ArrayList();
	}

    public void addToAllCourses(String id, String name) {
		
		//List的add（Object e）, 添加一个元素
		Course cs = new Course();  //创建一个Course对象，并设置其参数
		cs.setId(id);   
		cs.setName(name);
		allCourses.add(cs);	
	
		
		// List的addAll（Collection c）方法		
		Course[] courses = new Course[3];
		
		courses[0].setId("2");
		courses[0].setName("C语言");
		
		courses[1].setId("3");
		courses[1].setName("数据库");
		
		courses[2].setId("4");
		courses[2].setName("计算机网络");
		
	    allCourses.addAll(Arrays.asList(courses));  
	    //在该方法中 参数必须为collection类型，因此必须用工具类进行类型转换
     }	
}
```

 - 主函数测试

```
	public static void main(String[] args) {
		ListTest list = new ListTest();
		list.addToAllCourses("1", "数据结构");
		List<Course> li = list.getAllCourses();
       
		for(int i = 0; i < li.size(); i++) {
			System.out.println((i + 1) + ": " + li.get(i).getId() 
			                   + " " + li.get(i).getName());
		}
	}
```
&emsp;&emsp;乍看是没有问题的，但是一运行，问题就来了，myeclipse报出了空指针异常。异常抛出点为addToAllCourses()方法中 Course类数组courses在赋值时的代码。

&emsp;&emsp;既然是空指针异常，也就是说，courses[0],  courses[1], courses[2]都是没有被初始化的。

&emsp;&emsp;一般而言，如下的数组定义在myeclipse中是不会报错的：

```
String[] s = new String[3];
s[0] = "000000";
System.out.println(s[0]);
```

&emsp;&emsp;但是，我的代码中，数组的类型为自定义的类，而非Java本身提供的类（如String类），因而我怀疑是不是我的数组定义出了问题.  查阅资料后发现，自定义类的数组定义后的初始化应该如下：

```Java
		Course[] courses = new Course[3];
		
		courses[0] = new Course();
		
		courses[0].setName("0000000");
		
		System.out.println(courses[0].getName());
```
&emsp;&emsp;也就是说，在声明了自定义类的数组之后，对每一个数组元素的初始化，都要为其new一个对象出来使得指针指向该对象，Java语言本身是不提供在自定义类数组声明时候自动创建新对象的方式的。

&emsp;&emsp;此处顺便再补充一下类二维数组的定义及初始化，

```Java
/*Java提供类*/

        //方式一:
        String[][] s = new String[][] { {"a", "b", "c"},
                                        {"d", "e", "f"}  };
		
        //方式二
        int r = 0;
		String[][] s = new String[2][3];
		for (int i = 0; i < 2; i++) 
			for (int j = 0; j < 3; j++) {
				s[i][j] = String.valueOf(r++);
			}

/*自定义类*/
		Course[][] courses = new Course[2][3];  //声明
		courses[0][0] = new Course();  //使用时new一个实例
		courses[0][0].setId("0");
		courses[0][0].setName("000000");
		
        System.out.println(courses[0][0].getId() + "  " 
                           + courses[0][0].getName());
        //测试 不报空指针异常
```

