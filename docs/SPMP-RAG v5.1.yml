# SPMP-RAG: AI-Optimized Standard PHP-MVC-Principles (Lean & Adaptive v5.1)

## 1. Metadata & Overview
title: "SPMP-RAG Framework Guide"
version: "5.1"
language: "PHP"
architecture: "MVC"
features: [Lightweight, Secure, Extendable, AI-Ready, Scalable, Adaptive, Lean]
use_cases: [Notes-Taking, CMS, LMS, CRM, ERP, HRMS, PMS, WMS]
author: "Terry Lan"
last_updated: "2025-03-13"
description: "A lean, adaptive PHP MVC framework generating exact software for exact hardware and user needs."
scalability_note: "Scales from 1 user on 4GB Linux device to 50,000+ users on optimized cloud."
competitive_edge: "Rivals Laravel/Symfony with lightweight core, AI-driven adaptability, and hardware-specific optimization."

## 2. AI Query Instructions
ai_instructions:
  sample_queries:
    - "Generate a notes app for a 4GB RAM Linux device with 1 user."
    - "Build an ERP for 10,000 users on AWS cloud with cost estimate."
    - "Optimize my app for a small device with 2GB RAM."
    - "Set up a single-user notes-taking system like Google Keep."
    - "Generate an ERP with inventory, HR, and real-time reporting for 10,000 users."
  hardware_context:
    - field: "cpu"
      example: "1.1GHz Celeron N3450"
    - field: "ram"
      example: "4GB"
    - field: "os"
      example: "Linux"
    - field: "user_count"
      example: "1, 100, 10000"
  generation_rules:
    - rule: "If ram < 8GB, use file-based Cache.php, skip Redis and Swoole."
    - rule: "If user_count < 10, disable Queue.php and middleware."
    - rule: "For 'ERP' and user_count > 1000, enable full stack with cloud_recommendations."
  dynamic_templates:
    - name: "NotesSystem"
      function_tag: "@NOTES"
      template: |
        class NoteController {
            public function create($data) {
                $note = Note::create(['title' => $data['title'], 'content' => $data['content']]);
                [CACHE_LOGIC]
                return $note;
            }
        }
      variables:
        - name: "CACHE_LOGIC"
          options:
            - condition: "ram < 8GB"
              code: "$cache = new Cache(); $cache->setFile('note_{$note->id}', $note);"
            - condition: "ram >= 8GB"
              code: "$cache = new Cache(); $cache->setRedis('note_{$note->id}', $note);"
    - name: "ERPSystem"
      function_tag: "@ERP"
      template: |
        class InventoryController {
            public function addItem($data) {
                $item = Item::create($data);
                [QUEUE_LOGIC]
                return $item;
            }
        }
      variables:
        - name: "QUEUE_LOGIC"
          options:
            - condition: "user_count < 100"
              code: "// No queue for small scale"
            - condition: "user_count >= 100"
              code: "Queue::push('process_inventory', ['item_id' => $item->id]);"
  cloud_recommendations:
    - user_range: "1-100"
      spec: "AWS t3.micro (1 vCPU, 1GB RAM), ~$10/month"
      purpose: "Notes app or small CMS."
    - user_range: "1000-10000"
      spec: "AWS t3.large (2 vCPUs, 8GB RAM), ~$80/month"
      purpose: "Medium ERP or CRM."
    - user_range: "10000-50000"
      spec: "AWS m5.2xlarge (8 vCPUs, 32GB RAM), ~$400/month"
      purpose: "Full ERP with real-time features."

