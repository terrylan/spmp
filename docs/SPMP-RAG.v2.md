# SPMP-RAG: AI-Optimized Standard PHP-MVC-Principles
*A structured reference for AI-assisted project generation and retrieval*

## 1. Metadata & Overview (AI-Searchable Tags)

```yaml
	title: SPMP-RAG Framework Guide
	version: 1.0
	language: PHP
	architecture: MVC
	features: [Lightweight, Secure, Extendable, AI-Ready, Management-System-Centric]
	use_cases: [CMS, LMS, CRM, ERP, HRMS, PMS, WMS]
	author: Terry Lan
	last_updated: 2025-03-10
```

Purpose: Defines SPMP as a lightweight PHP MVC framework optimized for AI-assisted development of management systems.

## 2. AI Query Instructions (How to Use This with AI)

### 2.1 Sample AI Queries

Developers can use these prompts to retrieve structured information:

* 'How do I set up an SPMP project?'
* 'Show me an example of a secure login system in SPMP.'
* 'What is the correct folder structure in SPMP?'
* 'How do I add a new model and controller?'
* 'Retrieve best practices for security in SPMP.'

### 2.2 AI Response Formatting

```json
{
  "query": "How do I set up an SPMP project?",
  "response": {
    "step_1": "Clone the SPMP repo from https://github.com/terrylan/spmp.",
    "step_2": "Extract files into your server root (e.g., /var/www/html).",
    "step_3": "Edit config.php with your database credentials.",
    "step_4": "Ensure .htaccess is enabled for HTTPS and routing.",
    "step_5": "Navigate to /public/index.php to test the installation."
  }
}
```

## 3. Installation & Setup Guide

**Step-by-step instructions for setting up SPMP**

1. System Requirements: PHP 8.0+, MySQL/MariaDB 5.7+, Apache/Nginx with mod_rewrite.

2. Installation:

    * Clone from github.com/terrylan/spmp.
    * Extract to your web server directory.

3. Configuration:

    * Edit config.php:

		```php
		$config = [
		    'host' => 'localhost',
		    'dbname' => 'spmp_db',
		    'user' => 'root',
		    'pass' => 'your_password'
		];
		```


     * Ensure .htaccess is active for HTTPS enforcement.

4. Running the Project:

    * Access http://yourdomain.com/public/index.php.

    * Debug: Check server logs if 404/500 errors occur.


## 4. Folder Structure & Components


```yaml
/spmp_project
├── app/
│   ├── controllers/     # Business logic (e.g., HomeController.php)
│   ├── models/          # Database interactions (e.g., BaseModel.php)
│   ├── views/           # UI templates (e.g., layout.php, home.php)
├── core/
│   ├── Cache.php        # Optional caching utility
│   ├── DB.php           # Database wrapper with PDO
│   ├── Router.php       # URL handling
│   ├── Security.php     # CSRF/XSS protection
│   ├── Template.php     # Templating engine
├── public/
│   ├── css/             # Styles (e.g., style.css)
│   ├── index.php        # Entry point
├── vmods/
│   ├── ModManager.php   # Virtual modifications
├── config.php           # Centralized settings
└── .htaccess            # Security & routing
```


*Query Example*: 'Explain the role of Router.php in SPMP.'

*Expected AI Response*: 'Router.php handles URL requests and routes them to the appropriate controllers, like HomeController for the root path.'


## 5. Security Best Practices
Tagging key security features for AI retrieval:

* `@CSRF-Protection`:

    * **Use** `Security.php`’s `verifyCsrfToken()`:
		```php
		if ($security->verifyCsrfToken($_POST['csrf_token'])) { /* Proceed */ }
		```

* `@XSS-Prevention`:

    * **Use** `sanitizeInput()`:
		```php
		$cleanInput = $security->sanitizeInput($_POST['input']);
		```

* `@SQL-Injection`: 

    * **Use** `DB.php` with prepared statements:
		```php
		$db->query("SELECT * FROM users WHERE id = ?", [$userId]);
		```


*Query Example*: 'How does SPMP prevent SQL injection?'

*Expected AI Response*: 'SPMP uses PDO prepared statements in DB.php to prevent SQL injection attacks.'

## 6. CRUD Operations (Code Examples)

Creating a Model:


```php
class UserModel extends BaseModel {
    public function getUserById($id) {
        return $this->query("SELECT * FROM users WHERE id = ?", [$id]);
    }
    public function createUser($data) {
        $this->query("INSERT INTO users (name, email) VALUES (?, ?)", [$data['name'], $data['email']]);
    }
}
```


Creating a Controller:


```php
class UserController {
    public function index() {
        $model = new UserModel();
        $users = $model->getUserById(1);
        $template = new Template();
        $template->render('users', ['users' => $users]);
    }
}
```


