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
            --osi: #4361ee;
            --sisi: #38b000;
            --metalicos: #fca311;
            --tela: #9c27b0;
            --gel: #00bcd4;
            --envejecedores: #795548;
            --perlados: #ff9800;
            --neon: #ff4081;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #f5f7fa 0%, #e3e9f7 100%);
            min-height: 100vh;
            padding: 20px;
            color: #333;
            padding-bottom: 100px;
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
            font-size: 2.5rem;
            margin-bottom: 10px;
            font-weight: 700;
            background: linear-gradient(to right, var(--primary), var(--secondary));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        header p {
            font-size: 1.1rem;
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

        .search-container {
            display: flex;
            gap: 15px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        .search-container input {
            flex: 1;
            min-width: 250px;
        }

        .category-section {
            margin-bottom: 30px;
        }

        .category-title {
            font-size: 1.4rem;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 2px solid var(--primary);
            color: var(--dark);
            display: flex;
            align-items: center;
            cursor: pointer;
        }

        .category-title i {
            margin-right: 10px;
            color: var(--primary);
            transition: transform 0.3s;
        }

        .category-title.collapsed i {
            transform: rotate(-90deg);
        }

        .category-content {
            overflow: hidden;
            transition: max-height 0.3s ease;
        }

        .category-content.collapsed {
            max-height: 0;
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
            box-shadow: 0 3px 10px rgba(0,0,0,0.05);
        }

        .product-card:hover {
            border-color: var(--primary);
            box-shadow: 0 5px 15px rgba(67, 97, 238, 0.1);
            transform: translateY(-3px);
        }

        .product-card .image-container {
            width: 100%;
            height: 120px;
            display: flex;
            align-items: center;
            justify-content: center;
            background: #f8f9fa;
            border-radius: 8px;
            margin-bottom: 12px;
            position: relative;
        }

        .product-card .image-container img {
            max-width: 100%;
            max-height: 100%;
            object-fit: contain;
        }

        .product-card .image-container .no-image {
            color: #ccc;
            font-size: 12px;
            text-align: center;
            padding: 10px;
        }

        .product-card h3 {
            font-size: 14px;
            margin-bottom: 8px;
            color: var(--dark);
            font-weight: 600;
            line-height: 1.3;
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
            background: var(--sisi);
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
            max-height: 90vh;
            overflow-y: auto;
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

        .image-placeholder {
            width: 100%;
            height: 220px;
            display: flex;
            align-items: center;
            justify-content: center;
            background: #f8f9fa;
            border-radius: 10px;
            border: 1px solid #eee;
            margin: 15px 0;
            color: #ccc;
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
            box-shadow: 0 4px 6px rgba(67, 97, 238, 0.3);
        }

        .btn-add-to-cart:hover {
            background: var(--secondary);
            transform: translateY(-2px);
        }

        .btn-add-to-cart:active {
            transform: translateY(1px);
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
        }

        .cart-item .image-container {
            width: 80px;
            height: 80px;
            display: flex;
            align-items: center;
            justify-content: center;
            background: #f8f9fa;
            border-radius: 8px;
            margin-right: 15px;
            border: 1px solid #eee;
        }

        .cart-item .image-container img {
            max-width: 100%;
            max-height: 100%;
            object-fit: contain;
        }

        .cart-item .image-container .no-image {
            color: #ccc;
            font-size: 10px;
            text-align: center;
            padding: 5px;
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

        /* Categorías */
        .category-badge {
            display: inline-block;
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
            margin-top: 8px;
        }

        .category-osi { background: rgba(67, 97, 238, 0.1); color: var(--osi); }
        .category-sisi { background: rgba(56, 176, 0, 0.1); color: var(--sisi); }
        .category-metalicos { background: rgba(252, 163, 17, 0.1); color: var(--metalicos); }
        .category-tela { background: rgba(156, 39, 176, 0.1); color: var(--tela); }
        .category-gel { background: rgba(0, 188, 212, 0.1); color: var(--gel); }
        .category-envejecedores { background: rgba(121, 85, 72, 0.1); color: var(--envejecedores); }
        .category-perlados { background: rgba(255, 152, 0, 0.1); color: var(--perlados); }
        .category-neon { background: rgba(255, 64, 129, 0.1); color: var(--neon); }

        @media (max-width: 768px) {
            .product-grid {
                grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
            }
            
            .search-container {
                flex-direction: column;
            }
            
            .actions, .cart-actions {
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
            
            .cart-item .image-container {
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
            <p>Gestión de pedidos para todas las líneas de productos</p>
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
                    <h2>Productos Disponibles</h2>
                </div>
                
                <div class="search-container">
                    <input type="text" id="product-search" class="form-control" placeholder="Buscar por nombre de color...">
                    <select id="category-filter" class="form-control" style="max-width: 200px;">
                        <option value="">Todas las categorías</option>
                        <option value="osi">Pintura OSI 60CC</option>
                        <option value="sisi">Pintura SISI 60CC</option>
                        <option value="metalicos">Metalicos 22CC</option>
                        <option value="tela">Tela 30CC</option>
                        <option value="gel">Gel Escarchado 60CC</option>
                        <option value="envejecedores">Envejecedores Tradicionales 60CC</option>
                        <option value="perlados">Perlados 22CC</option>
                        <option value="neon">Tela Neon 30CC</option>
                        <option value="tempera">Tempera OSI</option>
                    </select>
                </div>
                
                <div id="categories-container">
                    <!-- Las categorías se generarán dinámicamente aquí -->
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
            
            <div id="modal-image-container">
                <div class="image-placeholder">
                    <div class="no-image">Imagen no disponible</div>
                </div>
            </div>
            
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
        let cartItems = [];
        let currentProduct = null;
        let selectedUnit = 'U';
        let quantity = 1;

        // Definición precisa de categorías con nombres específicos
        const categories = [
            {
                id: "envejecedores",
                name: "ENVEJECEDOR SISI 60CC",
                color: "envejecedores",
                products: [
                    {
                        id: 331,
                        name: "Envejecedor Tradicional SISI",
                        ref: 331,
                        category: "envejecedores",
                        image: `ENVEJECEDOR SISI 60CC/331.png`
                    }
                ]
            },
            {
                id: "gel",
                name: "GEL ESCARCHADO SISI 60CC",
                color: "gel",
                products: [
                    {id: 374, name: "Gel Escarchado Plata", ref: 374, category: "gel", image: `GEL ESCARCHADO SISI 60CC/374.png`},
                    {id: 375, name: "Gel Escarchado Oro", ref: 375, category: "gel", image: `GEL ESCARCHADO SISI 60CC/375.png`},
                    {id: 376, name: "Gel Escarchado Azul", ref: 376, category: "gel", image: `GEL ESCARCHADO SISI 60CC/376.png`},
                    {id: 377, name: "Gel Escarchado Rojo", ref: 377, category: "gel", image: `GEL ESCARCHADO SISI 60CC/377.png`},
                    {id: 378, name: "Gel Escarchado Verde", ref: 378, category: "gel", image: `GEL ESCARCHADO SISI 60CC/378.png`},
                    {id: 379, name: "Gel Escarchado Violeta", ref: 379, category: "gel", image: `GEL ESCARCHADO SISI 60CC/379.png`},
                    {id: 380, name: "Gel Escarchado Rosa", ref: 380, category: "gel", image: `GEL ESCARCHADO SISI 60CC/380.png`},
                    {id: 381, name: "Gel Escarchado Naranja", ref: 381, category: "gel", image: `GEL ESCARCHADO SISI 60CC/381.png`},
                    {id: 382, name: "Gel Escarchado Blanco", ref: 382, category: "gel", image: `GEL ESCARCHADO SISI 60CC/382.png`},
                    {id: 383, name: "Gel Escarchado Negro", ref: 383, category: "gel", image: `GEL ESCARCHADO SISI 60CC/383.png`},
                    {id: 384, name: "Gel Escarchado Cobre", ref: 384, category: "gel", image: `GEL ESCARCHADO SISI 60CC/384.png`}
                ]
            },
            {
                id: "osi",
                name: "LINEA OSI 60CC",
                color: "osi",
                products: [
                    {id: 701, name: "Amarillo Limón OSI", ref: 701, category: "osi", image: `LINEA OSI 60CC/701.png`},
                    {id: 702, name: "Naranja Brillante OSI", ref: 702, category: "osi", image: `LINEA OSI 60CC/702.png`},
                    {id: 703, name: "Rojo Fuego OSI", ref: 703, category: "osi", image: `LINEA OSI 60CC/703.png`},
                    {id: 704, name: "Rosa Intenso OSI", ref: 704, category: "osi", image: `LINEA OSI 60CC/704.png`},
                    {id: 705, name: "Violeta Vibrante OSI", ref: 705, category: "osi", image: `LINEA OSI 60CC/705.png`},
                    {id: 706, name: "Azul Cielo OSI", ref: 706, category: "osi", image: `LINEA OSI 60CC/706.png`},
                    {id: 707, name: "Azul Marino OSI", ref: 707, category: "osi", image: `LINEA OSI 60CC/707.png`},
                    {id: 708, name: "Verde Esmeralda OSI", ref: 708, category: "osi", image: `LINEA OSI 60CC/708.png`},
                    {id: 709, name: "Verde Oliva OSI", ref: 709, category: "osi", image: `LINEA OSI 60CC/709.png`},
                    {id: 710, name: "Marrón Chocolate OSI", ref: 710, category: "osi", image: `LINEA OSI 60CC/710.png`},
                    {id: 711, name: "Beige Arena OSI", ref: 711, category: "osi", image: `LINEA OSI 60CC/711.png`},
                    {id: 712, name: "Blanco Nieve OSI", ref: 712, category: "osi", image: `LINEA OSI 60CC/712.png`},
                    {id: 713, name: "Gris Perla OSI", ref: 713, category: "osi", image: `LINEA OSI 60CC/713.png`},
                    {id: 714, name: "Negro Ébano OSI", ref: 714, category: "osi", image: `LINEA OSI 60CC/714.png`},
                    {id: 715, name: "Plateado Metálico OSI", ref: 715, category: "osi", image: `LINEA OSI 60CC/715.png`},
                    {id: 716, name: "Dorado Brillante OSI", ref: 716, category: "osi", image: `LINEA OSI 60CC/716.png`},
                    {id: 717, name: "Cobre Reluciente OSI", ref: 717, category: "osi", image: `LINEA OSI 60CC/717.png`},
                    {id: 718, name: "Bronce Antiguo OSI", ref: 718, category: "osi", image: `LINEA OSI 60CC/718.png`},
                    {id: 719, name: "Turquesa OSI", ref: 719, category: "osi", image: `LINEA OSI 60CC/719.png`},
                    {id: 720, name: "Coral Atardecer OSI", ref: 720, category: "osi", image: `LINEA OSI 60CC/720.png`},
                    {id: 721, name: "Lavanda Suave OSI", ref: 721, category: "osi", image: `LINEA OSI 60CC/721.png`},
                    {id: 722, name: "Menta Refrescante OSI", ref: 722, category: "osi", image: `LINEA OSI 60CC/722.png`},
                    {id: 723, name: "Borgogna Elegante OSI", ref: 723, category: "osi", image: `LINEA OSI 60CC/723.png`},
                    {id: 724, name: "Mostaza Otoñal OSI", ref: 724, category: "osi", image: `LINEA OSI 60CC/724.png`},
                    {id: 725, name: "Caoba Clásico OSI", ref: 725, category: "osi", image: `LINEA OSI 60CC/725.png`},
                    {id: 726, name: "Azul Turquesa OSI", ref: 726, category: "osi", image: `LINEA OSI 60CC/726.png`},
                    {id: 727, name: "Verde Manzana OSI", ref: 727, category: "osi", image: `LINEA OSI 60CC/727.png`},
                    {id: 728, name: "Rojo Cereza OSI", ref: 728, category: "osi", image: `LINEA OSI 60CC/728.png`},
                    {id: 729, name: "Lila Soñador OSI", ref: 729, category: "osi", image: `LINEA OSI 60CC/729.png`},
                    {id: 730, name: "Amarillo Oro OSI", ref: 730, category: "osi", image: `LINEA OSI 60CC/730.png`},
                    {id: 731, name: "Verde Bosque OSI", ref: 731, category: "osi", image: `LINEA OSI 60CC/731.png`},
                    {id: 732, name: "Azul Real OSI", ref: 732, category: "osi", image: `LINEA OSI 60CC/732.png`},
                    {id: 733, name: "Rojo Pasión OSI", ref: 733, category: "osi", image: `LINEA OSI 60CC/733.png`},
                    {id: 735, name: "Violeta Profundo OSI", ref: 735, category: "osi", image: `LINEA OSI 60CC/735.png`}
                ]
            },
            {
                id: "sisi",
                name: "LINEA SISI 60CC",
                color: "sisi",
                products: [
                    {id: 1, name: "Amarillo Sol SISI", ref: 1, category: "sisi", image: `LINEA SISI 60CC/1.png`},
                    {id: 2, name: "Naranja Atardecer SISI", ref: 2, category: "sisi", image: `LINEA SISI 60CC/2.png`},
                    {id: 3, name: "Rojo Carmesí SISI", ref: 3, category: "sisi", image: `LINEA SISI 60CC/3.png`},
                    {id: 5, name: "Rosa Chicle SISI", ref: 5, category: "sisi", image: `LINEA SISI 60CC/5.png`},
                    {id: 9, name: "Lila Suave SISI", ref: 9, category: "sisi", image: `LINEA SISI 60CC/9.png`},
                    {id: 10, name: "Azul Bebé SISI", ref: 10, category: "sisi", image: `LINEA SISI 60CC/10.png`},
                    {id: 11, name: "Azul Profundo SISI", ref: 11, category: "sisi", image: `LINEA SISI 60CC/11.png`},
                    {id: 19, name: "Verde Lima SISI", ref: 19, category: "sisi", image: `LINEA SISI 60CC/19.png`},
                    {id: 25, name: "Verde Bosque SISI", ref: 25, category: "sisi", image: `LINEA SISI 60CC/25.png`},
                    {id: 27, name: "Marrón Café SISI", ref: 27, category: "sisi", image: `LINEA SISI 60CC/27.png`},
                    {id: 28, name: "Beige Claro SISI", ref: 28, category: "sisi", image: `LINEA SISI 60CC/28.png`},
                    {id: 29, name: "Blanco Hueso SISI", ref: 29, category: "sisi", image: `LINEA SISI 60CC/29.png`},
                    {id: 33, name: "Gris Pizarra SISI", ref: 33, category: "sisi", image: `LINEA SISI 60CC/33.png`},
                    {id: 34, name: "Negro Azabache SISI", ref: 34, category: "sisi", image: `LINEA SISI 60CC/34.png`},
                    {id: 41, name: "Plateado Espejo SISI", ref: 41, category: "sisi", image: `LINEA SISI 60CC/41.png`},
                    {id: 42, name: "Dorado Luxe SISI", ref: 42, category: "sisi", image: `LINEA SISI 60CC/42.png`},
                    {id: 44, name: "Cobre Caliente SISI", ref: 44, category: "sisi", image: `LINEA SISI 60CC/44.png`},
                    {id: 46, name: "Bronce Oscuro SISI", ref: 46, category: "sisi", image: `LINEA SISI 60CC/46.png`},
                    {id: 50, name: "Turquesa Vibrante SISI", ref: 50, category: "sisi", image: `LINEA SISI 60CC/50.png`},
                    {id: 51, name: "Coral Suave SISI", ref: 51, category: "sisi", image: `LINEA SISI 60CC/51.png`},
                    {id: 53, name: "Lavanda Profundo SISI", ref: 53, category: "sisi", image: `LINEA SISI 60CC/53.png`},
                    {id: 54, name: "Menta Suave SISI", ref: 54, category: "sisi", image: `LINEA SISI 60CC/54.png`},
                    {id: 64, name: "Borgogna Oscuro SISI", ref: 64, category: "sisi", image: `LINEA SISI 60CC/64.png`},
                    {id: 65, name: "Mostaza Oscuro SISI", ref: 65, category: "sisi", image: `LINEA SISI 60CC/65.png`},
                    {id: 66, name: "Caoba Oscuro SISI", ref: 66, category: "sisi", image: `LINEA SISI 60CC/66.png`},
                    {id: 67, name: "Azul Cobalto SISI", ref: 67, category: "sisi", image: `LINEA SISI 60CC/67.png`},
                    {id: 69, name: "Verde Esmeralda SISI", ref: 69, category: "sisi", image: `LINEA SISI 60CC/69.png`},
                    {id: 73, name: "Rojo Frambuesa SISI", ref: 73, category: "sisi", image: `LINEA SISI 60CC/73.png`},
                    {id: 74, name: "Lila Lavanda SISI", ref: 74, category: "sisi", image: `LINEA SISI 60CC/74.png`},
                    {id: 75, name: "Amarillo Miel SISI", ref: 75, category: "sisi", image: `LINEA SISI 60CC/75.png`},
                    {id: 76, name: "Verde Militar SISI", ref: 76, category: "sisi", image: `LINEA SISI 60CC/76.png`},
                    {id: 81, name: "Azul Noche SISI", ref: 81, category: "sisi", image: `LINEA SISI 60CC/81.png`},
                    {id: 82, name: "Rojo Vino SISI", ref: 82, category: "sisi", image: `LINEA SISI 60CC/82.png`},
                    {id: 84, name: "Violeta Real SISI", ref: 84, category: "sisi", image: `LINEA SISI 60CC/84.png`}
                ]
            },
            {
                id: "metalicos",
                name: "METALICOS SISI 22CC",
                color: "metalicos",
                products: [
                    {id: 251, name: "Oro 24K Metalico", ref: 251, category: "metalicos", image: `METALICOS SISI 22CC/251.png`},
                    {id: 252, name: "Plata Sterling Metalico", ref: 252, category: "metalicos", image: `METALICOS SISI 22CC/252.png`},
                    {id: 253, name: "Cobre Reluciente Metalico", ref: 253, category: "metalicos", image: `METALICOS SISI 22CC/253.png`},
                    {id: 254, name: "Bronce Antiguo Metalico", ref: 254, category: "metalicos", image: `METALICOS SISI 22CC/254.png`},
                    {id: 255, name: "Acero Inoxidable Metalico", ref: 255, category: "metalicos", image: `METALICOS SISI 22CC/255.png`},
                    {id: 256, name: "Grafito Oscuro Metalico", ref: 256, category: "metalicos", image: `METALICOS SISI 22CC/256.png`},
                    {id: 257, name: "Cromo Brillante Metalico", ref: 257, category: "metalicos", image: `METALICOS SISI 22CC/257.png`},
                    {id: 259, name: "Hierro Forjado Metalico", ref: 259, category: "metalicos", image: `METALICOS SISI 22CC/259.png`},
                    {id: 260, name: "Latón Dorado Metalico", ref: 260, category: "metalicos", image: `METALICOS SISI 22CC/260.png`},
                    {id: 261, name: "Níquel Satinado Metalico", ref: 261, category: "metalicos", image: `METALICOS SISI 22CC/261.png`}
                ]
            },
            {
                id: "perlados",
                name: "PERLADO SISI 22CC",
                color: "perlados",
                products: [
                    {id: 153, name: "Perla Blanca SISI", ref: 153, category: "perlados", image: `PERLADO SISI 22CC/153.png`},
                    {id: 154, name: "Perla Dorada SISI", ref: 154, category: "perlados", image: `PERLADO SISI 22CC/154.png`},
                    {id: 160, name: "Perla Plateada SISI", ref: 160, category: "perlados", image: `PERLADO SISI 22CC/160.png`},
                    {id: 166, name: "Perla Azul SISI", ref: 166, category: "perlados", image: `PERLADO SISI 22CC/166.png`},
                    {id: 169, name: "Perla Verde SISI", ref: 169, category: "perlados", image: `PERLADO SISI 22CC/169.png`},
                    {id: 171, name: "Perla Rosa SISI", ref: 171, category: "perlados", image: `PERLADO SISI 22CC/171.png`},
                    {id: 175, name: "Perla Violeta SISI", ref: 175, category: "perlados", image: `PERLADO SISI 22CC/175.png`},
                    {id: 177, name: "Perla Naranja SISI", ref: 177, category: "perlados", image: `PERLADO SISI 22CC/177.png`},
                    {id: 178, name: "Perla Roja SISI", ref: 178, category: "perlados", image: `PERLADO SISI 22CC/178.png`},
                    {id: 181, name: "Perla Negra SISI", ref: 181, category: "perlados", image: `PERLADO SISI 22CC/181.png`},
                    {id: 185, name: "Perla Cobre SISI", ref: 185, category: "perlados", image: `PERLADO SISI 22CC/185.png`},
                    {id: 187, name: "Perla Bronce SISI", ref: 187, category: "perlados", image: `PERLADO SISI 22CC/187.png`},
                    {id: 188, name: "Perla Iris SISI", ref: 188, category: "perlados", image: `PERLADO SISI 22CC/188.png`},
                    {id: 190, name: "Perla Champagne SISI", ref: 190, category: "perlados", image: `PERLADO SISI 22CC/190.png`}
                ]
            },
            {
                id: "neon",
                name: "TELA NEON 30CC",
                color: "neon",
                products: [
                    {id: 460, name: "Neón Rosa Brillante", ref: 460, category: "neon", image: `TELA NEON 30CC/460.png`},
                    {id: 461, name: "Neón Verde Fluorescente", ref: 461, category: "neon", image: `TELA NEON 30CC/461.png`},
                    {id: 462, name: "Neón Amarillo Limón", ref: 462, category: "neon", image: `TELA NEON 30CC/462.png`},
                    {id: 463, name: "Neón Naranja Eléctrico", ref: 463, category: "neon", image: `TELA NEON 30CC/463.png`},
                    {id: 464, name: "Neón Azul Cobalto", ref: 464, category: "neon", image: `TELA NEON 30CC/464.png`},
                    {id: 465, name: "Neón Rojo Intenso", ref: 465, category: "neon", image: `TELA NEON 30CC/465.png`},
                    {id: 466, name: "Neón Violeta Vibrante", ref: 466, category: "neon", image: `TELA NEON 30CC/466.png`},
                    {id: 467, name: "Neón Blanco Brillante", ref: 467, category: "neon", image: `TELA NEON 30CC/467.png`}
                ]
            },
            {
                id: "tela",
                name: "TELA SISI 30CC",
                color: "tela",
                products: [
                    {id: 401, name: "Rojo Pasión Tela", ref: 401, category: "tela", image: `TELA SISI 30CC/401.png`},
                    {id: 402, name: "Azul Real Tela", ref: 402, category: "tela", image: `TELA SISI 30CC/402.png`},
                    {id: 403, name: "Verde Esmeralda Tela", ref: 403, category: "tela", image: `TELA SISI 30CC/403.png`},
                    {id: 405, name: "Amarillo Canario Tela", ref: 405, category: "tela", image: `TELA SISI 30CC/405.png`},
                    {id: 406, name: "Naranja Mandarina Tela", ref: 406, category: "tela", image: `TELA SISI 30CC/406.png`},
                    {id: 407, name: "Rosa Fuscia Tela", ref: 407, category: "tela", image: `TELA SISI 30CC/407.png`},
                    {id: 410, name: "Violeta Intenso Tela", ref: 410, category: "tela", image: `TELA SISI 30CC/410.png`},
                    {id: 411, name: "Turquesa Vibrante Tela", ref: 411, category: "tela", image: `TELA SISI 30CC/411.png`},
                    {id: 413, name: "Blanco Puro Tela", ref: 413, category: "tela", image: `TELA SISI 30CC/413.png`},
                    {id: 419, name: "Negro Profundo Tela", ref: 419, category: "tela", image: `TELA SISI 30CC/419.png`},
                    {id: 420, name: "Gris Perla Tela", ref: 420, category: "tela", image: `TELA SISI 30CC/420.png`},
                    {id: 421, name: "Marrón Chocolate Tela", ref: 421, category: "tela", image: `TELA SISI 30CC/421.png`},
                    {id: 422, name: "Beige Arena Tela", ref: 422, category: "tela", image: `TELA SISI 30CC/422.png`},
                    {id: 423, name: "Lila Suave Tela", ref: 423, category: "tela", image: `TELA SISI 30CC/423.png`},
                    {id: 424, name: "Coral Atardecer Tela", ref: 424, category: "tela", image: `TELA SISI 30CC/424.png`},
                    {id: 426, name: "Verde Oliva Tela", ref: 426, category: "tela", image: `TELA SISI 30CC/426.png`},
                    {id: 428, name: "Azul Marino Tela", ref: 428, category: "tela", image: `TELA SISI 30CC/428.png`},
                    {id: 434, name: "Rojo Vino Tela", ref: 434, category: "tela", image: `TELA SISI 30CC/434.png`},
                    {id: 437, name: "Verde Bosque Tela", ref: 437, category: "tela", image: `TELA SISI 30CC/437.png`},
                    {id: 438, name: "Lavanda Tela", ref: 438, category: "tela", image: `TELA SISI 30CC/438.png`},
                    {id: 439, name: "Menta Tela", ref: 439, category: "tela", image: `TELA SISI 30CC/439.png`},
                    {id: 443, name: "Borgogna Tela", ref: 443, category: "tela", image: `TELA SISI 30CC/443.png`},
                    {id: 445, name: "Mostaza Tela", ref: 445, category: "tela", image: `TELA SISI 30CC/445.png`},
                    {id: 446, name: "Caoba Tela", ref: 446, category: "tela", image: `TELA SISI 30CC/446.png`},
                    {id: 447, name: "Azul Turquesa Tela", ref: 447, category: "tela", image: `TELA SISI 30CC/447.png`}
                ]
            },
            {
                id: "tempera",
                name: "TEMPERA",
                color: "osi",
                products: [
                    {
                        id: 600,
                        name: "Tempera OSI Multiusos",
                        ref: 600,
                        category: "tempera",
                        image: "TEMPERA/600.png"
                    }
                ]
            }
        ];

        // Inicialización
        document.addEventListener('DOMContentLoaded', function() {
            loadCategories();
            setupEventListeners();
            updateCartCount();
        });

        // Cargar categorías
        function loadCategories() {
            const container = document.getElementById('categories-container');
            container.innerHTML = '';
            
            categories.forEach(category => {
                const categorySection = document.createElement('div');
                categorySection.className = 'category-section';
                
                categorySection.innerHTML = `
                    <div class="category-title" data-category="${category.id}">
                        <i class="fas fa-chevron-down"></i>
                        <h3>${category.name}</h3>
                    </div>
                    <div class="category-content" id="category-${category.id}">
                        <div class="product-grid" id="product-list-${category.id}"></div>
                    </div>
                `;
                
                container.appendChild(categorySection);
                
                // Renderizar productos de esta categoría
                renderProductList(category.products, `product-list-${category.id}`, category.color);
            });
            
            // Agregar event listeners a los títulos de categoría para colapsar
            document.querySelectorAll('.category-title').forEach(title => {
                title.addEventListener('click', function() {
                    const categoryId = this.getAttribute('data-category');
                    const content = document.getElementById(`category-${categoryId}`);
                    const icon = this.querySelector('i');
                    
                    if (content.classList.contains('collapsed')) {
                        content.classList.remove('collapsed');
                        this.classList.remove('collapsed');
                        icon.style.transform = 'rotate(0deg)';
                    } else {
                        content.classList.add('collapsed');
                        this.classList.add('collapsed');
                        icon.style.transform = 'rotate(-90deg)';
                    }
                });
            });
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
            const categoryFilter = document.getElementById('category-filter').value;
            
            // Primero, mostrar/ocultar categorías según el filtro
            document.querySelectorAll('.category-section').forEach(section => {
                const categoryId = section.querySelector('.category-title').getAttribute('data-category');
                
                if (categoryFilter && categoryFilter !== categoryId) {
                    section.style.display = 'none';
                } else {
                    section.style.display = 'block';
                }
            });
            
            // Luego, filtrar productos dentro de cada categoría visible
            document.querySelectorAll('.product-grid').forEach(grid => {
                const categoryId = grid.id.split('-')[2];
                const category = categories.find(c => c.id === categoryId);
                
                if (category) {
                    const filtered = category.products.filter(product => {
                        const matchesSearch = product.name.toLowerCase().includes(searchTerm);
                        return matchesSearch;
                    });
                    
                    renderProductList(filtered, grid.id, category.color);
                }
            });
        }

        // Renderizar lista de productos
        function renderProductList(productsToRender, containerId, color) {
            const productList = document.getElementById(containerId);
            if (!productList) return;
            
            productList.innerHTML = '';

            if (productsToRender.length === 0) {
                productList.innerHTML = '<p class="empty-message">No se encontraron productos.</p>';
                return;
            }

            productsToRender.forEach(product => {
                const productCard = document.createElement('div');
                productCard.className = 'product-card';
                
                productCard.innerHTML = `
                    <div class="image-container">
                        <img src="${product.image}" alt="${product.name}">
                        <div class="no-image" style="display: none;">Imagen no disponible</div>
                    </div>
                    <h3>${product.name}</h3>
                    <span class="ref">Ref. ${product.ref}</span>
                    <span class="category-badge category-${color}">${product.category.toUpperCase()}</span>
                `;
                
                // Verificar si la imagen se carga correctamente
                const img = productCard.querySelector('img');
                img.onerror = function() {
                    this.style.display = 'none';
                    this.nextElementSibling.style.display = 'block';
                };
                
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
            const container = document.getElementById('modal-image-container');
            container.innerHTML = `
                <img src="${product.image}" class="modal-product-image" alt="${product.name}">
                <div class="image-placeholder" style="display: none;">
                    <div class="no-image">Imagen no disponible</div>
                </div>
            `;
            
            // Verificar si la imagen se carga correctamente
            const img = container.querySelector('img');
            img.onerror = function() {
                this.style.display = 'none';
                this.nextElementSibling.style.display = 'flex';
            };
            
            // Actualizar descripción del bulto según el producto
            const bulkDescription = document.getElementById('bulk-description');
            if (product.ref === 600) { // Tempera OSI
                bulkDescription.textContent = 'Seleccionar por bultos (100 unidades)';
            } else {
                bulkDescription.textContent = 'Seleccionar por bultos (288 unidades)';
            }
            
            document.getElementById('unit-modal').style.display = 'flex';
            document.getElementById('unit-modal').scrollIntoView({ behavior: 'smooth', block: 'center' });
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
                cartItems.push({
                    product: currentProduct,
                    unit: selectedUnit,
                    quantity: quantity
                });
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
                
                // Calcular unidades totales
                let totalUnits = 0;
                if (item.product.ref === 600) { // Tempera OSI
                    if (item.unit === 'U') totalUnits = item.quantity;
                    if (item.unit === 'DZ') totalUnits = item.quantity * 12;
                    if (item.unit === 'BUL') totalUnits = item.quantity * 100;
                } else {
                    if (item.unit === 'U') totalUnits = item.quantity;
                    if (item.unit === 'DZ') totalUnits = item.quantity * 12;
                    if (item.unit === 'BUL') totalUnits = item.quantity * 288;
                }
                
                cartItem.innerHTML = `
                    <div class="image-container">
                        <img src="${item.product.image}" alt="${item.product.name}">
                        <div class="no-image" style="display: none;">Imagen no disponible</div>
                    </div>
                    <div class="cart-item-info">
                        <h4>${item.product.name}</h4>
                        <div class="ref">Ref. ${item.product.ref}</div>
                        <span class="unit unit-${item.unit}">${getUnitName(item.unit)}: ${item.quantity}</span>
                        <span class="total-units">${totalUnits} unidades</span>
                    </div>
                    <div class="cart-item-controls">
                        <button class="quantity-btn" data-index="${index}" data-action="decrease">-</button>
                        <span class="quantity-display">${item.quantity}</span>
                        <button class="quantity-btn" data-index="${index}" data-action="increase">+</button>
                        <button class="remove-btn" data-index="${index}"><i class="fas fa-trash"></i></button>
                    </div>
                `;
                
                // Verificar si la imagen se carga correctamente
                const img = cartItem.querySelector('img');
                img.onerror = function() {
                    this.style.display = 'none';
                    this.nextElementSibling.style.display = 'block';
                };
                
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

        // Obtener nombre de la unidad
        function getUnitName(unit) {
            const units = {
                'U': 'Unidad',
                'DZ': 'Docena',
                'BUL': 'Bulto'
            };
            return units[unit] || unit;
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
            const paymentMethod = document.getElementById('payment-method').value;

            // Validar campos requeridos
            if (!sellerName || !clientName || !paymentMethod) {
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
            
            try {
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
                doc.setTextColor(67, 97, 238);
                doc.text('ORDEN DE PEDIDO - PINTURAS OSI', pageWidth / 2, yPos, { align: 'center' });
                yPos += 10;

                doc.setFontSize(12);
                doc.setFont(undefined, 'normal');
                doc.setTextColor(0, 0, 0);
                doc.text(`Fecha: ${new Date().toLocaleDateString()}`, margin, yPos);
                doc.text(`N° Pedido: ${Date.now().toString().slice(-6)}`, pageWidth - margin, yPos, { align: 'right' });
                yPos += 10;

                // Información del vendedor y cliente
                doc.setFontSize(12);
                doc.setFont(undefined, 'bold');
                doc.text('Vendedor:', margin, yPos);
                doc.text('Cliente:', margin, yPos + 10);
                doc.text('Método de Pago:', margin, yPos + 20);
                
                doc.setFont(undefined, 'normal');
                doc.text(sellerName, margin + 25, yPos);
                doc.text(clientName, margin + 25, yPos + 10);
                doc.text(paymentMethod === 'BOLIVAR' ? 'Bolívar' : 'Dólar', margin + 45, yPos + 20);
                yPos += 30;

                // Tabla de productos
                doc.setFont(undefined, 'bold');
                doc.setFillColor(67, 97, 238);
                doc.setTextColor(255, 255, 255);
                doc.rect(margin, yPos, maxWidth, 10, 'F');
                
                doc.text('Producto', margin + 5, yPos + 7);
                doc.text('Ref.', margin + 90, yPos + 7);
                doc.text('Unidad', margin + 110, yPos + 7);
                doc.text('Cant.', margin + 140, yPos + 7);
                doc.text('Unid. Tot.', margin + 160, yPos + 7);
                yPos += 12;

                doc.setFont(undefined, 'normal');
                doc.setTextColor(0, 0, 0);
                
                // Variable para el total general de unidades
                let totalGeneralUnidades = 0;
                
                cartItems.forEach(item => {
                    if (yPos > 250) {
                        doc.addPage();
                        yPos = margin;
                        
                        // Volver a dibujar encabezado de tabla en nueva página
                        doc.setFont(undefined, 'bold');
                        doc.setFillColor(67, 97, 238);
                        doc.setTextColor(255, 255, 255);
                        doc.rect(margin, yPos, maxWidth, 10, 'F');
                        doc.text('Producto', margin + 5, yPos + 7);
                        doc.text('Ref.', margin + 90, yPos + 7);
                        doc.text('Unidad', margin + 110, yPos + 7);
                        doc.text('Cant.', margin + 140, yPos + 7);
                        doc.text('Unid. Tot.', margin + 160, yPos + 7);
                        yPos += 12;
                        doc.setFont(undefined, 'normal');
                        doc.setTextColor(0, 0, 0);
                    }
                    
                    // Calcular unidades totales para este ítem
                    let totalUnits = 0;
                    if (item.product.ref === 600) { // Tempera OSI
                        if (item.unit === 'U') totalUnits = item.quantity;
                        if (item.unit === 'DZ') totalUnits = item.quantity * 12;
                        if (item.unit === 'BUL') totalUnits = item.quantity * 100;
                    } else {
                        if (item.unit === 'U') totalUnits = item.quantity;
                        if (item.unit === 'DZ') totalUnits = item.quantity * 12;
                        if (item.unit === 'BUL') totalUnits = item.quantity * 288;
                    }
                    
                    totalGeneralUnidades += totalUnits;
                    
                    // Nombre del producto (con recorte si es muy largo)
                    const productName = item.product.name.length > 30 ? 
                        item.product.name.substring(0, 27) + '...' : item.product.name;
                    
                    doc.text(productName, margin + 5, yPos + 7);
                    doc.text(item.product.ref.toString(), margin + 90, yPos + 7);
                    doc.text(getUnitName(item.unit), margin + 110, yPos + 7);
                    doc.text(item.quantity.toString(), margin + 140, yPos + 7);
                    doc.text(totalUnits.toString(), margin + 160, yPos + 7);
                    
                    // Línea divisoria
                    doc.setDrawColor(200, 200, 200);
                    doc.line(margin, yPos + 10, pageWidth - margin, yPos + 10);
                    
                    yPos += 10;
                });

                // Totales
                yPos += 10;
                doc.setFont(undefined, 'bold');
                doc.text(`Total de Productos: ${cartItems.length}`, margin, yPos);
                doc.text(`Total de Unidades: ${totalGeneralUnidades}`, margin + 100, yPos);
                yPos += 10;
                
                // Notas
                yPos += 5;
                doc.setFontSize(10);
                doc.text("Nota: Tempera OSI (Ref. 600) - 1 bulto = 100 unidades", margin, yPos);
                doc.text("Otras pinturas - 1 bulto = 288 unidades", margin, yPos + 5);
                
                // Pie de página
                yPos = 280;
                doc.setFontSize(10);
                doc.setFont(undefined, 'normal');
                doc.setTextColor(100, 100, 100);
                doc.text('Sistema de Gestión de Pedidos - Pinturas OSI', pageWidth / 2, yPos, { align: 'center' });
                doc.text(new Date().getFullYear().toString(), pageWidth / 2, yPos + 5, { align: 'center' });

                // Guardar el PDF
                doc.save(`pedido_osi_${clientName.replace(/\s+/g, '_')}_${Date.now().toString().slice(-6)}.pdf`);
                
                showLoading(false);
                showNotification('PDF generado exitosamente!');
            } catch (error) {
                console.error("Error al generar PDF:", error);
                showLoading(false);
                showNotification('Error al generar el PDF', 'error');
            }
        }

        // Enviar por WhatsApp
        function sendWhatsApp() {
            const sellerName = document.getElementById('seller-name').value.trim();
            const clientName = document.getElementById('client-name').value.trim();
            const paymentMethod = document.getElementById('payment-method').value;

            // Validar campos requeridos
            if (!sellerName || !clientName || !paymentMethod) {
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
            message += `*Método de Pago:* ${paymentMethod === 'BOLIVAR' ? 'Bolívar' : 'Dólar'}%0A%0A`;
            message += `*DETALLES DEL PEDIDO:*%0A%0A`;

            cartItems.forEach(item => {
                // Calcular unidades totales
                let totalUnits = 0;
                if (item.product.ref === 600) { // Tempera OSI
                    if (item.unit === 'U') totalUnits = item.quantity;
                    if (item.unit === 'DZ') totalUnits = item.quantity * 12;
                    if (item.unit === 'BUL') totalUnits = item.quantity * 100;
                } else {
                    if (item.unit === 'U') totalUnits = item.quantity;
                    if (item.unit === 'DZ') totalUnits = item.quantity * 12;
                    if (item.unit === 'BUL') totalUnits = item.quantity * 288;
                }
                
                message += `- ${item.product.name} (Ref. ${item.product.ref})%0A`;
                message += `  ▶ Unidad: ${getUnitName(item.unit)} | Cantidad: ${item.quantity} | Total unidades: ${totalUnits}%0A%0A`;
            });

            message += `%0A*Total de Productos:* ${cartItems.length}%0A`;

            // Abrir WhatsApp
            window.open(`https://wa.me/?text=${encodeURIComponent(message)}`, '_blank');
            
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
