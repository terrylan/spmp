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