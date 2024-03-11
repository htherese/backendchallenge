# backendchallenge
Adrian's challenge
// MainEntity.java
@Entity
public class MainEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    // other fields
    
    @OneToMany(mappedBy = "mainEntity", cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    private List<ChildEntity> childEntities;
    
    // getters and setters
}

// ChildEntity.java
@Entity
public class ChildEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    // other fields
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "main_entity_id")
    private MainEntity mainEntity;
    
    // getters and setters
}

// MainEntityController.java
@RestController
@RequestMapping("/main")
public class MainEntityController {
    @Autowired
    private MainEntityService mainEntityService;
    
    // Endpoint for inserting main entity
    @PostMapping("/")
    public ResponseEntity<MainEntity> insertMainEntity(@RequestBody MainEntity mainEntity) {
        MainEntity savedEntity = mainEntityService.insertMainEntity(mainEntity);
        return ResponseEntity.ok(savedEntity);
    }
    
    // Endpoint for updating main entity
    @PutMapping("/{id}")
    public ResponseEntity<MainEntity> updateMainEntity(@PathVariable Long id, @RequestBody MainEntity mainEntity) {
        MainEntity updatedEntity = mainEntityService.updateMainEntity(id, mainEntity);
        return ResponseEntity.ok(updatedEntity);
    }
    
    // Exception handling can be done using @ExceptionHandler or @ControllerAdvice
}

// MainEntityService.java
@Service
public class MainEntityService {
    @Autowired
    private MainEntityRepository mainEntityRepository;
    
    // Method to insert main entity
    @Transactional
    public MainEntity insertMainEntity(MainEntity mainEntity) {
        // implementation
    }
    
    // Method to update main entity
    @Transactional
    public MainEntity updateMainEntity(Long id, MainEntity mainEntity) {
        // implementation
    }
    
    // Optimistic locking can be handled here
}

// MainEntityRepository.java
@Repository
public interface MainEntityRepository extends JpaRepository<MainEntity, Long> {
    // custom queries if needed
}

// Unit tests for MainEntityService

// Integration tests for MainEntityController (mocking the service and repository)

// Dockerfile
FROM maven:3.8.4-openjdk-17-slim AS build
WORKDIR /app
COPY . .
RUN mvn clean package -DskipTests

FROM openjdk:17-slim
COPY --from=build /app/target/my-application.jar /app/my-application.jar
ENTRYPOINT ["java", "-jar", "/app/my-application.jar"]
