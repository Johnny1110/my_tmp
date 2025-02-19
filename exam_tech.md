# Senior Java Engineer Exam



<br>

---

<br>

Total Point 160 (basic: 130, bouns: 30)

100 up: passed.

<br>

## 1 (EASY) 10 point

Check out the following java code, and answer the question:

```java
class Data {
    int value;

    public Data(int value) {
        this.value = value;
    }
}

public class ReferenceExample {
    public static void modifyPrimitive(int num) {
        num = num * 2;
    }

    public static void modifyObject(Data data) {
        data = new Data(100);
    }

    public static void main(String[] args) {
        int number = 10;
        Data dataObj = new Data(50);

        modifyPrimitive(number);
        System.out.println("After modifyPrimitive: " + number); // Expected-1: ?

        modifyObject(dataObj);
        System.out.println("After modifyObject: " + dataObj.value); // Expected-2: ?
    }
}
```

* What is Expect-1 ?

* What is Expect-2 ? 

<br>
<br>

## 2 (EASY) 10 point

Check out the following java code, and answer the question:

```java
class Resource {
    private final String name;

    public Resource(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}

class Task implements Runnable {
    private final Resource resource1;
    private final Resource resource2;

    public Task(Resource resource1, Resource resource2) {
        this.resource1 = resource1;
        this.resource2 = resource2;
    }

    @Override
    public void run() {
        synchronized (resource1) {
            System.out.println(Thread.currentThread().getName() + " locked " + resource1.getName());

            // Simulating some work
            try { Thread.sleep(50); } catch (InterruptedException e) { }

            synchronized (resource2) {
                System.out.println(Thread.currentThread().getName() + " locked " + resource2.getName());
            }
        }
    }
}

public class Example1 {
    public static void main(String[] args) {
        Resource resA = new Resource("Resource A");
        Resource resB = new Resource("Resource B");

        Thread thread1 = new Thread(new Task(resA, resB), "Thread-1");
        Thread thread2 = new Thread(new Task(resB, resA), "Thread-2");

        thread1.start();
        thread2.start();
    }
}
```

* Are there any issue ? why and how to solve (at least 2 approach) ?


<br>

## 3 (MEDUIM) 20 point

Check out the following java code, and answer the question:

```java
public class IntegerLockExample {
    private static Integer count = 0;

    public static void main(String[] args) throws InterruptedException {
        Thread t1 = new Thread(() -> increment());
        Thread t2 = new Thread(() -> increment());

        t1.start();
        t2.start();

        t1.join();
        t2.join();

        System.out.println("Final Count: " + count);
    }

    public static void increment() {
        for (int i = 0; i < 1000; i++) {
            synchronized (count) {
                count++;
            }
        }
    }
}
```

* Are there any problem? why and how to fix it?

<br>
<br>

## 4 (MEDIUM) 20 point

The company often executes the following queries:

```sql
-- Query 1: Retrieve all orders placed by a specific user
SELECT * FROM orders WHERE user_id = 123;

-- Query 2: Get recent orders for a product
SELECT * FROM orders WHERE product_id = 456 ORDER BY order_date DESC LIMIT 10;

-- Query 3: Count all orders with status 'Shipped'
SELECT COUNT(*) FROM orders WHERE status = 'Shipped';
```

* Please the that company design the sql table index.

* Are there any problem with Query-3 ? Why and how to improve?  (Hint: Covering Index)

<br>
<br>

## 5 (MEDIUM) 20 point

What is SQL transaction isolation ? Plase list all the sql isolation type and explain what they do.


<br>

## 6 (MEDIUM) 20 point

Scenario:

You are developing a Spring Boot application with a service that saves user data into a MySQL database. The UserService class has a method that creates a user and intentionally throws an exception to test transaction rollback.

However, __the user record is still saved in the database!__

### Buggy Code

```java
@Service
public class UserService {

    @Autowired
    private final UserRepository userRepository;

    // Entry point here 
    public void processUserRequest(Object request) {
        createUser(request);
    }

    @Transactional
    public void createUser(Object request) {
        // 1. create userDetail
        createUserDetailInfo(request);
        // 2. create phone
        createUserPhone(request);
        // 3. create user wallet
        createUserWallet(request);

        // Simulating an error
        throw new RuntimeException("Intentional Exception: Should trigger rollback");
    }

    public void createUserDetailInfo(Object request) {
        // save to DB.
        // ...
    }

    public void createUserPhone(Object request) {
        // save to DB.
        // ...
    }

    @Transactional
    public void createUserWallet(Object request) {
        // save to DB.
        // ...
    }
}
```

Tasks:

1. Identify why the transaction is not rolling back.

2. Explain why @Transactional is not working.

3. Fix the code so that the transaction is correctly rolled back.

4. Bonus question (HARD): how does `@Transaction` work? how does it keep that sql session? 30 point


<br>
<br>


## 7 (MEDUIM-HARD) 30 point

Please draw The B+ Tree Struacure.

