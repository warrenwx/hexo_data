---
title: volatile
date: 2018-10-18 14:37:38
tags: volatile, synchronized, java, cas.
---
### what kind of occasion we need to deal with
consider code behind.

```java
public void A() throws InterruptedException {
		for(int i = 0; i < 50; i ++) {
			Thread thread = new Thread(new Runnable() {							
				@Override
				public void run() {
					for(int i = 0; i < 2; i++) {						
						++ number;
						System.out.println(Thread.currentThread().getName() + " get " + number);					
					}											
				}
			});
			thread.start();
		}
		Thread.sleep(3000);
		System.out.println("finally " + number);
	}
```
finally, the result of 'number' is not predictable, which might be 960, 977 or 999.
### what volatile can guarantee
it keeps every thread get 'global' variable with consitent value. one more thing, ++number is not atomic, update number in multi-thread is not safe. add synchronized symbol will make it.
```java
public void A() throws InterruptedException {
		for(int i = 0; i < 50; i ++) {
			Thread thread = new Thread(new Runnable() {							
				@Override
				public void run() {
					for(int i = 0; i < 2; i++) {
						synchronized (integerLock) {
							++ number;
							System.out.println(Thread.currentThread().getName() + " get " + number);
						}											
					}											
				}
			});
			thread.start();
		}
		Thread.sleep(3000);
		System.out.println("finally " + number);
	}```
so, synchronized can be a useable way in this occasion. compared to Optimistic Lock, synchronized cost too much, as it is a kind of Pessimistic Lock. 
### what volatile means
n. a volatile substance; a substance that changes readily from solid or liquid to a vapor, which means somthing can be changed at anytime.

As statement 'lock' will have bus or cache(cpu level) all to itself(core), which will influence multi-core performance. cas + volatile is a kind of Optimistic Lock, code follows.

```java
/**
 * @ClassName:     CASTest.java
 * @Description:   TODO
 * 
 * @author
         xiang_wang

 * @version
        V1.0  
 * @Date           2018年10月25日 下午3:13:08 
 */
import java.lang.reflect.Field;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;


import sun.misc.Unsafe;

/**
 * @author xiang_wang
 *
 */
public class CASTest {
	private volatile int value = 0;
	
	public int get() {
		return value;
		
	}
	
	public int increaseAndGet() throws NoSuchFieldException, SecurityException, InterruptedException {
		while(true) {
			int next = value + 1;
			if(innerCas(value, next)) {
				return next;
			}else {
				System.out.println(Thread.currentThread().getName() + " wait.");
				Thread.sleep(100);
			}
		}
	}
	
	private boolean innerCas(int before, int after) throws NoSuchFieldException, SecurityException {
		Unsafe unsafe = getUnsafe();
		return unsafe.compareAndSwapInt(this, unsafe.objectFieldOffset(CASTest.class.getDeclaredField("value")), before, after);
	}
	
	public static Unsafe getUnsafe() {
        try {
            Field singletonInstanceField = Unsafe.class.getDeclaredField("theUnsafe");
            singletonInstanceField.setAccessible(true);
            return (Unsafe) singletonInstanceField.get(null);
        } catch (NoSuchFieldException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        }
        return null;
    }

	/** 
	 * @Title:        main 
	 * @Description:  TODO 
	 * @param:        @param args    
	 * @return:       void    
	 * @throws 
	 * @author        xiang_wang
	 * @Date          2018年10月25日 下午3:13:09 
	 * @throws SecurityException 
	 * @throws NoSuchFieldException 
	 * @throws InterruptedException 
	*/
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		CASTest casTest = new CASTest();		
		ExecutorService es = Executors.newFixedThreadPool(10);
		for(int i = 0; i < 10; ++ i) {
			es.execute(new Runnable() {
				
				@Override
				public void run() {
					
					try {
						while(true) {
							System.out.println(Thread.currentThread().getName() + ":" + casTest.increaseAndGet());
							Thread.sleep(100);
						}					
					} catch (NoSuchFieldException e) {
						// TODO Auto-generated catch block
						e.printStackTrace();
					} catch (SecurityException e) {
						// TODO Auto-generated catch block
						e.printStackTrace();
					} catch (InterruptedException e) {
						// TODO Auto-generated catch block
						e.printStackTrace();
					}				
				}
			});
		}		
	}

}

```
Conclusion, the method is reliable. but I am more interested in the parameters in this program. 'Thread.sleep(100)' statement may influence program performance, At least that's what I believed when I wrote the program.
let's test it. In my opinion, the first statement will make increaseAndGet faster and the second will make thread competition more fiercely.


when interval of retry time decrease, and competition between threads, repetition will occur, as value may have been revised by other threads. 
consider this two statements:
```java
	int next = value + 1;	// statement 1
	if(innerCas(value, next)) {	// statement 2
```
1. In thread-1, value is 10, after statement 1, next is 11
2. In thread-2, value changes from 10 to 11.
3. In thread-1, statement 2 executes, value = 11, next = 11, uhuhuh

fix bug:
```java
	int before = value;	// save value for thread-x
	int next = before + 1; // save update value to thread-x
	if(innerCas(before, next)) {
		return next;
	}else {
	xxx
```
experiment result and analysis:

| retry/wait  	|50  			|100		   |200 		  |500 			 |1000   		|
| ------------  | ------------- | ------------ | ------------ | ------------ | ------------ |
|50   			|  479/7422		| 50/10253     | 22/20074     | 22/49760     | 18/99256     |
|100   			|  	60/5609		| 496/14968    | 40/20349     | 13/49951     | 23/100053    |
|200   			|   33/5698		|  103/12056   | 379/27471    | 17/50160     | 28/100057    |
|500   			|   21/6054		|  47/12355    |  18/20849	  | 495/74570    |  59/102554   |
|1000   		|   18/6810		|  18/11759    |  23/22253    | 107/60574    |  591/159076  |

when wait time is as short as possible, retry time influence conflict time significantly. the lower retry time is, the more times thread try to update 'value'.
when wait time is long enough, the times become reverse related. retry and wait range from 50 to 1000 has no significant effect to total execution time, which is main related to wait time.