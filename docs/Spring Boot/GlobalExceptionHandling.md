---
sidebar_position: 3
---

# Global Exception Handling

Saat terjadi error di Controller, seperti validation error, logic error dll. Kan secara default spring akan mengemnalikan errornya sesuai jenis errornya.
Nah kan kita terlkadang ingin membuat halaman atau resposnya sendiri. Nan ini bisa menggunakan @RestControllerAdvice

### RestContollerAdvice

ControllerAdvice adalah sebuaah class yang dipanggil ketika sebuah exception terjadi, dengan begitu kita bisa memanipulasi response yang akan dikembalikan ke user menggunakan restcontrolleradvice ini.

### Buat Class nya (ErrorController.java)

```jsx
@RestControllerAdvice
public class ErrorController {

    //  biasanya digunakan untuk menangani kesalahan validasi yang terjadi saat menyimpan atau memperbarui entitas dalam basis data, terutama ketika aturan validasi (seperti yang ditetapkan oleh anotasi Hibernate Validator atau anotasi validasi lainnya) tidak dipenuhi.
    @ExceptionHandler(ConstraintViolationException.class)
    public ResponseEntity<WebResponse<String>> constraintViolationException(ConstraintViolationException exception) {
        return ResponseEntity.status(HttpStatus.BAD_REQUEST)
                .body(WebResponse.<String>builder().errors(exception.getMessage()).build());
    }

    // pengecualian yang dapat Anda lemparkan secara manual dari kontroler untuk menunjukkan bahwa permintaan tidak berhasil dengan status kode tertentu. Ini sering digunakan untuk menangani kasus di mana terjadi kesalahan atau kondisi yang tidak terduga dalam logika bisnis aplikasi Anda dan Anda ingin memberikan respons dengan status kode tertentu kepada klien
    @ExceptionHandler(ResponseStatusException.class)
    public ResponseEntity<WebResponse<String>> apiException(ResponseStatusException exception) {
        return ResponseEntity.status(exception.getStatusCode())
                .body(WebResponse.<String>builder().errors(exception.getReason()).build());
    }
}

```

**WebResponse** ini cba liat dokumentasi sebelumnya (Web Respons)

### COntoh penggunaanya ini
```jsx

    @Service
public class UserService {

    @Autowired
    UserRepository userRepository;

    @Autowired
    ValidataionService validataionService;

    @Transactional
    public void register(RegisterUserRequest request) {
        // ini digunakan ConstraintViolationException
        validataionService.validate(request);

        // ini akan ditangap ResponseStatusException
        if(userRepository.findByUsername(request.getUsername()).isPresent()){
            throw new ResponseStatusException(HttpStatus.BAD_REQUEST, "Username already registered");
        }

        User user = new User();
        user.setUsername(request.getUsername());
        user.setPassword(request.getPassword());
        user.setName(request.getName());

        userRepository.save(user);
    }

}


```