## 3. Installation & Setup Guide
installation:
  requirements:
    - "PHP 8.0+ (with optional Swoole extension for async)"
    - "MySQL/MariaDB 5.7+ or Redis 5.0+ (optional)"
    - "Apache/Nginx with optional WebSocket support"
  lean_config:
    language: "PHP"
    code: |
      $config = [
          'host' => 'localhost',
          'dbname' => 'spmp_db',
          'user' => 'root',
          'pass' => 'your_password',
          'features' => [
              'redis' => false,    # Default off, enable if ram >= 8GB
              'swoole' => false,   # Default off, enable if cpu > 2GHz
              'queue' => false     # Default off, enable if user_count > 100
          ]
      ];
  cli_commands:
    - command: "spmp generate:model [NAME]"
      purpose: "Creates a model with ORM support (e.g., Note, Item)."
    - command: "spmp optimize:hardware [CPU] [RAM] [OS] [USERS]"
      purpose: "Sets config based on specs (e.g., spmp optimize:hardware 1.1GHz 4GB Linux 1)."

## 4. Folder Structure & Components
folder_structure:
  - path: "/spmp_project"
    subdirs:
      - path: "app/"
        components:
          - "controllers/ # Business logic (e.g., HomeController.php)"
          - "models/ # Database interactions (e.g., BaseModel.php)"
          - "views/ # UI templates (e.g., layout.php, home.php)"
      - path: "core/"
        components:
          - "Cache.php # Lean caching (file or Redis)"
          - "DB.php # Database wrapper with PDO"
          - "Router.php # URL handling (basic or async)"
          - "Security.php # CSRF/XSS protection"
          - "Template.php # Templating engine"
      - path: "public/"
        components:
          - "css/ # Styles (e.g., style.css)"
          - "index.php # Entry point"
      - path: "vmods/"
        components:
          - "ModManager.php # Virtual modifications"
      - "config.php # Centralized settings"
      - ".htaccess # Security & routing"

## 5. Security Best Practices
security:
  - tag: "@CSRF-Protection"
    method: "Use Security.php’s verifyCsrfToken()"
    context: "Prevents cross-site request forgery in forms."
    code:
      language: "PHP"
      snippet: |
        if ($security->verifyCsrfToken($_POST['csrf_token'])) { /* Proceed */ }
  - tag: "@XSS-Prevention"
    method: "Use sanitizeInput()"
    context: "Sanitizes user input to block XSS attacks."
    code:
      language: "PHP"
      snippet: |
        $cleanInput = $security->sanitizeInput($_POST['input']);
  - tag: "@SQL-Injection"
    method: "Use DB.php with prepared statements"
    context: "Protects database queries from injection."
    code:
      language: "PHP"
      snippet: |
        $db->query("SELECT * FROM users WHERE id = ?", [$userId]);

## 6. CRUD Operations
crud_examples:
  - model:
      name: "UserModel"
      code:
        language: "PHP"
        snippet: |
          class UserModel extends BaseModel {
              public function getUserById($id) {
                  return $this->query("SELECT * FROM users WHERE id = ?", [$id]);
              }
              public function createUser($data) {
                  $this->query("INSERT INTO users (name, email) VALUES (?, ?)", [$data['name'], $data['email']]);
              }
          }
  - controller:
      name: "UserController"
      code:
        language: "PHP"
        snippet: |
          class UserController {
              public function index() {
                  $model = new UserModel();
                  $users = $model->getUserById(1);
                  $template = new Template();
                  $template->render('users', ['users' => $users]);
              }
          }
  - view:
      name: "users.php"
      code:
        language: "PHP"
        snippet: |
          <!-- views/users.php -->
          <h1>Users</h1>
          <?php foreach ($users as $user): ?>
          <p><?php echo $user['name']; ?></p>
          <?php endforeach; ?>

