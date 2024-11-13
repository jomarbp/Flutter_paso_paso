*CRUD COMPLETO DE TIENDA*

**1. Agregar el siguiente código "main.dart"**
Escribir el comando: 

Linea::

  import 'package:flutter/material.dart';
  import 'package:google_fonts/google_fonts.dart';
  import 'package:apptienda/src/screens/categorias/crear_categoria.dart';
  import 'package:apptienda/src/screens/categorias/editar_categoria.dart';
  import 'package:apptienda/src/screens/categorias/lista_categorias.dart';
  import 'package:apptienda/src/screens/productos/crear_producto.dart';
  import 'package:apptienda/src/screens/productos/editar_producto.dart';
  import 'package:apptienda/src/screens/productos/lista_productos.dart';
  import 'package:apptienda/src/screens/proveedores/crear_proveedor.dart';
  import 'package:apptienda/src/screens/proveedores/editar_proveedor.dart';
  import 'package:apptienda/src/screens/proveedores/lista_proveedores.dart';
  import 'package:apptienda/src/screens/usuarios/lista_usuarios.dart';
  import 'package:apptienda/src/screens/usuarios/crear_usuario.dart';
  import 'package:apptienda/src/screens/usuarios/editar_usuario.dart';
  import 'package:apptienda/src/screens/auth/login_screen.dart';
  import 'package:shared_preferences/shared_preferences.dart';
  import 'dart:convert';
  
  void main() {
    runApp(MyApp());
  }
  
  class MyApp extends StatelessWidget {
    const MyApp({super.key});
  
    @override
    Widget build(BuildContext context) {
      return MaterialApp(
        title: 'Tienda App',
        theme: ThemeData(
          primarySwatch: Colors.blue,
          textTheme: GoogleFonts.poppinsTextTheme(),
          elevatedButtonTheme: ElevatedButtonThemeData(
            style: ElevatedButton.styleFrom(
              elevation: 8,
              shape: RoundedRectangleBorder(
                borderRadius: BorderRadius.circular(15),
              ),
            ),
          ),
          cardTheme: CardTheme(
            elevation: 5,
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(15),
            ),
          ),
        ),
        debugShowCheckedModeBanner: false,
        initialRoute: '/login',
        routes: {
          '/login': (context) => LoginScreen(),
          '/': (context) => HomePage(),
          'lista_categorias': (context) => ListaCategorias(),
          'crear_categoria': (context) => CrearCategoria(),
          'editar_categoria': (context) => EditarCategoria(),
          'lista_productos': (context) => ListaProductos(),
          'crear_producto': (context) => CrearProducto(),
          'editar_producto': (context) => EditarProducto(),
          'lista_proveedores': (context) => ListaProveedores(),
          'crear_proveedor': (context) => CrearProveedor(),
          'editar_proveedor': (context) => EditarProveedor(),
          'lista_usuarios': (context) => ListaUsuarios(),
           'crear_usuario': (context) => CrearUsuario(),
           'editar_usuario': (context) => EditarUsuario(), // Agregar esta línea
        },
      );
    }
  }
  
  class HomePage extends StatefulWidget {
    @override
    _HomePageState createState() => _HomePageState();
  }
  
  class _HomePageState extends State<HomePage> {
    Map<String, dynamic>? usuario;
    bool isAdmin = false;
  
    @override
    void initState() {
      super.initState();
      _cargarUsuario();
    }
  
    Future<void> _cargarUsuario() async {
      final prefs = await SharedPreferences.getInstance();
      final usuarioString = prefs.getString('usuario');
      if (usuarioString != null) {
        setState(() {
          usuario = json.decode(usuarioString);
          isAdmin = usuario?['rol'] == 'admin';
        });
      }
    }
  
    Future<void> _cerrarSesion() async {
      final prefs = await SharedPreferences.getInstance();
      await prefs.clear();
      Navigator.pushReplacementNamed(context, '/login');
    }
  
    @override
    Widget build(BuildContext context) {
      return Scaffold(
        body: Container(
          decoration: BoxDecoration(
            gradient: LinearGradient(
              begin: Alignment.topCenter,
              end: Alignment.bottomCenter,
              colors: [Colors.blue.shade50, Colors.white],
            ),
          ),
          child: SafeArea(
            child: Column(
              children: [
                // Header con información del usuario
                Container(
                  width: double.infinity,
                  padding: EdgeInsets.all(20),
                  decoration: BoxDecoration(
                    color: Colors.white,
                    borderRadius: BorderRadius.only(
                      bottomLeft: Radius.circular(30),
                      bottomRight: Radius.circular(30),
                    ),
                    boxShadow: [
                      BoxShadow(
                        color: Colors.grey.withOpacity(0.2),
                        spreadRadius: 2,
                        blurRadius: 5,
                        offset: Offset(0, 3),
                      ),
                    ],
                  ),
                  child: Column(
                    children: [
                      Row(
                        mainAxisAlignment: MainAxisAlignment.spaceBetween,
                        children: [
                          Column(
                            crossAxisAlignment: CrossAxisAlignment.start,
                            children: [
                              Text(
                                'Bienvenido,',
                                style: TextStyle(
                                  fontSize: 16,
                                  color: Colors.grey[600],
                                ),
                              ),
                              Text(
                                usuario?['nombre'] ?? '',
                                style: TextStyle(
                                  fontSize: 24,
                                  fontWeight: FontWeight.bold,
                                  color: Colors.blue.shade900,
                                ),
                              ),
                            ],
                          ),
                          PopupMenuButton<String>(
                            onSelected: (value) async {
                              if (value == 'logout') {
                                await _cerrarSesion();
                              }
                            },
                            itemBuilder: (BuildContext context) => [
                              PopupMenuItem(
                                value: 'logout',
                                child: Row(
                                  children: [
                                    Icon(Icons.logout),
                                    SizedBox(width: 8),
                                    Text('Cerrar Sesión'),
                                  ],
                                ),
                              ),
                            ],
                            child: CircleAvatar(
                              backgroundColor: Colors.blue,
                              child: Icon(Icons.person, color: Colors.white),
                            ),
                          ),
                        ],
                      ),
                      SizedBox(height: 20),
                      Image.asset(
                        'assets/images/logo.png',
                        height: 120,
                        errorBuilder: (context, error, stackTrace) {
                          return Icon(
                            Icons.store,
                            size: 120,
                            color: Colors.blue,
                          );
                        },
                      ),
                      SizedBox(height: 20),
                      Text(
                        'Sistema de Gestión',
                        style: TextStyle(
                          fontSize: 24,
                          fontWeight: FontWeight.bold,
                          color: Colors.blue.shade900,
                        ),
                      ),
                    ],
                  ),
                ),
  
                // Menú principal
                Expanded(
                  child: Padding(
                    padding: EdgeInsets.all(20),
                    child: GridView.count(
                      crossAxisCount: 2,
                      crossAxisSpacing: 20,
                      mainAxisSpacing: 20,
                      children: [
                        _buildMenuCard(
                          context,
                          'Categorías',
                          Icons.category,
                          Colors.blue,
                          'lista_categorias',
                        ),
                        _buildMenuCard(
                          context,
                          'Productos',
                          Icons.inventory,
                          Colors.green,
                          'lista_productos',
                        ),
                        _buildMenuCard(
                          context,
                          'Proveedores',
                          Icons.assignment_ind_outlined,
                          const Color.fromARGB(255, 232, 128, 62),
                          'lista_proveedores',
                        ),
                        if (isAdmin) // Solo mostrar si es administrador
                          _buildMenuCard(
                            context,
                            'Usuarios',
                            Icons.people,
                            Colors.purple,
                            'lista_usuarios',
                          ),
                      ],
                    ),
                  ),
                ),
  
                // Footer
                Padding(
                  padding: EdgeInsets.all(20),
                  child: Text(
                    '© 2024 Tienda App',
                    style: TextStyle(
                      color: Colors.grey,
                      fontSize: 12,
                    ),
                  ),
                ),
              ],
            ),
          ),
        ),
      );
    }
  
    Widget _buildMenuCard(
        BuildContext context, String title, IconData icon, Color color, String route) {
      return Card(
        elevation: 5,
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.circular(15),
        ),
        child: InkWell(
          onTap: () => Navigator.pushNamed(context, route),
          borderRadius: BorderRadius.circular(15),
          child: Container(
            decoration: BoxDecoration(
              gradient: LinearGradient(
                begin: Alignment.topLeft,
                end: Alignment.bottomRight,
                colors: [
                  color.withOpacity(0.7),
                  color,
                ],
              ),
              borderRadius: BorderRadius.circular(15),
            ),
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                Container(
                  padding: EdgeInsets.all(15),
                  decoration: BoxDecoration(
                    color: Colors.white.withOpacity(0.2),
                    shape: BoxShape.circle,
                  ),
                  child: Icon(
                    icon,
                    size: 40,
                    color: Colors.white,
                  ),
                ),
                SizedBox(height: 15),
                Text(
                  title,
                  style: TextStyle(
                    color: Colors.white,
                    fontSize: 16,
                    fontWeight: FontWeight.bold,
                  ),
                ),
                SizedBox(height: 5),
                Text(
                  'Gestionar',
                  style: TextStyle(
                    color: Colors.white.withOpacity(0.8),
                    fontSize: 12,
                  ),
                ),
              ],
            ),
          ),
        ),
      );
    }
  }
