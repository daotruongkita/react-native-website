
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hệ Thống Đăng Nhập</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; margin: 20px; }
        input, button { margin: 5px; padding: 8px; }
    </style>
</head>
<body>
    <h2>Đăng Ký</h2>
    <input type="text" id="regUsername" placeholder="Tên đăng nhập">
    <input type="password" id="regPassword" placeholder="Mật khẩu">
    <input type="text" id="regFullname" placeholder="Họ và tên">
    <button onclick="registerUser()">Đăng Ký</button>

    <h2>Đăng Nhập</h2>
    <input type="text" id="loginUsername" placeholder="Tên đăng nhập">
    <input type="password" id="loginPassword" placeholder="Mật khẩu">
    <button onclick="loginUser()">Đăng Nhập</button>

    <h2>Quản lý tài khoản</h2>
    <button onclick="logoutUser()">Đăng Xuất</button>
    <button onclick="showBalance()">Xem Số Dư</button>
    <button onclick="addMoney(5000)">+5.000 VNĐ</button>
    <button onclick="subtractMoney(3000)">-3.000 VNĐ</button>
    <button onclick="lockAccount()">Khóa Tài Khoản</button>
    <p id="message"></p>

    <script>
        class User {
            constructor(username, password, fullname, balance = 10000) {
                this.username = username;
                this.password = password;
                this.fullname = fullname;
                this.balance = balance;
                this.locked = false;
            }

            formatBalance() {
                return this.balance.toLocaleString('vi-VN') + ' VNĐ';
            }
        }

        class AuthSystem {
            constructor() {
                this.users = [];
                this.currentUser = null;
            }

            register(username, password, fullname) {
                if (this.users.some(user => user.username === username)) {
                    return 'Tài khoản đã tồn tại!';
                }
                const newUser = new User(username, password, fullname);
                this.users.push(newUser);
                return 'Đăng ký thành công!';
            }

            login(username, password) {
                const user = this.users.find(user => user.username === username);
                if (!user) return 'Tài khoản không tồn tại!';
                if (user.locked) return 'Tài khoản đã bị khóa!';
                if (user.password !== password) return 'Mật khẩu không chính xác!';
                this.currentUser = user;
                return `Đăng nhập thành công! Xin chào, ${user.fullname}`;
            }

            logout() {
                if (!this.currentUser) return 'Bạn chưa đăng nhập!';
                this.currentUser = null;
                return 'Đã đăng xuất!';
            }

            lockAccount() {
                if (!this.currentUser) return 'Bạn chưa đăng nhập!';
                this.currentUser.locked = true;
                return `Tài khoản ${this.currentUser.username} đã bị khóa!`;
            }

            addBalance(amount) {
                if (!this.currentUser) return 'Bạn chưa đăng nhập!';
                if (this.currentUser.locked) return 'Tài khoản của bạn đã bị khóa!';
                this.currentUser.balance += amount;
                return `Số dư mới: ${this.currentUser.formatBalance()}`;
            }

            subtractBalance(amount) {
                if (!this.currentUser) return 'Bạn chưa đăng nhập!';
                if (this.currentUser.locked) return 'Tài khoản của bạn đã bị khóa!';
                if (this.currentUser.balance < amount) return 'Số dư không đủ!';
                this.currentUser.balance -= amount;
                return `Số dư mới: ${this.currentUser.formatBalance()}`;
            }

            showBalance() {
                if (!this.currentUser) return 'Bạn chưa đăng nhập!';
                return `Số dư của bạn: ${this.currentUser.formatBalance()}`;
            }
        }

        const auth = new AuthSystem();

        function registerUser() {
            const username = document.getElementById('regUsername').value;
            const password = document.getElementById('regPassword').value;
            const fullname = document.getElementById('regFullname').value;
            showMessage(auth.register(username, password, fullname));
        }

        function loginUser() {
            const username = document.getElementById('loginUsername').value;
            const password = document.getElementById('loginPassword').value;
            showMessage(auth.login(username, password));
        }

        function logoutUser() {
            showMessage(auth.logout());
        }

        function showBalance() {
            showMessage(auth.showBalance());
        }

        function addMoney(amount) {
            showMessage(auth.addBalance(amount));
        }

        function subtractMoney(amount) {
            showMessage(auth.subtractBalance(amount));
        }

        function lockAccount() {
            showMessage(auth.lockAccount());
        }

        function showMessage(message) {
            document.getElementById('message').innerText = message;
        }
    </script>
</body>
</html>
