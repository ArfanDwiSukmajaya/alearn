---
sidebar_position: 1
---

# Anotation

Anotasi yang digunakan pada Spring Boot

### @SpringApplication

- Anotasi ini digunakan pada class utama(main class) dari aplikasi Spring Boot.
- Ini menggabungkan beberapa anotasi seprti **@Configuration**, **@EnableAutoConfiguration**, **@componentScan**
- Ini menandakan bahwa kelas tersebut adalah kelas konfigurasi utama dan memulai pemindaian komponen dari paket tempat kelas tersebut berada

```jsx
@SpringBootApplication
public class MyApplication {
  public static void main(String[] args) {
    SpringApplication.run(MyApplication.class, args);
  }
}
```

### @Controller

- Menandakan bahwa kelas tersebut adalah bagian dari lapisan presentasi dan berfungsi sebagai controller dalam pola desain MVC.
- Menghandle permintaan HTTP dan mereturn respons.

```jsx
@Controller
public class MyController {
    // Metode yang menangani permintaan HTTP
    @GetMapping("/hello")
    public String sayHello() {
        return "Hello, World!";
    }
}
```

### @RestController

- Merupakan varian dari **@Controller** yang dirancang khusus untuk mengembalikan data RESTful.
- Setara dengan **@Controller** dan **@ResponseBody**.

```jsx
@RestController
public class MyRestController {
    // Metode yang mengembalikan data JSON
    @GetMapping("/api/data")
    public Map<String, String> getData() {
        Map<String, String> data = new HashMap<>();
        data.put("key", "value");
        return data;
    }
}
```

### @RequestMapping:

- Mengaitkan permintaan HTTP dengan metode pengendali.
- Dapat digunakan pada tingkat kelas atau metode.

```jsx
@Controller
@RequestMapping("/app")
public class MyController {
    @GetMapping("/hello")
    public String sayHello() {
        return "Hello, World!";
    }
}

```

### @Service

- Menandakan bahwa kelas tersebut adalah bagian dari lapisan layanan.
- Biasanya digunakan untuk logika bisnis.

```jsx
@Service
public class MyService {
    public String processData() {
        // Logika bisnis
        return "Processed Data";
    }
}
```

### @Repository

- Menandakan bahwa kelas tersebut adalah bagian dari lapisan akses data (Data Access Object).- Digunakan untuk interaksi dengan basis data.

```jsx
@Repository
public class MyRepository {
    // Metode-metode akses data
}


```
