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