**2. lib/src/screens/auth/login_screen.dart**

Linea::

  import 'package:flutter/material.dart';
  import 'package:http/http.dart' as http;
  import 'dart:convert';
  import 'package:shared_preferences/shared_preferences.dart';
  
  class LoginScreen extends StatefulWidget {
    @override
    _LoginScreenState createState() => _LoginScreenState();
  }
  
  class _LoginScreenState extends State<LoginScreen> {
    final _formKey = GlobalKey<FormState>();
    final _emailController = TextEditingController();
    final _passwordController = TextEditingController();
    bool _isLoading = false;
    bool _obscurePassword = true;
  
    Future<void> _login() async {
      if (_formKey.currentState!.validate()) {
        setState(() {
          _isLoading = true;
        });
        try {
          final response = await http.post(
            Uri.parse('http://localhost/tienda/api_tienda.php?resource=auth/login'),
            headers: {'Content-Type': 'application/json'},
            body: jsonEncode({
              'email': _emailController.text,
              'password': _passwordController.text,
            }),
          );
          final data = jsonDecode(response.body);
          if (response.statusCode == 200 && data['success']) {
            // Guardar el token y datos del usuario
            final prefs = await SharedPreferences.getInstance();
            await prefs.setString('token', data['token']);
            await prefs.setString('usuario', jsonEncode(data['usuario']));
  
            Navigator.pushReplacementNamed(context, '/');
          } else {
            ScaffoldMessenger.of(context).showSnackBar(
              SnackBar(
                content: Text(data['message'] ?? 'Error al iniciar sesión'),
                backgroundColor: Colors.red,
              ),
            );
          }
        } catch (e) {
          ScaffoldMessenger.of(context).showSnackBar(
            SnackBar(
              content: Text('Error de conexión'),
              backgroundColor: Colors.red,
            ),
          );
        } finally {
          setState(() {
            _isLoading = false;
          });
        }
      }
    }
  
  
    @override
    Widget build(BuildContext context) {
      return Scaffold(
        body: Container(
          decoration: BoxDecoration(
            gradient: LinearGradient(
              begin: Alignment.topCenter,
              end: Alignment.bottomCenter,
              colors: [Colors.blue.shade50, Colors.white],
            ),
          ),
          child: SafeArea(
            child: Center(
              child: SingleChildScrollView(
                padding: EdgeInsets.all(24),
                child: Column(
                  mainAxisAlignment: MainAxisAlignment.center,
                  children: [
                    // Logo o Ícono
                    Container(
                      padding: EdgeInsets.all(16),
                      decoration: BoxDecoration(
                        shape: BoxShape.circle,
                        color: Colors.white,
                        boxShadow: [
                          BoxShadow(
                            color: Colors.blue.withOpacity(0.1),
                            spreadRadius: 3,
                            blurRadius: 10,
                          ),
                        ],
                      ),
                      child: Icon(
                        Icons.store,
                        size: 80,
                        color: Colors.blue,
                      ),
                    ),
                    SizedBox(height: 40),
                    // Título
                    Text(
                      'Bienvenido',
                      style: TextStyle(
                        fontSize: 32,
                        fontWeight: FontWeight.bold,
                        color: Colors.blue.shade900,
                      ),
                    ),
                    SizedBox(height: 8),
                    Text(
                      'Inicia sesión para continuar',
                      style: TextStyle(
                        fontSize: 16,
                        color: Colors.grey[600],
                      ),
                    ),
                    SizedBox(height: 40),
                    // Formulario
                    Card(
                      elevation: 5,
                      shape: RoundedRectangleBorder(
                        borderRadius: BorderRadius.circular(15),
                      ),
                      child: Padding(
                        padding: EdgeInsets.all(24),
                        child: Form(
                          key: _formKey,
                          child: Column(
                            children: [
                              TextFormField(
                                controller: _emailController,
                                decoration: InputDecoration(
                                  labelText: 'Correo electrónico',
                                  prefixIcon: Icon(Icons.email),
                                  border: OutlineInputBorder(
                                    borderRadius: BorderRadius.circular(10),
                                  ),
                                ),
                                keyboardType: TextInputType.emailAddress,
                                validator: (value) {
                                  if (value == null || value.isEmpty) {
                                    return 'Por favor ingrese su correo';
                                  }
                                  if (!RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$')
                                      .hasMatch(value)) {
                                    return 'Ingrese un correo válido';
                                  }
                                  return null;
                                },
                              ),
                              SizedBox(height: 20),
                              TextFormField(
                                controller: _passwordController,
                                decoration: InputDecoration(
                                  labelText: 'Contraseña',
                                  prefixIcon: Icon(Icons.lock),
                                  suffixIcon: IconButton(
                                    icon: Icon(
                                      _obscurePassword
                                          ? Icons.visibility
                                          : Icons.visibility_off,
                                    ),
                                    onPressed: () {
                                      setState(() {
                                        _obscurePassword = !_obscurePassword;
                                      });
                                    },
                                  ),
                                  border: OutlineInputBorder(
                                    borderRadius: BorderRadius.circular(10),
                                  ),
                                ),
                                obscureText: _obscurePassword,
                                validator: (value) {
                                  if (value == null || value.isEmpty) {
                                    return 'Por favor ingrese su contraseña';
                                  }
                                  return null;
                                },
                              ),
                              SizedBox(height: 24),
                              ElevatedButton(
                                onPressed: _isLoading ? null : _login,
                                child: _isLoading
                                    ? SizedBox(
                                        height: 20,
                                        width: 20,
                                        child: CircularProgressIndicator(
                                          strokeWidth: 2,
                                          valueColor:
                                              AlwaysStoppedAnimation<Color>(
                                                  Colors.white),
                                        ),
                                      )
                                    : Text('Iniciar Sesión'),
                                style: ElevatedButton.styleFrom(
                                  minimumSize: Size(double.infinity, 50),
                                  shape: RoundedRectangleBorder(
                                    borderRadius: BorderRadius.circular(10),
                                  ),
                                ),
                              ),
                            ],
                          ),
                        ),
                      ),
                    ),
                  ],
                ),
              ),
            ),
          ),
        ),
      );
    }
  }

