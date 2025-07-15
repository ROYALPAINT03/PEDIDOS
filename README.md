<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Pedidos - Pinturas OSI</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <style>
        :root {
            --primary: #4361ee;
            --secondary: #3f37c9;
            --success: #4cc9f0;
            --dark: #1d3557;
            --light: #f8f9fa;
            --gray: #6c757d;
            --danger: #e63946;
            --warning: #fca311;
            --card-bg: rgba(255, 255, 255, 0.95);
            --shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            min-height: 100vh;
            padding: 20px;
            color: #333;
            animation: gradientBG 15s ease infinite;
            background-size: 300% 300%;
            padding-bottom: 100px;
        }

        @keyframes gradientBG {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
        }

        header {
            text-align: center;
            padding: 20px 0;
            margin-bottom: 30px;
            color: var(--dark);
            position: relative;
            animation: fadeInDown 0.8s ease;
        }

        @keyframes fadeInDown {
            from {
                opacity: 0;
                transform: translateY(-20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        header h1 {
            font-size: 2.8rem;
            margin-bottom: 10px;
            font-weight: 700;
            background: linear-gradient(to right, var(--primary), var(--secondary));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        header p {
            font-size: 1.2rem;
            color: var(--gray);
            max-width: 600px;
            margin: 0 auto;
        }

        .app-container {
            display: grid;
            grid-template-columns: 1fr;
            gap: 25px;
        }

        .card {
            background: var(--card-bg);
            border-radius: 16px;
            box-shadow: var(--shadow);
            padding: 25px;
            margin-bottom: 25px;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
            border: 1px solid rgba(0,0,0,0.05);
            animation: slideUp 0.5s ease;
            animation-fill-mode: both;
        }

        .card:nth-child(1) { animation-delay: 0.1s; }
        .card:nth-child(2) { animation-delay: 0.2s; }

        @keyframes slideUp {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
        }

        .card-title {
            display: flex;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 2px solid rgba(67, 97, 238, 0.1);
            color: var(--dark);
        }

        .card-title i {
            margin-right: 12px;
            background: var(--primary);
            color: white;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 18px;
        }

        .form-group {
            margin-bottom: 20px;
            position: relative;
        }

        .form-group label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: var(--dark);
            font-size: 15px;
        }

        .form-control {
            width: 100%;
            padding: 14px 15px;
            border: 1px solid #ddd;
            border-radius: 10px;
            font-size: 16px;
            transition: all 0.3s;
            background: #f9fafc;
        }

        .form-control:focus {
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(67, 97, 238, 0.2);
            outline: none;
        }

        .search-filter {
            display: flex;
            gap: 15px;
            margin-bottom: 20px;
        }

        .search-filter input {
            flex: 1;
        }

        .category-section {
            margin-bottom: 30px;
        }

        .category-title {
            font-size: 1.5rem;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 2px solid var(--primary);
            color: var(--dark);
            display: flex;
            align-items: center;
        }

        .category-title i {
            margin-right: 10px;
            color: var(--primary);
        }

        .product-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
            gap: 15px;
        }

        .product-card {
            border: 1px solid #eaeaea;
            border-radius: 12px;
            padding: 15px;
            background: white;
            cursor: pointer;
            transition: all 0.3s ease;
            text-align: center;
            position: relative;
            overflow: hidden;
        }

        .product-card:hover {
            border-color: var(--primary);
            box-shadow: 0 5px 15px rgba(67, 97, 238, 0.1);
            transform: translateY(-3px);
        }

        .product-card img {
            width: 100%;
            height: 120px;
            object-fit: contain;
            margin-bottom: 12px;
            border-radius: 8px;
            background: #f8f9fa;
            padding: 5px;
        }

        .product-card h3 {
            font-size: 15px;
            margin-bottom: 8px;
            color: var(--dark);
            font-weight: 600;
        }

        .product-card .ref {
            display: inline-block;
            padding: 4px 10px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
            background: #eef2ff;
            color: var(--primary);
        }

        .actions {
            display: flex;
            gap: 15px;
            flex-wrap: wrap;
            margin-top: 20px;
        }

        .btn {
            padding: 14px 25px;
            border-radius: 10px;
            border: none;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            flex: 1;
            min-width: 180px;
        }

        .btn-primary {
            background: var(--primary);
            color: white;
        }

        .btn-primary:hover {
            background: var(--secondary);
            transform: translateY(-3px);
        }

        .btn-success {
            background: #38b000;
            color: white;
        }

        .btn-success:hover {
            background: #2d8c00;
            transform: translateY(-3px);
        }

        .btn-warning {
            background: var(--warning);
            color: white;
        }

        .btn-warning:hover {
            background: #e69500;
            transform: translateY(-3px);
        }

        .btn-danger {
            background: var(--danger);
            color: white;
        }

        .btn-danger:hover {
            background: #d00000;
            transform: translateY(-3px);
        }

        .notification {
            position: fixed;
            top: 25px;
            right: 25px;
            background: white;
            color: var(--dark);
            padding: 18px 25px;
            border-radius: 12px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.15);
            display: flex;
            align-items: center;
            gap: 15px;
            z-index: 1000;
            transform: translateX(200%);
            transition: transform 0.4s ease;
            border-left: 4px solid var(--success);
        }

        .notification.show {
            transform: translateX(0);
        }

        .notification i {
            font-size: 24px;
            color: var(--success);
        }

        .notification.error {
            border-left-color: var(--danger);
        }

        .notification.error i {
            color: var(--danger);
        }

        .loading {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(255,255,255,0.9);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 2000;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.3s;
        }

        .loading.active {
            opacity: 1;
            pointer-events: all;
        }

        .spinner {
            width: 60px;
            height: 60px;
            border: 5px solid rgba(67, 97, 238, 0.2);
            border-top: 5px solid var(--primary);
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin-bottom: 20px;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .unit-badge {
            display: inline-block;
            padding: 3px 10px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
        }

        .unit-U { background: #e3f2fd; color: #1976d2; }
        .unit-DZ { background: #e8f5e9; color: #388e3c; }
        .unit-BUL { background: #f3e5f5; color: #7b1fa2; }

        /* Modal de selección de unidad */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.7);
            z-index: 2000;
            align-items: center;
            justify-content: center;
        }

        .modal-content {
            background: white;
            border-radius: 16px;
            width: 90%;
            max-width: 500px;
            padding: 25px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
            animation: modalIn 0.4s ease;
        }

        @keyframes modalIn {
            from {
                opacity: 0;
                transform: translateY(-30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 1px solid #eee;
        }

        .modal-header h2 {
            font-size: 1.5rem;
            color: var(--dark);
        }

        .close-modal {
            background: none;
            border: none;
            font-size: 24px;
            cursor: pointer;
            color: var(--gray);
            transition: color 0.3s;
        }

        .close-modal:hover {
            color: var(--danger);
        }
        
        .modal-product-image {
            width: 100%;
            height: 220px;
            object-fit: contain;
            margin: 15px 0;
            border-radius: 10px;
            background: #f8f9fa;
            border: 1px solid #eee;
        }

        .unit-options {
            display: flex;
            flex-direction: column;
            gap: 15px;
            margin-bottom: 20px;
        }

        .unit-option {
            display: flex;
            align-items: center;
            padding: 15px;
            border: 2px solid #eee;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s;
        }

        .unit-option:hover {
            border-color: var(--primary);
            background: #f0f4ff;
        }

        .unit-option.selected {
            border-color: var(--primary);
            background: #e6edff;
        }

        .unit-option i {
            font-size: 24px;
            margin-right: 15px;
            color: var(--primary);
        }

        .unit-text {
            flex: 1;
        }

        .unit-text h3 {
            font-size: 18px;
            margin-bottom: 5px;
            color: var(--dark);
        }

        .unit-text p {
            font-size: 14px;
            color: var(--gray);
        }

        .quantity-selector {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px;
            margin-bottom: 20px;
        }

        .quantity-btn {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: #f1f3f9;
            border: none;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            font-size: 20px;
            color: var(--dark);
            transition: all 0.2s;
        }

        .quantity-btn:hover {
            background: var(--primary);
            color: white;
        }

        .quantity-display {
            font-size: 24px;
            font-weight: 600;
            min-width: 50px;
            text-align: center;
        }

        .btn-add-to-cart {
            width: 100%;
            padding: 15px;
            background: var(--primary);
            color: white;
            border: none;
            border-radius: 10px;
            font-size: 18px;
            font-weight: 600;
            cursor: pointer;
            transition: background 0.3s;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }

        .btn-add-to-cart:hover {
            background: var(--secondary);
        }

        /* Carrito flotante */
        .floating-cart {
            position: fixed;
            bottom: 30px;
            right: 30px;
            width: 70px;
            height: 70px;
            background: var(--primary);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            box-shadow: 0 6px 15px rgba(0,0,0,0.2);
            z-index: 100;
            transition: all 0.3s;
        }

        .floating-cart:hover {
            transform: scale(1.1);
            box-shadow: 0 8px 20px rgba(0,0,0,0.3);
        }

        .cart-icon {
            position: relative;
            color: white;
            font-size: 28px;
        }

        .cart-count {
            position: absolute;
            top: -10px;
            right: -10px;
            background: var(--danger);
            color: white;
            width: 26px;
            height: 26px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 14px;
            font-weight: bold;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
        }

        /* Modal del carrito */
        .cart-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.7);
            z-index: 2000;
            align-items: center;
            justify-content: center;
        }

        .cart-modal-content {
            background: white;
            border-radius: 16px;
            width: 90%;
            max-width: 800px;
            max-height: 90vh;
            overflow: hidden;
            display: flex;
            flex-direction: column;
            animation: modalIn 0.4s ease;
        }

        .cart-modal-header {
            background: var(--primary);
            color: white;
            padding: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .cart-modal-header h2 {
            font-size: 1.8rem;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .cart-modal-body {
            padding: 20px;
            flex: 1;
            overflow-y: auto;
        }

        .cart-item {
            display: flex;
            align-items: center;
            padding: 15px 0;
            border-bottom: 1px solid #eee;
            animation: fadeIn 0.3s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        .cart-item img {
            width: 80px;
            height: 80px;
            object-fit: contain;
            border-radius: 8px;
            margin-right: 15px;
            background: #f8f9fa;
            padding: 5px;
            border: 1px solid #eee;
        }

        .cart-item-info {
            flex: 1;
        }

        .cart-item-info h4 {
            font-size: 18px;
            margin-bottom: 5px;
            color: var(--dark);
        }

        .cart-item-info .ref {
            font-size: 14px;
            color: var(--gray);
            margin-bottom: 8px;
        }

        .cart-item-info .unit {
            font-size: 14px;
            color: var(--gray);
            background: #f1f3f9;
            padding: 4px 10px;
            border-radius: 10px;
            display: inline-block;
            margin-right: 8px;
        }
        
        .cart-item-info .total-units {
            display: inline-block;
            font-size: 14px;
            color: var(--dark);
            background: #e6f7ff;
            padding: 4px 10px;
            border-radius: 10px;
            font-weight: 600;
        }

        .cart-item-controls {
            display: flex;
            align-items: center;
        }

        .cart-item .quantity-btn {
            width: 35px;
            height: 35px;
        }

        .cart-item .quantity-display {
            font-size: 18px;
            min-width: 40px;
        }

        .cart-item .remove-btn {
            background: none;
            border: none;
            color: var(--danger);
            cursor: pointer;
            font-size: 22px;
            margin-left: 15px;
            transition: transform 0.2s;
        }

        .cart-item .remove-btn:hover {
            transform: scale(1.2);
        }

        .cart-empty {
            text-align: center;
            padding: 40px 20px;
            color: var(--gray);
        }

        .cart-empty i {
            font-size: 60px;
            margin-bottom: 20px;
            color: #dee2e6;
        }

        .cart-modal-footer {
            padding: 20px;
            background: #f8f9fa;
            border-top: 1px solid #eee;
        }

        .cart-actions {
            display: flex;
            gap: 15px;
        }

        .cart-actions .btn {
            flex: 1;
        }

        @media (max-width: 768px) {
            .product-grid {
                grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
            }
            
            .search-filter {
                flex-direction: column;
            }
            
            .actions {
                flex-direction: column;
            }
            
            .btn {
                width: 100%;
            }
            
            .cart-modal-content {
                width: 95%;
                max-height: 85vh;
            }
            
            .cart-item {
                flex-direction: column;
                align-items: flex-start;
            }
            
            .cart-item img {
                margin-bottom: 10px;
            }
            
            .cart-item-controls {
                width: 100%;
                justify-content: space-between;
                margin-top: 10px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1><i class="fas fa-paint-roller"></i> Sistema de Pedidos - Pinturas OSI</h1>
            <p>Gestión de pedidos para la línea OSI 60CC</p>
        </header>

        <div class="app-container">
            <div class="card">
                <div class="card-title">
                    <i class="fas fa-user"></i>
                    <h2>Información del Pedido</h2>
                </div>
                
                <div class="form-group">
                    <label for="seller-name"><i class="fas fa-user-tie"></i> Nombre del Vendedor</label>
                    <input type="text" id="seller-name" class="form-control" placeholder="Ingrese su nombre">
                </div>
                
                <div class="form-group">
                    <label for="client-name"><i class="fas fa-user"></i> Nombre del Cliente</label>
                    <input type="text" id="client-name" class="form-control" placeholder="Ingrese nombre del cliente">
                </div>
                
                <div class="form-group">
                    <label for="client-phone"><i class="fas fa-phone"></i> Teléfono del Cliente</label>
                    <input type="tel" id="client-phone" class="form-control" placeholder="Ingrese teléfono del cliente">
                </div>
                
                <div class="form-group">
                    <label for="payment-method"><i class="fas fa-money-bill-wave"></i> Método de Pago</label>
                    <select id="payment-method" class="form-control">
                        <option value="">Seleccione método de pago</option>
                        <option value="BOLIVAR">Bolívar</option>
                        <option value="DOLAR">Dólar</option>
                    </select>
                </div>
            </div>

            <div class="card">
                <div class="card-title">
                    <i class="fas fa-paint-brush"></i>
                    <h2>Productos Disponibles - Línea OSI 60CC</h2>
                </div>
                
                <div class="search-filter">
                    <input type="text" id="product-search" class="form-control" placeholder="Buscar por referencia...">
                    <select id="category-filter" class="form-control">
                        <option value="">Todas las referencias</option>
                        <option value="701-715">Ref. 701-715</option>
                        <option value="716-730">Ref. 716-730</option>
                        <option value="731-735">Ref. 731-735</option>
                    </select>
                </div>
                
                <div class="category-section">
                    <div class="category-title">
                        <i class="fas fa-palette"></i>
                        <h3>Línea OSI 60CC</h3>
                    </div>
                    
                    <div class="product-grid" id="product-list">
                        <!-- Productos se cargarán aquí -->
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Carrito flotante -->
    <div class="floating-cart" id="floating-cart">
        <div class="cart-icon">
            <i class="fas fa-shopping-cart"></i>
            <div class="cart-count" id="cart-count">0</div>
        </div>
    </div>

    <!-- Modal de selección de unidad -->
    <div class="modal" id="unit-modal">
        <div class="modal-content">
            <div class="modal-header">
                <h2 id="modal-product-name">Seleccionar Unidad</h2>
                <button class="close-modal" id="close-unit-modal">&times;</button>
            </div>
            
            <img id="modal-product-image" class="modal-product-image" src="" alt="Product Image">
            
            <div class="unit-options">
                <div class="unit-option" data-unit="U">
                    <i class="fas fa-box"></i>
                    <div class="unit-text">
                        <h3>Unidad</h3>
                        <p>Seleccionar por unidades individuales</p>
                    </div>
                </div>
                
                <div class="unit-option" data-unit="DZ">
                    <i class="fas fa-boxes"></i>
                    <div class="unit-text">
                        <h3>Docena</h3>
                        <p>Seleccionar por docenas (12 unidades)</p>
                    </div>
                </div>
                
                <div class="unit-option" data-unit="BUL">
                    <i class="fas fa-pallet"></i>
                    <div class="unit-text">
                        <h3>Bulto</h3>
                        <p id="bulk-description">Seleccionar por bultos (288 unidades)</p>
                    </div>
                </div>
            </div>
            
            <div class="quantity-selector">
                <button class="quantity-btn" id="decrease-qty">-</button>
                <div class="quantity-display" id="quantity-display">1</div>
                <button class="quantity-btn" id="increase-qty">+</button>
            </div>
            
            <button class="btn-add-to-cart" id="add-to-cart-btn">
                <i class="fas fa-cart-plus"></i> Agregar al Carrito
            </button>
        </div>
    </div>

    <!-- Modal del carrito -->
    <div class="cart-modal" id="cart-modal">
        <div class="cart-modal-content">
            <div class="cart-modal-header">
                <h2><i class="fas fa-shopping-cart"></i> Resumen del Pedido</h2>
                <button class="close-modal" id="close-cart-modal">&times;</button>
            </div>
            
            <div class="cart-modal-body" id="cart-modal-body">
                <!-- Ítems del carrito se mostrarán aquí -->
            </div>
            
            <div class="cart-modal-footer">
                <div class="cart-actions">
                    <button class="btn btn-danger" id="btn-clear">
                        <i class="fas fa-trash"></i> Limpiar Todo
                    </button>
                    <button class="btn btn-warning" id="btn-pdf">
                        <i class="fas fa-file-pdf"></i> Generar PDF
                    </button>
                    <button class="btn btn-success" id="btn-whatsapp">
                        <i class="fab fa-whatsapp"></i> Enviar por WhatsApp
                    </button>
                </div>
            </div>
        </div>
    </div>

    <div class="notification" id="notification">
        <i class="fas fa-check-circle"></i>
        <div>
            <strong>¡Éxito!</strong>
            <p>Pedido enviado correctamente!</p>
        </div>
    </div>

    <div class="loading" id="loading">
        <div class="spinner"></div>
        <h3>Procesando pedido...</h3>
    </div>

    <script>
        // Variables globales
        let products = [];
        let cartItems = [];
        let currentProduct = null;
        let selectedUnit = 'U';
        let quantity = 1;

        // Clase Producto
        class Product {
            constructor(id, name, ref, image) {
                this.id = id;
                this.name = name;
                this.ref = ref;
                this.image = image;
            }
        }

        // Clase Ítem de Carrito
        class CartItem {
            constructor(product, unit, quantity) {
                this.product = product;
                this.unit = unit;
                this.quantity = quantity;
            }
            
            getUnitName() {
                const units = {
                    'U': 'Unidad',
                    'DZ': 'Docena',
                    'BUL': 'Bulto'
                };
                return units[this.unit] || this.unit;
            }
            
            // Calcular unidades totales según el tipo de producto
            getTotalUnits() {
                if (this.product.ref === 734) {
                    // Tempera OSI
                    if (this.unit === 'U') return this.quantity;
                    if (this.unit === 'DZ') return this.quantity * 12;
                    if (this.unit === 'BUL') return this.quantity * 100;
                } else {
                    // Pinturas normales
                    if (this.unit === 'U') return this.quantity;
                    if (this.unit === 'DZ') return this.quantity * 12;
                    if (this.unit === 'BUL') return this.quantity * 288;
                }
            }
        }

        // Inicialización
        document.addEventListener('DOMContentLoaded', function() {
            loadProducts();
            setupEventListeners();
            updateCartCount();
        });

        // Cargar productos OSI
        function loadProducts() {
            const basePath = 'LINEA OSI 60CC/';
            const temperaPath = 'Tempera/';
            const refs = [];
            
            // Generar referencias del 701 al 735, excepto 734
            for (let i = 701; i <= 735; i++) {
                if (i !== 734) {
                    refs.push(i);
                }
            }
            
            // Crear productos normales
            products = refs.map(ref => {
                return new Product(
                    ref,
                    `Pintura OSI Ref. ${ref}`,
                    ref,
                    `${basePath}${ref}.png`
                );
            });
            
            // Agregar tempera OSI (Ref. 734)
            products.push(new Product(
                734,
                "Tempera OSI Ref. 734",
                734,
                `${temperaPath}Tempera.png`
            ));

            renderProductList(products);
        }

        // Configurar event listeners
        function setupEventListeners() {
            // Búsqueda y filtrado
            document.getElementById('product-search').addEventListener('input', filterProducts);
            document.getElementById('category-filter').addEventListener('change', filterProducts);
            
            // Modal de unidad
            document.querySelectorAll('.unit-option').forEach(option => {
                option.addEventListener('click', function() {
                    document.querySelectorAll('.unit-option').forEach(el => {
                        el.classList.remove('selected');
                    });
                    this.classList.add('selected');
                    selectedUnit = this.getAttribute('data-unit');
                });
            });
            
            // Botones de cantidad
            document.getElementById('decrease-qty').addEventListener('click', function() {
                if (quantity > 1) {
                    quantity--;
                    document.getElementById('quantity-display').textContent = quantity;
                }
            });
            
            document.getElementById('increase-qty').addEventListener('click', function() {
                quantity++;
                document.getElementById('quantity-display').textContent = quantity;
            });
            
            // Agregar al carrito
            document.getElementById('add-to-cart-btn').addEventListener('click', addToCartFromModal);
            
            // Cerrar modales
            document.getElementById('close-unit-modal').addEventListener('click', closeUnitModal);
            document.getElementById('close-cart-modal').addEventListener('click', closeCartModal);
            
            // Carrito flotante
            document.getElementById('floating-cart').addEventListener('click', openCartModal);
            
            // Botones de acción
            document.getElementById('btn-clear').addEventListener('click', clearAll);
            document.getElementById('btn-pdf').addEventListener('click', generatePDF);
            document.getElementById('btn-whatsapp').addEventListener('click', sendWhatsApp);
        }

        // Filtrar productos
        function filterProducts() {
            const searchTerm = document.getElementById('product-search').value.toLowerCase();
            const category = document.getElementById('category-filter').value;

            const filtered = products.filter(product => {
                const matchesSearch = product.ref.toString().includes(searchTerm) || 
                                     product.name.toLowerCase().includes(searchTerm);
                
                let matchesCategory = true;
                if (category === "701-715") {
                    matchesCategory = product.ref >= 701 && product.ref <= 715;
                } else if (category === "716-730") {
                    matchesCategory = product.ref >= 716 && product.ref <= 730;
                } else if (category === "731-735") {
                    matchesCategory = product.ref >= 731 && product.ref <= 735;
                }
                
                return matchesSearch && matchesCategory;
            });

            renderProductList(filtered);
        }

        // Renderizar lista de productos
        function renderProductList(productsToRender) {
            const productList = document.getElementById('product-list');
            productList.innerHTML = '';

            if (productsToRender.length === 0) {
                productList.innerHTML = '<p class="empty-message">No se encontraron productos.</p>';
                return;
            }

            productsToRender.forEach(product => {
                const productCard = document.createElement('div');
                productCard.className = 'product-card';
                
                productCard.innerHTML = `
                    <img src="${product.image}" alt="${product.name}" onerror="this.src='https://via.placeholder.com/150?text=Imagen+no+disponible'">
                    <h3>${product.name}</h3>
                    <span class="ref">Ref. ${product.ref}</span>
                `;
                
                productCard.addEventListener('click', () => openUnitModal(product));
                productList.appendChild(productCard);
            });
        }

        // Abrir modal de selección de unidad
        function openUnitModal(product) {
            currentProduct = product;
            selectedUnit = 'U';
            quantity = 1;
            
            document.querySelectorAll('.unit-option').forEach(el => {
                el.classList.remove('selected');
                if (el.getAttribute('data-unit') === selectedUnit) {
                    el.classList.add('selected');
                }
            });
            
            document.getElementById('modal-product-name').textContent = product.name;
            document.getElementById('quantity-display').textContent = quantity;
            
            // Actualizar imagen del producto
            document.getElementById('modal-product-image').src = product.image;
            
            // Actualizar descripción del bulto según el producto
            const bulkDescription = document.getElementById('bulk-description');
            if (product.ref === 734) {
                bulkDescription.textContent = 'Seleccionar por bultos (100 unidades)';
            } else {
                bulkDescription.textContent = 'Seleccionar por bultos (288 unidades)';
            }
            
            document.getElementById('unit-modal').style.display = 'flex';
        }

        // Cerrar modal de unidad
        function closeUnitModal() {
            document.getElementById('unit-modal').style.display = 'none';
        }

        // Agregar producto al carrito desde el modal
        function addToCartFromModal() {
            if (!currentProduct) return;
            
            // Verificar si ya existe el mismo producto con la misma unidad
            const existingItem = cartItems.find(item => 
                item.product.id === currentProduct.id && item.unit === selectedUnit);
            
            if (existingItem) {
                existingItem.quantity += quantity;
            } else {
                cartItems.push(new CartItem(currentProduct, selectedUnit, quantity));
            }
            
            updateCartCount();
            showNotification(`${currentProduct.name} agregado al carrito`);
            closeUnitModal();
        }

        // Actualizar contador del carrito
        function updateCartCount() {
            const totalItems = cartItems.reduce((sum, item) => sum + item.quantity, 0);
            document.getElementById('cart-count').textContent = totalItems;
        }

        // Abrir modal del carrito
        function openCartModal() {
            renderCartModal();
            document.getElementById('cart-modal').style.display = 'flex';
        }

        // Cerrar modal del carrito
        function closeCartModal() {
            document.getElementById('cart-modal').style.display = 'none';
        }

        // Renderizar ítems del carrito en el modal
        function renderCartModal() {
            const cartModalBody = document.getElementById('cart-modal-body');
            
            if (cartItems.length === 0) {
                cartModalBody.innerHTML = `
                    <div class="cart-empty">
                        <i class="fas fa-shopping-basket"></i>
                        <h3>Tu carrito está vacío</h3>
                        <p>Selecciona productos para agregar al pedido</p>
                    </div>
                `;
                return;
            }

            cartModalBody.innerHTML = '';
            
            cartItems.forEach((item, index) => {
                const cartItem = document.createElement('div');
                cartItem.className = 'cart-item';
                
                const totalUnits = item.getTotalUnits();
                
                cartItem.innerHTML = `
                    <img src="${item.product.image}" alt="${item.product.name}" onerror="this.src='https://via.placeholder.com/150?text=Imagen+no+disponible'">
                    <div class="cart-item-info">
                        <h4>${item.product.name}</h4>
                        <div class="ref">Ref. ${item.product.ref}</div>
                        <span class="unit unit-${item.unit}">${item.getUnitName()}: ${item.quantity}</span>
                        <span class="total-units">${totalUnits} unidades</span>
                    </div>
                    <div class="cart-item-controls">
                        <button class="quantity-btn" data-index="${index}" data-action="decrease">-</button>
                        <span class="quantity-display">${item.quantity}</span>
                        <button class="quantity-btn" data-index="${index}" data-action="increase">+</button>
                        <button class="remove-btn" data-index="${index}"><i class="fas fa-trash"></i></button>
                    </div>
                `;
                
                cartModalBody.appendChild(cartItem);
            });

            // Agregar event listeners a los botones de cantidad
            document.querySelectorAll('.cart-item .quantity-btn').forEach(button => {
                button.addEventListener('click', function() {
                    const index = parseInt(this.getAttribute('data-index'));
                    const action = this.getAttribute('data-action');
                    
                    if (action === 'increase') {
                        cartItems[index].quantity += 1;
                    } else if (action === 'decrease' && cartItems[index].quantity > 1) {
                        cartItems[index].quantity -= 1;
                    }
                    
                    renderCartModal();
                    updateCartCount();
                });
            });

            // Agregar event listeners a los botones de eliminar
            document.querySelectorAll('.cart-item .remove-btn').forEach(button => {
                button.addEventListener('click', function() {
                    const index = parseInt(this.getAttribute('data-index'));
                    const productName = cartItems[index].product.name;
                    cartItems.splice(index, 1);
                    renderCartModal();
                    updateCartCount();
                    showNotification(`${productName} eliminado del carrito`, 'error');
                });
            });
        }

        // Limpiar todo
        function clearAll() {
            if (cartItems.length === 0) {
                showNotification('El carrito ya está vacío', 'error');
                return;
            }
            
            if (confirm('¿Está seguro que desea limpiar todo el pedido?')) {
                cartItems = [];
                renderCartModal();
                updateCartCount();
                showNotification('Pedido limpiado correctamente');
            }
        }

        // Generar PDF
        function generatePDF() {
            const sellerName = document.getElementById('seller-name').value.trim();
            const clientName = document.getElementById('client-name').value.trim();
            const clientPhone = document.getElementById('client-phone').value.trim();
            const paymentMethod = document.getElementById('payment-method').value;

            // Validar campos requeridos
            if (!sellerName || !clientName || !clientPhone || !paymentMethod) {
                showNotification('Por favor complete todos los campos requeridos', 'error');
                return;
            }

            if (cartItems.length === 0) {
                showNotification('No hay productos en el pedido para generar PDF', 'error');
                return;
            }

            showLoading(true);

            // Usar jsPDF para generar el PDF
            const { jsPDF } = window.jspdf;
            const doc = new jsPDF({
                orientation: 'portrait',
                unit: 'mm',
                format: 'a4'
            });

            // Configuración
            const margin = 15;
            let yPos = margin;
            const pageWidth = doc.internal.pageSize.getWidth();
            const maxWidth = pageWidth - 2 * margin;

            // Encabezado
            doc.setFontSize(20);
            doc.setFont(undefined, 'bold');
            doc.text('ORDEN DE PEDIDO - PINTURAS OSI', pageWidth / 2, yPos, { align: 'center' });
            yPos += 10;

            doc.setFontSize(12);
            doc.setFont(undefined, 'normal');
            doc.text(`Fecha: ${new Date().toLocaleDateString()}`, margin, yPos);
            doc.text(`N°: ${Date.now()}`, pageWidth - margin, yPos, { align: 'right' });
            yPos += 10;

            // Información del vendedor y cliente
            doc.setFontSize(12);
            doc.setFont(undefined, 'bold');
            doc.text('Vendedor:', margin, yPos);
            doc.text('Cliente:', pageWidth / 2, yPos);
            yPos += 5;

            doc.setFont(undefined, 'normal');
            doc.text(sellerName, margin + 20, yPos);
            doc.text(clientName, pageWidth / 2 + 15, yPos);
            yPos += 10;

            doc.text('Teléfono:', pageWidth / 2, yPos);
            doc.text(clientPhone, pageWidth / 2 + 20, yPos);
            yPos += 5;
            
            doc.text('Método de Pago:', margin, yPos);
            doc.text(paymentMethod === 'BOLIVAR' ? 'Bolívar' : 'Dólar', margin + 40, yPos);
            yPos += 15;

            // Tabla de productos
            doc.setFont(undefined, 'bold');
            doc.setFillColor(67, 97, 238);
            doc.setTextColor(255, 255, 255);
            doc.rect(margin, yPos, maxWidth, 10, 'F');
            
            doc.text('Producto', margin + 5, yPos + 7);
            doc.text('Referencia', margin + 100, yPos + 7);
            doc.text('Unidad', margin + 130, yPos + 7);
            doc.text('Cantidad', margin + 160, yPos + 7);
            doc.text('Unidades', margin + 190, yPos + 7);
            yPos += 12;

            doc.setFont(undefined, 'normal');
            doc.setTextColor(0, 0, 0);
            
            cartItems.forEach(item => {
                if (yPos > 250) {
                    doc.addPage();
                    yPos = margin;
                }
                
                doc.text(item.product.name, margin + 5, yPos + 7);
                doc.text(`Ref. ${item.product.ref}`, margin + 100, yPos + 7);
                
                doc.text(item.getUnitName(), margin + 130, yPos + 7);
                doc.text(item.quantity.toString(), margin + 160, yPos + 7);
                
                // Mostrar unidades totales
                const totalUnits = item.getTotalUnits();
                doc.text(totalUnits.toString(), margin + 190, yPos + 7);
                
                // Línea divisoria
                doc.setDrawColor(200, 200, 200);
                doc.line(margin, yPos + 10, pageWidth - margin, yPos + 10);
                
                yPos += 10;
            });

            // Total de productos
            yPos += 10;
            doc.setFont(undefined, 'bold');
            doc.text(`Total de Productos: ${cartItems.length}`, margin, yPos);
            
            // Pie de página
            yPos = 280;
            doc.setFontSize(10);
            doc.setFont(undefined, 'normal');
            doc.setTextColor(100, 100, 100);
            doc.text('Sistema de Gestión de Pedidos - Pinturas OSI - ' + new Date().getFullYear(), pageWidth / 2, yPos, { align: 'center' });

            // Guardar el PDF
            doc.save(`pedido_osi_${clientName}_${Date.now()}.pdf`);
            
            showLoading(false);
            showNotification('PDF generado exitosamente!');
        }

        // Enviar por WhatsApp
        function sendWhatsApp() {
            const sellerName = document.getElementById('seller-name').value.trim();
            const clientName = document.getElementById('client-name').value.trim();
            const clientPhone = document.getElementById('client-phone').value.trim();
            const paymentMethod = document.getElementById('payment-method').value;

            // Validar campos requeridos
            if (!sellerName || !clientName || !clientPhone || !paymentMethod) {
                showNotification('Por favor complete todos los campos requeridos', 'error');
                return;
            }

            if (cartItems.length === 0) {
                showNotification('No hay productos en el pedido', 'error');
                return;
            }

            // Construir mensaje
            let message = `*PEDIDO PINTURAS OSI*%0A%0A`;
            message += `*Vendedor:* ${sellerName}%0A`;
            message += `*Cliente:* ${clientName}%0A`;
            message += `*Teléfono:* ${clientPhone}%0A`;
            message += `*Método de Pago:* ${paymentMethod === 'BOLIVAR' ? 'Bolívar' : 'Dólar'}%0A%0A`;
            message += `*DETALLES DEL PEDIDO:*%0A%0A`;

            cartItems.forEach(item => {
                const totalUnits = item.getTotalUnits();
                message += `- ${item.product.name} (Ref. ${item.product.ref})%0A`;
                message += `  Unidad: ${item.getUnitName()} | Cantidad: ${item.quantity} | Total unidades: ${totalUnits}%0A%0A`;
            });

            message += `%0A*Total de Productos:* ${cartItems.length}`;

            // Abrir WhatsApp
            window.open(`https://wa.me/${clientPhone}?text=${message}`, '_blank');
            
            showNotification('Mensaje de WhatsApp preparado!');
        }

        // Mostrar notificación
        function showNotification(message, type = 'success') {
            const notification = document.getElementById('notification');
            
            if (type === 'error') {
                notification.innerHTML = `
                    <i class="fas fa-exclamation-circle"></i>
                    <div>
                        <strong>¡Error!</strong>
                        <p>${message}</p>
                    </div>
                `;
                notification.classList.add('error');
            } else {
                notification.innerHTML = `
                    <i class="fas fa-check-circle"></i>
                    <div>
                        <strong>¡Éxito!</strong>
                        <p>${message}</p>
                    </div>
                `;
                notification.classList.remove('error');
            }
            
            notification.classList.add('show');
            
            setTimeout(() => {
                notification.classList.remove('show');
            }, 3000);
        }

        // Mostrar/ocultar loading
        function showLoading(show) {
            const loading = document.getElementById('loading');
            if (show) {
                loading.classList.add('active');
            } else {
                loading.classList.remove('active');
            }
        }
    </script>
</body>
</html>