## 7. AI-Optimized Code Snippets
snippets:
  - tag: "@CACHE-LEAN"
    language: "PHP"
    code: |
      class Cache {
          private $redis = null;
          private $fileDir = '../cache/';
          public function __construct($config) {
              if ($config['features']['redis'] && extension_loaded('redis')) {
                  $this->redis = new Redis();
                  $this->redis->connect($config['redis']['host'], $config['redis']['port']);
              }
          }
          public function setFile($key, $value) {
              file_put_contents($this->fileDir . md5($key), serialize($value));
          }
          public function setRedis($key, $value) {
              $this->redis->setex($key, 3600, serialize($value));
          }
          public function set($key, $value) {
              $this->redis ? $this->setRedis($key, $value) : $this->setFile($key, $value);
          }
      }
    purpose: "Lean caching: file-based for low spec, Redis for high spec."
  - tag: "@ROUTING-LEAN"
    language: "PHP"
    code: |
      class Router {
          public function route($uri) {
              [SWOOLE_CHECK]
              switch ($uri) {
                  case '/': return (new HomeController())->index();
              }
          }
      }
    variables:
      - name: "SWOOLE_CHECK"
        options:
          - condition: "swoole = false"
            code: "// Basic routing"
          - condition: "swoole = true"
            code: "Swoole\\Coroutine\\run(function() { /* Async logic */ });"

## 8. Common AI Retrieval Scenarios
retrieval_scenarios:
  error_handling:
    - query: "What should I do if I get a database connection error?"
      response: "Check config.php credentials and ensure MySQL is running. DB.php will display the error message."
    - query: "How do I debug routing issues?"
      response: "Verify Router.php’s switch cases match your URLs and check .htaccess rewrite rules."
  best_practices:
    - query: "How do I optimize SPMP for performance?"
      response: "Use Cache.php for frequent queries and batchInsert for bulk data."
    - query: "What are the best security practices in SPMP?"
      response: "Enable HTTPS, use verifyCsrfToken(), and sanitize all inputs."

## 9. API & External Integration
api:
  external_sources:
    - name: "Google Keep API (mock)"
      url: "https://api.example.com/keep"
      purpose: "Sync notes for single-user app."
    - name: "SAP ERP API"
      url: "https://api.sap.com/erp"
      purpose: "Integrate enterprise data."