3. **C:/xampp/htdocs/tienda/api_tienda.php**

Linea::

    <?php
    // Configuración de CORS
    header('Access-Control-Allow-Origin: *');
    header('Access-Control-Allow-Methods: GET, POST, PUT, DELETE, OPTIONS');
    header('Access-Control-Allow-Headers: Origin, Content-Type, X-Auth-Token, Authorization, X-Requested-With');
    header('Access-Control-Expose-Headers: Content-Length, X-JSON');
    header('Access-Control-Max-Age: 86400');
    
    // Manejar solicitudes OPTIONS
    if ($_SERVER['REQUEST_METHOD'] == 'OPTIONS') {
       header('HTTP/1.1 200 OK');
       exit();
    }
    
    // Configuración de errores y logs
    error_reporting(E_ALL);
    ini_set('display_errors', 1);
    ini_set('log_errors', 1);
    ini_set('error_log', __DIR__ . '/error.log');
    
    // Conexión a la base de datos
    $conn = new mysqli("localhost", "root", "", "tienda_app");
    if ($conn->connect_error) {
       error_log("Error de conexión: " . $conn->connect_error);
       die('Conexión Fallida: ' . $conn->connect_error);
    }
    
    $resource = isset($_GET['resource']) ? $_GET['resource'] : '';
    
    // Determinar el método real
    $method = $_SERVER['REQUEST_METHOD'];
    if ($method === 'POST' && isset($_GET['_method'])) {
       $method = strtoupper($_GET['_method']);
    }
    
    error_log("Método detectado: " . $method);
    error_log("Resource: " . $resource);
    
    switch ($resource) {
        // Añadir al switch principal en api_tienda.php
        case 'auth/login':
        handleLogin($method, $conn);
        break;
    
        case 'usuarios':
        handleUsuarios($method, $conn);
        break;
    
       case 'categorias':
           handleCategorias($method, $conn);
           break;
       case 'productos':
           handleProductos($method, $conn);
           break;
       case 'proveedores':
           handleProveedores($method, $conn);
           break;
       default:
           echo json_encode(["error" => "Recurso no encontrado"]);
           break;
    }
    
    // Añadir esta función
    function handleLogin($method, $conn) {
        if ($method !== 'POST') {
            http_response_code(405);
            echo json_encode(['error' => 'Método no permitido']);
            return;
        }
    
        $data = json_decode(file_get_contents("php://input"), true);
        
        if (!isset($data['email']) || !isset($data['password'])) {
            http_response_code(400);
            echo json_encode(['error' => 'Datos incompletos']);
            return;
        }
    
        $email = $data['email'];
        $password = $data['password'];
    
        $stmt = $conn->prepare("SELECT id, nombre, email, password, rol FROM usuarios WHERE email = ? AND estado = 1");
        $stmt->bind_param("s", $email);
        $stmt->execute();
        $result = $stmt->get_result();
    
        if ($result->num_rows === 0) {
            http_response_code(401);
            echo json_encode(['success' => false, 'message' => 'Credenciales incorrectas']);
            return;
        }
    
        $usuario = $result->fetch_assoc();
    
        if (!password_verify($password, $usuario['password'])) {
            http_response_code(401);
            echo json_encode(['success' => false, 'message' => 'Credenciales incorrectas']);
            return;
        }
    
        // Generar token JWT o similar
        $token = bin2hex(random_bytes(32));
    
        // Actualizar último acceso
        $stmt = $conn->prepare("UPDATE usuarios SET ultimo_acceso = CURRENT_TIMESTAMP WHERE id = ?");
        $stmt->bind_param("i", $usuario['id']);
        $stmt->execute();
    
        // Eliminar password del resultado
        unset($usuario['password']);
    
        echo json_encode([
            'success' => true,
            'message' => 'Login exitoso',
            'token' => $token,
            'usuario' => $usuario
        ]);
    }
    
    
    // Agregar esta función
    function handleUsuarios($method, $conn) {
        switch ($method) {
            case 'GET':
                $sql = "SELECT id, nombre, email, rol, estado, ultimo_acceso FROM usuarios WHERE estado = 1";
                $result = $conn->query($sql);
                $usuarios = [];
                while($row = $result->fetch_assoc()) {
                    unset($row['password']); // No enviar la contraseña
                    $usuarios[] = $row;
                }
                echo json_encode($usuarios);
                break;
    
            case 'POST':
                $data = json_decode(file_get_contents("php://input"), true);
                
                // Verificar si el email ya existe
                $stmt = $conn->prepare("SELECT id FROM usuarios WHERE email = ? AND estado = 1");
                $stmt->bind_param("s", $data['email']);
                $stmt->execute();
                if($stmt->get_result()->num_rows > 0) {
                    http_response_code(400);
                    echo json_encode(["error" => "El email ya está registrado"]);
                    return;
                }
    
                // Encriptar contraseña
                $password = password_hash($data['password'], PASSWORD_DEFAULT);
                
                $stmt = $conn->prepare("INSERT INTO usuarios (nombre, email, password, rol) VALUES (?, ?, ?, ?)");
                $stmt->bind_param("ssss", $data['nombre'], $data['email'], $password, $data['rol']);
                
                if($stmt->execute()) {
                    echo json_encode(["message" => "Usuario creado con éxito"]);
                } else {
                    http_response_code(500);
                    echo json_encode(["error" => "Error al crear usuario"]);
                }
                break;
    
            case 'PUT':
                $data = json_decode(file_get_contents("php://input"), true);
                
                // Verificar email duplicado excluyendo el usuario actual
                $stmt = $conn->prepare("SELECT id FROM usuarios WHERE email = ? AND id != ? AND estado = 1");
                $stmt->bind_param("si", $data['email'], $data['id']);
                $stmt->execute();
                if($stmt->get_result()->num_rows > 0) {
                    http_response_code(400);
                    echo json_encode(["error" => "El email ya está registrado"]);
                    return;
                }
    
                $sql = "UPDATE usuarios SET nombre = ?, email = ?, rol = ?";
                $params = [$data['nombre'], $data['email'], $data['rol']];
                $types = "sss";
    
                if(!empty($data['password'])) {
                    $password = password_hash($data['password'], PASSWORD_DEFAULT);
                    $sql .= ", password = ?";
                    $params[] = $password;
                    $types .= "s";
                }
    
                $sql .= " WHERE id = ?";
                $params[] = $data['id'];
                $types .= "i";
    
                $stmt = $conn->prepare($sql);
                $stmt->bind_param($types, ...$params);
                
                if($stmt->execute()) {
                    echo json_encode(["message" => "Usuario actualizado con éxito"]);
                } else {
                    http_response_code(500);
                    echo json_encode(["error" => "Error al actualizar usuario"]);
                }
                break;
    
                case 'DELETE':
                $id = $_GET['id'];
                
                // Verificar que no sea el último administrador
                $stmt = $conn->prepare("SELECT COUNT(*) as admin_count FROM usuarios WHERE rol = 'admin' AND estado = 1");
                $stmt->execute();
                $result = $stmt->get_result();
                $admin_count = $result->fetch_assoc()['admin_count'];
    
                $stmt = $conn->prepare("SELECT rol FROM usuarios WHERE id = ?");
                $stmt->bind_param("i", $id);
                $stmt->execute();
                $result = $stmt->get_result();
                $usuario = $result->fetch_assoc();
    
                if ($usuario['rol'] == 'admin' && $admin_count <= 1) {
                    http_response_code(400);
                    echo json_encode(["error" => "No se puede eliminar el último administrador"]);
                    return;
                }
    
                $stmt = $conn->prepare("UPDATE usuarios SET estado = 0 WHERE id = ?");
                $stmt->bind_param("i", $id);
                if ($stmt->execute()) {
                    echo json_encode(["message" => "Usuario eliminado con éxito"]);
                } else {
                    http_response_code(500);
                    echo json_encode(["error" => "Error al eliminar usuario"]);
                }
                break;
            
        }
    }
    
    
    function handleCategorias($method, $conn) {
       switch ($method) {
           case 'GET':
               $sql = "SELECT * FROM categorias";
               $result = $conn->query($sql);
               $categorias = [];
               while($row = $result->fetch_assoc()) {
                   $categorias[] = $row;
               }
               echo json_encode($categorias);
               break;
    
           case 'POST':
               $data = json_decode(file_get_contents("php://input"), true);
               $stmt = $conn->prepare("INSERT INTO categorias (nombre, descripcion) VALUES (?, ?)");
               $stmt->bind_param("ss", $data['nombre'], $data['descripcion']);
               
               if($stmt->execute()) {
                   echo json_encode(["message" => "Categoría creada con éxito"]);
               } else {
                   error_log("Error al crear categoría: " . $stmt->error);
                   echo json_encode(["error" => $stmt->error]);
               }
               break;
    
           case 'PUT':
               $data = json_decode(file_get_contents("php://input"), true);
               $stmt = $conn->prepare("UPDATE categorias SET nombre = ?, descripcion = ? WHERE id = ?");
               $stmt->bind_param("ssi", $data['nombre'], $data['descripcion'], $data['id']);
               
               if($stmt->execute()) {
                   echo json_encode(["message" => "Categoría actualizada con éxito"]);
               } else {
                   error_log("Error al actualizar categoría: " . $stmt->error);
                   echo json_encode(["error" => $stmt->error]);
               }
               break;
    
           case 'DELETE':
               $id = $_GET['id'];
               $stmt = $conn->prepare("DELETE FROM categorias WHERE id = ?");
               $stmt->bind_param("i", $id);
               
               if($stmt->execute()) {
                   echo json_encode(["message" => "Categoría eliminada con éxito"]);
               } else {
                   error_log("Error al eliminar categoría: " . $stmt->error);
                   echo json_encode(["error" => $stmt->error]);
               }
               break;
       }
    }
    
    function handleProveedores($method, $conn) {
       switch ($method) {
           case 'GET':
               $sql = "SELECT * FROM proveedores WHERE estado = 1";
               $result = $conn->query($sql);
               $proveedores = [];
               while($row = $result->fetch_assoc()) {
                   $proveedores[] = $row;
               }
               echo json_encode($proveedores);
               break;
    
           case 'POST':
               $data = json_decode(file_get_contents("php://input"), true);
               
               // Verificar si el RUC ya existe
               $stmt = $conn->prepare("SELECT id FROM proveedores WHERE ruc = ? AND estado = 1");
               $stmt->bind_param("s", $data['ruc']);
               $stmt->execute();
               $result = $stmt->get_result();
               
               if ($result->num_rows > 0) {
                   http_response_code(400);
                   echo json_encode(["error" => "El RUC ya está registrado"]);
                   return;
               }
               
               $stmt = $conn->prepare("INSERT INTO proveedores (ruc, razon_social, direccion, telefono, email, persona_contacto) VALUES (?, ?, ?, ?, ?, ?)");
               $stmt->bind_param("ssssss",
                   $data['ruc'],
                   $data['razon_social'],
                   $data['direccion'],
                   $data['telefono'],
                   $data['email'],
                   $data['persona_contacto']
               );
               
               if($stmt->execute()) {
                   echo json_encode(["message" => "Proveedor creado con éxito"]);
               } else {
                   error_log("Error al crear proveedor: " . $stmt->error);
                   http_response_code(500);
                   echo json_encode(["error" => $stmt->error]);
               }
               break;
    
           case 'PUT':
               $data = json_decode(file_get_contents("php://input"), true);
               
               // Verificar RUC duplicado excluyendo el registro actual
               $stmt = $conn->prepare("SELECT id FROM proveedores WHERE ruc = ? AND id != ? AND estado = 1");
               $stmt->bind_param("si", $data['ruc'], $data['id']);
               $stmt->execute();
               $result = $stmt->get_result();
               
               if ($result->num_rows > 0) {
                   http_response_code(400);
                   echo json_encode(["error" => "El RUC ya está registrado para otro proveedor"]);
                   return;
               }
               
               $stmt = $conn->prepare("UPDATE proveedores SET ruc = ?, razon_social = ?, direccion = ?, telefono = ?, email = ?, persona_contacto = ? WHERE id = ?");
               $stmt->bind_param("ssssssi",
                   $data['ruc'],
                   $data['razon_social'],
                   $data['direccion'],
                   $data['telefono'],
                   $data['email'],
                   $data['persona_contacto'],
                   $data['id']
               );
               
               if($stmt->execute()) {
                   echo json_encode(["message" => "Proveedor actualizado con éxito"]);
               } else {
                   error_log("Error al actualizar proveedor: " . $stmt->error);
                   http_response_code(500);
                   echo json_encode(["error" => $stmt->error]);
               }
               break;
    
           case 'DELETE':
               $id = $_GET['id'];
               $stmt = $conn->prepare("UPDATE proveedores SET estado = 0 WHERE id = ?");
               $stmt->bind_param("i", $id);
               
               if($stmt->execute()) {
                   echo json_encode(["message" => "Proveedor eliminado con éxito"]);
               } else {
                   error_log("Error al eliminar proveedor: " . $stmt->error);
                   http_response_code(500);
                   echo json_encode(["error" => $stmt->error]);
               }
               break;
       }
    }
    
    function handleProductos($method, $conn) {
        error_log("\n=== INICIO HANDLE PRODUCTOS ===");
        error_log("Método recibido: " . $method);
       
       switch ($method) {
    
        case 'PUT':
        try {
            if (!isset($_POST['id'])) {
                throw new Exception("ID no proporcionado");
            }
    
            // Validar y sanitizar datos básicos
            $id = intval($_POST['id']);
            $nombre = $conn->real_escape_string($_POST['nombre']);
            $descripcion = $conn->real_escape_string($_POST['descripcion']);
            $precio = floatval($_POST['precio']);
            $stock = intval($_POST['stock']);
            $categoria_id = !empty($_POST['categoria_id']) ? intval($_POST['categoria_id']) : null;
            $proveedor_id = !empty($_POST['proveedor_id']) ? intval($_POST['proveedor_id']) : null;
            
            // Obtener la imagen actual del producto
            $stmt = $conn->prepare("SELECT imagen_url FROM productos WHERE id = ?");
            $stmt->bind_param("i", $id);
            $stmt->execute();
            $result = $stmt->get_result();
            $producto_actual = $result->fetch_assoc();
            $imagen_url_actual = $producto_actual['imagen_url'];
            
            // Inicializar variable para la nueva imagen_url
            $imagen_url = isset($_POST['imagen_url']) ? $_POST['imagen_url'] : null;
            
            // Si se envió una nueva imagen
            if (isset($_FILES['imagen']) && $_FILES['imagen']['error'] === UPLOAD_ERR_OK) {
                $imagen = $_FILES['imagen'];
                $imagen_nombre = time() . '_' . basename($imagen['name']);
                $ruta_destino = 'imagenes/' . $imagen_nombre;
                
                // Crear directorio si no existe
                if (!file_exists('imagenes')) {
                    mkdir('imagenes', 0777, true);
                }
                
                // Eliminar imagen anterior si existe
                if ($imagen_url_actual) {
                    $ruta_anterior = __DIR__ . str_replace('/tienda', '', $imagen_url_actual);
                    error_log("Intentando eliminar imagen anterior: " . $ruta_anterior);
                    if (file_exists($ruta_anterior)) {
                        if (unlink($ruta_anterior)) {
                            error_log("Imagen anterior eliminada con éxito");
                        } else {
                            error_log("No se pudo eliminar la imagen anterior");
                        }
                    }
                }
                
                // Subir nueva imagen
                if (move_uploaded_file($imagen['tmp_name'], $ruta_destino)) {
                    $imagen_url = '/tienda/' . $ruta_destino;
                    error_log("Nueva imagen guardada en: " . $imagen_url);
                } else {
                    throw new Exception("Error al guardar la nueva imagen");
                }
            }
    
            // Actualizar el producto en la base de datos
            $sql = "UPDATE productos SET 
                    nombre = ?,
                    descripcion = ?,
                    precio = ?,
                    stock = ?,
                    categoria_id = ?,
                    proveedor_id = ?,
                    imagen_url = ?,
                    updated_at = CURRENT_TIMESTAMP
                    WHERE id = ?";
    
            $stmt = $conn->prepare($sql);
            if (!$stmt) {
                throw new Exception("Error en preparación: " . $conn->error);
            }
    
            $stmt->bind_param("ssdiissi",
                $nombre,
                $descripcion,
                $precio,
                $stock,
                $categoria_id,
                $proveedor_id,
                $imagen_url,
                $id
            );
    
            if (!$stmt->execute()) {
                throw new Exception("Error al ejecutar la actualización: " . $stmt->error);
            }
    
            echo json_encode([
                "success" => true,
                "message" => "Producto actualizado con éxito",
                "imagen_url" => $imagen_url
            ]);
    
        } catch (Exception $e) {
            error_log("Error en actualización de producto: " . $e->getMessage());
            http_response_code(500);
            echo json_encode([
                "success" => false,
                "error" => $e->getMessage()
            ]);
        }
        break;
    
                
           case 'GET':
               $sql = "SELECT p.*, c.nombre as categoria_nombre, pr.razon_social as proveedor_nombre 
                      FROM productos p 
                      LEFT JOIN categorias c ON p.categoria_id = c.id
                      LEFT JOIN proveedores pr ON p.proveedor_id = pr.id";
               $result = $conn->query($sql);
               $productos = [];
               while($row = $result->fetch_assoc()) {
                   $productos[] = $row;
               }
               echo json_encode($productos);
               break;
    
           case 'POST':
               try {
                   if (isset($_GET['_method']) && $_GET['_method'] === 'PUT') {
                       handleProductoUpdate($conn);
                   } else {
                       handleProductoCreate($conn);
                   }
               } catch (Exception $e) {
                   error_log("Error en POST/PUT de producto: " . $e->getMessage());
                   http_response_code(500);
                   echo json_encode(["error" => $e->getMessage()]);
               }
               break;
    
           case 'DELETE':
               try {
                   $id = $_GET['id'];
                   
                   // Obtener información de la imagen
                   $stmt = $conn->prepare("SELECT imagen_url FROM productos WHERE id = ?");
                   $stmt->bind_param("i", $id);
                   $stmt->execute();
                   $result = $stmt->get_result();
                   $producto = $result->fetch_assoc();
    
                   // Eliminar el producto
                   $stmt = $conn->prepare("DELETE FROM productos WHERE id = ?");
                   $stmt->bind_param("i", $id);
                   
                   if($stmt->execute()) {
                       // Eliminar imagen si existe
                       if ($producto && $producto['imagen_url']) {
                           $imagen_path = str_replace('/tienda/', '', $producto['imagen_url']);
                           $ruta_completa = __DIR__ . '/' . $imagen_path;
                           
                           if (file_exists($ruta_completa)) {
                               unlink($ruta_completa);
                           }
                       }
                       
                       echo json_encode(["message" => "Producto eliminado con éxito"]);
                   } else {
                       throw new Exception("Error al eliminar el producto: " . $stmt->error);
                   }
               } catch (Exception $e) {
                   error_log("Error al eliminar producto: " . $e->getMessage());
                   http_response_code(500);
                   echo json_encode(["error" => $e->getMessage()]);
               }
               break;
       }
    }
    
    function handleProductoCreate($conn) {
       $data = $_POST;
       error_log("Datos recibidos para crear producto: " . print_r($data, true));
       
       $imagen_url = null;
       
       if (isset($_FILES['imagen'])) {
           $imagen = $_FILES['imagen'];
           $imagen_nombre = time() . '_' . basename($imagen['name']);
           $ruta_destino = 'imagenes/' . $imagen_nombre;
           
           if (!file_exists('imagenes')) {
               mkdir('imagenes', 0777, true);
           }
           
           if (move_uploaded_file($imagen['tmp_name'], $ruta_destino)) {
               $imagen_url = '/tienda/' . $ruta_destino;
           } else {
               throw new Exception("Error al subir la imagen");
           }
       }
    
       $stmt = $conn->prepare("INSERT INTO productos (nombre, descripcion, precio, stock, categoria_id, proveedor_id, imagen_url) VALUES (?, ?, ?, ?, ?, ?, ?)");
       
       // Convertir valores vacíos a NULL
       $proveedor_id = empty($data['proveedor_id']) ? null : $data['proveedor_id'];
       
       $stmt->bind_param("ssdiiss", 
           $data['nombre'], 
           $data['descripcion'], 
           $data['precio'], 
           $data['stock'], 
           $data['categoria_id'],
           $proveedor_id,
           $imagen_url
       );
       
       if($stmt->execute()) {
           echo json_encode(["message" => "Producto creado con éxito"]);
       } else {
           throw new Exception("Error al crear el producto: " . $stmt->error);
       }
    }
    
    
    function handleProductoUpdate($conn) {
        error_log("\n=== INICIO DE ACTUALIZACIÓN DE PRODUCTO ===");
        error_log("Método: " . $_SERVER['REQUEST_METHOD']);
        error_log("POST data: " . print_r($_POST, true));
        error_log("FILES data: " . print_r($_FILES, true));
    
        try {
            if (!isset($_POST['id'])) {
                throw new Exception("ID no proporcionado");
            }
    
            // Validar y registrar datos recibidos
            $id = intval($_POST['id']);
            $nombre = $conn->real_escape_string($_POST['nombre']);
            $descripcion = $conn->real_escape_string($_POST['descripcion']);
            $precio = floatval($_POST['precio']);
            $stock = intval($_POST['stock']);
            $categoria_id = !empty($_POST['categoria_id']) ? intval($_POST['categoria_id']) : null;
            $proveedor_id = !empty($_POST['proveedor_id']) ? intval($_POST['proveedor_id']) : null;
    
            error_log("\n=== DATOS PROCESADOS ===");
            error_log("ID: $id");
            error_log("Nombre: $nombre");
            error_log("Descripción: $descripcion");
            error_log("Precio: $precio");
            error_log("Stock: $stock");
            error_log("Categoría ID: " . var_export($categoria_id, true));
            error_log("Proveedor ID: " . var_export($proveedor_id, true));
    
            // Verificar producto existente
            $check = $conn->prepare("SELECT id FROM productos WHERE id = ?");
            $check->bind_param("i", $id);
            $check->execute();
            $result = $check->get_result();
            
            error_log("Verificación de existencia del producto: " . ($result->num_rows > 0 ? "Existe" : "No existe"));
    
            if ($result->num_rows === 0) {
                throw new Exception("Producto no encontrado");
            }
    
            // Procesar imagen
            $imagen_url = isset($_POST['imagen_url']) ? $_POST['imagen_url'] : null;
            error_log("Imagen URL inicial: " . var_export($imagen_url, true));
    
            if (isset($_FILES['imagen']) && $_FILES['imagen']['error'] === UPLOAD_ERR_OK) {
                error_log("Procesando nueva imagen...");
                $imagen = $_FILES['imagen'];
                $imagen_nombre = time() . '_' . basename($imagen['name']);
                $ruta_destino = 'imagenes/' . $imagen_nombre;
                
                if (move_uploaded_file($imagen['tmp_name'], $ruta_destino)) {
                    $imagen_url = '/tienda/' . $ruta_destino;
                    error_log("Nueva imagen guardada: $imagen_url");
                } else {
                    error_log("Error al mover la imagen");
                }
            }
    
            // Preparar y ejecutar la consulta SQL
            $sql = "UPDATE productos SET 
                    nombre = ?,
                    descripcion = ?,
                    precio = ?,
                    stock = ?,
                    categoria_id = ?,
                    proveedor_id = ?,
                    imagen_url = ?,
                    updated_at = CURRENT_TIMESTAMP
                    WHERE id = ?";
    
            error_log("\n=== CONSULTA SQL ===");
            error_log($sql);
    
            $stmt = $conn->prepare($sql);
            if (!$stmt) {
                throw new Exception("Error en la preparación de la consulta: " . $conn->error);
            }
    
            $stmt->bind_param("ssdiissi",
                $nombre,
                $descripcion,
                $precio,
                $stock,
                $categoria_id,
                $proveedor_id,
                $imagen_url,
                $id
            );
    
            error_log("\n=== PARÁMETROS VINCULADOS ===");
            error_log(print_r([
                'nombre' => $nombre,
                'descripcion' => $descripcion,
                'precio' => $precio,
                'stock' => $stock,
                'categoria_id' => $categoria_id,
                'proveedor_id' => $proveedor_id,
                'imagen_url' => $imagen_url,
                'id' => $id
            ], true));
    
            if (!$stmt->execute()) {
                throw new Exception("Error al ejecutar la actualización: " . $stmt->error);
            }
    
            error_log("Filas afectadas: " . $stmt->affected_rows);
    
            $response = [
                "success" => true,
                "message" => "Producto actualizado con éxito",
                "id" => $id,
                "affected_rows" => $stmt->affected_rows
            ];
    
            error_log("\n=== RESPUESTA ===");
            error_log(json_encode($response));
    
            echo json_encode($response);
    
        } catch (Exception $e) {
            error_log("\n=== ERROR ===");
            error_log($e->getMessage());
            http_response_code(500);
            echo json_encode([
                "success" => false,
                "error" => $e->getMessage()
            ]);
        }
    }
    $conn->close();
    ?>

