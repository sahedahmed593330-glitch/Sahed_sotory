<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>আমার সোশ্যাল মিডিয়া প্ল্যাটফর্ম</title>
    <!-- Google Fonts থেকে Inter এবং Noto Sans Bengali ফন্ট -->
    <link rel="preconnect" href="https://googleapis.com">
    <link rel="preconnect" href="https://gstatic.com" crossorigin>
    <link href="https://googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Noto+Sans+Bengali:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    
    <style>
        /* ডিফল্ট সেটিংস */
        body {
            font-family: 'Inter', 'Noto Sans Bengali', sans-serif;
            background-color: #f0f2f5;
            color: #1c1e21;
            margin: 0;
            padding: 20px;
        }

        /* পোস্ট ফিড কন্টেইনার */
        #feed-container {
            max-width: 500px;
            margin: 0 auto;
            display: flex;
            flex-direction: column;
            gap: 16px; /* পোস্টগুলোর মাঝের দূরত্ব */
        }

        /* পোস্ট কার্ড */
        .post-card {
            background-color: #ffffff;
            border-radius: 8px;
            box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
            padding: 15px;
        }

        /* প্রোফাইল সেকশন */
        .post-header {
            display: flex;
            align-items: center;
            margin-bottom: 12px;
        }

        .profile-pic {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            margin-right: 10px;
            text-transform: uppercase;
        }

        .user-info {
            display: flex;
            flex-direction: column;
        }

        .user-name {
            font-size: 15px;
            font-weight: 600;
            margin: 0;
        }

        .post-time {
            font-size: 12px;
            color: #65676b;
            margin-top: 2px;
        }

        /* পোস্টের মূল লেখা */
        .post-content {
            font-size: 15px;
            font-weight: 400;
            line-height: 1.5;
            margin-bottom: 12px;
        }

        /* পোস্টের ছবি */
        .post-image {
            width: calc(100% + 30px); /* কার্ডের প্যাডিং কাভার করার জন্য */
            margin-left: -15px;
            margin-right: -15px;
            max-height: 400px;
            object-fit: cover;
            margin-bottom: 12px;
            display: block;
        }

        /* লাইক-কমেন্ট কাউন্টার */
        .post-stats {
            display: flex;
            justify-content: space-between;
            font-size: 13px;
            color: #65676b;
            padding-bottom: 8px;
            border-bottom: 1px solid #e4e6eb;
            margin-bottom: 5px;
        }

        /* অ্যাকশন বাটন সেকশন */
        .post-actions {
            display: flex;
            justify-content: space-between;
        }

        .action-btn {
            flex: 1;
            background: none;
            border: none;
            padding: 8px;
            font-size: 14px;
            font-weight: 600;
            color: #65676b;
            border-radius: 4px;
            cursor: pointer;
            transition: background 0.2s;
        }

        .action-btn:hover {
            background-color: #f2f2f2;
            color: #007fff;
        }
    </style>
</head>
<body>

    <!-- যেখানে সব পোস্ট লোড হবে -->
    <div id="feed-container"></div>

    <script>
        // ১. ডেমো পোস্ট ডাটা (এখানে নতুন অবজেক্ট যোগ করলেই নতুন পোস্ট তৈরি হবে)
        const postsData = [
            {
                name: "Alex Arfin",
                time: "১০ মিনিট আগে",
                content: "স্বাগতম! আমি আমার নতুন সোশ্যাল মিডিয়া প্ল্যাটফর্ম তৈরি করা শুরু করেছি। <br><br> Welcome to the future of connecting people! Stay tuned for more updates.",
                image: "", // ছবি না থাকলে খালি রাখবেন
                likes: 25,
                comments: 5
            },
            {
                name: "Rahat Khan",
                time: "২ ঘণ্টা আগে",
                content: "আজকের আবহাওয়া চমৎকার! বন্ধুদের সাথে ঘুরতে যাওয়ার জন্য একদম পারফেক্ট দিন। ☀️🌦️",
                image: "https://picsum.photos", // ডেমো ছবি
                likes: 142,
                comments: 18
            }
        ];

        // ২. প্রোফাইলের জন্য রেন্ডম আকর্ষণীয় কালার লিস্ট
        const avatarColors = ["#007fff", "#ff5722", "#4caf50", "#9c27b0", "#e91e63", "#009688"];

        // ৩. নামের প্রথম দুই অক্ষর বের করার ফাংশন
        function getInitials(name) {
            return name.split(' ').map(word => word[0]).join('').slice(0, 2);
        }

        // ৪. ফিড কন্টেইনারে পোস্ট তৈরি ও রেন্ডার করার লুপ
        const feed = document.getElementById("feed-container");

        postsData.forEach((post, index) => {
            // রেন্ডম কালার সিলেক্ট করা
            const randomColor = avatarColors[index % avatarColors.length];
            
            // ছবির অংশ (যদি ডাটাতে ছবি থাকে তবেই কোড যোগ হবে)
            const imageHtml = post.image ? `<img class="post-image" src="${post.image}" alt="Post image">` : '';

            // কার্ডের ভেতরের HTML স্ট্রাকচার
            const cardHtml = `
                <div class="post-card">
                    <div class="post-header">
                        <div class="profile-pic" style="background-color: ${randomColor};">
                            ${getInitials(post.name)}
                        </div>
                        <div class="user-info">
                            <h2 class="user-name">${post.name}</h2>
                            <div class="post-time">${post.time}</div>
                        </div>
                    </div>
                    
                    <p class="post-content">${post.content}</p>
                    
                    ${imageHtml}
                    
                    <div class="post-stats">
                        <span>👍 ${post.likes} জন</span>
                        <span>${post.comments}টি কমেন্ট</span>
                    </div>
                    
                    <div class="post-actions">
                        <button class="action-btn">👍 লাইক</button>
                        <button class="action-btn">💬 কমেন্ট</button>
                        <button class="action-btn">↩ শেয়ার</button>
                    </div>
                </div>
            `;
            
            // কন্টেইনারে পুশ করা
            feed.innerHTML += cardHtml;
        });
    </script>
</body>
</html>