## 10. Future Enhancements & RAG Integration Plan
future_enhancements:
  - name: "Frontend UI"
    description: "Add a built-in frontend framework (e.g., lightweight PHP-based templating with CSS/JS) for rapid UI development."
    implementation: "Extend Template.php with a component system (e.g., navbar, forms) and bundle minimal CSS/JS in public/."
    details:
      - component_system:
          purpose: "Enable reusable UI elements for consistency and speed."
          example:
            language: "PHP"
            code: |
              class Template {
                  public function renderComponent($name, $data = []) {
                      extract($data);
                      require_once "../app/views/components/{$name}.php";
                  }
                  public function render($file, $data = []) {
                      extract($data);
                      $this->renderComponent('header', ['title' => $title]);
                      require_once "../app/views/{$file}.php";
                      $this->renderComponent('footer');
                  }
              }
      - assets:
          purpose: "Provide minimal styling and interactivity."
          example: "public/css/spmp.css with CSS variables (e.g., --primary-color) and public/js/spmp.js for basic DOM manipulation."
    ai_integration: "Allow AI to generate UI components via dynamic_templates (e.g., 'Generate a navbar with 3 links')."

  - name: "Authentication"
    description: "Integrate a secure authentication system with login, logout, and session management."
    implementation: "Add Auth.php to core/ with password hashing (e.g., bcrypt) and JWT support; CLI: spmp generate:auth."
    details:
      - auth_class:
          purpose: "Handle user authentication and session management."
          example:
            language: "PHP"
            code: |
              class Auth {
                  private $db;
                  public function __construct($config) {
                      $this->db = new DB($config);
                  }
                  public function login($email, $password) {
                      $user = $this->db->query("SELECT * FROM users WHERE email = ?", [$email])[0];
                      if ($user && password_verify($password, $user['password'])) {
                          $_SESSION['user_id'] = $user['id'];
                          return true;
                      }
                      return false;
                  }
                  public function logout() {
                      unset($_SESSION['user_id']);
                      session_destroy();
                  }
                  public function generateJWT($userId) {
                      $payload = ['user_id' => $userId, 'exp' => time() + 3600];
                      return JWT::encode($payload, 'secret_key');
                  }
              }
      - cli_setup:
          purpose: "Generate authentication scaffolding (controller, model, view)."
          example: "spmp generate:auth creates AuthController.php, UserModel.php with password field, and login.php view."
    ai_integration: "Enable AI to generate auth flows (e.g., 'Generate a login system with JWT') via dynamic_templates."

  - name: "Role-Based Access Control (RBAC)"
    description: "Implement RBAC for user permissions (e.g., admin, editor, viewer) to control access within applications."
    implementation: "Extend Security.php with role checks (e.g., $security->hasRole('admin')); store roles in DB.php and integrate with Auth.php."
    details:
      - rbac_system:
          purpose: "Manage user roles and permissions for secure, granular access."
          example:
            language: "PHP"
            code: |
              class Security {
                  private $db;
                  public function __construct($config) {
                      $this->db = new DB($config);
                  }
                  public function hasRole($userId, $role) {
                      $roles = $this->db->query("SELECT role FROM user_roles WHERE user_id = ?", [$userId]);
                      return in_array($role, array_column($roles, 'role'));
                  }
                  public function restrict($requiredRole) {
                      $userId = $_SESSION['user_id'] ?? null;
                      if (!$userId || !$this->hasRole($userId, $requiredRole)) {
                          die("Access denied");
                      }
                  }
              }
              class InventoryController {
                  public function deleteItem($id) {
                      $security = new Security(require 'config.php');
                      $security->restrict('admin');
                      // Delete logic...
                  }
              }
      - schema:
          purpose: "Store roles in the database."
          example:
            language: "SQL"
            code: |
              CREATE TABLE user_roles (
                  user_id INT,
                  role VARCHAR(50),
                  PRIMARY KEY (user_id, role),
                  FOREIGN KEY (user_id) REFERENCES users(id)
              );
              INSERT INTO user_roles (user_id, role) VALUES (1, 'admin'), (2, 'editor');
      - cli_setup:
          purpose: "Generate RBAC scaffolding (role management, permissions)."
          example: "spmp generate:rbac creates RoleController.php, updates UserModel.php, and adds role views."
    ai_integration: "Enable AI to generate role-specific logic (e.g., 'Restrict this to admins') via dynamic_templates."

  - name: "GraphQL Support"
    description: "Add GraphQL API endpoints for flexible data querying, enhancing REST capabilities."
    implementation: "Introduce GraphQL.php in core/ using a lightweight library (e.g., webonyx/graphql-php); CLI: spmp generate:graphql [MODEL]."
    details:
      - graphql_system:
          purpose: "Provide a schema-driven API for efficient data access."
          example:
            language: "PHP"
            code: |
              use GraphQL\Type\Definition\Type;
              use GraphQL\Type\Definition\ObjectType;
              class GraphQL {
                  private $db;
                  public function __construct($config) {
                      $this->db = new DB($config);
                  }
                  public function setupSchema() {
                      $noteType = new ObjectType([
                          'name' => 'Note',
                          'fields' => [
                              'id' => Type::int(),
                              'title' => Type::string(),
                              'content' => Type::string()
                          ]
                      ]);
                      $queryType = new ObjectType([
                          'name' => 'Query',
                          'fields' => [
                              'notes' => [
                                  'type' => Type::listOf($noteType),
                                  'resolve' => function() {
                                      return $this->db->query("SELECT * FROM notes");
                                  }
                              ]
                          ]
                      ]);
                      return new GraphQL\Schema(['query' => $queryType]);
                  }
                  public function handle($query) {
                      $schema = $this->setupSchema();
                      return GraphQL::executeQuery($schema, $query)->toArray();
                  }
              }
              class ApiController {
                  public function graphql() {
                      $graphql = new GraphQL(require 'config.php');
                      $query = file_get_contents('php://input');
                      return json_encode($graphql->handle($query));
                  }
              }
      - cli_setup:
          purpose: "Generate GraphQL scaffolding (schema, resolvers)."
          example: "spmp generate:graphql Note creates NoteType.php and integrates with ApiController.php."
    ai_integration: "Enable AI to generate GraphQL schemas (e.g., 'Create a GraphQL API for notes') via dynamic_templates."

  - name: "Multi-Tenancy"
    description: "Enable tenant-based database switching for multi-client applications."
    implementation: "Modify DB.php to switch schemas (e.g., $db->setTenant('client1')); config.php tenant mapping."
    details:
      - tenant_system:
          purpose: "Support multiple clients with isolated data in one app instance."
          example:
            language: "PHP"
            code: |
              class DB {
                  private $pdo;
                  private $tenant;
                  public function __construct($config) {
                      $this->connect($config['default']);
                  }
                  public function setTenant($tenantId) {
                      $this->tenant = $tenantId;
                      $config = $this->loadTenantConfig($tenantId);
                      $this->connect($config);
                  }
                  private function connect($config) {
                      $dsn = "mysql:host={$config['host']};dbname={$config['dbname']}_{$this->tenant}";
                      $this->pdo = new PDO($dsn, $config['user'], $config['pass']);
                  }
                  private function loadTenantConfig($tenantId) {
                      return require 'config.php'['tenants'][$tenantId];
                  }
                  public function query($sql, $params = []) {
                      $stmt = $this->pdo->prepare($sql);
                      $stmt->execute($params);
                      return $stmt->fetchAll();
                  }
              }
              class NoteController {
                  public function create($data) {
                      $db = new DB(require 'config.php');
                      $db->setTenant('client1');
                      $note = $db->query("INSERT INTO notes (title, content) VALUES (?, ?)", [$data['title'], $data['content']]);
                      return $note;
                  }
              }
      - config_example:
          purpose: "Define tenant-specific database settings."
          example:
            language: "PHP"
            code: |
              $config = [
                  'default' => ['host' => 'localhost', 'user' => 'root', 'pass' => ''],
                  'tenants' => [
                      'client1' => ['dbname' => 'spmp_client1'],
                      'client2' => ['dbname' => 'spmp_client2']
                  ]
              ];
      - cli_setup:
          purpose: "Generate multi-tenant scaffolding."
          example: "spmp generate:tenant [TENANT_ID] creates tenant DB and updates config.php."
    ai_integration: "Enable AI to generate tenant-specific logic (e.g., 'Set up client1’s DB') via dynamic_templates."

  - name: "Background Processing"
    description: "Enhance queue system for robust background tasks (e.g., emails, reports)."
    implementation: "Expand Queue.php with worker management and retry logic; integrate with Redis or file-based queues."
    details:
      - queue_system:
          purpose: "Manage asynchronous tasks with reliability and scalability."
          example:
            language: "PHP"
            code: |
              class Queue {
                  private $storage;
                  public function __construct($config) {
                      $this->storage = $config['features']['redis'] ? new Redis() : new FileStorage();
                  }
                  public function push($job, $data) {
                      $task = ['job' => $job, 'data' => $data, 'attempts' => 0];
                      $this->storage->add('queue', json_encode($task));
                  }
                  public function worker() {
                      while ($task = $this->storage->pop('queue')) {
                          $task = json_decode($task, true);
                          try {
                              $this->process($task['job'], $task['data']);
                          } catch (Exception $e) {
                              if ($task['attempts']++ < 3) {
                                  $this->storage->add('queue', json_encode($task));
                              }
                          }
                      }
                  }
                  private function process($job, $data) {
                      // e.g., send email, generate report
                  }
              }
              $queue = new Queue(require 'config.php');
              $queue->push('send_email', ['to' => 'user@example.com', 'msg' => 'Hi']);
      - cli_setup:
          purpose: "Generate queue scaffolding and workers."
          example: "spmp generate:queue [JOB_NAME] creates QueueJob_[JOB_NAME].php."
    ai_integration: "Enable AI to generate task-specific queues (e.g., 'Queue email sends') via dynamic_templates."

  - name: "Cloud File Storage"
    description: "Support cloud storage (e.g., AWS S3, Google Cloud Storage) for file uploads."
    implementation: "Add Storage.php in core/ with adapters for S3/GCS; CLI: spmp generate:storage [PROVIDER]."
    details:
      - storage_system:
          purpose: "Enable scalable file management in the cloud."
          example:
            language: "PHP"
            code: |
              class Storage {
                  private $adapter;
                  public function __construct($config) {
                      $this->adapter = $config['storage']['provider'] === 's3' 
                          ? new S3Adapter($config['s3']) 
                          : new GCSAdapter($config['gcs']);
                  }
                  public function upload($filePath, $destination) {
                      $this->adapter->put($filePath, $destination);
                  }
                  public function download($source, $localPath) {
                      $this->adapter->get($source, $localPath);
                  }
              }
              $storage = new Storage(require 'config.php');
              $storage->upload('note.txt', 'client1/notes/note.txt');
      - config_example:
          purpose: "Define cloud provider settings."
          example:
            language: "PHP"
            code: |
              $config = [
                  'storage' => [
                      'provider' => 's3',
                      's3' => ['key' => 'YOUR_KEY', 'secret' => 'YOUR_SECRET', 'bucket' => 'spmp-bucket']
                  ]
              ];
      - cli_setup:
          purpose: "Generate storage scaffolding."
          example: "spmp generate:storage s3 creates S3Adapter.php and updates config.php."
    ai_integration: "Enable AI to generate storage logic (e.g., 'Upload to S3') via dynamic_templates."

  - name: "WebSockets (Real-Time)"
    description: "Add WebSocket support for real-time features (e.g., live updates, chat)."
    implementation: "Integrate Swoole or Ratchet in core/ via WebSocket.php; CLI: spmp generate:websocket [ENDPOINT]."
    details:
      - websocket_system:
          purpose: "Enable real-time bidirectional communication."
          example:
            language: "PHP"
            code: |
              use Swoole\WebSocket\Server;
              class WebSocket {
                  private $server;
                  public function __construct($config) {
                      $this->server = new Server($config['host'], $config['port']);
                      $this->server->on('open', [$this, 'onOpen']);
                      $this->server->on('message', [$this, 'onMessage']);
                  }
                  public function onOpen($server, $request) {
                      echo "Client connected: {$request->fd}\n";
                  }
                  public function onMessage($server, $frame) {
                      $server->push($frame->fd, "Echo: {$frame->data}");
                  }
                  public function start() {
                      $this->server->start();
                  }
              }
              // Usage
              $ws = new WebSocket(['host' => '0.0.0.0', 'port' => 9501]);
              $ws->start();
      - cli_setup:
          purpose: "Generate WebSocket endpoint scaffolding."
          example: "spmp generate:websocket chat creates WebSocketChat.php with onMessage logic."
    ai_integration: "Enable AI to generate real-time logic (e.g., 'Create a chat endpoint') via dynamic_templates."

  - name: "Plugin System"
    description: "Implement a plugin system for extensible functionality (e.g., custom modules)."
    implementation: "Add Plugin.php in core/ with a registry; CLI: spmp generate:plugin [NAME]."
    details:
      - plugin_system:
          purpose: "Allow modular extensions without core modification."
          example:
            language: "PHP"
            code: |
              class Plugin {
                  private $registry = [];
                  public function register($name, $class) {
                      $this->registry[$name] = new $class();
                  }
                  public function load($name) {
                      return $this->registry[$name] ?? null;
                  }
              }
              // Plugin example: app/plugins/LoggerPlugin.php
              class LoggerPlugin {
                  public function log($message) {
                      file_put_contents('log.txt', $message . PHP_EOL, FILE_APPEND);
                  }
              }
              // Usage in core
              $plugin = new Plugin();
              $plugin->register('logger', 'LoggerPlugin');
              $logger = $plugin->load('logger');
              $logger->log('User logged in');
      - cli_setup:
          purpose: "Generate plugin scaffolding."
          example: "spmp generate:plugin logger creates app/plugins/LoggerPlugin.php and registers it."
    ai_integration: "Enable AI to generate plugins (e.g., 'Create a logging plugin') via dynamic_templates."

  - name: "AI IDE Integration"
    description: "Connect SPMP to AI-driven IDEs for seamless code generation and editing."
    implementation: "Add IDEApi.php in core/ with endpoints for AI tools; CLI: spmp generate:ide-hook [TASK]."
    details:
      - ide_api:
          purpose: "Expose SPMP’s generation capabilities to IDEs (e.g., VS Code plugins)."
          example:
            language: "PHP"
            code: |
              class IDEApi {
                  private $templates;
                  public function __construct($config) {
                      $this->templates = new DynamicTemplates($config);
                  }
                  public function generate($task) {
                      return $this->templates->generate($task);  # e.g., "Create a controller"
                  }
              }
              // API endpoint
              class ApiController {
                  public function ideHook() {
                      $api = new IDEApi(require 'config.php');
                      $task = json_decode(file_get_contents('php://input'), true)['task'];
                      return json_encode($api->generate($task));
                  }
              }
      - cli_setup:
          purpose: "Generate IDE hook scaffolding."
          example: "spmp generate:ide-hook model creates an API endpoint for model generation."
    ai_integration: "Enable AI to interface with IDEs via dynamic_templates (e.g., 'Generate a model in VS Code')."

  - "Auto-detect hardware specs via PHP (e.g., memory_get_usage()) for real-time optimization."
  - "Expand cloud_recommendations with multi-provider options (AWS, GCP, Azure)."