4.  **Tabla: usuarios**

Linea::
  CREATE TABLE `usuarios` (
    `id` int(11) NOT NULL AUTO_INCREMENT,
    `nombre` varchar(100) NOT NULL,
    `email` varchar(100) NOT NULL,
    `password` varchar(255) NOT NULL,
    `rol` enum('admin','vendedor') NOT NULL DEFAULT 'vendedor',
    `estado` tinyint(1) DEFAULT 1,
    `ultimo_acceso` timestamp NULL DEFAULT NULL,
    `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
    `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp(),
    PRIMARY KEY (`id`),
    UNIQUE KEY `email` (`email`)
  ) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;
  
  /*Data for the table `usuarios` */
  
  insert  into `usuarios`(`id`,`nombre`,`email`,`password`,`rol`,`estado`,`ultimo_acceso`,`created_at`,`updated_at`) values 
  (1,'Administrador','admin@example.com','$2y$10$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi','admin',1,'2024-11-13 14:26:27','2024-11-06 09:05:30','2024-11-13 14:26:27'),
  (2,'José Aradiel','josea@gmail.com','$2y$10$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi','vendedor',1,'2024-11-13 14:14:25','2024-11-06 09:31:58','2024-11-13 14:14:25'),
  (3,'Gioirgina Hurtado','ghurtado@gmail.com','$2y$10$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi','vendedor',1,'2024-11-13 14:11:19','2024-11-06 09:49:08','2024-11-13 14:11:19');
