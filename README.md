<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>برنامه لینکونی - نسخه پیشرفته</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.10.5/font/bootstrap-icons.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --primary-color: #6a11cb;
            --secondary-color: #2575fc;
            --light-bg: #f8f9fa;
            --dark-bg: #1a1a2e;
            --light-text: #f8f9fa;
            --dark-text: #212529;
            --card-light: #ffffff;
            --card-dark: #2d2d44;
        }

        body {
            font-family: 'Vazir', sans-serif;
            transition: background-color 0.3s, color 0.3s;
            background-color: var(--light-bg);
            color: var(--dark-text);
        }

        body.dark-mode {
            background-color: var(--dark-bg);
            color: var(--light-text);
        }

        .navbar-brand {
            font-weight: bold;
        }

        .banner-container {
            background: linear-gradient(135deg, var(--primary-color) 0%, var(--secondary-color) 100%);
            border-radius: 10px;
            padding: 20px;
            color: white;
            margin-bottom: 20px;
        }

        .chat-container {
            height: 400px;
            overflow-y: auto;
            border: 1px solid #ddd;
            border-radius: 10px;
            padding: 15px;
            margin-bottom: 15px;
            background-color: #f9f9f9;
        }

        .dark-mode .chat-container {
            background-color: #2a2a3c;
            border-color: #444;
        }

        .message {
            padding: 10px;
            margin: 5px 0;
            border-radius: 10px;
            max-width: 80%;
            position: relative;
        }

        .user-message {
            background-color: #d1ecf1;
            margin-left: auto;
            text-align: left;
        }

        .dark-mode .user-message {
            background-color: #2a4d69;
        }

        .admin-message {
            background-color: #f8d7da;
            margin-right: auto;
            text-align: right;
        }

        .dark-mode .admin-message {
            background-color: #5c2a3c;
        }

        .login-container {
            max-width: 400px;
            margin: 100px auto;
            padding: 20px;
            border: 1px solid #ddd;
            border-radius: 10px;
            background-color: white;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }

        .dark-mode .login-container {
            background-color: var(--card-dark);
            border-color: #444;
        }

        .admin-panel {
            background-color: white;
            border-radius: 10px;
            padding: 20px;
            margin-top: 20px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }

        .dark-mode .admin-panel {
            background-color: var(--card-dark);
        }

        .banner-card {
            transition: transform 0.3s;
        }

        .banner-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 20px rgba(0,0,0,0.1);
        }

        .dark-mode .card {
            background-color: var(--card-dark);
            color: var(--light-text);
        }

        .user-avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            object-fit: cover;
            margin-left: 10px;
        }

        .profile-avatar {
            width: 100px;
            height: 100px;
            border-radius: 50%;
            object-fit: cover;
            border: 3px solid var(--primary-color);
        }

        .fullscreen-chat {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: white;
            z-index: 1000;
            display: none;
            flex-direction: column;
            padding: 20px;
        }

        .dark-mode .fullscreen-chat {
            background-color: var(--dark-bg);
        }

        .fullscreen-header {
            padding-bottom: 15px;
            border-bottom: 1px solid #ddd;
            margin-bottom: 15px;
        }

        .dark-mode .fullscreen-header {
            border-bottom-color: #444;
        }

        .message-actions {
            position: absolute;
            bottom: -25px;
            right: 10px;
            display: none;
            background: white;
            border-radius: 15px;
            padding: 5px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }

        .dark-mode .message-actions {
            background: #333;
        }

        .message:hover .message-actions {
            display: flex;
        }

        .reaction-btn {
            font-size: 16px;
            padding: 3px 8px;
            border-radius: 50%;
            margin: 0 2px;
        }

        .notification-badge {
            position: absolute;
            top: -5px;
            right: -5px;
            background-color: red;
            color: white;
            border-radius: 50%;
            width: 18px;
            height: 18px;
            font-size: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .emoji-picker {
            position: absolute;
            bottom: 50px;
            right: 10px;
            background: white;
            border-radius: 10px;
            padding: 10px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
            display: none;
            z-index: 1000;
        }

        .dark-mode .emoji-picker {
            background: #333;
        }

        .theme-switch {
            position: fixed;
            bottom: 20px;
            left: 20px;
            z-index: 100;
        }

        .activity-item {
            border-left: 3px solid var(--primary-color);
            padding: 10px 15px;
            margin-bottom: 10px;
            background-color: rgba(106, 17, 203, 0.05);
        }

        .dark-mode .activity-item {
            background-color: rgba(106, 17, 203, 0.2);
        }
    </style>
</head>
<body>
    <!-- نوار ناوبری -->
    <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
        <div class="container">
            <a class="navbar-brand" href="#">
                <i class="bi bi-link-45deg"></i> لینکونی
            </a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav me-auto">
                    <li class="nav-item">
                        <a class="nav-link active" href="#" onclick="showPage('home')">
                            <i class="bi bi-house"></i> صفحه اصلی
                        </a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#" onclick="showPage('banner-register')">
                            <i class="bi bi-plus-square"></i> ثبت بنر
                        </a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#" onclick="openFullscreenChat()">
                            <i class="bi bi-chat-dots"></i> چت روم
                            <span class="notification-badge" id="chat-notification">3</span>
                        </a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#" onclick="showPage('message-inbox')">
                            <i class="bi bi-inbox"></i> صندوق پیام
                            <span class="notification-badge" id="message-notification">2</span>
                        </a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#" onclick="showPage('profile')">
                            <i class="bi bi-person"></i> پروفایل
                        </a>
                    </li>
                </ul>
                <div class="d-flex">
                    <button class="btn btn-outline-light me-2" onclick="toggleTheme()">
                        <i class="bi bi-moon"></i>
                    </button>
                    <button class="btn btn-outline-light" onclick="showPage('admin-login')">
                        <i class="bi bi-shield-lock"></i> ورود ادمین
                    </button>
                </div>
            </div>
        </div>
    </nav>

    <div class="container mt-4">
        <!-- صفحه اصلی -->
        <div id="home-page" class="page">
            <div class="banner-container text-center">
                <h2>به برنامه لینکونی خوش آمدید</h2>
                <p>سیستم مدیریت بنر و پیام پیشرفته</p>
            </div>
            
            <div class="d-flex justify-content-between align-items-center mb-3">
                <h4>بنرهای کاربران</h4>
                <div class="d-flex">
                    <button class="btn btn-outline-primary me-2" onclick="sortBanners('newest')">
                        <i class="bi bi-sort-down"></i> جدیدترین
                    </button>
                    <button class="btn btn-outline-primary" onclick="sortBanners('popular')">
                        <i class="bi bi-hand-thumbs-up"></i> محبوب‌ترین
                    </button>
                </div>
            </div>
            
            <div id="banners-container" class="row">
                <!-- بنرها در اینجا نمایش داده می شوند -->
            </div>
        </div>

        <!-- صفحه ثبت بنر -->
        <div id="banner-register-page" class="page" style="display: none;">
            <div class="card">
                <div class="card-header bg-primary text-white">
                    <h5><i class="bi bi-plus-square"></i> ثبت بنر جدید</h5>
                </div>
                <div class="card-body">
                    <form id="banner-form">
                        <div class="mb-3">
                            <label for="banner-title" class="form-label">عنوان بنر</label>
                            <input type="text" class="form-control" id="banner-title" required>
                        </div>
                        <div class="mb-3">
                            <label for="banner-url" class="form-label">لینک بنر</label>
                            <input type="url" class="form-control" id="banner-url" required>
                        </div>
                        <div class="mb-3">
                            <label for="banner-image" class="form-label">آدرس تصویر بنر</label>
                            <input type="url" class="form-control" id="banner-image" required>
                        </div>
                        <div class="mb-3">
                            <label for="banner-category" class="form-label">دسته‌بندی</label>
                            <select class="form-select" id="banner-category">
                                <option value="عمومی">عمومی</option>
                                <option value="فروشگاه">فروشگاه</option>
                                <option value="خدمات">خدمات</option>
                                <option value="آموزشی">آموزشی</option>
                            </select>
                        </div>
                        <div class="mb-3 form-check">
                            <input type="checkbox" class="form-check-input" id="banner-terms">
                            <label class="form-check-label" for="banner-terms">با شرایط و قوانین موافقم</label>
                        </div>
                        <button type="submit" class="btn btn-primary">
                            <i class="bi bi-check-circle"></i> ثبت بنر
                        </button>
                    </form>
                </div>
            </div>
        </div>

        <!-- صفحه چت روم -->
        <div id="chat-room-page" class="page" style="display: none;">
            <div class="card">
                <div class="card-header bg-success text-white d-flex justify-content-between align-items-center">
                    <h5><i class="bi bi-chat-dots"></i> چت روم کاربران</h5>
                    <button class="btn btn-sm btn-light" onclick="openFullscreenChat()">
                        <i class="bi bi-arrows-fullscreen"></i> تمام صفحه
                    </button>
                </div>
                <div class="card-body">
                    <div id="online-users" class="mb-3">
                        <span class="badge bg-info">کاربران آنلاین: <span id="online-count">5</span></span>
                        <div class="mt-2">
                            <img src="https://ui-avatars.com/api/?name=کاربر۱&background=random" class="user-avatar" title="کاربر۱">
                            <img src="https://ui-avatars.com/api/?name=کاربر۲&background=random" class="user-avatar" title="کاربر۲">
                            <img src="https://ui-avatars.com/api/?name=کاربر۳&background=random" class="user-avatar" title="کاربر۳">
                            <img src="https://ui-avatars.com/api/?name=کاربر۴&background=random" class="user-avatar" title="کاربر۴">
                            <img src="https://ui-avatars.com/api/?name=شما&background=0d6efd" class="user-avatar" title="شما">
                        </div>
                    </div>
                    <div id="chat-messages" class="chat-container">
                        <!-- پیام های چت در اینجا نمایش داده می شوند -->
                    </div>
                    <div class="input-group mt-2">
                        <button class="btn btn-outline-secondary" type="button" onclick="toggleEmojiPicker()">
                            <i class="bi bi-emoji-smile"></i>
                        </button>
                        <input type="text" id="chat-input" class="form-control" placeholder="پیام خود را بنویسید...">
                        <button class="btn btn-success" onclick="sendMessage()">
                            <i class="bi bi-send"></i> ارسال
                        </button>
                    </div>
                    <div id="emoji-picker" class="emoji-picker">
                        <div class="d-flex flex-wrap">
                            <span class="emoji-btn" onclick="addEmoji('😀')">😀</span>
                            <span class="emoji-btn" onclick="addEmoji('😃')">😃</span>
                            <span class="emoji-btn" onclick="addEmoji('😄')">😄</span>
                            <span class="emoji-btn" onclick="addEmoji('😁')">😁</span>
                            <span class="emoji-btn" onclick="addEmoji('😆')">😆</span>
                            <span class="emoji-btn" onclick="addEmoji('😅')">😅</span>
                            <span class="emoji-btn" onclick="addEmoji('😂')">😂</span>
                            <span class="emoji-btn" onclick="addEmoji('🤣')">🤣</span>
                            <span class="emoji-btn" onclick="addEmoji('😊')">😊</span>
                            <span class="emoji-btn" onclick="addEmoji('😇')">😇</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- صفحه صندوق پیام -->
        <div id="message-inbox-page" class="page" style="display: none;">
            <div class="card">
                <div class="card-header bg-info text-white">
                    <h5><i class="bi bi-inbox"></i> صندوق پیام ها</h5>
                </div>
                <div class="card-body">
                    <div class="d-flex mb-3">
                        <button class="btn btn-outline-info me-2 active">همه</button>
                        <button class="btn btn-outline-info me-2">خوانده نشده</button>
                        <button class="btn btn-outline-info">مهم</button>
                    </div>
                    <div id="messages-container">
                        <!-- پیام های ادمین در اینجا نمایش داده می شوند -->
                    </div>
                </div>
            </div>
        </div>

        <!-- صفحه پروفایل -->
        <div id="profile-page" class="page" style="display: none;">
            <div class="card">
                <div class="card-header bg-primary text-white">
                    <h5><i class="bi bi-person"></i> پروفایل کاربری</h5>
                </div>
                <div class="card-body">
                    <div class="row">
                        <div class="col-md-4 text-center">
                            <img src="https://ui-avatars.com/api/?name=کاربر+لینکونی&size=100&background=0d6efd&color=fff" class="profile-avatar mb-3">
                            <h4>کاربر لینکونی</h4>
                            <p class="text-muted">عضو since 1402</p>
                            <div class="d-grid gap-2">
                                <button class="btn btn-outline-primary">
                                    <i class="bi bi-pencil"></i> ویرایش پروفایل
                                </button>
                            </div>
                        </div>
                        <div class="col-md-8">
                            <div class="row mb-3">
                                <div class="col-md-6">
                                    <div class="card bg-light">
                                        <div class="card-body text-center">
                                            <h6>بنرهای ثبت شده</h6>
                                            <h3>12</h3>
                                        </div>
                                    </div>
                                </div>
                                <div class="col-md-6">
                                    <div class="card bg-light">
                                        <div class="card-body text-center">
                                            <h6>پیام های ارسال شده</h6>
                                            <h3>47</h3>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            
                            <h5>اطلاعات کاربری</h5>
                            <table class="table">
                                <tr>
                                    <th>ایمیل:</th>
                                    <td>user@linkoni.ir</td>
                                </tr>
                                <tr>
                                    <th>تلفن:</th>
                                    <td>0912***1234</td>
                                </tr>
                                <tr>
                                    <th>تاریخ عضویت:</th>
                                    <td>1402/05/01</td>
                                </tr>
                            </table>
                            
                            <h5>آخرین فعالیت‌ها</h5>
                            <div id="user-activities">
                                <div class="activity-item">
                                    <div>ثبت بنر جدید "فروشگاه اینترنتی"</div>
                                    <small class="text-muted">2 ساعت پیش</small>
                                </div>
                                <div class="activity-item">
                                    <div>ارسال پیام در چت روم</div>
                                    <small class="text-muted">5 ساعت پیش</small>
                                </div>
                                <div class="activity-item">
                                    <div>ثبت بنر جدید "آموزش برنامه نویسی"</div>
                                    <small class="text-muted">1 روز پیش</small>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- صفحه ورود ادمین -->
        <div id="admin-login-page" class="page" style="display: none;">
            <div class="login-container">
                <h3 class="text-center mb-4"><i class="bi bi-shield-lock"></i> ورود ادمین</h3>
                <form id="admin-login-form">
                    <div class="mb-3">
                        <label for="admin-username" class="form-label">نام کاربری</label>
                        <input type="text" class="form-control" id="admin-username" required>
                    </div>
                    <div class="mb-3">
                        <label for="admin-password" class="form-label">رمز عبور</label>
                        <input type="password" class="form-control" id="admin-password" required>
                    </div>
                    <div class="mb-3 form-check">
                        <input type="checkbox" class="form-check-input" id="remember-me">
                        <label class="form-check-label" for="remember-me">مرا به خاطر بسپار</label>
                    </div>
                    <button type="submit" class="btn btn-primary w-100">ورود</button>
                </form>
            </div>
        </div>

        <!-- پنل ادمین -->
        <div id="admin-panel-page" class="page" style="display: none;">
            <div class="admin-panel">
                <div class="d-flex justify-content-between align-items-center mb-4">
                    <h4><i class="bi bi-speedometer2"></i> پنل مدیریت ادمین</h4>
                    <button class="btn btn-danger" onclick="adminLogout()">
                        <i class="bi bi-box-arrow-right"></i> خروج
                    </button>
                </div>
                
                <div class="row">
                    <div class="col-md-6">
                        <div class="card mb-4">
                            <div class="card-header bg-warning">
                                <h5><i class="bi bi-megaphone"></i> ارسال پیام به همه کاربران</h5>
                            </div>
                            <div class="card-body">
                                <form id="admin-message-form">
                                    <div class="mb-3">
                                        <label for="admin-message" class="form-label">متن پیام</label>
                                        <textarea class="form-control" id="admin-message" rows="3" required></textarea>
                                    </div>
                                    <div class="mb-3">
                                        <label class="form-label">اولویت پیام</label>
                                        <div class="form-check">
                                            <input class="form-check-input" type="radio" name="priority" id="priority-low" value="low" checked>
                                            <label class="form-check-label" for="priority-low">کم</label>
                                        </div>
                                        <div class="form-check">
                                            <input class="form-check-input" type="radio" name="priority" id="priority-medium" value="medium">
                                            <label class="form-check-label" for="priority-medium">متوسط</label>
                                        </div>
                                        <div class="form-check">
                                            <input class="form-check-input" type="radio" name="priority" id="priority-high" value="high">
                                            <label class="form-check-label" for="priority-high">بالا</label>
                                        </div>
                                    </div>
                                    <button type="submit" class="btn btn-warning">
                                        <i class="bi bi-send"></i> ارسال پیام
                                    </button>
                                </form>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="card">
                            <div class="card-header bg-info">
                                <h5><i class="bi bi-images"></i> مدیریت بنرها</h5>
                            </div>
                            <div class="card-body">
                                <div class="input-group mb-3">
                                    <input type="text" class="form-control" placeholder="جستجوی بنر...">
                                    <button class="btn btn-outline-secondary" type="button">
                                        <i class="bi bi-search"></i>
                                    </button>
                                </div>
                                <div id="admin-banners-container">
                                    <!-- بنرها برای مدیریت در اینجا نمایش داده می شوند -->
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- چت روم تمام صفحه -->
    <div id="fullscreen-chat" class="fullscreen-chat">
        <div class="fullscreen-header d-flex justify-content-between align-items-center">
            <h4><i class="bi bi-chat-dots"></i> چت روم</h4>
            <div>
                <button class="btn btn-sm btn-outline-secondary me-2" onclick="toggleTheme()">
                    <i class="bi bi-moon"></i>
                </button>
                <button class="btn btn-sm btn-danger" onclick="closeFullscreenChat()">
                    <i class="bi bi-x-lg"></i> بستن
                </button>
            </div>
        </div>
        
        <div id="fs-chat-messages" class="flex-grow-1" style="overflow-y: auto; padding: 15px;">
            <!-- پیام های چت در اینجا نمایش داده می شوند -->
        </div>
        
        <div class="mt-3">
            <div class="input-group">
                <button class="btn btn-outline-secondary" type="button" onclick="toggleEmojiPicker()">
                    <i class="bi bi-emoji-smile"></i>
                </button>
                <input type="text" id="fs-chat-input" class="form-control" placeholder="پیام خود را بنویسید...">
                <button class="btn btn-success" onclick="sendFSMessage()">
                    <i class="bi bi-send"></i> ارسال
                </button>
            </div>
        </div>
    </div>

    <!-- دکمه تغییر تم -->
    <div class="theme-switch">
        <button class="btn btn-primary btn-lg rounded-circle" onclick="toggleTheme()">
            <i class="bi bi-moon"></i>
        </button>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
    <script>
        // داده های نمونه
        let banners = [
            { id: 1, title: "فروشگاه اینترنتی", url: "https://example.com", image: "https://via.placeholder.com/300x100?text=فروشگاه+اینترنتی", approved: true, likes: 12, category: "فروشگاه" },
            { id: 2, title: "آموزش برنامه نویسی", url: "https://example.com", image: "https://via.placeholder.com/300x100?text=آموزش+برنامه+نویسی", approved: true, likes: 24, category: "آموزشی" },
            { id: 3, title: "خدمات طراحی وب", url: "https://example.com", image: "https://via.placeholder.com/300x100?text=خدمات+طراحی+وب", approved: false, likes: 7, category: "خدمات" }
        ];
        
        let adminMessages = [
            { id: 1, content: "به روزرسانی جدید سیستم انجام شد. لطفا مشکلات را گزارش دهید.", date: "1402/05/15", priority: "high", read: false },
            { id: 2, content: "همایش بزرگ کاربران فردا برگزار خواهد شد.", date: "1402/05/10", priority: "medium", read: true }
        ];
        
        let chatMessages = [
            { user: "کاربر ۱", message: "سلام چطوری؟", time: "10:30", avatar: "https://ui-avatars.com/api/?name=کاربر۱&background=random", likes: 2 },
            { user: "کاربر ۲", message: "سلام خوبم ممنون. شما چطور؟", time: "10:32", avatar: "https://ui-avatars.com/api/?name=کاربر۲&background=random", likes: 5 },
            { user: "کاربر ۳", message: "کسی میتونه در مورد بنرها کمکم کنه؟", time: "10:35", avatar: "https://ui-avatars.com/api/?name=کاربر۳&background=random", likes: 3 }
        ];
        
        let isAdminLoggedIn = false;
        const adminCredentials = { username: "admin", password: "1234" };
        let currentTheme = "light";

        // نمایش صفحه مورد نظر و مخفی کردن بقیه
        function showPage(pageId) {
            document.querySelectorAll('.page').forEach(page => {
                page.style.display = 'none';
            });
            
            if (pageId === 'home') {
                document.getElementById('home-page').style.display = 'block';
                loadBanners();
            } else if (pageId === 'banner-register') {
                document.getElementById('banner-register-page').style.display = 'block';
            } else if (pageId === 'chat-room') {
                document.getElementById('chat-room-page').style.display = 'block';
                loadChatMessages();
            } else if (pageId === 'message-inbox') {
                document.getElementById('message-inbox-page').style.display = 'block';
                loadAdminMessages();
            } else if (pageId === 'profile') {
                document.getElementById('profile-page').style.display = 'block';
            } else if (pageId === 'admin-login') {
                document.getElementById('admin-login-page').style.display = 'block';
            } else if (pageId === 'admin-panel' && isAdminLoggedIn) {
                document.getElementById('admin-panel-page').style.display = 'block';
                loadAdminBanners();
            }
        }

        // بارگذاری بنرها در صفحه اصلی
        function loadBanners() {
            const bannersContainer = document.getElementById('banners-container');
            bannersContainer.innerHTML = '';
            
            banners.filter(banner => banner.approved).forEach(banner => {
                const bannerElement = `
                    <div class="col-md-4 mb-4">
                        <div class="card banner-card h-100">
                            <img src="${banner.image}" class="card-img-top" alt="${banner.title}">
                            <div class="card-body">
                                <span class="badge bg-secondary mb-2">${banner.category}</span>
                                <h5 class="card-title">${banner.title}</h5>
                                <p class="card-text">لایک‌ها: ${banner.likes}</p>
                                <div class="d-flex justify-content-between">
                                    <a href="${banner.url}" class="btn btn-primary btn-sm">مشاهده</a>
                                    <button class="btn btn-outline-secondary btn-sm" onclick="likeBanner(${banner.id})">
                                        <i class="bi bi-hand-thumbs-up"></i> لایک
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>
                `;
                bannersContainer.innerHTML += bannerElement;
            });
        }

        // لایک کردن بنر
        function likeBanner(bannerId) {
            const banner = banners.find(b => b.id === bannerId);
            if (banner) {
                banner.likes++;
                loadBanners();
                showNotification(`بنر "${banner.title}" لایک شد!`, 'success');
            }
        }

        // بارگذاری پیام های ادمین
        function loadAdminMessages() {
            const messagesContainer = document.getElementById('messages-container');
            messagesContainer.innerHTML = '';
            
            adminMessages.forEach(msg => {
                const priorityClass = msg.priority === 'high' ? 'danger' : (msg.priority === 'medium' ? 'warning' : 'info');
                const readClass = msg.read ? '' : 'border-primary';
                
                const messageElement = `
                    <div class="card mb-3 ${readClass}">
                        <div class="card-header d-flex justify-content-between">
                            <span>پیام مدیریت</span>
                            <span class="badge bg-${priorityClass}">${msg.priority === 'high' ? 'بالا' : (msg.priority === 'medium' ? 'متوسط' : 'کم')}</span>
                        </div>
                        <div class="card-body">
                            <p class="card-text">${msg.content}</p>
                            <small class="text-muted">تاریخ: ${msg.date}</small>
                        </div>
                    </div>
                `;
                messagesContainer.innerHTML += messageElement;
            });
        }

        // بارگذاری پیام های چت
        function loadChatMessages() {
            const chatContainer = document.getElementById('chat-messages');
            chatContainer.innerHTML = '';
            
            chatMessages.forEach(msg => {
                const messageElement = `
                    <div class="message user-message">
                        <div class="d-flex align-items-center">
                            <img src="${msg.avatar}" class="user-avatar">
                            <strong>${msg.user}:</strong>
                        </div>
                        <div class="mt-1">${msg.message}</div>
                        <div class="d-flex justify-content-between mt-2">
                            <div class="text-muted small">${msg.time}</div>
                            <div class="d-flex">
                                <span class="badge bg-light text-dark me-2">
                                    <i class="bi bi-hand-thumbs-up"></i> ${msg.likes}
                                </span>
                                <button class="btn btn-sm p-0 me-1" onclick="likeMessage(this)">
                                    <i class="bi bi-hand-thumbs-up"></i>
                                </button>
                                <button class="btn btn-sm p-0" onclick="replyToMessage('${msg.user}')">
                                    <i class="bi bi-reply"></i>
                                </button>
                            </div>
                        </div>
                    </div>
                `;
                chatContainer.innerHTML += messageElement;
            });
            
            // اسکرول به پایین
            chatContainer.scrollTop = chatContainer.scrollHeight;
        }

        // بارگذاری پیام های چت در حالت تمام صفحه
        function loadFSChatMessages() {
            const chatContainer = document.getElementById('fs-chat-messages');
            chatContainer.innerHTML = '';
            
            chatMessages.forEach(msg => {
                const messageElement = `
                    <div class="message user-message mb-3">
                        <div class="d-flex align-items-center">
                            <img src="${msg.avatar}" class="user-avatar">
                            <strong>${msg.user}:</strong>
                        </div>
                        <div class="mt-1">${msg.message}</div>
                        <div class="d-flex justify-content-between mt-2">
                            <div class="text-muted small">${msg.time}</div>
                            <div class="d-flex">
                                <span class="badge bg-light text-dark me-2">
                                    <i class="bi bi-hand-thumbs-up"></i> ${msg.likes}
                                </span>
                                <button class="btn btn-sm p-0 me-1" onclick="likeMessage(this)">
                                    <i class="bi bi-hand-thumbs-up"></i>
                                </button>
                                <button class="btn btn-sm p-0" onclick="replyToMessage('${msg.user}')">
                                    <i class="bi bi-reply"></i>
                                </button>
                            </div>
                        </div>
                    </div>
                `;
                chatContainer.innerHTML += messageElement;
            });
            
            // اسکرول به پایین
            chatContainer.scrollTop = chatContainer.scrollHeight;
        }

        // ارسال پیام در چت
        function sendMessage() {
            const input = document.getElementById('chat-input');
            const message = input.value.trim();
            
            if (message) {
                const now = new Date();
                const time = `${now.getHours()}:${now.getMinutes()}`;
                
                chatMessages.push({
                    user: "شما",
                    message: message,
                    time: time,
                    avatar: "https://ui-avatars.com/api/?name=شما&background=0d6efd",
                    likes: 0
                });
                
                loadChatMessages();
                input.value = '';
                
                // شبیه‌سازی پاسخ پس از 1 ثانیه
                setTimeout(() => {
                    const replies = [
                        "عالی به نظر میاد!",
                        "ممنون از اطلاعاتت",
                        "چه ایده جالبی!",
                        "منم همین نظر رو دارم"
                    ];
                    
                    const randomReply = replies[Math.floor(Math.random() * replies.length)];
                    
                    chatMessages.push({
                        user: "کاربر " + (Math.floor(Math.random() * 3) + 1),
                        message: randomReply,
                        time: `${now.getHours()}:${now.getMinutes() + 1}`,
                        avatar: `https://ui-avatars.com/api/?name=کاربر${Math.floor(Math.random() * 3) + 1}&background=random`,
                        likes: 0
                    });
                    
                    loadChatMessages();
                    
                    // افزایش شمارنده نوتیفیکیشن
                    const notificationBadge = document.getElementById('chat-notification');
                    notificationBadge.textContent = parseInt(notificationBadge.textContent) + 1;
                }, 1000);
            }
        }

        // ارسال پیام در چت تمام صفحه
        function sendFSMessage() {
            const input = document.getElementById('fs-chat-input');
            const message = input.value.trim();
            
            if (message) {
                const now = new Date();
                const time = `${now.getHours()}:${now.getMinutes()}`;
                
                chatMessages.push({
                    user: "شما",
                    message: message,
                    time: time,
                    avatar: "https://ui-avatars.com/api/?name=شما&background=0d6efd",
                    likes: 0
                });
                
                loadFSChatMessages();
                input.value = '';
                
                // شبیه‌سازی پاسخ پس از 1 ثانیه
                setTimeout(() => {
                    const replies = [
                        "این ویژگی جدید واقعا عالیه!",
                        "منم موافقم",
                        "چه ایده جالبی!",
                        "خوشم اومد از این قابلیت"
                    ];
                    
                    const randomReply = replies[Math.floor(Math.random() * replies.length)];
                    
                    chatMessages.push({
                        user: "کاربر " + (Math.floor(Math.random() * 3) + 1),
                        message: randomReply,
                        time: `${now.getHours()}:${now.getMinutes() + 1}`,
                        avatar: `https://ui-avatars.com/api/?name=کاربر${Math.floor(Math.random() * 3) + 1}&background=random`,
                        likes: 0
                    });
                    
                    loadFSChatMessages();
                }, 1000);
            }
        }

        // لایک کردن پیام
        function likeMessage(btn) {
            const messageElement = btn.closest('.message');
            const likeCount = messageElement.querySelector('.badge');
            likeCount.textContent = parseInt(likeCount.textContent) + 1;
            btn.classList.add('text-primary');
        }

        // پاسخ دادن به پیام
        function replyToMessage(username) {
            const input = document.getElementById('chat-input') || document.getElementById('fs-chat-input');
            input.value = `@${username} `;
            input.focus();
        }

        // مدیریت فرم ثبت بنر
        document.getElementById('banner-form').addEventListener('submit', function(e) {
            e.preventDefault();
            
            const title = document.getElementById('banner-title').value;
            const url = document.getElementById('banner-url').value;
            const image = document.getElementById('banner-image').value;
            const category = document.getElementById('banner-category').value;
            const terms = document.getElementById('banner-terms').checked;
            
            if (!terms) {
                showNotification('لطفا با شرایط و قوانین موافقت کنید', 'error');
                return;
            }
            
            const newBanner = {
                id: banners.length + 1,
                title: title,
                url: url,
                image: image,
                category: category,
                approved: false,
                likes: 0
            };
            
            banners.push(newBanner);
            
            showNotification('بنر شما با موفقیت ثبت شد و پس از تایید ادمین نمایش داده خواهد شد.', 'success');
            this.reset();
            showPage('home');
        });

        // مدیریت فرم ورود ادمین
        document.getElementById('admin-login-form').addEventListener('submit', function(e) {
            e.preventDefault();
            
            const username = document.getElementById('admin-username').value;
            const password = document.getElementById('admin-password').value;
            
            if (username === adminCredentials.username && password === adminCredentials.password) {
                isAdminLoggedIn = true;
                showNotification('با موفقیت وارد پنل مدیریت شدید', 'success');
                showPage('admin-panel');
            } else {
                showNotification('نام کاربری یا رمز عبور اشتباه است.', 'error');
            }
        });

        // مدیریت فرم ارسال پیام توسط ادمین
        document.getElementById('admin-message-form').addEventListener('submit', function(e) {
            e.preventDefault();
            
            const message = document.getElementById('admin-message').value;
            const priority = document.querySelector('input[name="priority"]:checked').value;
            const now = new Date();
            const date = `${now.getFullYear()}/${now.getMonth() + 1}/${now.getDate()}`;
            
            adminMessages.unshift({
                id: adminMessages.length + 1,
                content: message,
                date: date,
                priority: priority,
                read: false
            });
            
            showNotification('پیام شما با موفقیت برای همه کاربران ارسال شد.', 'success');
            
            // افزایش شمارنده نوتیفیکیشن
            const notificationBadge = document.getElementById('message-notification');
            notificationBadge.textContent = parseInt(notificationBadge.textContent) + 1;
            
            this.reset();
        });

        // بارگذاری بنرها در پنل ادمین
        function loadAdminBanners() {
            const adminBannersContainer = document.getElementById('admin-banners-container');
            adminBannersContainer.innerHTML = '';
            
            banners.forEach(banner => {
                const statusBadge = banner.approved ? 
                    '<span class="badge bg-success">تایید شده</span>' : 
                    '<span class="badge bg-warning">در انتظار تایید</span>';
                
                const bannerElement = `
                    <div class="card mb-2">
                        <div class="card-body">
                            <div class="d-flex justify-content-between align-items-center">
                                <h6 class="card-title mb-0">${banner.title}</h6>
                                ${statusBadge}
                            </div>
                            <p class="mb-1 mt-2">دسته: ${banner.category}</p>
                            <p class="mb-2">لایک: ${banner.likes}</p>
                            <div class="btn-group btn-group-sm">
                                <button class="btn btn-outline-success" onclick="approveBanner(${banner.id})">تایید</button>
                                <button class="btn btn-outline-info" onclick="editBanner(${banner.id})">ویرایش</button>
                                <button class="btn btn-outline-danger" onclick="deleteBanner(${banner.id})">حذف</button>
                            </div>
                        </div>
                    </div>
                `;
                adminBannersContainer.innerHTML += bannerElement;
            });
        }

        // تایید بنر توسط ادمین
        function approveBanner(bannerId) {
            const banner = banners.find(b => b.id === bannerId);
            if (banner) {
                banner.approved = true;
                loadAdminBanners();
                showNotification('بنر تایید شد.', 'success');
            }
        }

        // ویرایش بنر
        function editBanner(bannerId) {
            showNotification('این ویژگی به زودی اضافه خواهد شد.', 'info');
        }

        // حذف بنر توسط ادمین
        function deleteBanner(bannerId) {
            if (confirm('آیا از حذف این بنر اطمینان دارید؟')) {
                banners = banners.filter(b => b.id !== bannerId);
                loadAdminBanners();
                showNotification('بنر حذف شد.', 'success');
            }
        }

        // خروج ادمین
        function adminLogout() {
            isAdminLoggedIn = false;
            showNotification('از پنل مدیریت خارج شدید.', 'info');
            showPage('home');
        }

        // باز کردن چت تمام صفحه
        function openFullscreenChat() {
            document.getElementById('fullscreen-chat').style.display = 'flex';
            loadFSChatMessages();
        }

        // بستن چت تمام صفحه
        function closeFullscreenChat() {
            document.getElementById('fullscreen-chat').style.display = 'none';
        }

        // تغییر تم
        function toggleTheme() {
            const body = document.body;
            const themeIcon = document.querySelector('.theme-switch i');
            
            if (currentTheme === "light") {
                body.classList.add('dark-mode');
                themeIcon.classList.remove('bi-moon');
                themeIcon.classList.add('bi-sun');
                currentTheme = "dark";
                localStorage.setItem('theme', 'dark');
            } else {
                body.classList.remove('dark-mode');
                themeIcon.classList.remove('bi-sun');
                themeIcon.classList.add('bi-moon');
                currentTheme = "light";
                localStorage.setItem('theme', 'light');
            }
        }

        // نمایش نوتیفیکیشن
        function showNotification(message, type) {
            const notification = document.createElement('div');
            notification.className = `alert alert-${type === 'success' ? 'success' : type === 'error' ? 'danger' : 'info'} alert-dismissible fade show position-fixed`;
            notification.style.cssText = 'top: 20px; right: 20px; z-index: 1050; min-width: 300px;';
            notification.innerHTML = `
                ${message}
                <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
            `;
            
            document.body.appendChild(notification);
            
            // حذف خودکار پس از 3 ثانیه
            setTimeout(() => {
                if (notification.parentNode) {
                    notification.parentNode.removeChild(notification);
                }
            }, 3000);
        }

        // باز و بستن انتخاب ایموجی
        function toggleEmojiPicker() {
            const picker = document.getElementById('emoji-picker');
            picker.style.display = picker.style.display === 'block' ? 'none' : 'block';
        }

        // اضافه کردن ایموجی به پیام
        function addEmoji(emoji) {
            const input = document.getElementById('chat-input') || document.getElementById('fs-chat-input');
            input.value += emoji;
            document.getElementById('emoji-picker').style.display = 'none';
        }

        // مرتب‌سازی بنرها
        function sortBanners(method) {
            if (method === 'newest') {
                banners.sort((a, b) => b.id - a.id);
            } else if (method === 'popular') {
                banners.sort((a, b) => b.likes - a.likes);
            }
            loadBanners();
            showNotification(`بنرها بر اساس ${method === 'newest' ? 'جدیدترین' : 'محبوب‌ترین'} مرتب شدند.`, 'success');
        }

        // مقداردهی اولیه
        document.addEventListener('DOMContentLoaded', function() {
            showPage('home');
            
            // بررسی تم ذخیره شده
            const savedTheme = localStorage.getItem('theme');
            if (savedTheme === 'dark') {
                toggleTheme();
            }
            
            // شبیه‌سازی کاربران آنلاین
            setInterval(() => {
                const onlineCount = document.getElementById('online-count');
                const count = parseInt(onlineCount.textContent);
                onlineCount.textContent = Math.floor(Math.random() * 10) + 5;
            }, 10000);
        });
    </script>
</body>
</html>
