<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Controle de Estoque</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
        }

        body {
            background: #0a0a0a;
            min-height: 100vh;
            color: #e0e0e0;
        }

        .app-container {
            display: flex;
            min-height: 100vh;
        }

        /* Menu Lateral */
        .sidebar {
            width: 260px;
            background: #1a1a1a;
            box-shadow: 2px 0 12px rgba(0,0,0,0.5);
            padding: 32px 0;
            border-right: 1px solid #333;
            height: auto;
            align-self: flex-start;
        }

        .logo {
            padding: 0 24px 32px 24px;
            border-bottom: 1px solid #333;
            margin-bottom: 24px;
        }

        .logo h2 {
            color: #f97316;
            font-size: 20px;
            font-weight: 700;
            letter-spacing: -0.02em;
        }

        .logo p {
            color: #aaa;
            font-size: 13px;
            margin-top: 4px;
        }

        .menu-item {
            display: flex;
            align-items: center;
            padding: 12px 24px;
            color: #ccc;
            text-decoration: none;
            transition: all 0.2s;
            cursor: pointer;
            border-left: 3px solid transparent;
            margin: 4px 0;
        }

        .menu-item:hover {
            background: #2a2a2a;
            color: #f97316;
        }

        .menu-item.active {
            background: #2a2a2a;
            color: #f97316;
            border-left-color: #f97316;
        }

        .menu-item span {
            font-size: 20px;
            margin-right: 14px;
        }

        .menu-item .menu-text {
            font-size: 14px;
            font-weight: 500;
        }

        /* Conteúdo Principal */
        .main-content {
            flex: 1;
            padding: 32px;
        }

        /* Header */
        .header {
            background: #1a1a1a;
            border-radius: 20px;
            padding: 24px 32px;
            margin-bottom: 32px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.3);
            display: flex;
            justify-content: space-between;
            align-items: center;
            border: 1px solid #333;
        }

        .header h1 {
            color: #f97316;
            font-size: 24px;
            font-weight: 700;
        }

        .user-info {
            display: flex;
            align-items: center;
            gap: 24px;
        }

        .user-name {
            color: #ccc;
            font-weight: 500;
            font-size: 14px;
        }

        .logout-btn {
            background: #f97316;
            color: #0a0a0a;
            border: none;
            padding: 8px 20px;
            border-radius: 40px;
            cursor: pointer;
            font-weight: 500;
            font-size: 13px;
            transition: all 0.2s;
        }

        .logout-btn:hover {
            background: #fb923c;
        }

        /* Cards de Estatísticas (agora clicáveis) */
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
            gap: 24px;
            margin-bottom: 32px;
        }

        .stat-card {
            background: #1a1a1a;
            border-radius: 20px;
            padding: 24px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.3);
            display: flex;
            align-items: center;
            transition: transform 0.2s, box-shadow 0.2s;
            border: 1px solid #333;
            cursor: pointer;
        }

        .stat-card:hover {
            transform: translateY(-4px);
            box-shadow: 0 12px 24px rgba(0,0,0,0.5);
            border-color: #f97316;
        }

        .stat-icon {
            width: 56px;
            height: 56px;
            background: #2a2a2a;
            border-radius: 16px;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-right: 20px;
        }

        .stat-icon span {
            font-size: 28px;
        }

        .stat-info h3 {
            color: #aaa;
            font-size: 13px;
            font-weight: 500;
            margin-bottom: 4px;
        }

        .stat-info .number {
            color: #f97316;
            font-size: 28px;
            font-weight: 700;
        }

        .stat-info .label {
            color: #888;
            font-size: 12px;
            margin-top: 2px;
        }

        /* Seções */
        .section {
            background: #1a1a1a;
            border-radius: 20px;
            padding: 28px;
            margin-bottom: 32px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.3);
            border: 1px solid #333;
        }

        .section-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 24px;
        }

        .section-header h2 {
            color: #f97316;
            font-size: 18px;
            font-weight: 700;
        }

        .section-header button {
            background: #f97316;
            color: #0a0a0a;
            border: none;
            padding: 10px 20px;
            border-radius: 40px;
            cursor: pointer;
            font-weight: 500;
            font-size: 13px;
            display: flex;
            align-items: center;
            gap: 8px;
            transition: all 0.2s;
        }

        .section-header button:hover {
            background: #fb923c;
            transform: scale(1.02);
        }

        /* Listas */
        .list-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 16px;
            border-bottom: 1px solid #333;
            transition: background 0.2s;
        }

        .list-item:last-child {
            border-bottom: none;
        }

        .list-item:hover {
            background: #2a2a2a;
            border-radius: 12px;
        }

        .item-info h4 {
            color: #f97316;
            font-size: 16px;
            font-weight: 600;
            margin-bottom: 4px;
        }

        .item-info p {
            color: #ccc;
            font-size: 13px;
        }

        .item-info small {
            color: #888;
            font-size: 12px;
        }

        .item-status {
            padding: 4px 12px;
            border-radius: 40px;
            font-size: 12px;
            font-weight: 600;
        }

        .status-low {
            background: #442a2a;
            color: #f97316;
        }

        .status-ok {
            background: #1a3a1a;
            color: #4caf50;
        }

        .status-loan {
            background: #1a3a4a;
            color: #42a5f5;
        }

        .status-archived {
            background: #333;
            color: #aaa;
        }

        .item-actions {
            display: flex;
            gap: 8px;
        }

        .item-actions button {
            padding: 6px 12px;
            border: none;
            border-radius: 40px;
            cursor: pointer;
            font-size: 12px;
            font-weight: 500;
            transition: all 0.2s;
        }

        .item-actions button:hover {
            transform: translateY(-2px);
            filter: brightness(0.9);
        }

        .btn-edit {
            background: #2a2a2a;
            color: #f97316;
        }

        .btn-loan {
            background: #42a5f5;
            color: #0a0a0a;
        }

        .btn-delete {
            background: #f44336;
            color: #0a0a0a;
        }

        .btn-return {
            background: #4caf50;
            color: #0a0a0a;
        }

        .btn-permanent {
            background: #f97316;
            color: #0a0a0a;
        }

        .empty-state {
            text-align: center;
            padding: 48px 20px;
            color: #888;
        }

        .empty-state span {
            font-size: 48px;
            display: block;
            margin-bottom: 16px;
            opacity: 0.6;
        }

        .empty-state p {
            font-size: 14px;
        }

        .filters {
            display: flex;
            gap: 16px;
            margin-bottom: 24px;
            flex-wrap: wrap;
        }

        .search-box {
            flex: 1;
            min-width: 250px;
            padding: 12px 16px;
            border: 2px solid #333;
            border-radius: 40px;
            font-size: 14px;
            background: #2a2a2a;
            color: #e0e0e0;
            transition: border-color 0.2s;
        }

        .search-box:focus {
            outline: none;
            border-color: #f97316;
        }

        .category-filter {
            display: flex;
            gap: 8px;
            overflow-x: auto;
            padding: 4px 0;
        }

        .category-filter button {
            padding: 8px 16px;
            border: none;
            border-radius: 40px;
            background: #2a2a2a;
            color: #ccc;
            font-weight: 500;
            font-size: 13px;
            cursor: pointer;
            white-space: nowrap;
            transition: all 0.2s;
        }

        .category-filter button.active {
            background: #f97316;
            color: #0a0a0a;
        }

        .date-filter {
            padding: 10px 16px;
            border: 2px solid #333;
            border-radius: 40px;
            font-size: 13px;
            background: #2a2a2a;
            color: #e0e0e0;
        }

        /* Modal */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0,0,0,0.8);
            justify-content: center;
            align-items: center;
            z-index: 1000;
            backdrop-filter: blur(4px);
        }

        .modal.active {
            display: flex;
        }

        .modal-content {
            background: #1a1a1a;
            border-radius: 24px;
            padding: 32px;
            max-width: 500px;
            width: 90%;
            max-height: 90vh;
            overflow-y: auto;
            box-shadow: 0 20px 40px rgba(0,0,0,0.5);
            border: 1px solid #333;
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 24px;
        }

        .modal-header h2 {
            color: #f97316;
            font-size: 20px;
            font-weight: 700;
        }

        .close-btn {
            background: none;
            border: none;
            font-size: 24px;
            cursor: pointer;
            color: #888;
        }

        .modal-form {
            display: flex;
            flex-direction: column;
            gap: 16px;
        }

        .modal-form input,
        .modal-form select,
        .modal-form textarea {
            padding: 14px;
            border: 2px solid #333;
            border-radius: 12px;
            font-size: 14px;
            background: #2a2a2a;
            color: #e0e0e0;
            transition: border-color 0.2s;
        }

        .modal-form input:focus,
        .modal-form select:focus,
        .modal-form textarea:focus {
            outline: none;
            border-color: #f97316;
        }

        .modal-form button {
            background: #f97316;
            color: #0a0a0a;
            border: none;
            padding: 16px;
            border-radius: 12px;
            font-weight: 600;
            font-size: 15px;
            cursor: pointer;
            margin-top: 8px;
            transition: background 0.2s;
        }

        .modal-form button:hover {
            background: #fb923c;
        }

        .datetime-row {
            display: flex;
            gap: 10px;
        }

        .datetime-row input {
            flex: 1;
        }

        /* Login */
        .login-container {
            max-width: 380px;
            margin: 120px auto;
            background: #1a1a1a;
            border-radius: 32px;
            padding: 40px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.5);
            border: 1px solid #333;
        }

        .login-header {
            text-align: center;
            margin-bottom: 32px;
        }

        .login-header h1 {
            color: #f97316;
            font-size: 28px;
            font-weight: 700;
            margin-bottom: 8px;
        }

        .login-header p {
            color: #aaa;
            font-size: 14px;
        }

        .login-form {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .login-form input {
            padding: 16px;
            border: 2px solid #333;
            border-radius: 16px;
            font-size: 15px;
            background: #2a2a2a;
            color: #e0e0e0;
        }

        .login-form button {
            background: #f97316;
            color: #0a0a0a;
            border: none;
            padding: 16px;
            border-radius: 16px;
            font-size: 15px;
            font-weight: 600;
            cursor: pointer;
            transition: background 0.2s;
        }

        .login-form button:hover {
            background: #fb923c;
        }

        .hidden {
            display: none !important;
        }
    </style>
</head>
<body>
    <!-- Tela de Login -->
    <div id="loginScreen" class="login-container">
        <div class="login-header">
            <h1>Controle de Estoque</h1>
            <p>Faça login para acessar o sistema</p>
        </div>
        <div class="login-form">
            <input type="text" id="username" placeholder="Usuário" value="admin">
            <input type="password" id="password" placeholder="Senha" value="123456">
            <button onclick="login()">Entrar</button>
        </div>
    </div>

    <!-- Aplicativo Principal -->
    <div id="appScreen" class="app-container hidden">
        <!-- Menu Lateral -->
        <div class="sidebar">
            <div class="logo">
                <h2>Controle de Estoque</h2>
                <p>Menu Principal</p>
            </div>
            
            <div class="menu-item active" onclick="showSection('dashboard')">
                <span>📊</span>
                <span class="menu-text">Painel</span>
            </div>
            <div class="menu-item" onclick="showSection('products')">
                <span>📦</span>
                <span class="menu-text">Produtos</span>
            </div>
            <div class="menu-item" onclick="showSection('loans')">
                <span>🔧</span>
                <span class="menu-text">Empréstimos</span>
            </div>
            <div class="menu-item" onclick="showSection('permanent')">
                <span>🗑️</span>
                <span class="menu-text">Retiradas</span>
            </div>
            <div class="menu-item" onclick="showSection('history')">
                <span>📋</span>
                <span class="menu-text">Histórico</span>
            </div>
        </div>

        <!-- Conteúdo Principal -->
        <div class="main-content">
            <!-- Header -->
            <div class="header">
                <h1 id="sectionTitle">Painel</h1>
                <div class="user-info">
                    <span class="user-name">admin</span>
                    <button class="logout-btn" onclick="logout()">Sair</button>
                </div>
            </div>

            <!-- Seção Painel (sem listas duplicadas) -->
            <div id="dashboardSection">
                <div class="stats-grid">
                    <div class="stat-card" onclick="showSection('products')">
                        <div class="stat-icon">
                            <span>📦</span>
                        </div>
                        <div class="stat-info">
                            <h3>Total de Produtos</h3>
                            <div class="number" id="totalProducts">0</div>
                            <div class="label">itens ativos</div>
                        </div>
                    </div>
                    <div class="stat-card" onclick="filterByLowStock()">
                        <div class="stat-icon">
                            <span>⚠️</span>
                        </div>
                        <div class="stat-info">
                            <h3>Estoque Baixo</h3>
                            <div class="number" id="lowStockCount">0</div>
                            <div class="label">produtos</div>
                        </div>
                    </div>
                    <div class="stat-card" onclick="filterByActiveLoans()">
                        <div class="stat-icon">
                            <span>🔧</span>
                        </div>
                        <div class="stat-info">
                            <h3>Emprestados</h3>
                            <div class="number" id="loanedCount">0</div>
                            <div class="label">itens</div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Seção Produtos -->
            <div id="productsSection" class="hidden">
                <div class="section">
                    <div class="section-header">
                        <h2 id="productsTitle">📦 Produtos Ativos</h2>
                        <button onclick="openProductModal()">
                            <span>+</span> Novo Produto
                        </button>
                    </div>

                    <div class="filters">
                        <input type="text" class="search-box" id="searchProduct" placeholder="Buscar por nome ou local..." onkeyup="filterProducts()">
                        <div class="category-filter" id="categoryFilter"></div>
                    </div>

                    <div id="productsList"></div>
                </div>
            </div>

            <!-- Seção Empréstimos -->
            <div id="loansSection" class="hidden">
                <div class="section">
                    <div class="section-header">
                        <h2>🔧 Gerenciar Empréstimos</h2>
                        <button onclick="openLoanModal()">
                            <span>+</span> Novo Empréstimo
                        </button>
                    </div>

                    <div id="allLoansList"></div>
                </div>
            </div>

            <!-- Seção Retiradas -->
            <div id="permanentSection" class="hidden">
                <div class="section">
                    <div class="section-header">
                        <h2>🗑️ Retiradas Permanentes</h2>
                        <p style="color: #aaa;">Itens retirados para uso único (não serão devolvidos)</p>
                    </div>

                    <div id="permanentList"></div>
                </div>
            </div>

            <!-- Seção Histórico -->
            <div id="historySection" class="hidden">
                <div class="section">
                    <div class="section-header">
                        <h2>📋 Histórico de Movimentações</h2>
                        <div>
                            <input type="date" id="historyDate" class="date-filter" onchange="loadHistory()">
                        </div>
                    </div>

                    <div id="historyList"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal de Produto -->
    <div id="productModal" class="modal">
        <div class="modal-content">
            <div class="modal-header">
                <h2 id="modalTitle">Novo Produto</h2>
                <button class="close-btn" onclick="closeModal('productModal')">&times;</button>
            </div>
            <div class="modal-form">
                <input type="text" id="productName" placeholder="Nome do produto">
                <input type="text" id="productLocal" placeholder="Localização">
                <select id="productCategory">
                    <option value="">Selecione a categoria</option>
                    <option value="Jardim">Jardim</option>
                    <option value="Elétrica">Elétrica</option>
                    <option value="Ferramentas">Ferramentas</option>
                    <option value="EPI">EPI</option>
                    <option value="Hidráulica">Hidráulica</option>
                    <option value="Geral">Geral</option>
                </select>
                <input type="number" id="productQuantity" placeholder="Quantidade">
                <input type="number" id="productMinQuantity" placeholder="Quantidade mínima">
                <input type="hidden" id="editingProductId">
                <button onclick="saveProduct()">Salvar</button>
            </div>
        </div>
    </div>

    <!-- Modal de Empréstimo -->
    <div id="loanModal" class="modal">
        <div class="modal-content">
            <div class="modal-header">
                <h2>Novo Empréstimo</h2>
                <button class="close-btn" onclick="closeModal('loanModal')">&times;</button>
            </div>
            <div class="modal-form">
                <input type="text" id="loanUserName" placeholder="Nome de quem está pegando">
                <select id="loanProduct"></select>
                <input type="number" id="loanQuantity" placeholder="Quantidade" value="1">
                <div class="datetime-row">
                    <input type="date" id="loanDate">
                    <input type="time" id="loanTime" value="08:00">
                </div>
                <textarea id="loanObservation" placeholder="Observação"></textarea>
                <button onclick="saveLoan()">Registrar Empréstimo</button>
            </div>
        </div>
    </div>

    <!-- Modal de Devolução -->
    <div id="returnModal" class="modal">
        <div class="modal-content">
            <div class="modal-header">
                <h2>Registrar Devolução</h2>
                <button class="close-btn" onclick="closeModal('returnModal')">&times;</button>
            </div>
            <div class="modal-form">
                <div style="background: #2a2a2a; padding: 16px; border-radius: 12px; margin-bottom: 16px;" id="returnInfo"></div>
                <div class="datetime-row">
                    <input type="date" id="returnDate">
                    <input type="time" id="returnTime" value="08:00">
                </div>
                <textarea id="returnObservation" placeholder="Observação"></textarea>
                <input type="hidden" id="returnLoanId">
                <button onclick="saveReturn()">Confirmar Devolução</button>
            </div>
        </div>
    </div>

    <!-- Modal de Retirada Permanente -->
    <div id="permanentModal" class="modal">
        <div class="modal-content">
            <div class="modal-header">
                <h2>Registrar Retirada Permanente</h2>
                <button class="close-btn" onclick="closeModal('permanentModal')">&times;</button>
            </div>
            <div class="modal-form">
                <div style="background: #2a2a2a; padding: 16px; border-radius: 12px; margin-bottom: 16px;" id="permanentInfo"></div>
                <input type="text" id="permanentUserName" placeholder="Nome de quem está retirando">
                <input type="number" id="permanentQuantity" placeholder="Quantidade" value="1">
                <div class="datetime-row">
                    <input type="date" id="permanentDate">
                    <input type="time" id="permanentTime" value="08:00">
                </div>
                <textarea id="permanentObservation" placeholder="Motivo da retirada (opcional)"></textarea>
                <input type="hidden" id="permanentProductId">
                <button onclick="savePermanent()">Confirmar Retirada</button>
            </div>
        </div>
    </div>

    <script>
        // DADOS INICIAIS
        let products = JSON.parse(localStorage.getItem('products')) || [
            { id: 1, name: 'Tesoura de Poda', local: 'Prateleira A1', category: 'Jardim', quantity: 8, minQuantity: 3 },
            { id: 2, name: 'Alicate Universal', local: 'Gaveta 10', category: 'Elétrica', quantity: 5, minQuantity: 2 },
            { id: 3, name: 'Martelo', local: 'Prateleira B1', category: 'Ferramentas', quantity: 10, minQuantity: 4 },
            { id: 4, name: 'Luva de Raspa', local: 'Armário D1', category: 'EPI', quantity: 12, minQuantity: 5 },
            { id: 5, name: 'Fita Veda-rosca', local: 'Gaveta 16', category: 'Hidráulica', quantity: 15, minQuantity: 5 },
            { id: 6, name: 'Cadeado', local: 'Gaveta 17', category: 'Geral', quantity: 8, minQuantity: 3 }
        ];
        
        let loans = JSON.parse(localStorage.getItem('loans')) || [];
        let movements = JSON.parse(localStorage.getItem('movements')) || [];
        let currentUser = null;
        let currentCategory = 'todos';

        // LOGIN
        function login() {
            const username = document.getElementById('username').value;
            const password = document.getElementById('password').value;
            
            if (username === 'admin' && password === '123456') {
                currentUser = { name: username };
                document.getElementById('loginScreen').classList.add('hidden');
                document.getElementById('appScreen').classList.remove('hidden');
                updateDashboard();
                loadProducts();
                loadCategoryFilter();
                loadLoanProductSelect();
                loadPermanentItems();
                loadHistory();
            } else {
                alert('Usuário ou senha incorretos! Use admin / 123456');
            }
        }

        function logout() {
            currentUser = null;
            document.getElementById('loginScreen').classList.remove('hidden');
            document.getElementById('appScreen').classList.add('hidden');
        }

        // NAVEGAÇÃO
        function showSection(section) {
            document.querySelectorAll('.menu-item').forEach(item => item.classList.remove('active'));
            event.currentTarget.classList.add('active');
            
            document.getElementById('dashboardSection').classList.add('hidden');
            document.getElementById('productsSection').classList.add('hidden');
            document.getElementById('loansSection').classList.add('hidden');
            document.getElementById('historySection').classList.add('hidden');
            document.getElementById('permanentSection').classList.add('hidden');
            
            document.getElementById(section + 'Section').classList.remove('hidden');
            
            const titles = {
                'dashboard': 'Painel',
                'products': 'Produtos',
                'loans': 'Empréstimos',
                'history': 'Histórico',
                'permanent': 'Retiradas'
            };
            document.getElementById('sectionTitle').textContent = titles[section];
            
            if (section === 'products') {
                loadProducts();
                loadCategoryFilter();
                document.getElementById('productsTitle').textContent = '📦 Produtos Ativos';
            } else if (section === 'loans') {
                loadAllLoans();
            } else if (section === 'history') {
                loadHistory();
            } else if (section === 'permanent') {
                loadPermanentItems();
            }
        }

        // FILTROS ESPECIAIS (para os cards)
        function filterByLowStock() {
            showSection('products');
            // Aqui você pode definir um filtro especial, mas por enquanto só vai para produtos
            // Idealmente, você aplicaria um filtro de "estoque baixo" automaticamente
        }

        function filterByActiveLoans() {
            showSection('loans');
            // Vai para a seção de empréstimos
        }

        // DASHBOARD
        function updateDashboard() {
            document.getElementById('totalProducts').textContent = products.length;
            
            const lowStock = products.filter(p => p.quantity <= p.minQuantity);
            document.getElementById('lowStockCount').textContent = lowStock.length;
            
            const activeLoans = loans.filter(l => !l.returned);
            document.getElementById('loanedCount').textContent = activeLoans.length;
        }

        // Utilitário para formatar data/hora
        function formatDateTime(datetimeStr) {
            if (!datetimeStr) return '';
            const [date, time] = datetimeStr.split(' ');
            return `${date} ${time}`;
        }

        // PRODUTOS
        function loadCategoryFilter() {
            const cats = ['todos', ...new Set(products.map(p => p.category))];
            const filterDiv = document.getElementById('categoryFilter');
            
            filterDiv.innerHTML = cats.map(cat => 
                `<button class="${cat === 'todos' ? 'active' : ''}" onclick="filterByCategory('${cat}')">
                    ${cat === 'todos' ? 'Todos' : cat}
                </button>`
            ).join('');
        }

        function filterByCategory(cat) {
            currentCategory = cat;
            document.querySelectorAll('#categoryFilter button').forEach(btn => {
                btn.classList.remove('active');
                if (btn.textContent === (cat === 'todos' ? 'Todos' : cat)) {
                    btn.classList.add('active');
                }
            });
            filterProducts();
        }

        function filterProducts() {
            const search = document.getElementById('searchProduct').value.toLowerCase();
            const filtered = products.filter(p => 
                (currentCategory === 'todos' || p.category === currentCategory) &&
                (p.name.toLowerCase().includes(search) || p.local.toLowerCase().includes(search))
            );
            displayProducts(filtered);
        }

        function displayProducts(list) {
            const activeLoans = loans.filter(l => !l.returned);
            
            document.getElementById('productsList').innerHTML = list.map(p => {
                const loaned = activeLoans.filter(l => l.productId === p.id).reduce((s, l) => s + l.quantity, 0);
                const available = p.quantity - loaned;
                
                return `
                    <div class="list-item">
                        <div class="item-info">
                            <h4>${p.name}</h4>
                            <p>📍 ${p.local}</p>
                            <p>🏷️ ${p.category}</p>
                            ${loaned > 0 ? `<small>🔧 ${loaned} emprestado(s)</small>` : ''}
                        </div>
                        <div class="item-status ${p.quantity <= p.minQuantity ? 'status-low' : 'status-ok'}">
                            Disponível: ${available}
                        </div>
                        <div class="item-actions">
                            <button class="btn-edit" onclick="editProduct(${p.id})">Editar</button>
                            <button class="btn-loan" onclick="openLoanModal(${p.id})">Emprestar</button>
                            <button class="btn-permanent" onclick="openPermanentModal(${p.id})">Retirar</button>
                            <button class="btn-delete" onclick="deleteProduct(${p.id})">Excluir</button>
                        </div>
                    </div>
                `;
            }).join('') || '<div class="empty-state"><span>🔍</span><p>Nenhum item encontrado</p></div>';
        }

        function loadProducts() {
            filterProducts();
        }

        function openProductModal() {
            document.getElementById('modalTitle').textContent = 'Novo Produto';
            document.getElementById('productName').value = '';
            document.getElementById('productLocal').value = '';
            document.getElementById('productCategory').value = '';
            document.getElementById('productQuantity').value = '';
            document.getElementById('productMinQuantity').value = '';
            document.getElementById('editingProductId').value = '';
            document.getElementById('productModal').classList.add('active');
        }

        function editProduct(id) {
            const p = products.find(p => p.id === id);
            if (p) {
                document.getElementById('modalTitle').textContent = 'Editar Produto';
                document.getElementById('productName').value = p.name;
                document.getElementById('productLocal').value = p.local;
                document.getElementById('productCategory').value = p.category;
                document.getElementById('productQuantity').value = p.quantity;
                document.getElementById('productMinQuantity').value = p.minQuantity;
                document.getElementById('editingProductId').value = id;
                document.getElementById('productModal').classList.add('active');
            }
        }

        function saveProduct() {
            const id = document.getElementById('editingProductId').value;
            const product = {
                name: document.getElementById('productName').value,
                local: document.getElementById('productLocal').value,
                category: document.getElementById('productCategory').value,
                quantity: parseInt(document.getElementById('productQuantity').value) || 0,
                minQuantity: parseInt(document.getElementById('productMinQuantity').value) || 0
            };
            
            if (!product.name || !product.local || !product.category) {
                alert('Preencha todos os campos!');
                return;
            }
            
            if (id) {
                const index = products.findIndex(p => p.id == id);
                products[index] = { ...products[index], ...product };
            } else {
                product.id = Date.now();
                products.push(product);
            }
            
            localStorage.setItem('products', JSON.stringify(products));
            closeModal('productModal');
            loadProducts();
            updateDashboard();
            loadCategoryFilter();
            loadLoanProductSelect();
        }

        function deleteProduct(id) {
            if (confirm('Excluir permanentemente este item?')) {
                products = products.filter(p => p.id !== id);
                localStorage.setItem('products', JSON.stringify(products));
                loadProducts();
                updateDashboard();
                loadCategoryFilter();
                loadPermanentItems();
            }
        }

        // EMPRÉSTIMOS
        function loadLoanProductSelect() {
            const select = document.getElementById('loanProduct');
            const activeLoans = loans.filter(l => !l.returned);
            
            select.innerHTML = '<option value="">Selecione um item</option>' +
                products.map(p => {
                    const loaned = activeLoans.filter(l => l.productId === p.id).reduce((s, l) => s + l.quantity, 0);
                    const available = p.quantity - loaned;
                    return `<option value="${p.id}" ${available <= 0 ? 'disabled' : ''}>
                        ${p.name} (Disponível: ${available})
                    </option>`;
                }).join('');
        }

        function openLoanModal(productId = null) {
            document.getElementById('loanUserName').value = '';
            document.getElementById('loanQuantity').value = '1';
            document.getElementById('loanDate').value = new Date().toISOString().split('T')[0];
            document.getElementById('loanTime').value = new Date().toTimeString().slice(0,5);
            document.getElementById('loanObservation').value = '';
            if (productId) document.getElementById('loanProduct').value = productId;
            document.getElementById('loanModal').classList.add('active');
        }

        function saveLoan() {
            const user = document.getElementById('loanUserName').value;
            const pid = parseInt(document.getElementById('loanProduct').value);
            const qty = parseInt(document.getElementById('loanQuantity').value);
            const date = document.getElementById('loanDate').value;
            const time = document.getElementById('loanTime').value;
            
            if (!user || !pid || !qty || !date || !time) {
                alert('Preencha todos os campos!');
                return;
            }
            
            const product = products.find(p => p.id === pid);
            const datetime = `${date} ${time}`;
            
            loans.push({
                id: Date.now(),
                userName: user,
                productId: pid,
                productName: product.name,
                productLocal: product.local,
                quantity: qty,
                datetime: datetime,
                returned: false
            });
            
            movements.push({
                id: Date.now() + 1,
                productId: pid,
                productName: product.name,
                quantity: qty,
                datetime: datetime,
                type: 'loan',
                observation: `Emprestado para ${user}`
            });
            
            localStorage.setItem('loans', JSON.stringify(loans));
            localStorage.setItem('movements', JSON.stringify(movements));
            
            closeModal('loanModal');
            updateDashboard();
            loadProducts();
            loadLoanProductSelect();
            showSection('dashboard');
        }

        function loadAllLoans() {
            const list = document.getElementById('allLoansList');
            const active = loans.filter(l => !l.returned);
            const returned = loans.filter(l => l.returned);
            
            if (active.length === 0 && returned.length === 0) {
                list.innerHTML = '<div class="empty-state"><span>📭</span><p>Nenhum empréstimo registrado</p></div>';
                return;
            }
            
            let html = '<h3 style="margin-bottom: 16px; color: #f97316;">🔧 Empréstimos Ativos</h3>';
            
            if (active.length > 0) {
                html += active.map(l => `
                    <div class="list-item">
                        <div class="item-info">
                            <h4>${l.productName}</h4>
                            <p>👤 ${l.userName}</p>
                            <p>📍 ${l.productLocal}</p>
                            <small>📅 ${formatDateTime(l.datetime)}</small>
                        </div>
                        <div class="item-status status-loan">
                            ${l.quantity} unid.
                        </div>
                        <div class="item-actions">
                            <button class="btn-return" onclick="openReturnModal(${l.id})">Devolver</button>
                        </div>
                    </div>
                `).join('');
            } else {
                html += '<p style="color: #aaa; padding: 16px;">Nenhum empréstimo ativo</p>';
            }
            
            if (returned.length > 0) {
                html += '<h3 style="margin: 32px 0 16px; color: #f97316;">✅ Histórico de Devoluções</h3>';
                html += returned.slice(0, 5).map(l => `
                    <div class="list-item">
                        <div class="item-info">
                            <h4>${l.productName}</h4>
                            <p>👤 ${l.userName}</p>
                            <p>📅 Empréstimo: ${formatDateTime(l.datetime)}</p>
                            <p>✅ Devolução: ${formatDateTime(l.returnDatetime)}</p>
                        </div>
                        <div class="item-status status-ok">
                            ${l.quantity} unid.
                        </div>
                    </div>
                `).join('');
            }
            
            list.innerHTML = html;
        }

        function openReturnModal(loanId) {
            const loan = loans.find(l => l.id === loanId);
            if (loan) {
                document.getElementById('returnInfo').innerHTML = `
                    <strong>Devolução:</strong><br>
                    👤 ${loan.userName}<br>
                    🔧 ${loan.productName}<br>
                    📦 Quantidade: ${loan.quantity}
                `;
                document.getElementById('returnDate').value = new Date().toISOString().split('T')[0];
                document.getElementById('returnTime').value = new Date().toTimeString().slice(0,5);
                document.getElementById('returnLoanId').value = loanId;
                document.getElementById('returnModal').classList.add('active');
            }
        }

        function saveReturn() {
            const loanId = parseInt(document.getElementById('returnLoanId').value);
            const returnDate = document.getElementById('returnDate').value;
            const returnTime = document.getElementById('returnTime').value;
            
            const index = loans.findIndex(l => l.id === loanId);
            if (index === -1) return;
            
            loans[index].returned = true;
            loans[index].returnDatetime = `${returnDate} ${returnTime}`;
            
            movements.push({
                id: Date.now(),
                productId: loans[index].productId,
                productName: loans[index].productName,
                quantity: loans[index].quantity,
                datetime: `${returnDate} ${returnTime}`,
                type: 'return',
                observation: `Devolvido por ${loans[index].userName}`
            });
            
            localStorage.setItem('loans', JSON.stringify(loans));
            localStorage.setItem('movements', JSON.stringify(movements));
            
            closeModal('returnModal');
            updateDashboard();
            loadProducts();
            loadLoanProductSelect();
            showSection('dashboard');
        }

        // RETIRADA PERMANENTE
        function openPermanentModal(productId) {
            const product = products.find(p => p.id === productId);
            if (product) {
                document.getElementById('permanentInfo').innerHTML = `
                    <strong>Registrar retirada permanente:</strong><br>
                    🔧 ${product.name}<br>
                    📦 Quantidade atual: ${product.quantity}
                `;
                document.getElementById('permanentUserName').value = '';
                document.getElementById('permanentQuantity').value = 1;
                document.getElementById('permanentDate').value = new Date().toISOString().split('T')[0];
                document.getElementById('permanentTime').value = new Date().toTimeString().slice(0,5);
                document.getElementById('permanentObservation').value = '';
                document.getElementById('permanentProductId').value = productId;
                document.getElementById('permanentModal').classList.add('active');
            }
        }

        function savePermanent() {
            const productId = parseInt(document.getElementById('permanentProductId').value);
            const userName = document.getElementById('permanentUserName').value;
            const qty = parseInt(document.getElementById('permanentQuantity').value);
            const date = document.getElementById('permanentDate').value;
            const time = document.getElementById('permanentTime').value;
            const observation = document.getElementById('permanentObservation').value;
            
            if (!userName || !qty || !date || !time) {
                alert('Preencha nome, quantidade, data e hora!');
                return;
            }
            
            const product = products.find(p => p.id === productId);
            if (!product) return;

            if (qty > product.quantity) {
                alert('Quantidade indisponível em estoque!');
                return;
            }

            // Reduzir do estoque
            product.quantity -= qty;
            
            const datetime = `${date} ${time}`;

            // Registrar movimentação de baixa
            movements.push({
                id: Date.now(),
                productId: product.id,
                productName: product.name,
                quantity: qty,
                datetime: datetime,
                type: 'permanent',
                observation: `Retirada permanente - ${userName} - ${observation}`
            });

            // Registrar na lista de retiradas
            let permanentItems = JSON.parse(localStorage.getItem('permanent')) || [];
            permanentItems.push({
                id: Date.now(),
                userName: userName,
                productName: product.name,
                productLocal: product.local,
                quantity: qty,
                datetime: datetime,
                observation: observation
            });
            localStorage.setItem('permanent', JSON.stringify(permanentItems));
            
            localStorage.setItem('products', JSON.stringify(products));
            localStorage.setItem('movements', JSON.stringify(movements));
            
            closeModal('permanentModal');
            loadProducts();
            updateDashboard();
            loadPermanentItems();
            loadHistory();
        }

        // CARREGAR ITENS DE RETIRADA PERMANENTE
        function loadPermanentItems() {
            const permanentItems = JSON.parse(localStorage.getItem('permanent')) || [];
            const list = document.getElementById('permanentList');
            
            if (permanentItems.length > 0) {
                list.innerHTML = permanentItems.sort((a,b) => b.id - a.id).map(p => `
                    <div class="list-item">
                        <div class="item-info">
                            <h4>${p.productName}</h4>
                            <p>👤 ${p.userName}</p>
                            <p>📍 ${p.productLocal}</p>
                            <small>📅 ${formatDateTime(p.datetime)}</small>
                            ${p.observation ? `<small>📝 ${p.observation}</small>` : ''}
                        </div>
                        <div class="item-status status-archived">
                            ${p.quantity} unid.
                        </div>
                    </div>
                `).join('');
            } else {
                list.innerHTML = '<div class="empty-state"><span>🗑️</span><p>Nenhuma retirada registrada</p></div>';
            }
        }

        // HISTÓRICO
        function loadHistory() {
            const dateFilter = document.getElementById('historyDate').value;
            let filtered = movements;
            if (dateFilter) {
                filtered = movements.filter(m => m.datetime && m.datetime.startsWith(dateFilter));
            }
            
            const list = document.getElementById('historyList');
            if (filtered.length === 0) {
                list.innerHTML = '<div class="empty-state"><span>📋</span><p>Nenhuma movimentação encontrada</p></div>';
                return;
            }
            
            list.innerHTML = filtered.sort((a,b) => b.id - a.id).map(m => {
                let icon = m.type === 'entry' ? '📥' : m.type === 'exit' ? '📤' : m.type === 'loan' ? '🔧' : m.type === 'return' ? '↩️' : '🗑️';
                let typeName = m.type === 'entry' ? 'Entrada' : m.type === 'exit' ? 'Saída' : m.type === 'loan' ? 'Empréstimo' : m.type === 'return' ? 'Devolução' : 'Retirada';
                let color = m.type === 'entry' ? '#4caf50' : m.type === 'exit' ? '#f44336' : m.type === 'loan' ? '#42a5f5' : m.type === 'return' ? '#4caf50' : '#f97316';
                
                return `
                    <div class="list-item">
                        <div class="item-info">
                            <h4>${m.productName}</h4>
                            <p>${typeName} • ${m.quantity} unid.</p>
                            <small>${formatDateTime(m.datetime)} ${m.observation ? ' • ' + m.observation : ''}</small>
                        </div>
                        <div style="background: ${color}20; color: ${color}; padding: 4px 12px; border-radius: 40px; font-size: 12px; font-weight: 600;">
                            ${icon}
                        </div>
                    </div>
                `;
            }).join('');
        }

        function closeModal(id) {
            document.getElementById(id).classList.remove('active');
        }
    </script>
</body>
</html>
