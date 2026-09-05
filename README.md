# Exp-04-Spring-Boot-with-REST-API-and-Hibernate-Integration

## AIM:
To develop a Spring Boot application to store and retrieve data from a Movies database using Object Relational Mapping (ORM) with Hibernate and expose it via REST APIs.

## ALGORITHM:
Create Spring Boot project with dependencies:

Spring Web

Spring Data JPA

H2 or MySQL Database

Configure application.properties with DB connection and JPA settings.

Create Movie entity with fields like id, title, genre, rating, and year.

Create MovieRepository interface extending JpaRepository.

Create MovieController to define REST endpoints for CRUD operations:

GET /movies

GET /movies/{id}

POST /movies

PUT /movies/{id}

DELETE /movies/{id}


## PROGRAM CODE (Main Files):
### application.properties
```
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```
### Movie.java
```
@Entity
public class Movie {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String title;
    private String genre;
    private int year;
    private double rating;

    // Getters and Setters
}
### MovieRepository.java
java
Copy
Edit
public interface MovieRepository extends JpaRepository<Movie, Long> {}
```

### MovieController.java
```
@RestController
@RequestMapping("/movies")
public class MovieController {
    @Autowired
    private MovieRepository repo;

    @GetMapping
    public List<Movie> getAllMovies() {
        return repo.findAll();
    }

    @GetMapping("/{id}")
    public ResponseEntity<Movie> getMovieById(@PathVariable Long id) {
        return repo.findById(id)
                .map(ResponseEntity::ok)
                .orElse(ResponseEntity.notFound().build());
    }

    @PostMapping
    public Movie addMovie(@RequestBody Movie movie) {
        return repo.save(movie);
    }

    @PutMapping("/{id}")
    public ResponseEntity<Movie> updateMovie(@PathVariable Long id, @RequestBody Movie movieDetails) {
        return repo.findById(id).map(movie -> {
            movie.setTitle(movieDetails.getTitle());
            movie.setGenre(movieDetails.getGenre());
            movie.setYear(movieDetails.getYear());
            movie.setRating(movieDetails.getRating());
            return ResponseEntity.ok(repo.save(movie));
        }).orElse(ResponseEntity.notFound().build());
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteMovie(@PathVariable Long id) {
        return repo.findById(id).map(movie -> {
            repo.delete(movie);
            return ResponseEntity.ok().build();
        }).orElse(ResponseEntity.notFound().build());
    }
}
```

### OUTPUT:

### POST /movies

<img width="1378" height="713" alt="646697155-cb6b928c-bd83-46f8-9d0c-56d59dca9458" src="https://github.com/user-attachments/assets/c1bf1aaa-f7ac-4fd7-b377-9d66ff6a0cc5" />

### GET /movies

<img width="1376" height="746" alt="646697192-a8e5f47c-c46f-4769-8021-783a71105744" src="https://github.com/user-attachments/assets/88a164a4-9dda-4066-a639-033e6eed1f2e" />


### PUT /movies

<img width="1377" height="743" alt="646697220-4d773863-62f3-4ec2-9fb1-04fef45ffeed" src="https://github.com/user-attachments/assets/bd01be95-e6db-4dae-aa06-16c59cf143c0" />


### DELETE /movies


<img width="1377" height="745" alt="image" src="https://github.com/user-attachments/assets/40690bb6-37d6-4386-a424-606227376042" />


### RESULT

Thus the development of a Spring Boot application to store and retrieve data from a Movies database is completed successfully.
