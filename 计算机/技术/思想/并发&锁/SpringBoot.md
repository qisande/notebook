## 并发

在 Spring Boot 项目中进行多线程开发通常使用 Java 的 `java.util.concurrent` 包和 Spring 提供的线程池功能。以下是一个简单的示例，展示如何在 Spring Boot 项目中进行多线程开发。

### 步骤：

1. **添加依赖**： 如果您的项目还没有 Spring Boot 依赖，请在 `pom.xml` 文件中添加以下依赖：

   ```
   xml复制代码<dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter</artifactId>
   </dependency>
   ```

2. **配置线程池**： 创建一个配置类来定义一个线程池。

   ```
   java复制代码import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   import org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor;
   
   import java.util.concurrent.Executor;
   
   @Configuration
   public class AsyncConfig {
       @Bean(name = "taskExecutor")
       public Executor taskExecutor() {
           ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
           executor.setCorePoolSize(5);
           executor.setMaxPoolSize(10);
           executor.setQueueCapacity(25);
           executor.setThreadNamePrefix("MyThread-");
           executor.initialize();
           return executor;
       }
   }
   ```

3. **使用 `@Async` 注解**： 在需要异步执行的方法上使用 `@Async` 注解。

   ```
   java复制代码import org.springframework.scheduling.annotation.Async;
   import org.springframework.stereotype.Service;
   
   @Service
   public class MyService {
       @Async("taskExecutor")
       public void asyncMethod() {
           System.out.println("Execute method asynchronously - " + Thread.currentThread().getName());
           // 模拟长时间任务
           try {
               Thread.sleep(5000);
           } catch (InterruptedException e) {
               e.printStackTrace();
           }
           System.out.println("Method execution completed - " + Thread.currentThread().getName());
       }
   }
   ```

4. **启用异步支持**： 在 Spring Boot 应用程序的主类中启用异步支持。

   ```
   java复制代码import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.scheduling.annotation.EnableAsync;
   
   @SpringBootApplication
   @EnableAsync
   public class AsyncApplication {
       public static void main(String[] args) {
           SpringApplication.run(AsyncApplication.class, args);
       }
   }
   ```

5. **调用异步方法**： 在控制器或其他服务中调用异步方法。

   ```
   java复制代码import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.RestController;
   
   @RestController
   public class MyController {
       @Autowired
       private MyService myService;
   
       @GetMapping("/async")
       public String callAsyncMethod() {
           myService.asyncMethod();
           return "Async method called!";
       }
   }
   ```

### 运行项目

启动 Spring Boot 应用程序，并访问 `http://localhost:8080/async`。您将在控制台中看到异步方法的执行日志，表明该方法在单独的线程中执行。

### 解释

- **配置线程池**：使用 `ThreadPoolTaskExecutor` 配置了一个简单的线程池，指定了核心线程数、最大线程数和队列容量。
- **`@Async` 注解**：标记方法为异步方法，该方法将在线程池中的线程上执行。
- **启用异步支持**：使用 `@EnableAsync` 注解启用了 Spring 的异步支持。

这种方式允许您在 Spring Boot 应用程序中轻松地进行多线程开发，提升应用的并发能力和性能。


## 锁

在 Spring Boot 项目中进行多线程开发时，可能会遇到需要同步资源访问的情况，这时就需要使用锁来保证线程安全。以下是一个示例，展示如何在 Spring Boot 项目中应用锁进行多线程开发。

### 1. 使用 `synchronized` 关键字

`synchronized` 关键字可以用于方法或代码块上，确保同一时刻只有一个线程可以访问同步代码。

```
java复制代码import org.springframework.stereotype.Service;

@Service
public class SynchronizedService {
    private int counter = 0;

    public synchronized void increment() {
        counter++;
        System.out.println("Counter: " + counter + " - " + Thread.currentThread().getName());
    }

    public synchronized int getCounter() {
        return counter;
    }
}
```

### 2. 使用 `ReentrantLock`

`ReentrantLock` 是一个更灵活的锁实现，它提供了 `synchronized` 关键字没有的一些高级功能，例如尝试加锁、超时和中断。

```
java复制代码import org.springframework.stereotype.Service;
import java.util.concurrent.locks.ReentrantLock;

@Service
public class LockService {
    private final ReentrantLock lock = new ReentrantLock();
    private int counter = 0;

    public void increment() {
        lock.lock();
        try {
            counter++;
            System.out.println("Counter: " + counter + " - " + Thread.currentThread().getName());
        } finally {
            lock.unlock();
        }
    }

    public int getCounter() {
        return counter;
    }
}
```

### 3. 使用 `ReadWriteLock`

`ReadWriteLock` 允许多个读线程并发访问，但写线程独占访问。它适用于读多写少的场景。

```
java复制代码import org.springframework.stereotype.Service;
import java.util.concurrent.locks.ReentrantReadWriteLock;

@Service
public class ReadWriteLockService {
    private final ReentrantReadWriteLock lock = new ReentrantReadWriteLock();
    private int counter = 0;

    public void increment() {
        lock.writeLock().lock();
        try {
            counter++;
            System.out.println("Counter: " + counter + " - " + Thread.currentThread().getName());
        } finally {
            lock.writeLock().unlock();
        }
    }

    public int getCounter() {
        lock.readLock().lock();
        try {
            return counter;
        } finally {
            lock.readLock().unlock();
        }
    }
}
```

### 4. 创建控制器调用锁方法

创建一个简单的控制器来调用上述服务方法，以测试多线程环境下的锁功能。

```
java复制代码import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

@RestController
public class LockController {
    @Autowired
    private SynchronizedService synchronizedService;

    @Autowired
    private LockService lockService;

    @Autowired
    private ReadWriteLockService readWriteLockService;

    private final ExecutorService executorService = Executors.newFixedThreadPool(5);

    @GetMapping("/synchronized")
    public String testSynchronized() {
        for (int i = 0; i < 10; i++) {
            executorService.submit(() -> synchronizedService.increment());
        }
        return "Synchronized method called!";
    }

    @GetMapping("/lock")
    public String testLock() {
        for (int i = 0; i < 10; i++) {
            executorService.submit(() -> lockService.increment());
        }
        return "Lock method called!";
    }

    @GetMapping("/readWriteLock")
    public String testReadWriteLock() {
        for (int i = 0; i < 10; i++) {
            executorService.submit(() -> readWriteLockService.increment());
        }
        return "ReadWriteLock method called!";
    }
}
```

### 5. 运行项目并测试

启动 Spring Boot 应用程序，并访问以下 URL 以测试锁功能：

- `http://localhost:8080/synchronized`
- `http://localhost:8080/lock`
- `http://localhost:8080/readWriteLock`

### 总结

- **`synchronized` 关键字**：适用于简单的同步需求，易于使用但缺乏灵活性。
- **`ReentrantLock`**：提供了更多控制能力，例如尝试加锁、可中断的锁获取和超时。
- **`ReadWriteLock`**：适用于读多写少的场景，提高并发性。

通过这些锁机制，可以确保在多线程环境中安全地访问共享资源。