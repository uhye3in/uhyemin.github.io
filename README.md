# uhyemin.github.io
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>캠퍼스 중고거래 플랫폼</title>
    <style>
        :root {
            --primary: #2196F3;
            --primary-hover: #1976D2;
            --bg: #F4F7F6;
            --card-bg: #FFFFFF;
            --text-main: #333333;
            --text-muted: #777777;
            --border: #E0E0E0;
            --shadow: 0 4px 6px rgba(0,0,0,0.05);
            --danger: #e74c3c;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Pretendard', 'Malgun Gothic', sans-serif; }

        body { background-color: var(--bg); color: var(--text-main); line-height: 1.6; }

        /* Navigation */
        nav { background-color: var(--card-bg); box-shadow: var(--shadow); position: sticky; top: 0; z-index: 100; }
        .nav-container { max-width: 1000px; margin: 0 auto; padding: 15px 20px; display: flex; justify-content: space-between; align-items: center; }
        #logo { font-size: 20px; color: var(--primary); cursor: pointer; font-weight: bold; }
        .nav-links button { background: none; border: none; margin-left: 15px; font-size: 15px; cursor: pointer; color: var(--text-main); font-weight: 500; }
        .nav-links button:hover { color: var(--primary); }
        .nav-links .btn-outline { border: 1px solid var(--border); padding: 5px 12px; border-radius: 20px; }

        /* General Layout */
        #app-container { max-width: 1000px; margin: 30px auto; padding: 0 20px; }
        .view { display: none; }
        .view.active { display: block; animation: fadeIn 0.3s; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .hidden { display: none !important; }

        /* Buttons & Inputs */
        input, textarea, select { width: 100%; padding: 12px; margin-bottom: 15px; border: 1px solid var(--border); border-radius: 8px; outline: none; background: #fff; }
        input:focus, textarea:focus, select:focus { border-color: var(--primary); }
        .btn-primary { width: 100%; padding: 12px; background: var(--primary); color: white; border: none; border-radius: 8px; font-size: 16px; cursor: pointer; transition: 0.2s; font-weight: bold; }
        .btn-primary:hover { background: var(--primary-hover); }

        /* Auth Views */
        .auth-card { max-width: 400px; margin: 50px auto; background: var(--card-bg); padding: 30px; border-radius: 12px; box-shadow: var(--shadow); }
        .auth-card h2 { text-align: center; margin-bottom: 20px; }
        .switch-auth { text-align: center; margin-top: 15px; font-size: 14px; color: var(--text-muted); }
        .switch-auth span { color: var(--primary); cursor: pointer; font-weight: bold; }

        /* Home & Categories */
        .search-bar { display: flex; gap: 10px; margin-bottom: 15px; }
        .search-bar input { margin-bottom: 0; }
        .search-bar button { width: 120px; }
        .category-filters { display: flex; gap: 10px; margin-bottom: 25px; overflow-x: auto; padding-bottom: 5px; }
        .category-btn { padding: 8px 16px; border: 1px solid var(--border); background: var(--card-bg); border-radius: 20px; cursor: pointer; white-space: nowrap; font-size: 14px; transition: 0.2s; }
        .category-btn.active { background: var(--primary); color: white; border-color: var(--primary); }

        /* Product Grid & Card */
        .product-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(220px, 1fr)); gap: 20px; }
        .product-card { background: var(--card-bg); border-radius: 12px; overflow: hidden; box-shadow: var(--shadow); position: relative; transition: transform 0.2s; }
        .product-card:hover { transform: translateY(-5px); }
        .product-img { width: 100%; height: 180px; object-fit: cover; background-color: #eaeaea; cursor: pointer; }
        .product-info { padding: 15px; cursor: pointer; }
        .product-category { font-size: 12px; color: var(--text-muted); margin-bottom: 4px; }
        .product-title { font-size: 16px; font-weight: bold; margin-bottom: 5px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
        .product-price { color: var(--primary); font-weight: bold; font-size: 18px; }
        
        /* Wish Icon on Card */
        .wish-icon { position: absolute; top: 10px; right: 10px; background: rgba(255,255,255,0.8); border-radius: 50%; width: 32px; height: 32px; display: flex; align-items: center; justify-content: center; cursor: pointer; font-size: 18px; box-shadow: var(--shadow); transition: transform 0.1s; z-index: 10; }
        .wish-icon:active { transform: scale(0.9); }

        /* Image Preview in Upload */
        .preview-box { width: 100%; height: 200px; border: 2px dashed var(--border); border-radius: 8px; margin-bottom: 15px; display: flex; align-items: center; justify-content: center; overflow: hidden; background: #f9f9f9; color: var(--text-muted); }
        .preview-box img { width: 100%; height: 100%; object-fit: cover; display: none; }

        /* Detail View */
        .detail-container { background: var(--card-bg); border-radius: 12px; padding: 30px; box-shadow: var(--shadow); display: flex; gap: 30px; flex-wrap: wrap; }
        .detail-img { flex: 1; min-width: 300px; height: 350px; object-fit: cover; border-radius: 8px; background: #eaeaea;}
        .detail-info { flex: 1; min-width: 300px; display: flex; flex-direction: column; justify-content: center; }
        .seller-info { margin: 15px 0; padding: 15px; background: var(--bg); border-radius: 8px; font-size: 14px; }
        .detail-actions { display: flex; gap: 10px; margin-top: 20px; }
        .detail-actions button { flex: 1; }

        /* Forms & MyPage */
        .form-container, .mypage-container { background: var(--card-bg); padding: 30px; border-radius: 12px; box-shadow: var(--shadow); }
        
        .profile-header { display: flex; align-items: center; gap: 20px; padding-bottom: 20px; border-bottom: 1px solid var(--border); margin-bottom: 20px; }
        .profile-avatar { width: 60px; height: 60px; background: var(--primary); color: white; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 24px; font-weight: bold; }
        .profile-info h3 { margin-bottom: 5px; }
        
        .tabs { display: flex; gap: 10px; border-bottom: 2px solid var(--border); margin-bottom: 20px; padding-bottom: 10px; }
        .tab { background: none; border: none; font-size: 16px; font-weight: bold; color: var(--text-muted); cursor: pointer; }
        .tab.active { color: var(--primary); border-bottom: 2px solid var(--primary); padding-bottom: 12px; margin-bottom: -12px; }
        
        .msg-card { padding: 15px; border: 1px solid var(--border); border-radius: 8px; margin-bottom: 10px; transition: background 0.2s; color: var(--text-main); text-align: left; }
        .msg-card:hover { background: #f9f9f9; }
    </style>
</head>
<body>
    <nav id="navbar" class="hidden">
        <div class="nav-container">
            <h1 id="logo" onclick="app.showView('home')">🎓 캠퍼스 마켓</h1>
            <div class="nav-links">
                <button onclick="app.showView('home')">홈</button>
                <button onclick="app.showView('upload')">상품 등록</button>
                <button onclick="app.showView('mypage')">마이페이지</button>
                <button onclick="app.logout()" class="btn-outline">로그아웃</button>
            </div>
        </div>
    </nav>

    <main id="app-container">
        
        <section id="view-login" class="view active">
            <div class="auth-card">
                <h2>로그인</h2>
                <input type="text" id="login-id" placeholder="학번 입력 (예: 20240001)" required>
                <input type="password" id="login-pw" placeholder="비밀번호 입력" required>
                <button onclick="app.login()" class="btn-primary">로그인</button>
                <p class="switch-auth">계정이 없으신가요? <span onclick="app.toggleAuth()">회원가입</span></p>
            </div>
            
            <div class="auth-card hidden" id="register-card">
                <h2>회원가입</h2>
                <input type="text" id="reg-id" placeholder="학번 (예: 20240001)" required>
                <input type="email" id="reg-email" placeholder="학교 이메일" required>
                <input type="password" id="reg-pw" placeholder="비밀번호" required>
                <button onclick="app.register()" class="btn-primary">가입하기</button>
                <p class="switch-auth">이미 계정이 있으신가요? <span onclick="app.toggleAuth()">로그인</span></p>
            </div>
        </section>

        <section id="view-home" class="view">
            <div class="search-bar">
                <input type="text" id="search-input" placeholder="어떤 상품을 찾고 계신가요?">
                <button onclick="app.search()" class="btn-primary">검색</button>
            </div>
            <div class="category-filters" id="category-filters">
                <button class="category-btn active" onclick="app.setCategory('전체')">전체</button>
                <button class="category-btn" onclick="app.setCategory('전자기기')">💻 전자기기</button>
                <button class="category-btn" onclick="app.setCategory('도서/교재')">📚 도서/교재</button>
                <button class="category-btn" onclick="app.setCategory('생활용품')">🧴 생활용품</button>
                <button class="category-btn" onclick="app.setCategory('기타')">기타</button>
            </div>
            <div class="product-grid" id="product-list">
                </div>
        </section>

        <section id="view-upload" class="view">
            <div class="form-container">
                <h2>새 상품 등록</h2>
                <div class="preview-box">
                    <span id="preview-text">이미지 미리보기</span>
                    <img id="preview-img" src="" alt="preview">
                </div>
                <input type="file" id="upload-img" accept="image/*" onchange="app.previewImage(event)">
                
                <select id="upload-category">
                    <option value="전자기기">💻 전자기기</option>
                    <option value="도서/교재">📚 도서/교재</option>
                    <option value="생활용품">🧴 생활용품</option>
                    <option value="기타">기타</option>
                </select>
                
                <input type="text" id="upload-title" placeholder="상품명" required>
                <input type="number" id="upload-price" placeholder="가격 (원)" required>
                <textarea id="upload-desc" placeholder="상품 설명 (구매 시기, 사용감, 하자 여부 등을 상세히 적어주세요)" rows="5"></textarea>
                <button onclick="app.addProduct()" class="btn-primary">등록하기</button>
            </div>
        </section>

        <section id="view-detail" class="view">
            <div class="detail-container" id="detail-content">
                </div>
        </section>

        <section id="view-mypage" class="view">
            <div class="mypage-container">
                <div class="profile-header">
                    <div class="profile-avatar">🎓</div>
                    <div class="profile-info" id="profile-info">
                        </div>
                </div>

                <div class="tabs">
                    <button class="tab active" onclick="app.switchTab('my-items')">내 상품</button>
                    <button class="tab" onclick="app.switchTab('my-wishlist')">찜 목록</button>
                    <button class="tab" onclick="app.switchTab('my-messages')">받은 메시지</button>
                </div>
                <div id="mypage-content">
                    </div>
            </div>
        </section>

    </main>

    <script>
        const app = {
            currentUser: null,
            currentCategory: '전체',
            db: {
                users: JSON.parse(localStorage.getItem('users')) || [],
                products: JSON.parse(localStorage.getItem('products')) || [],
                wishlists: JSON.parse(localStorage.getItem('wishlists')) || [],
                messages: JSON.parse(localStorage.getItem('messages')) || []
            },

            init() {
                const session = sessionStorage.getItem('currentUser');
                if (session) {
                    this.currentUser = JSON.parse(session);
                    document.getElementById('navbar').classList.remove('hidden');
                    this.showView('home');
                } else {
                    this.showView('login');
                }
            },

            saveDB() {
                localStorage.setItem('users', JSON.stringify(this.db.users));
                localStorage.setItem('products', JSON.stringify(this.db.products));
                localStorage.setItem('wishlists', JSON.stringify(this.db.wishlists));
                localStorage.setItem('messages', JSON.stringify(this.db.messages));
            },

            // --- Navigation ---
            showView(viewId) {
                document.querySelectorAll('.view').forEach(el => el.classList.remove('active'));
                document.getElementById(`view-${viewId}`).classList.add('active');
                
                if (viewId === 'home') this.renderProducts();
                if (viewId === 'mypage') {
                    this.renderProfile();
                    this.switchTab('my-items');
                }
            },

            // --- Authentication ---
            toggleAuth() {
                document.getElementById('view-login').firstElementChild.classList.toggle('hidden');
                document.getElementById('register-card').classList.toggle('hidden');
            },

            register() {
                const id = document.getElementById('reg-id').value;
                const email = document.getElementById('reg-email').value;
                const pw = document.getElementById('reg-pw').value;

                if (!id || !email || !pw) return alert("모든 항목을 입력해주세요.");
                if (this.db.users.find(u => u.id === id)) return alert("이미 존재하는 학번입니다.");

                this.db.users.push({ id, email, pw });
                this.saveDB();
                alert("회원가입 완료! 로그인해주세요.");
                
                document.getElementById('reg-id').value = '';
                document.getElementById('reg-email').value = '';
                document.getElementById('reg-pw').value = '';
                this.toggleAuth();
            },

            login() {
                const id = document.getElementById('login-id').value;
                const pw = document.getElementById('login-pw').value;
                const user = this.db.users.find(u => u.id === id && u.pw === pw);

                if (user) {
                    this.currentUser = { id: user.id };
                    sessionStorage.setItem('currentUser', JSON.stringify(this.currentUser));
                    document.getElementById('navbar').classList.remove('hidden');
                    this.showView('home');
                    document.getElementById('login-id').value = '';
                    document.getElementById('login-pw').value = ''; 
                } else {
                    alert("학번이나 비밀번호가 일치하지 않습니다.");
                }
            },

            logout() {
                this.currentUser = null;
                sessionStorage.removeItem('currentUser');
                document.getElementById('navbar').classList.add('hidden');
                this.showView('login');
            },

            // --- Products & Home ---
            setCategory(category) {
                this.currentCategory = category;
                
                document.querySelectorAll('.category-btn').forEach(btn => {
                    btn.classList.remove('active');
                    if(btn.innerText.includes(category)) btn.classList.add('active');
                });
                
                this.renderProducts();
            },

            previewImage(event) {
                const reader = new FileReader();
                reader.onload = function() {
                    const img = document.getElementById('preview-img');
                    const text = document.getElementById('preview-text');
                    img.src = reader.result;
                    img.style.display = 'block';
                    text.style.display = 'none';
                }
                if(event.target.files[0]) {
                    reader.readAsDataURL(event.target.files[0]);
                }
            },

            addProduct() {
                const category = document.getElementById('upload-category').value;
                const title = document.getElementById('upload-title').value;
                const price = document.getElementById('upload-price').value;
                const desc = document.getElementById('upload-desc').value;
                const imgInput = document.getElementById('upload-img');

                if (!title || !price) return alert("상품명과 가격을 입력해주세요.");

                const processProduct = (imgData) => {
                    const newProduct = {
                        id: Date.now(),
                        category, title, price, desc,
                        sellerId: this.currentUser.id,
                        img: imgData || '',
                        date: new Date().toLocaleDateString()
                    };
                    this.db.products.push(newProduct);
                    this.saveDB();
                    alert("상품이 성공적으로 등록되었습니다.");
                    
                    document.getElementById('upload-title').value = '';
                    document.getElementById('upload-price').value = '';
                    document.getElementById('upload-desc').value = '';
                    document.getElementById('upload-img').value = '';
                    document.getElementById('preview-img').style.display = 'none';
                    document.getElementById('preview-text').style.display = 'block';
                    
                    this.setCategory('전체');
                    this.showView('home');
                };

                if (imgInput.files && imgInput.files[0]) {
                    const reader = new FileReader();
                    reader.onload = (e) => processProduct(e.target.result);
                    reader.readAsDataURL(imgInput.files[0]);
                } else {
                    processProduct('');
                }
            },

            renderProducts(query = "") {
                const container = document.getElementById('product-list');
                container.innerHTML = '';
                
                let filtered = this.db.products.filter(p => 
                    (p.title.includes(query) || p.desc.includes(query))
                );

                if (this.currentCategory !== '전체') {
                    filtered = filtered.filter(p => p.category === this.currentCategory);
                }
                
                filtered = filtered.reverse();

                if (filtered.length === 0) {
                    container.innerHTML = "<p style='grid-column: 1 / -1; text-align: center; color: #777; margin-top: 50px; font-size: 18px;'>해당하는 상품이 없습니다. 😢</p>";
                    return;
                }

                filtered.forEach(p => {
                    const isWish = this.db.wishlists.find(w => w.userId === this.currentUser.id && w.productId === p.id);
                    
                    const card = document.createElement('div');
                    card.className = 'product-card';
                    card.innerHTML = `
                        <div class="wish-icon" onclick="app.toggleWishHome(${p.id}, event)">
                            ${isWish ? '❤️' : '🤍'}
                        </div>
                        <img src="${p.img || 'https://via.placeholder.com/300?text=No+Image'}" class="product-img" onclick="app.showDetail(${p.id})">
                        <div class="product-info" onclick="app.showDetail(${p.id})">
                            <div class="product-category">${p.category}</div>
                            <div class="product-title">${p.title}</div>
                            <div class="product-price">${Number(p.price).toLocaleString()}원</div>
                        </div>
                    `;
                    container.appendChild(card);
                });
            },

            search() {
                const q = document.getElementById('search-input').value;
                this.renderProducts(q);
            },

            // --- Detail & Interactions ---
            showDetail(id) {
                const p = this.db.products.find(p => p.id === id);
                if(!p) return;

                const isWish = this.db.wishlists.find(w => w.userId === this.currentUser.id && w.productId === p.id);

                const container = document.getElementById('detail-content');
                container.innerHTML = `
                    <img src="${p.img || 'https://via.placeholder.com/400?text=No+Image'}" class="detail-img">
                    <div class="detail-info">
                        <span style="color: var(--text-muted); font-size: 14px;">${p.category}</span>
                        <h2 style="margin-top: 5px;">${p.title}</h2>
                        <h1 style="color: var(--primary); margin: 15px 0;">${Number(p.price).toLocaleString()}원</h1>
                        <div style="background: #f9f9f9; padding: 15px; border-radius: 8px; margin-bottom: 20px; white-space: pre-wrap; min-height: 100px; text-align: left;">${p.desc}</div>
                        <div class="seller-info">
                            <strong>판매자:</strong> 학우 (${p.sellerId})<br>
                            <strong>등록일:</strong> ${p.date}
                        </div>
                        <div class="detail-actions">
                            <button class="btn-primary" style="background: ${isWish ? 'var(--danger)' : '#bdc3c7'}; color: white;" onclick="app.toggleWishDetail(${p.id})">
                                ${isWish ? '❤️ 찜 취소' : '🤍 찜하기'}
                            </button>
                            ${p.sellerId !== this.currentUser.id ? 
                                `<button class="btn-primary" onclick="app.sendMessage('${p.sellerId}', '${p.title}')">쪽지 보내기</button>` 
                                : ''}
                        </div>
                    </div>
                `;
                this.showView('detail');
            },

            toggleWishHome(productId, event) {
                event.stopPropagation();
                this.toggleWishLogic(productId);
                this.renderProducts();
            },

            toggleWishDetail(productId) {
                this.toggleWishLogic(productId);
                this.showDetail(productId);
            },

            toggleWishLogic(productId) {
                const idx = this.db.wishlists.findIndex(w => w.userId === this.currentUser.id && w.productId === productId);
                if (idx > -1) {
                    this.db.wishlists.splice(idx, 1);
                } else {
                    this.db.wishlists.push({ userId: this.currentUser.id, productId });
                }
                this.saveDB();
            },

            sendMessage(toId, itemTitle) {
                const text = prompt(`[${toId}] 판매자에게 보낼 메시지를 입력하세요:`);
                if (text && text.trim() !== "") {
                    this.db.messages.push({
                        from: this.currentUser.id,
                        to: toId,
                        item: itemTitle,
                        text: text,
                        date: new Date().toLocaleString()
                    });
                    this.saveDB();
                    alert("메시지가 성공적으로 전송되었습니다.");
                }
            },

            // --- My Page ---
            renderProfile() {
                const myProductsCount = this.db.products.filter(p => p.sellerId === this.currentUser.id).length;
                document.getElementById('profile-info').innerHTML = `
                    <h3>학번: ${this.currentUser.id}</h3>
                    <p style="color: var(--text-muted); font-size: 14px;">등록한 상품: <strong>${myProductsCount}</strong>개</p>
                `;
            },

            switchTab(tabId) {
                document.querySelectorAll('.tab').forEach(el => el.classList.remove('active'));
                
                const tabs = document.querySelectorAll('.tab');
                tabs.forEach(tab => {
                    if (tab.getAttribute('onclick') && tab.getAttribute('onclick').includes(tabId)) {
                        tab.classList.add('active');
                    }
                });
                
                const container = document.getElementById('mypage-content');
                container.innerHTML = '';

                if (tabId === 'my-items' || tabId === 'my-wishlist') {
                    container.className = 'product-grid';
                    
                    let itemsToRender = [];
                    if (tabId === 'my-items') {
                        itemsToRender = this.db.products.filter(p => p.sellerId === this.currentUser.id).reverse();
                    } else {
                        const wlist = this.db.wishlists.filter(w => w.userId === this.currentUser.id).reverse();
                        wlist.forEach(w => {
                            const p = this.db.products.find(prod => prod.id === w.productId);
                            if(p) itemsToRender.push(p);
                        });
                    }

                    if(itemsToRender.length === 0) {
                        container.className = '';
                        container.innerHTML = `<p style='color:#777; padding: 5px 0; text-align: center; grid-column: 1/-1;'>${tabId === 'my-items' ? '등록한 상품이 없습니다.' : '찜한 상품이 없습니다.'}</p>`;
                        return;
                    }

                    itemsToRender.forEach(p => {
                        const isWish = this.db.wishlists.find(w => w.userId === this.currentUser.id && w.productId === p.id);
                        const card = document.createElement('div');
                        card.className = 'product-card';
                        card.innerHTML = `
                            <div class="wish-icon" onclick="app.toggleWishMyPage(${p.id}, '${tabId}', event)">
                                ${isWish ? '❤️' : '🤍'}
                            </div>
                            <img src="${p.img || 'https://via.placeholder.com/300?text=No+Image'}" class="product-img" onclick="app.showDetail(${p.id})">
                            <div class="product-info" onclick="app.showDetail(${p.id})">
                                <div class="product-title">${p.title}</div>
                                <div class="product-price">${Number(p.price).toLocaleString()}원</div>
                            </div>
                        `;
                        container.appendChild(card);
                    });
                } 
                else if (tabId === 'my-messages') {
                    container.className = '';
                    const msgs = this.db.messages.filter(m => m.to === this.currentUser.id).reverse();
                    if(msgs.length === 0) {
                        container.innerHTML = "<p style='color:#777; padding: 30px 0; text-align: center;'>받은 메시지가 없습니다.</p>";
                        return;
                    }
                    msgs.forEach(m => {
                        container.innerHTML += `
                            <div class="msg-card">
                                <div style="display: flex; justify-content: space-between; margin-bottom: 5px;">
                                    <strong>보낸사람: 학우 (${m.from})</strong>
                                    <small style="color: #888;">${m.date}</small>
                                </div>
                                <div style="font-size: 13px; color: var(--primary); margin-bottom: 10px;">문의 상품: ${m.item}</div>
                                <p style="padding: 12px; background: #f4f7f6; border-radius: 6px;">${m.text}</p>
                            </div>
                        `;
                    });
                }
            },
            
            toggleWishMyPage(productId, tabId, event) {
                event.stopPropagation();
                this.toggleWishLogic(productId);
                this.switchTab(tabId);
            }
        };

        window.onload = () => app.init();
    </script>
</body>
</html>
