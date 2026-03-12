MapStruct est une bibliothèque Java qui sert à mapper des objets Java entre eux
Entity → DTO
DTO → Entity

Exemple :

    @Entity
    public class User {
    
        private Long id;
        private String username;
        private String password;
    
    }
    
    public class UserDTO {
    
        private Long id;
        private String username;
    
    }

Sans Mapstruct :

pour convertir de entity vers dto

    UserDTO dto = new UserDTO();
    
    dto.setId(user.getId());
    dto.setUsername(user.getUsername());

Avec Mapstruct :

MapStruct génère automatiquement le code de mapping.

    @Mapper
    public interface UserMapper {
    
        UserDTO toDTO(User user);

        User toEntity(UserDTO dto);
    
    }

Architecture du projet :

Controller
↓
Service
↓
Mapper (MapStruct)
↓
Entity
↓
Database

    @Service
    public class UserService {
    
        @Autowired
        private UserMapper mapper;
    
        public UserDTO getUser(Long id) {
    
            User user = userRepository.findById(id).get();
    
            return mapper.toDTO(user);
    
        }
    }

MapStruct génère une classe UserMapperImpl au compile time

    @Mapping(source = "email", target = "userEmail") // si les champs n'ont pas le meme nom
    UserDTO toDTO(User user);

Controller
↓
DTO
↓
Mapper
↓
Entity
↓
Repository
↓
Database