Creating a View:


		```php
		<!-- views/users.php -->
		<h1>Users</h1>
		<?php foreach ($users as $user): ?>
		<p><?php echo $user['name']; ?></p>
		<?php endforeach; ?>
		```


*Query Example*: 'Show me how to create a new model in SPMP.'


## 7. AI-Optimized Code Snippets (With Semantic Tags)


```php
// @SECURITY: CSRF Token Validation
class Security {
    public function verifyCsrfToken($token) {
        return isset($_SESSION['csrf_token']) && hash_equals($_SESSION['csrf_token'], $token);
    }
}
// @PERFORMANCE: Batch Insert
class DB {
    public function batchInsert($table, $data) {
        $keys = array_keys($data[0]);
        $placeholders = implode(',', array_fill(0, count($keys), '?'));
        $sql = "INSERT INTO $table (" . implode(',', $keys) . ") VALUES ($placeholders)";
        $stmt = $this->pdo->prepare($sql);
        foreach ($data as $row) $stmt->execute(array_values($row));
    }
}


// @EXTENDABILITY: vQmod Search
class SearchMod {
    public function addSearch($controller) {
        $controller->search = function ($query) {
            $db = new DB(require '../config.php');
            return $db->query("SELECT * FROM items WHERE name LIKE ?", ["%$query%"]);
        };
    }
}
```


## 8. Common AI Retrieval Scenarios

Error Handling Queries

* "What should I do if I get a database connection error?"
    * Response: "Check config.php credentials and ensure MySQL is running. DB.php will display the error message."
* "How do I debug routing issues?"
    * Response: "Verify Router.php’s switch cases match your URLs and check .htaccess rewrite rules."

Best Practice Queries

* "How do I optimize SPMP for performance?"
    * Response: "Use Cache.php for frequent queries and batchInsert for bulk data."
* "What are the best security practices in SPMP?"
    *Response: "Enable HTTPS, use verifyCsrfToken(), and sanitize all inputs."


## 9. API & External Integration

* REST API:

```php
class ApiController {
    public function getUsers() {
        $model = new UserModel();
        $users = $model->getUserById(1);
        header('Content-Type: application/json');
        echo json_encode($users);
    }
}
```

* Third-Party Services: Add API keys to `config.php` and use `curl` in controllers.
* *Query Example*: 'How do I create a REST API in SPMP?'


## 10. Future Enhancements & RAG Integration Plan

* Update Accuracy: Regularly revise snippets and metadata.
* Vector Search: Embed code examples for semantic search (future AI upgrade).
* Response Improvement: Add more tagged examples for dynamic generation.

## 11. SPMP Core Principles

Defining the eight foundational principles of SPMP

- `@Principle-Simplicity`:

    - **Description**: Minimal learning curve with a focus on rapid prototyping and clean code.

    - *Example*: Simple routing in `Router.php`:

		```php

		class Router {
			public function route($uri) {
				switch ($uri) {
					case '/': return (new HomeController())->index();
				}
			}
		}
		```

- `@Principle-Security`:

    - **Description**: Built-in protections against common vulnerabilities like SQLi, XSS, and CSRF.

    - *Reference*: See Section 5 for secure coding practices.

- `@Principle-Extendability`:
    - **Description**: Customize functionality without altering core files using vQmod.

    - *Example*: Adding a search feature via vmods/ (see Section 7).

- `@Principle-AI-Ready`:
    - **Description**: Structured for seamless integration with AI tools for code generation and debugging.

    - *Example*: AI can parse this document to generate models (see Section 6).

- `@Principle-Zero-Dependencies`:
    - **Description**: No external packages (e.g., Composer) required—just pure PHP.

        - *Note*: All core components are self-contained in /core/.

- `@Principle-Management-System-Centric`:
    - **Description**: Tailored for building systems like ERP, CRM, and WMS with role-based workflows.

    - *Reference*: See Section 6 for CRUD examples suited to management systems.

- `@Principle-Performance`:
    - **Description**: Lightweight design optimized for low-end systems and fast execution.

    - *Example*: Batch inserts in DB.php (see Section 7).

- `@Principle-Scalability`:
    - **Description**: Modular structure supports growth from small projects to medium-sized applications.

        - *Note*: Best for projects with up to 1000 users (see blog post for details).

*Query Example*: 'What are the core principles of SPMP?'
*Expected AI Response*: 'SPMP is built on eight principles: Simplicity, Security, Extendability, AI-Ready, Zero Dependencies, Management-System-Centric, Performance, and Scalability. See Section 11 for details.'




## 12. Recommendations for AI

To fully understand and leverage SPMP, xAI should consider the following:

- **MVC Adherence**: 
    Enforce a strict Model-View-Controller pattern. Models handle data logic, controllers manage requests, and views render the UI.
