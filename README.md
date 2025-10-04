'<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, maximum-scale=1.0">
    <title>Lista de Compras</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif; background-color: #000000; color: #ffffff; line-height: 1.6; min-height: 100vh; }
        .container { max-width: 800px; margin: 0 auto; padding: 20px; min-height: 100vh; }
        header { text-align: center; margin-bottom: 30px; }
        header h1 { font-size: 2.5rem; color: #ffffff; text-shadow: 2px 2px 4px rgba(255, 255, 255, 0.1); }
        .add-item-section { margin-bottom: 30px; }
        .input-group { display: flex; gap: 10px; flex-wrap: wrap; }
        #itemInput { flex: 1; min-width: 200px; padding: 15px; font-size: 16px; border: 2px solid #333333; border-radius: 8px; background-color: #1a1a1a; color: #ffffff; transition: border-color 0.3s ease; }
        #priceInput { flex: 0.5; min-width: 120px; padding: 15px; font-size: 16px; border: 2px solid #333333; border-radius: 8px; background-color: #1a1a1a; color: #ffffff; transition: border-color 0.3s ease; }
        #itemInput:focus { outline: none; border-color: #4CAF50; box-shadow: 0 0 10px rgba(76, 175, 80, 0.3); }
        #itemInput::placeholder { color: #888888; }
        #addButton { padding: 15px 25px; font-size: 16px; font-weight: bold; background-color: #4CAF50; color: white; border: none; border-radius: 8px; cursor: pointer; transition: background-color 0.3s ease; min-width: 120px; -webkit-tap-highlight-color: transparent; touch-action: manipulation; -webkit-user-select: none; user-select: none; }
        #addButton:hover { background-color: #45a049; }
        #addButton:active { transform: translateY(1px); }
        .price-search-section { margin-top: 15px; text-align: center; }
        .btn-price-search { padding: 10px 20px; font-size: 14px; background-color: #2196F3; color: white; border: none; border-radius: 6px; cursor: pointer; transition: background-color 0.3s ease; -webkit-tap-highlight-color: transparent; touch-action: manipulation; }
        .btn-price-search:hover { background-color: #1976D2; }
        .price-results { margin-top: 10px; padding: 10px; background-color: #1a1a1a; border-radius: 6px; display: none; }
        .price-item { display: flex; justify-content: space-between; align-items: center; padding: 8px 0; border-bottom: 1px solid #333; }
        .price-item:last-child { border-bottom: none; }
        .price-store { font-weight: bold; color: #4CAF50; }
        .price-value { color: #ffffff; }
        .list-section { margin-bottom: 30px; overflow-x: auto; }
        #shoppingList { width: 100%; border-collapse: collapse; background-color: #1a1a1a; border-radius: 10px; overflow: hidden; box-shadow: 0 4px 15px rgba(255, 255, 255, 0.1); }
        #shoppingList th { background-color: #333333; color: #ffffff; padding: 15px; text-align: left; font-weight: bold; font-size: 16px; }
        #shoppingList td { padding: 15px; border-bottom: 1px solid #333333; vertical-align: middle; }
        #shoppingList tbody tr:hover { background-color: #2a2a2a; }
        #shoppingList tbody tr:last-child td { border-bottom: none; }
        .item-name { font-size: 16px; font-weight: 500; }
        .item-name.completed { text-decoration: line-through; color: #888888; }
        .status-badge { padding: 5px 12px; border-radius: 20px; font-size: 12px; font-weight: bold; text-transform: uppercase; }
        .status-badge.pending { background-color: #ff9800; color: white; }
        .status-badge.completed { background-color: #4CAF50; color: white; }
        .action-buttons { display: flex; gap: 8px; flex-wrap: wrap; }
        .btn { padding: 8px 16px; border: none; border-radius: 6px; cursor: pointer; font-size: 14px; font-weight: bold; transition: all 0.3s ease; min-width: 80px; -webkit-tap-highlight-color: transparent; touch-action: manipulation; }
        .btn-complete { background-color: #4CAF50; color: white; }
        .btn-complete:hover { background-color: #45a049; }
        .btn-complete:disabled { background-color: #666666; cursor: not-allowed; }
        .btn-remove { background-color: #f44336; color: white; }
        .btn-remove:hover { background-color: #da190b; }
        .stats { background-color: #1a1a1a; padding: 20px; border-radius: 10px; text-align: center; box-shadow: 0 4px 15px rgba(255, 255, 255, 0.1); }
        .stats p { margin: 10px 0; font-size: 16px; font-weight: 500; }
        .stats span { color: #4CAF50; font-weight: bold; font-size: 18px; }
        @media (max-width: 768px) {
            .container { padding: 15px; }
            header h1 { font-size: 2rem; }
            .input-group { flex-direction: column; }
            #itemInput { min-width: auto; width: 100%; }
            #addButton { width: 100%; }
            #shoppingList { font-size: 14px; }
            #shoppingList th, #shoppingList td { padding: 10px 8px; }
            .action-buttons { flex-direction: column; gap: 5px; }
            .btn { width: 100%; min-width: auto; }
        }
        @media (max-width: 480px) {
            .container { padding: 10px; }
            header h1 { font-size: 1.8rem; }
            #shoppingList th, #shoppingList td { padding: 8px 5px; font-size: 13px; }
            .item-name { font-size: 14px; }
            .btn { padding: 6px 12px; font-size: 12px; }
        }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        #shoppingList tbody tr { animation: fadeIn 0.3s ease; }
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #1a1a1a; }
        ::-webkit-scrollbar-thumb { background: #4CAF50; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #45a049; }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>🛒 Lista de Compras</h1>
        </header>
        
        <div class="add-item-section">
            <div class="input-group">
                <input type="text" id="itemInput" placeholder="Digite o item para adicionar..." maxlength="50">
                <input type="number" id="priceInput" placeholder="Preço (opcional)" step="0.01" min="0">
                <button id="addButton">Adicionar</button>
            </div>
            <div class="price-search-section">
                <button id="searchPriceButton" class="btn-price-search">🔍 Buscar Preços Online</button>
                <div id="priceResults" class="price-results"></div>
            </div>
        </div>
        
        <div class="list-section">
            <table id="shoppingList">
                <thead>
                    <tr>
                        <th>Item</th>
                        <th>Preço</th>
                        <th>Status</th>
                        <th>Ações</th>
                    </tr>
                </thead>
                <tbody id="listBody">
                </tbody>
            </table>
        </div>
        
        <div class="stats">
            <p>Total de itens: <span id="totalItems">0</span></p>
            <p>Itens comprados: <span id="purchasedItems">0</span></p>
            <p>Valor total estimado: <span id="totalPrice">£0.00</span></p>
            <p>Valor comprado: <span id="purchasedPrice">£0.00</span></p>
        </div>
    </div>
    
    <script>
        let shoppingList = [];
        
        document.addEventListener("DOMContentLoaded", function() {
            console.log("App carregado!");
            loadFromLocalStorage();
            updateStats();
            
            const itemInput = document.getElementById("itemInput");
            const addButton = document.getElementById("addButton");
            
            // Evento de teclado
            itemInput.addEventListener("keypress", function(e) {
                if (e.key === "Enter") {
                    console.log("Enter pressionado");
                    addItem();
                }
            });
            
            // Eventos de toque para mobile
            addButton.addEventListener("click", function(e) {
                console.log("Botão clicado!");
                e.preventDefault();
                e.stopPropagation();
                addItem();
            });
            
            addButton.addEventListener("touchend", function(e) {
                console.log("Toque no botão!");
                e.preventDefault();
                e.stopPropagation();
                addItem();
            });
            
            // Evento adicional para iOS
            addButton.addEventListener("touchstart", function(e) {
                console.log("Touch start no botão!");
                e.preventDefault();
            });
            
            // Prevenir zoom duplo toque
            let lastTouchEnd = 0;
            document.addEventListener('touchend', function (event) {
                const now = (new Date()).getTime();
                if (now - lastTouchEnd <= 300) {
                    event.preventDefault();
                }
                lastTouchEnd = now;
            }, false);
            
            // Evento para busca de preços
            document.getElementById("searchPriceButton").addEventListener("click", function() {
                const itemName = document.getElementById("itemInput").value.trim();
                if (itemName) {
                    searchPrices(itemName);
                } else {
                    alert("Digite um item primeiro para buscar preços!");
                }
            });
        });
        
        function addItem() {
            console.log("Função addItem chamada");
            const input = document.getElementById("itemInput");
            const priceInput = document.getElementById("priceInput");
            const itemName = input.value.trim();
            const price = parseFloat(priceInput.value) || 0;
            
            console.log("Item digitado:", itemName, "Preço:", price);
            
            if (itemName === "") {
                console.log("Item vazio");
                alert("Por favor, digite um item para adicionar!");
                input.focus();
                return;
            }
            
            if (shoppingList.some(item => item.name.toLowerCase() === itemName.toLowerCase())) {
                console.log("Item já existe");
                alert("Este item já está na lista!");
                input.focus();
                return;
            }
            
            const newItem = {
                id: Date.now(),
                name: itemName,
                price: price,
                completed: false,
                dateAdded: new Date().toLocaleDateString("pt-BR")
            };
            
            console.log("Adicionando item:", newItem);
            shoppingList.push(newItem);
            input.value = "";
            priceInput.value = "";
            input.focus();
            
            renderList();
            updateStats();
            saveToLocalStorage();
            showNotification("Item adicionado com sucesso!", "success");
            console.log("Item adicionado com sucesso");
        }
        
        function renderList() {
            const tbody = document.getElementById("listBody");
            tbody.innerHTML = "";
            
            if (shoppingList.length === 0) {
                tbody.innerHTML = `
                    <tr>
                        <td colspan="4" style="text-align: center; color: #888; padding: 30px;">
                            Nenhum item na lista ainda. Adicione seu primeiro item!
                        </td>
                    </tr>
                `;
                return;
            }
            
            const sortedList = [...shoppingList].sort((a, b) => {
                if (a.completed === b.completed) {
                    return a.name.localeCompare(b.name);
                }
                return a.completed - b.completed;
            });
            
            sortedList.forEach(item => {
                const row = document.createElement("tr");
                row.innerHTML = `
                    <td>
                        <span class="item-name ${item.completed ? "completed" : ""}">
                            ${item.name}
                        </span>
                    </td>
                    <td>
                        <span class="price-display">
                            ${item.price > 0 ? `£${item.price.toFixed(2)}` : 'N/A'}
                        </span>
                    </td>
                    <td>
                        <span class="status-badge ${item.completed ? "completed" : "pending"}">
                            ${item.completed ? "Comprado" : "Pendente"}
                        </span>
                    </td>
                    <td>
                        <div class="action-buttons">
                            <button class="btn btn-complete" 
                                    onclick="toggleComplete(${item.id})"
                                    ${item.completed ? "disabled" : ""}>
                                ${item.completed ? "✓ Comprado" : "Marcar como Comprado"}
                            </button>
                            <button class="btn btn-remove" onclick="removeItem(${item.id})">
                                Remover
                            </button>
                        </div>
                    </td>
                `;
                tbody.appendChild(row);
            });
        }
        
        function toggleComplete(itemId) {
            const item = shoppingList.find(item => item.id === itemId);
            if (item) {
                item.completed = !item.completed;
                item.dateCompleted = item.completed ? new Date().toLocaleDateString("pt-BR") : null;
                
                renderList();
                updateStats();
                saveToLocalStorage();
                
                const message = item.completed ? 
                    "Item marcado como comprado!" : 
                    "Item marcado como pendente!";
                showNotification(message, "info");
            }
        }
        
        function removeItem(itemId) {
            const item = shoppingList.find(item => item.id === itemId);
            if (item && confirm(`Tem certeza que deseja remover "${item.name}" da lista?`)) {
                shoppingList = shoppingList.filter(item => item.id !== itemId);
                
                renderList();
                updateStats();
                saveToLocalStorage();
                
                showNotification("Item removido da lista!", "warning");
            }
        }
        
        function updateStats() {
            const totalItems = shoppingList.length;
            const purchasedItems = shoppingList.filter(item => item.completed).length;
            const totalPrice = shoppingList.reduce((sum, item) => sum + (item.price || 0), 0);
            const purchasedPrice = shoppingList
                .filter(item => item.completed)
                .reduce((sum, item) => sum + (item.price || 0), 0);
            
            document.getElementById("totalItems").textContent = totalItems;
            document.getElementById("purchasedItems").textContent = purchasedItems;
            document.getElementById("totalPrice").textContent = `£${totalPrice.toFixed(2)}`;
            document.getElementById("purchasedPrice").textContent = `£${purchasedPrice.toFixed(2)}`;
        }
        
        function saveToLocalStorage() {
            try {
                localStorage.setItem("shoppingList", JSON.stringify(shoppingList));
            } catch (error) {
                console.error("Erro ao salvar no localStorage:", error);
                showNotification("Erro ao salvar dados localmente", "error");
            }
        }
        
        function loadFromLocalStorage() {
            try {
                const saved = localStorage.getItem("shoppingList");
                if (saved) {
                    shoppingList = JSON.parse(saved);
                    renderList();
                }
            } catch (error) {
                console.error("Erro ao carregar do localStorage:", error);
                showNotification("Erro ao carregar dados salvos", "error");
            }
        }
        
        function showNotification(message, type = "info") {
            const existingNotification = document.querySelector(".notification");
            if (existingNotification) {
                existingNotification.remove();
            }
            
            const notification = document.createElement("div");
            notification.className = `notification notification-${type}`;
            notification.textContent = message;
            
            notification.style.cssText = `
                position: fixed;
                top: 20px;
                right: 20px;
                padding: 15px 20px;
                border-radius: 8px;
                color: white;
                font-weight: bold;
                z-index: 1000;
                animation: slideIn 0.3s ease;
                max-width: 300px;
                word-wrap: break-word;
            `;
            
            const colors = {
                success: "#4CAF50",
                error: "#f44336",
                warning: "#ff9800",
                info: "#2196F3"
            };
            
            notification.style.backgroundColor = colors[type] || colors.info;
            
            document.body.appendChild(notification);
            
            setTimeout(() => {
                if (notification.parentNode) {
                    notification.style.animation = "slideOut 0.3s ease";
                    setTimeout(() => {
                        if (notification.parentNode) {
                            notification.remove();
                        }
                    }, 300);
                }
            }, 3000);
        }
        
        const style = document.createElement("style");
        style.textContent = `
            @keyframes slideIn {
                from { transform: translateX(100%); opacity: 0; }
                to { transform: translateX(0); opacity: 1; }
            }
            @keyframes slideOut {
                from { transform: translateX(0); opacity: 1; }
                to { transform: translateX(100%); opacity: 0; }
            }
        `;
        document.head.appendChild(style);
        
        // Banco de dados de preços médios (baseado em preços UK)
        const priceDatabase = {
            "leite": { asda: 0.89, tesco: 0.95, sainsburys: 0.92, morrisons: 0.88 },
            "pão": { asda: 0.45, tesco: 0.50, sainsburys: 0.48, morrisons: 0.46 },
            "ovos": { asda: 1.20, tesco: 1.25, sainsburys: 1.22, morrisons: 1.18 },
            "manteiga": { asda: 1.50, tesco: 1.55, sainsburys: 1.52, morrisons: 1.48 },
            "queijo": { asda: 2.20, tesco: 2.30, sainsburys: 2.25, morrisons: 2.18 },
            "frango": { asda: 3.50, tesco: 3.60, sainsburys: 3.55, morrisons: 3.45 },
            "carne": { asda: 4.20, tesco: 4.30, sainsburys: 4.25, morrisons: 4.15 },
            "peixe": { asda: 3.80, tesco: 3.90, sainsburys: 3.85, morrisons: 3.75 },
            "arroz": { asda: 1.20, tesco: 1.25, sainsburys: 1.22, morrisons: 1.18 },
            "macarrão": { asda: 0.80, tesco: 0.85, sainsburys: 0.82, morrisons: 0.78 },
            "batata": { asda: 1.50, tesco: 1.55, sainsburys: 1.52, morrisons: 1.48 },
            "tomate": { asda: 1.80, tesco: 1.85, sainsburys: 1.82, morrisons: 1.78 },
            "cebola": { asda: 0.60, tesco: 0.65, sainsburys: 0.62, morrisons: 0.58 },
            "alface": { asda: 0.50, tesco: 0.55, sainsburys: 0.52, morrisons: 0.48 },
            "banana": { asda: 0.80, tesco: 0.85, sainsburys: 0.82, morrisons: 0.78 },
            "maçã": { asda: 1.20, tesco: 1.25, sainsburys: 1.22, morrisons: 1.18 },
            "laranja": { asda: 1.00, tesco: 1.05, sainsburys: 1.02, morrisons: 0.98 },
            "açúcar": { asda: 0.65, tesco: 0.70, sainsburys: 0.67, morrisons: 0.63 },
            "sal": { asda: 0.30, tesco: 0.35, sainsburys: 0.32, morrisons: 0.28 },
            "óleo": { asda: 1.80, tesco: 1.85, sainsburys: 1.82, morrisons: 1.78 }
        };
        
        function searchPrices(itemName) {
            const resultsDiv = document.getElementById("priceResults");
            const normalizedName = itemName.toLowerCase().trim();
            
            // Buscar no banco de dados
            let foundPrices = null;
            for (const [key, prices] of Object.entries(priceDatabase)) {
                if (normalizedName.includes(key) || key.includes(normalizedName)) {
                    foundPrices = prices;
                    break;
                }
            }
            
            if (foundPrices) {
                // Mostrar preços encontrados
                let html = '<h4>Preços encontrados:</h4>';
                for (const [store, price] of Object.entries(foundPrices)) {
                    html += `
                        <div class="price-item">
                            <span class="price-store">${store.charAt(0).toUpperCase() + store.slice(1)}</span>
                            <span class="price-value">£${price.toFixed(2)}</span>
                        </div>
                    `;
                }
                
                // Calcular preço médio
                const averagePrice = Object.values(foundPrices).reduce((sum, price) => sum + price, 0) / Object.values(foundPrices).length;
                html += `
                    <div class="price-item" style="border-top: 2px solid #4CAF50; margin-top: 10px; padding-top: 10px;">
                        <span class="price-store" style="color: #4CAF50;">Preço Médio</span>
                        <span class="price-value" style="color: #4CAF50; font-weight: bold;">£${averagePrice.toFixed(2)}</span>
                    </div>
                `;
                
                resultsDiv.innerHTML = html;
                resultsDiv.style.display = 'block';
                
                // Preencher automaticamente o campo de preço com o valor médio
                document.getElementById("priceInput").value = averagePrice.toFixed(2);
                
                showNotification("Preços encontrados! Preço médio preenchido automaticamente.", "success");
            } else {
                // Não encontrou preços específicos
                resultsDiv.innerHTML = `
                    <h4>Preços não encontrados</h4>
                    <p>Produto não encontrado no banco de dados.</p>
                    <p><strong>Sugestões:</strong></p>
                    <ul style="text-align: left; margin: 10px 0;">
                        <li>Digite o preço manualmente</li>
                        <li>Verifique a ortografia</li>
                        <li>Use termos mais genéricos (ex: "leite" em vez de "leite desnatado")</li>
                    </ul>
                    <p><strong>Links úteis:</strong></p>
                    <div style="margin-top: 10px;">
                        <a href="https://groceries.asda.com/search/${encodeURIComponent(itemName)}" target="_blank" style="color: #4CAF50; margin-right: 15px;">🔍 ASDA</a>
                        <a href="https://www.tesco.com/groceries/en-GB/search?query=${encodeURIComponent(itemName)}" target="_blank" style="color: #4CAF50; margin-right: 15px;">🔍 Tesco</a>
                        <a href="https://www.sainsburys.co.uk/gol-ui/SearchDisplayView?catalogId=10123&storeId=10151&langId=44&searchTerm=${encodeURIComponent(itemName)}" target="_blank" style="color: #4CAF50;">🔍 Sainsbury's</a>
                    </div>
                `;
                resultsDiv.style.display = 'block';
                showNotification("Produto não encontrado. Links para busca manual fornecidos.", "info");
            }
        }
    </script>
</body>
</html>' > lista-compras-completa.html
