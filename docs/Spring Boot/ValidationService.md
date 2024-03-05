---
sidebar_position: 4
---

# Validation Service


### Buat Class nya (ValidationService.java)

```jsx
    import jakarta.validation.ConstraintViolation;
    import jakarta.validation.ConstraintViolationException;
    import jakarta.validation.Validator;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.stereotype.Service;

    import java.util.Set;

    @Service
    public class ValidataionService {

        @Autowired
        Validator validator;

        public void validate(Object request) {
            Set<ConstraintViolation<Object>> constraintViolations = validator.validate(request);
            if(constraintViolations.size() != 0) {
                throw new ConstraintViolationException(constraintViolations);
            }
        }

    }
```

nanti ini akan dipakai di semua validasi di service, contoh

``jsx
    @Service
public class UserService {

    @Autowired
    UserRepository userRepository;

    // ini diinject
    @Autowired
    ValidataionService validataionService;

    @Transactional
    public void register(RegisterUserRequest request) {
        // dipakai disini
        validataionService.validate(request);

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