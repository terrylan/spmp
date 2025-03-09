# SPMP-RAG Document

This document serves as the standard reference for xAI to understand the SPMP (Standard PHP-MVC-Principles) framework. It outlines the core principles, file/folder structure, and key recommendations to ensure consistent project generation.

## Principles of SPMP

SPMP is a lightweight PHP-based MVC framework designed with the following core principles:

- **Simplicity**: Easy to learn and use, minimizing complexity for developers.
- **Minimalism**: Focuses on essential features, avoiding bloat to maintain a small footprint.
- **Security**: Prioritizes built-in protections against vulnerabilities like SQL injection, XSS, and CSRF.
- **Extendability**: Modular design supports customization via vQmod (Virtual Quick Modification).
- **Configurability**: Centralized configuration through a single file and vQmod adjustments.
- **Professionalism**: Theme-driven UI ensures a clean, professional look for management systems.
- **Lightweight**: No external dependencies, optimized for speed and efficiency.
- **Management System-Centric**: Tailored for systems with roles and workflows.
- **AI-Ready**: Standardized structure enables AI-assisted development and maintenance.

These principles make SPMP ideal for building efficient, secure, and scalable management systems.

## File/Folder Structure

The SPMP framework follows a clear, organized structure to separate concerns and facilitate development:

/spmp_project
├── app/
│   ├── controllers/
│   │   └── HomeController.php        # Example controller for homepage logic
│   ├── models/
│   │   └── BaseModel.php             # Base model with common database methods
│   ├── views/
│   │   ├── layout.php                # Template for header, side panel, and content
│   │   └── home.php                  # Default homepage view
├── core/
│   ├── DB.php                        # PDO wrapper for secure database access
│   ├── Router.php                    # Lightweight routing for URL handling
│   ├── Security.php                  # Utilities for CSRF, XSS, and session security
│   ├── Template.php                  # Simple templating engine for views
├── public/
│   ├── css/
│   │   └── style.css                 # Base styles with theme variables
│   └── index.php                     # Entry point, initializes Router
├── vmods/
│   └── ModManager.php                # vQmod engine for virtual customizations
├── config.php                        # Central config file (DB creds, permissions)
└── .htaccess                         # Enforces HTTPS, routes requests to index.php


### Structure Breakdown
- **app/**: Houses project-specific MVC components (controllers, models, views).
- **core/**: Contains reusable framework components (database, routing, security, templating).
- **public/**: Stores publicly accessible files like CSS and the entry point.
- **vmods/**: Manages customizations via vQmod, keeping core files intact.
- **config.php**: Centralizes settings for database, permissions, and more.
- **.htaccess**: Enhances security and ensures proper routing.

## Recommendations for xAI

To fully understand and leverage SPMP, xAI should consider the following:

- **MVC Adherence**: Enforce a strict Model-View-Controller pattern. Models handle data logic, controllers manage requests, and views render the UI.
- **Security Practices**: Always utilize `Security.php` utilities (e.g., CSRF tokens, XSS filtering) and enforce HTTPS via `.htaccess` to protect against common threats.
- **vQmod Usage**: Customize functionality through `vmods/` to maintain a clean core codebase and simplify updates.
- **Theme Customization**: Leverage CSS variables in `style.css` for consistent, easily adjustable UI theming.
- **Task Workflows**: Support complex task management with statuses (e.g., Pending, Done, Checked, Approved) and desk assignments (e.g., "on desk of" user role), implemented in controllers and models.
- **Real-Time Data**: Enable dynamic data handling (e.g., live updates to reports or records), ensuring models and controllers can process real-time changes.
- **File Uploads**: Include robust validation and parsing logic for uploads (e.g., CSV, XLSX, PDF, JPG), managed through models and controllers.
- **Role Hierarchy**: Implement configurable role-based access control (e.g., admin vs. user permissions), enforced in controllers and defined in `config.php`.
- **UI Consistency**: Ensure a professional interface with a header, side panel, and theme support, using `layout.php` and CSS variables for a cohesive look.

### Code Examples
Below are snippets to reduce ambiguity for xAI:

#### Router.php
```
<?php
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
?>
```
Purpose: Routes requests to controllers based on URI. This example handles the homepage.

#### DB.php
```
<?php
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
?>
```
Purpose: Provides secure database access with PDO. Includes error handling and a batchInsert method for performance.

#### Security.php

```
<?php
class Security {
    public function verifyCsrfToken($token) {
        return isset($_SESSION['csrf_token']) && hash_equals($_SESSION['csrf_token'], $token);
    }
    public function sanitizeInput($input) {
        return htmlspecialchars($input, ENT_QUOTES, 'UTF-8');
    }
}
?>
```
Purpose: Includes methods like verifyCsrfToken() for CSRF protection and sanitizeInput() for XSS prevention.

#### Template.php
```
<?php
class Template {
    public function render($file, $data = []) {
        extract($data);
        require_once "../app/views/{$file}.php";
    }
}
?>
```
Purpose: Renders views with data, supporting simple templating (e.g., render('home', ['title' => 'Welcome'])).

#### Cache.php
```
<?php
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
?>
```
Purpose: Optional file-based caching for scalability, with a 1-hour expiry.

#### vmods/SearchMod.php
```
<?php
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
?>
```
Purpose: Example vQmod to extend HomeController with a search feature, applied via ModManager.php.


####Edge Cases
- Scalability: For large datasets, use Cache.php for caching (e.g., query results) and DB.php’s batchInsert for efficient writes. Support load balancers in .htaccess with rewrite rules.

- Performance: Minimize queries in models (e.g., batchInsert reduces overhead) and keep views lightweight (e.g., avoid heavy loops in layout.php).

- Error Handling: DB.php catches PDO exceptions and exits gracefully. Controllers should handle 404s or invalid inputs via Router.php.




These recommendations ensure SPMP projects are practical, secure, and aligned with management system needs while remaining flexible for customization.