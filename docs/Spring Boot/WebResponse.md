---
sidebar_position: 2
---

# Web Respons

Web response ini digunakan untuk membuat custome dari response

### Buat Class nya (WebResponse.java)

```jsx
@Data
@Builder
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class WebResponse <T> {
    private boolean success;
    private String message;
    private String error;
    private T data;
    private PaggingResponse pagging;
}
```

```jsx
@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class PagingResponse {
    private Integer currentPage;
    private Integer totalPage;
    private Integer size;
}
```

ini contoh penggunaanya di controller

```jsx
    @GetMapping("/ok")
    public WebResponse<List<Region>> getAllRegions() {
        List<Region> regions = regionService.getAll();
//        return WebResponse.<List<Region>>builder()
////                .success(true)
////                .message("Successfully retrieved regions")
//                .data(regions)
//                .build();


        // tanpa builder
        WebResponse<List<Region>> response = new WebResponse<>(true, "Successfully retrieved regions", regions, null);
        return response;

    }


    @GetMapping("/test")
    public WebResponse<List<Region>> getAllRegions(
            @RequestParam(defaultValue = "0") int currentPage,
            @RequestParam(defaultValue = "3") int size) {

        List<Region> regions = regionService.getRegionsWithPagination(currentPage, size);
        long totalSize = regionService.getTotalsize();

        PaggingResponse pagging = PaggingResponse.builder()
                .currentPage(currentPage)
                .totalSize((int) totalSize )
                .size(size)
                .build();

        return WebResponse.<List<Region>>builder()
                .success(Boolean.TRUE)
                .message("Get Data Region")
                .data(regions)
                .pagging(pagging)
                .build();
    }
```

ini servicenya

```jsx
public class RegionService {

//    inject region repository
    private RegionRepository regionRepository;

    @Autowired
    public RegionService(RegionRepository regionRepository) {
        this.regionRepository = regionRepository;
    }


    public Long getTotalsize(){
        return regionRepository.count();
    }

    public List<Region> getRegionsWithPagination(int currentPage, int size) {
         return regionRepository.findAll(PageRequest.of(currentPage, size)).getContent();
    }

//    Get all data
    public List<Region> getAll(){
        return regionRepository.findAll();
    }

```

### Generic Web Response

Ini contoh menggunakan generic ygy

```java

    @Data
    @Builder
    @AllArgsConstructor
    public class GenericResponse<T> {

        private  Boolean success;
        private  String message;
        private T data;

        public static <T> GenericResponse<T> empty(){
            return GenericResponse.<T>builder()
                    .success(false)
                    .message("")
                    .data(null)
                    .build();
        }

        public static <T> GenericResponse<T> success(T data){
            return GenericResponse.<T>builder()
                    .success(true)
                    .message("success")
                    .data(data)
                    .build();
        }

        public static <T> GenericResponse<T> error(T data){
            return GenericResponse.<T>builder()
                    .success(false)
                    .message("error")
                    .build();
        }

        public static <T> GenericResponse<T> success(T data, String message){
            return GenericResponse.<T>builder()
                    .success(true)
                    .message(message)
                    .data(data)
                    .build();
        }

        public static <T> GenericResponse<T> error(T data, String message){
            return GenericResponse.<T>builder()
                    .success(false)
                    .message(message)
                    .build();
        }



    }

```

**Penerapanya di controller**

```Java

@RestController
public class EmployeeController {

    @Autowired
    private EmployeeService employeeService;

    @GetMapping("/employee/name/{id}")
    public ResponseEntity<GenericResponse<String>> getEmployeeName(@PathVariable("id") Long id){
        return ResponseEntity
                .status(HttpStatus.OK)
                .header("custome-header", "123")
                .body(GenericResponse.success(employeeService.getEmployeeName(id)," Get Name successfully."));
    }

    @GetMapping("/employee")
    public ResponseEntity<GenericResponse<List<Employee>>> getAllEmployee(){
        return ResponseEntity
                .status(HttpStatus.OK)
                .header("custome-header", "value")
                .body(GenericResponse.success(employeeService.getAllEmployee()," Get all employee successfully."));

    }

    @GetMapping("/employee/{id}")
    public ResponseEntity<GenericResponse<Employee>> getEmployeeById(@PathVariable("id") Long id){
        return ResponseEntity
                .status(HttpStatus.OK)
                .header("custome-header", "value")
                .body(GenericResponse.success(employeeService.getEmployeeById(id)," Get employee successfully."));
    }

    @PostMapping("/employee")
    public ResponseEntity<GenericResponse<Employee>> addEmployee(@RequestBody Employee employee){
        return ResponseEntity
                .status(HttpStatus.CREATED)
                .header("custome-header", "value")
                .body(GenericResponse.success(employeeService.addEmployee(employee),"Created employee successfully."));
    }

    @PutMapping("/employee/{id}")
    public ResponseEntity<GenericResponse<Employee>> updateEmployee(@PathVariable("id") Long id, @RequestBody Employee employee){
        return ResponseEntity
                .status(HttpStatus.OK)
                .header("custome-header", "value")
                .body(GenericResponse.success(employeeService.updateEmployee(id, employee),"Updated employee successfully."));
    }

    @DeleteMapping("/employee/{id}")
    public ResponseEntity<GenericResponse<Employee>> deleteEmployee(@PathVariable("id") Long id){
        return ResponseEntity
                .status(HttpStatus.OK)
                .header("custome-header", "value")
                .body(GenericResponse.success(employeeService.deleteEmployee(id),"Deleted employee successfully."));
    }

}

```
