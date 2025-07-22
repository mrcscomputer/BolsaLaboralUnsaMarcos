# Convenciones de Codificacion
1. Estándares Generales de Java
Clases: nombradas en PascalCase
Métodos: nombrados en camelCase
``` java
public class InactivarOfertaService {
    private final OfertaRepository ofertaRepository;

    public InactivarOfertaService(OfertaRepository ofertaRepository) {
        this.ofertaRepository = ofertaRepository;
    }

```
Variables: en camelCase con nombres descriptivos (ofertaRepository, createdAt, empresaTipoId).
Encapsulamiento: uso adecuado de private para atributos y public para métodos públicos.
JavaBeans: uso de get, set, e is para acceso a atributos.

# Estilos de programacion

1. Trinity: Estilo de capas usadas.
    Entrada (API), Procesamiento (Servicios), Datos (Repositorio).

2. RESTful: en esta aplicación web usamos recursos, verbos HTTP, rutas limpias
    @GetMapping, @PostMapping, @PutMapping, etc.
    Rutas limpias

3. Things: Se hace uso clases y objetos, se encapsula datos.

# Uso de SonarLint para clase Oferta.

```java
public Oferta(UUID id, String titulo, String descripcion, String area,
                  Double sueldo, LocalDateTime fecha, LocalDateTime createdAt,
                  LocalDateTime updatedAt, Boolean activa, Empresa empresa) {
        this.id = id;
```
Uso de patron de diseño buillder

```java
public static class Builder {
    private UUID id;
    private String titulo;
    private String descripcion;
    private String area;
    private Double sueldo;
    private LocalDateTime fecha;
    private LocalDateTime createdAt;
    private LocalDateTime updatedAt;
    private Boolean activa;
    private Empresa empresa;
```

# CLEAN CODE
1. Nombre
``` java
public class InactivarOfertaService {
    private final OfertaRepository ofertaRepository;
```
2. Funcion y estructura de codigo
```java
public Builder descripcion(String descripcion) {
    this.descripcion = descripcion;
    return this;
    }
```
3. Objeto y estructura de datos
```java
public class Oferta {

    @Id
    @GeneratedValue
    @Column(name = "id", nullable = false, updatable = false)
    private UUID id;

    @Column(name = "titulo", nullable = false, length = 100)
    private String titulo;

    @Column(name = "descripcion", columnDefinition = "TEXT")
    private String descripcion;

```

# PRINCIPIOS SOLID 

1. Responsabilidad Única: Cada clase del controlador maneja las solicitudes y delegar la lógica
```java
private final RegistrarOfertaService registrarOfertaService;
private final EditarOfertaService editarOfertaService;
...
@PostMapping
    public ResponseEntity<Oferta> registrar(@RequestBody Oferta oferta) {
        Oferta registrada = registrarOfertaService.ejecutar(oferta);
        return ResponseEntity.ok(registrada);
    }
```

2. Abierto-Cerrado: Se puede ampliar funciones mediante nuevos servicios sin la necesidad de modificar el controlador.
```java
public OfertaController(
    RegistrarOfertaService registrarOfertaService,
    EditarOfertaService editarOfertaService,
```

3. Inversión de Dependencias: El controlador depende de servicios

```java
private final RegistrarOfertaService registrarOfertaService;

public OfertaController(RegistrarOfertaService registrarOfertaService, ...) {
    this.registrarOfertaService = registrarOfertaService;
}

```