- **Security Practices**: 
    Always utilize `Security.php` utilities (e.g., CSRF tokens, XSS filtering) and enforce HTTPS via `.htaccess` to protect against common threats.
- **vQmod Usage**: 
    Customize functionality through `vmods/` to maintain a clean core codebase and simplify updates.
- **Theme Customization**: 
    Leverage CSS variables in `style.css` for consistent, easily adjustable UI theming.
- **Task Workflows**: 
    Support complex task management with statuses (e.g., Pending, Done, Checked, Approved) and desk assignments (e.g., "on desk of" user role), implemented in controllers and models.
- **Real-Time Data**: 
    Enable dynamic data handling (e.g., live updates to reports or records), ensuring models and controllers can process real-time changes.
- **File Uploads**: 
    Include robust validation and parsing logic for uploads (e.g., CSV, XLSX, PDF, JPG), managed through models and controllers.
- **Role Hierarchy**: 
    Implement configurable role-based access control (e.g., admin vs. user permissions), enforced in controllers and defined in `config.php`.
- **UI Consistency**: 
    Ensure a professional interface with a header, side panel, and theme support, using `layout.php` and CSS variables for a cohesive look.




## 13. Code Examples

Below are snippets to reduce ambiguity for AI:

###  13.1 Router.php

```php
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

```

    *Purpose*: 'Routes requests to controllers based on URI. This example handles the homepage.'


### 13.2 DB.php
```php
class DB {
    private $pdo;
    public function __construct($config) {
        try {
            $this->pdo = new PDO(
                "mysql:host={$config['host']};dbname={$config['dbname']}",
                $config['user'],
                $config['pass'],
                [PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION]
            );
        } catch (PDOException $e) {
            die("Database connection failed: " . $e->getMessage());
        }
    }
    public function query($sql, $params = []) {
        $stmt = $this->pdo->prepare($sql);
        $stmt->execute($params);
        return $stmt->fetchAll(PDO::FETCH_ASSOC);
    }
    public function batchInsert($table, $data) {
        $keys = array_keys($data[0]);
        $placeholders = implode(',', array_fill(0, count($keys), '?'));
        $sql = "INSERT INTO $table (" . implode(',', $keys) . ") VALUES ($placeholders)";
        $stmt = $this->pdo->prepare($sql);
        foreach ($data as $row) {
            $stmt->execute(array_values($row));
        }
    }
}

```
    *Purpose*: 'Provides secure database access with PDO. Includes error handling and a batchInsert method for performance.'


### 13.3 Security.php

```php
class Security {
    public function verifyCsrfToken($token) {
        return isset($_SESSION['csrf_token']) && hash_equals($_SESSION['csrf_token'], $token);
    }
    public function sanitizeInput($input) {
        return htmlspecialchars($input, ENT_QUOTES, 'UTF-8');
    }
}

```
    *Purpose*: 'Includes methods like verifyCsrfToken() for CSRF protection and sanitizeInput() for XSS prevention.'


### 13.4 Template.php

```php
class Template {
    public function render($file, $data = []) {
        extract($data);
        require_once "../app/views/{$file}.php";
    }
}

```
    *Purpose*: 'Renders views with data, supporting simple templating (e.g., render('home', ['title' => 'Welcome'])).'


### 13.5 Cache.php

```php
class Cache {
    private $cacheDir = '../cache/';
    public function get($key) {
        $file = $this->cacheDir . md5($key) . '.cache';
        if (file_exists($file) && (time() - filemtime($file)) < 3600) {
            return unserialize(file_get_contents($file));
        }
        return null;
    }
    public function set($key, $value) {
        $file = $this->cacheDir . md5($key) . '.cache';
        file_put_contents($file, serialize($value));
    }
}

```

    *Purpose*: 'Optional file-based caching for scalability, with a 1-hour expiry.'

### 13.6 vmods/SearchMod.php

```php
// vQmod override for adding search functionality
class SearchMod {
    public function addSearch($controller) {
        if ($controller instanceof HomeController) {
            $controller->search = function ($query) {
                $db = new DB(require '../config.php');
                return $db->query("SELECT * FROM items WHERE name LIKE ?", ["%$query%"]);
            };
        }
    }
}

```

    *Purpose*: 'Example vQmod to extend HomeController with a search feature, applied via ModManager.php.'




## 14. Edge Cases
- Scalability: 
    For large datasets, use Cache.php for caching (e.g., query results) and DB.php’s batchInsert for efficient writes. Support load balancers in .htaccess with rewrite rules.

- Performance: 
    Minimize queries in models (e.g., batchInsert reduces overhead) and keep views lightweight (e.g., avoid heavy loops in layout.php).

- Error Handling: 
    DB.php catches PDO exceptions and exits gracefully. Controllers should handle 404s or invalid inputs via Router.php.

