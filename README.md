## Annotations

@Entity   << Entidade do banco de dados`

@RestContoller << Usado para retornar JSON ou XML`

@Controller <<  Usamos para retornar um `views`  normalmente um `HTML`

@Autowired << Serve pro Spring fazer automaticamente  a injeção de dependência 

@Injeção de dependências Exemplo com garçom:  
Sem injeção de dependência, o garçom tem a cozinha embutida, conhecendo detalhes específicos da preparação dos pratos. Com injeção de dependência, o garçom recebe a cozinha como uma peça externa, tornando-o mais flexível para lidar com diferentes métodos de preparo, promovendo uma melhor manutenção e teste.

@RequestMapping: Especifica o mapeamento entre as solicitações HTTP e os métodos do controlador. Pode ser usado a nível de classe ou método.

@PostMapping: Usada para mapear um método como manipulador de solicitações HTTP POST. Geralmente, é utilizado para criar ou processar dados enviados no corpo da solicitação. Exemplo:

@GetMapping : Especifica o mapeamento entre as solicitações HTTP e os métodos do controlador. Pode ser usado a nível de classe ou método.

@PutMapping : Especifica o mapeamento entre as solicitações HTTP e os métodos do controlador. Pode ser usado a nível de classe ou método.

@DeleteMapping : Especifica o mapeamento entre as solicitações HTTP e os métodos do controlador. Pode ser usado a nível de classe ou método.

@Id: Anotação usada em classes de entidade no contexto do Spring Data JPA. Indica que um campo específico dentro da classe representa a chave primária da entidade no banco de dados. Essa anotação é crucial para identificar exclusivamente as instâncias da entidade no banco de dados. 

@Value: Injeta valores de propriedades diretamente em campos.

@Service: Anotação usada para marcar uma classe de serviço no padrão de design Service.

## Estrutura de um projeto Spring.
- **src**
  - **main**
    - **java**
      - **controller**
        - Classes que atuam como controladores na arquitetura MVC.
          - Anotações comuns: `@Controller` ou `@RestController`.
      - **service**
        - Classes que fornecem lógica de negócios.
          - Anotação comum: `@Service`.
      - **repository**
        - Interfaces que estendem frameworks como Spring Data JPA.
          - Anotação comum: `@Repository`.
      - **model**s
        - Classes que representam entidades de dados (geralmente mapeadas para tabelas em um banco de dados).
          - Anotação comum: `@Entity`.
      - **dto**s
        - Classes de Transferência de Dados (DTO) para transferir dados entre camadas.
          - Não requer anotações específicas do Spring.
    - **resources**
      - **config**
        - Classes de configuração.
          - Anotação comum: `@Configuration`.

## Controller
```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    private final UserService userService;

    @Autowired
    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping("/{userId}")
    public ResponseEntity<UserDTO> getUserById(@PathVariable Long userId) {
        UserDTO userDTO = userService.getUserById(userId);
        if (userDTO != null) {
            return ResponseEntity.ok(userDTO);
        } else {
            return ResponseEntity.notFound().build();
        }
    }
}
```

## Service 
```java
@Service
public class UserService {

    private final UserRepository userRepository;

    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public UserDTO getUserById(Long userId) {
        Optional<User> optionalUser = userRepository.findById(userId);
        return optionalUser.map(this::convertToDTO).orElse(null);
    }

    private UserDTO convertToDTO(User user) {
        // Lógica de conversão de User para UserDTO
        return new UserDTO(user.getId(), user.getUsername(), user.getEmail());
    }
}
```

## Repository
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {

    // Métodos personalizados do repositório, se necessário
    Optional<User> findByUsername(String username);
}
```

## Models
```java
@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String username;
    private String email;

    // Getters e Setters

    // Outros métodos e lógica, se necessário
}
```

## DTOS
```java
public record UserDTO(Long id, String username, String email) {
// Você ainda pode adicionar métodos adicionais ou personalizar o comportamento aqui
}

```
## Config
```java
@Configuration
public class AppConfig {

    // Configurações do aplicativo aqui

    // Métodos de configuração, se necessário
}
```