## 11. SPMP Core Principles
core_principles:
  - tag: "@Principle-Simplicity"
    description: "Minimal learning curve with a focus on rapid prototyping and clean code."
    example:
      language: "PHP"
      code: |
        class Router {
            public function route($uri) {
                switch ($uri) {
                    case '/': return (new HomeController())->index();
                }
            }
        }
  - tag: "@Principle-Security"
    description: "Built-in protections against common vulnerabilities like SQLi, XSS, and CSRF."
    reference: "See Section 5 for secure coding practices."
  - tag: "@Principle-Extendability"
    description: "Customize functionality without altering core files using vQmod."
    example: "Adding a search feature via vmods/ (see Section 7)."

## 12. Recommendations for AI
recommendations:
  - name: "Hardware Adaptation"
    description: "Use hardware_context to tailor features (e.g., file cache for low RAM, Redis for high RAM)."
  - name: "Cloud Optimization"
    description: "Leverage cloud_recommendations for cost-effective scaling (e.g., t3.micro for small apps)."

## 13. Code Examples
code_examples:
  - name: "Router.php"
    code:
      language: "PHP"
      snippet: |
        class Router {
            public function route($uri) {
                switch ($uri) {
                    case '/':
                        require_once '../app/controllers/HomeController.php';
                        $controller = new HomeController();
                        $controller->index();
                        break;
                    default:
                        http_response_code(404);
                        echo 'Page not found';
                }
            }
        }
    purpose: "Routes requests to controllers based on URI. This example handles the homepage."

## 14. Edge Cases
edge_cases:
  - name: "Low-Spec Device"
    description: "For devices with <4GB RAM, use file-based caching and disable async features."
  - name: "High-Concurrency Cloud"
    description: "For cloud deployments with >1000 users, enable Swoole, Redis, and queues for optimal performance."
