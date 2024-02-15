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

    @ExceptionHandler(ConstraintViolationException.class)
    public ResponseEntity<WebResponse<String>> constraintViolationException(ConstraintViolationException exception) {
        return ResponseEntity.status(HttpStatus.BAD_REQUEST)
                .body(WebResponse.<String>builder().errors(exception.getMessage()).build());
    }

    @ExceptionHandler(ResponseStatusException.class)
    public ResponseEntity<WebResponse<String>> apiException(ResponseStatusException exception) {
        return ResponseEntity.status(exception.getStatusCode())
                .body(WebResponse.<String>builder().errors(exception.getReason()).build());
    }
}

```

**WebResponse** ini cba liat dokumentasi sebelumnya (Web Respons)
