<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Netflix Clone</title>
    <style>
        /* Basic Reset & Typography */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
            background-color: #141414;
            color: #fff;
            -webkit-font-smoothing: antialiased;
            -moz-osx-font-smoothing: grayscale;
        }

        a {
            text-decoration: none;
            color: #e5e5e5;
            transition: color 0.3s;
        }

        a:hover {
            color: #b3b3b3;
        }

        h2 {
            font-size: 1.4vw;
            margin-bottom: 15px;
            font-weight: 700;
        }

        /* Header Navigation */
        .header {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            display: flex;
            align-items: center;
            padding: 20px 60px;
            z-index: 100;
            background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.7) 10%, rgba(0, 0, 0, 0));
            transition: background-color 0.4s;
        }
        
        /* Add a background to the header on scroll for better visibility */
        /* This is a visual effect, will be static here but would be triggered by JS in a real app */
        .header-scrolled {
             background-color: #141414;
        }

        .logo {
            width: 100px;
            height: auto;
            margin-right: 25px;
        }
        
        .logo svg{
            fill: #e50914;
            width: 100%;
        }

        .nav {
            display: flex;
            gap: 20px;
        }

        .nav a {
            font-weight: 500;
            font-size: 0.9rem;
        }
        
        .nav a.active {
            color: #fff;
            font-weight: 700;
        }

        .header-right {
            margin-left: auto;
            display: flex;
            align-items: center;
            gap: 20px;
        }

        .search-icon, .notification-icon, .profile-icon {
            width: 24px;
            height: 24px;
            cursor: pointer;
        }

        .profile-icon img {
            width: 100%;
            border-radius: 4px;
        }
        
        /* Hero Section */
        .hero {
            height: 56.25vw; /* 16:9 Aspect Ratio */
            min-height: 450px;
            max-height: 800px;
            position: relative;
            display: flex;
            flex-direction: column;
            justify-content: center;
            padding-left: 60px;
        }

        .hero::before {
            content: "";
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-image: url('https://placehold.co/1600x900/000000/FFFFFF?text=Movie+Banner');
            background-size: cover;
            background-position: center;
            opacity: 0.6;
            z-index: -2;
        }

        .hero::after {
            content: "";
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100%;
            height: 140px;
            background-image: linear-gradient(to top, #141414, transparent);
            z-index: -1;
        }

        .hero-content {
            width: 40%;
            max-width: 600px;
        }

        .hero-title {
            font-size: 3.5vw;
            font-weight: 800;
            margin-bottom: 20px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.45);
        }

        .hero-description {
            font-size: 1.2vw;
            line-height: 1.5;
            margin-bottom: 25px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.45);
        }

        .hero-buttons {
            display: flex;
            gap: 10px;
        }

        .btn {
            padding: 12px 25px;
            border: none;
            border-radius: 4px;
            font-size: 1rem;
            font-weight: 700;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 8px;
            transition: background-color 0.3s;
        }

        .btn-play {
            background-color: #fff;
            color: #000;
        }

        .btn-play:hover {
            background-color: rgba(255, 255, 255, 0.75);
        }

        .btn-more-info {
            background-color: rgba(109, 109, 110, 0.7);
            color: #fff;
        }

        .btn-more-info:hover {
            background-color: rgba(109, 109, 110, 0.4);
        }
        
        .btn-icon{
            width: 24px;
        }

        /* Main Content - Movie Rows */
        .main-content {
            padding: 0 0 50px 60px;
            position: relative;
            margin-top: -140px; /* Pulls the content up over the hero gradient */
        }
        
        .movie-row {
            margin-bottom: 40px;
        }

        .row-content {
            display: flex;
            overflow-x: auto;
            overflow-y: hidden;
            padding-bottom: 20px; /* For scrollbar */
            scrollbar-width: thin;
            scrollbar-color: #141414 #141414; /* For Firefox */
        }
        
        /* Webkit scrollbar styling */
        .row-content::-webkit-scrollbar {
            height: 8px;
        }

        .row-content::-webkit-scrollbar-track {
            background: transparent;
        }

        .row-content::-webkit-scrollbar-thumb {
            background-color: rgba(255,255,255,0.1);
            border-radius: 20px;
        }
        
        .row-content::-webkit-scrollbar-thumb:hover {
            background-color: rgba(255,255,255,0.2);
        }

        .movie-card {
            flex: 0 0 16.66%; /* 6 items per view on large screens */
            max-width: 16.66%;
            padding: 0 4px;
            transition: transform 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94), z-index 0.3s;
        }

        .movie-card-img {
            width: 100%;
            height: auto;
            border-radius: 4px;
            cursor: pointer;
            display: block;
        }

        .movie-card:hover {
            transform: scale(1.2);
            z-index: 10;
        }
        
        /* Top 10 Row Specific Styling */
        .top-10 .movie-card {
            flex: 0 0 20%;
            max-width: 20%;
        }

        .top-10 .movie-card-content {
            display: flex;
            align-items: flex-end;
        }

        .top-10 .rank {
            font-size: 8vw;
            font-weight: 800;
            line-height: 0.7;
            margin-right: -10px;
            text-shadow: -2px 0 black, 0 2px black, 2px 0 black, 0 -2px black;
        }
        
        /* Footer */
        .footer {
            max-width: 980px;
            margin: 20px auto 0;
            padding: 0 4%;
            color: #808080;
        }
        
        .social-links {
            display: flex;
            gap: 25px;
            margin-bottom: 1em;
        }
        
        .social-links a svg {
            width: 24px;
            height: 24px;
            fill: #fff;
        }
        
        .footer-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 15px;
            margin-bottom: 1em;
        }
        
        .footer-grid a {
            font-size: 13px;
        }
        
        .service-code {
            border: 1px solid #808080;
            padding: 0.5em;
            font-size: 13px;
            cursor: pointer;
            display: inline-block;
            margin-bottom: 1em;
        }
        
        .service-code:hover {
            color: #fff;
        }
        
        .copyright {
            font-size: 11px;
            margin-bottom: 1em;
        }

        /* Responsive Design */
        @media (max-width: 1024px) {
            h2 {
                font-size: 1.8vw;
            }
            .hero-title {
                font-size: 4vw;
            }
            .hero-description {
                font-size: 1.5vw;
            }
            .movie-card {
                flex: 0 0 25%; /* 4 items */
                max-width: 25%;
            }
            .top-10 .movie-card {
                flex: 0 0 33.33%;
                max-width: 33.33%;
            }
        }

        @media (max-width: 768px) {
            .header {
                padding: 15px 20px;
            }
            .nav {
                display: none; /* Hide nav links on smaller screens */
            }
            .logo {
                width: 80px;
            }
            .hero {
                padding-left: 20px;
            }
            .hero-content {
                width: 70%;
            }
            h2 {
                font-size: 2.5vw;
            }
            .hero-title {
                font-size: 5vw;
            }
            .hero-description {
                font-size: 2vw;
                width: 90%;
            }
            .btn {
                padding: 8px 15px;
                font-size: 0.9rem;
            }
             .btn-icon{
                width: 20px;
            }
            .main-content {
                padding: 0 0 30px 20px;
            }
            .movie-card {
                flex: 0 0 33.33%; /* 3 items */
                max-width: 33.33%;
            }
            .top-10 .movie-card {
                flex: 0 0 50%;
                max-width: 50%;
            }
            .footer-grid {
                grid-template-columns: repeat(2, 1fr);
            }
        }
        
         @media (max-width: 480px) {
             h2 {
                font-size: 4vw;
            }
            .hero-title {
                font-size: 7vw;
            }
            .hero-description {
                font-size: 3vw;
            }
             .movie-card {
                flex: 0 0 50%; /* 2 items */
                max-width: 50%;
            }
         }
    </style>
</head>
<body>

    <!-- Header Section -->
    <header class="header header-scrolled">
        <a href="#" class="logo">
            <svg viewBox="0 0 111 30" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M105.062 14.28L111 30V0H105.062V14.28Z" />
                <path d="M99.125 0V30H93.187V0H99.125Z" />
                <path d="M87.25 0V30H81.312V0H87.25Z" />
                <path d="M75.375 0V24.338L69.438 0H62.5V30H68.438V5.662L74.375 30H81.312V0H75.375Z" />
                <path d="M53.438 0L62.5 30V0H53.438Z" />
                <path d="M47.5 30V0H41.562V30H47.5Z" />
                <path d="M35.563 0L26.5 30V0H35.563Z" />
                <path d="M17.688 0L17.688 30H23.625V11.25L29.562 30H35.5L26.562 13.05L26.5 11.25V0H17.688Z" />
                <path d="M0 0V30H5.938V11.25L11.875 30H17.812L8.813 13.05L8.75 11.25V0H0Z" />
            </svg>
        </a>
        <nav class="nav">
            <a href="#" class="active">Home</a>
            <a href="#">TV Shows</a>
            <a href="#">Movies</a>
            <a href="#">New & Popular</a>
            <a href="#">My List</a>
            <a href="#">Browse by Languages</a>
        </nav>
        <div class="header-right">
            <!-- Search Icon SVG -->
            <svg class="search-icon" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" clip-rule="evenodd" d="M15.031 16.594a7.5 7.5 0 111.563-1.563l4.812 4.813-1.562 1.562-4.813-4.812zM16.25 9.375a6.25 6.25 0 11-12.5 0 6.25 6.25 0 0112.5 0z" fill="currentColor"></path></svg>
            <!-- Notification Icon SVG -->
            <svg class="notification-icon" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" clip-rule="evenodd" d="M13 19.5c0 .28-.05.55-.15.8-.3.8-.93 1.44-1.78 1.63-.1.02-.2.03-.3.03-.13 0-.25-.02-.38-.05-1.03-.2-1.8-1.02-1.95-2.07-.05-.32-.07-.65-.07-.98h4.63zM12 2.25c-4.28 0-7.75 3.47-7.75 7.75V14.5s-.33 1.83.25 2.42c.58.58 1.92.58 1.92.58h11.16s1.34 0 1.92-.58c.58-.59.25-2.42.25-2.42V10c0-4.28-3.47-7.75-7.75-7.75zm-3.25 15.25c0 .28.05.55.15.8.3.8.93 1.44 1.78 1.63.1.02.2.03.3.03s.25-.02.38-.05c1.03-.2 1.8-1.02 1.95-2.07.05-.32.07-.65.07-.98H8.75z" fill="currentColor"></path></svg>
            <div class="profile-icon">
                <img src="https://placehold.co/32x32/8B0000/FFFFFF?text=P" alt="Profile">
            </div>
        </div>
    </header>

    <!-- Main Content -->
    <main>
        <!-- Hero Section -->
        <section class="hero">
            <div class="hero-content">
                <h1 class="hero-title">Movie of the Day</h1>
                <p class="hero-description">
                    This is a compelling description of the featured movie or show. It's designed to grab your attention and make you want to watch it right away. Experience the drama, the action, and the suspense.
                </p>
                <div class="hero-buttons">
                    <button class="btn btn-play">
                        <svg class="btn-icon" viewBox="0 0 24 24"><path d="M6 4l15 8-15 8V4z" fill="currentColor"></path></svg>
                        Play
                    </button>
                    <button class="btn btn-more-info">
                        <svg class="btn-icon" viewBox="0 0 24 24"><path fill-rule="evenodd" clip-rule="evenodd" d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-1 15v-8h2v8h-2zm1-11.5c-.83 0-1.5-.67-1.5-1.5s.67-1.5 1.5-1.5 1.5.67 1.5 1.5-.67 1.5-1.5 1.5z" fill="currentColor"></path></svg>
                        More Info
                    </button>
                </div>
            </div>
        </section>

        <!-- Movie Rows Section -->
        <section class="main-content">
            <!-- Row 1: Trending Now -->
            <div class="movie-row">
                <h2>Trending Now</h2>
                <div class="row-content">
                    <div class="movie-card"><img class="movie-card-img" src="https://placehold.co/300x450/333/fff?text=Movie+1" alt="Movie 1"></div>
                    <div class="movie-card"><img class="movie-card-img" src="https://placehold.co/300x450/444/fff?text=Movie+2" alt="Movie 2"></div>
                    <div class="movie-card"><img class="movie-card-img" src="https://placehold.co/300x450/555/fff?text=Movie+3" alt="Movie 3"></div>
                    <div class="movie-card"><img class="movie-card-img" src="https://placehold.co/300x450/666/fff?text=Movie+4" alt="Movie 4"></div>
                    <div class="movie-card"><img class="movie-card-img" src="https://placehold.co/300x450/777/fff?text=Movie+5" alt="Movie 5"></div>
                    <div class="movie-card"><img class="movie-card-img" src="https://placehold.co/300x450/888/fff?text=Movie+6" alt="Movie 6"></div>
                    <div class="movie-card"><img class="movie-card-img" src="https://placehold.co/300x450/999/fff?text=Movie+7" alt="Movie 7"></div>
                </div>
            </div>

            <!-- Row 2: Top 10 in Your Country -->
            <div class="movie-row top-10">
                <h2>Top 10 in Your Country Today</h2>
                <div class="row-content">
                    <div class="movie-card">
                       <div class="movie-card-content">
                           <div class="rank">1</div>
                           <img class="movie-card-img" src="https://placehold.co/300x450/e50914/fff?text=Top+1" alt="Top 1">
                       </div>
                    </div>
                    <div class="movie-card">
                       <div class="movie-card-content">
                           <div class="rank">2</div>
                           <img class="movie-card-img" src="https://placehold.co/300x450/f44336/fff?text=Top+2" alt="Top 2">
                       </div>
                    </div>
                    <div class="movie-card">
                       <div class="movie-card-content">
                           <div class="rank">3</div>
                           <img class="movie-card-img" src="https://placehold.co/300x450/ff5722/fff?text=Top+3" alt="Top 3">
                       </div>
                    </div>
                    <div class="movie-card">
                       <div class="movie-card-content">
                           <div class="rank">4</div>
                           <img class="movie-card-img" src="https://placehold.co/300x450/ff9800/fff?text=Top+4" alt="Top 4">
                       </div>
                    </div>
                     <div class="movie-card">
                       <div class="movie-card-content">
                           <div class="rank">5</div>
                           <img class="movie-card-img" src="https://placehold.co/300x450/ffc107/fff?text=Top+5" alt="Top 5">
                       </div>
                    </div>
                    <div class="movie-card">
                       <div class="movie-card-content">
                           <div class="rank">6</div>
                           <img class="movie-card-img" src="https://placehold.co/300x450/ffc107/fff?text=Top+6" alt="Top 6">
                       </div>
                    </div>
                    <div class="movie-card">
                       <div class="movie-card-content">
                           <div class="rank">7</div>
                           <img class="movie-card-img" src="https://placehold.co/300x450/ff9800/fff?text=Top+7" alt="Top 7">
                       </div>
                    </div>
                    <div class="movie-card">
                       <div class="movie-card-content">
                           <div class="rank">8</div>
                           <img class="movie-card-img" src="https://placehold.co/300x450/ff5722/fff?text=Top+8" alt="Top 8">
                       </div>
                    </div>
                    <div class="movie-card">
                       <div class="movie-card-content">
                           <div class="rank">9</div>
                           <img class="movie-card-img" src="https://placehold.co/300x450/f44336/fff?text=Top+9" alt="Top 9">
                       </div>
                    </div>
                    <div class="movie-card">
                       <div class="movie-card-content">
                           <div class="rank">10</div>
                           <img class="movie-card-img" src="https://placehold.co/300x450/e50914/fff?text=Top+10" alt="Top 10">
                       </div>
                    </div>
                </div>
            </div>

            <!-- Row 3: New Releases -->
            <div class="movie-row">
                <h2>New Releases</h2>
                <div class="row-content">
                    <div class="movie-card"><img class="movie-card-img" src="https://placehold.co/300x450/8BC34A/fff?text=New+1" alt="New 1"></div>
                    <div class="movie-card"><img class="movie-card-img" src="https://placehold.co/300x450/4CAF50/fff?text=New+2" alt="New 2"></div>
                    <div class="movie-card"><img class="movie-card-img" src="https://placehold.co/300x450/009688/fff?text=New+3" alt="New 3"></div>
                    <div class="movie-card"><img class="movie-card-img" src="https://placehold.co/300x450/00BCD4/fff?text=New+4" alt="New 4"></div>
                    <div class="movie-card"><img class="movie-card-img" src="https://placehold.co/300x450/03A9F4/fff?text=New+5" alt="New 5"></div>
                    <div class="movie-card"><img class="movie-card-img" src="https://placehold.co/300x450/2196F3/fff?text=New+6" alt="New 6"></div>
                    <div class="movie-card"><img class="movie-card-img" src="https://placehold.co/300x450/3F51B5/fff?text=New+7" alt="New 7"></div>
                </div>
            </div>
            
            <!-- Row 4: Critically Acclaimed -->
            <div class="movie-row">
                <h2>Critically Acclaimed</h2>
                <div class="row-content">
                    <div class="movie-card"><img class="movie-card-img" src="https://placehold.co/300x450/673AB7/fff?text=Crit+1" alt="Crit 1"></div>
                    <div class="movie-card"><img class="movie-card-img" src="https://placehold.co/300x450/9C27B0/fff?text=Crit+2" alt="Crit 2"></div>
                    <div class="movie-card"><img class="movie-card-img" src="https://placehold.co/300x450/E91E63/fff?text=Crit+3" alt="Crit 3"></div>
                    <div class="movie-card"><img class="movie-card-img" src="https://placehold.co/300x450/F44336/fff?text=Crit+4" alt="Crit 4"></div>
                    <div class="movie-card"><img class="movie-card-img" src="https://placehold.co/300x450/FF5722/fff?text=Crit+5" alt="Crit 5"></div>
                    <div class="movie-card"><img class="movie-card-img" src="https://placehold.co/300x450/FF9800/fff?text=Crit+6" alt="Crit 6"></div>
                    <div class="movie-card"><img class="movie-card-img" src="https://placehold.co/300x450/FFC107/fff?text=Crit+7" alt="Crit 7"></div>
                </div>
            </div>
        </section>
    </main>
    
    <!-- Footer Section -->
    <footer class="footer">
        <div class="social-links">
            <a href="#"><!-- Facebook --><svg viewBox="0 0 24 24"><path d="M22.676 0H1.324C.593 0 0 .593 0 1.324v21.352C0 23.407.593 24 1.324 24h11.494v-9.294H9.692v-3.622h3.128V8.413c0-3.1 1.893-4.788 4.659-4.788 1.325 0 2.463.099 2.795.143v3.24l-1.918.001c-1.504 0-1.795.715-1.795 1.763v2.313h3.587l-.467 3.622h-3.12V24h6.116c.73 0 1.323-.593 1.323-1.324V1.324C24 .593 23.407 0 22.676 0z" /></svg></a>
            <a href="#"><!-- Instagram --><svg viewBox="0 0 24 24"><path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.85s-.011 3.584-.069 4.85c-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07s-3.584-.012-4.85-.07c-3.252-.148-4.771-1.691-4.919-4.919-.058-1.265-.069-1.645-.069-4.85s.011-3.584.069-4.85c.149-3.225 1.664-4.771 4.919-4.919 1.266-.057 1.644-.069 4.85-.069zM12 0C8.741 0 8.333.014 7.053.072 2.695.272.273 2.69.073 7.052.014 8.333 0 8.741 0 12s.014 3.667.072 4.947c.2 4.358 2.618 6.78 6.98 6.98C8.333 23.986 8.741 24 12 24s3.667-.014 4.947-.072c4.358-.2 6.78-2.618 6.98-6.98.058-1.28.072-1.687.072-4.947s-.014-3.667-.072-4.947c-.2-4.358-2.618-6.78-6.98-6.98C15.667.014 15.259 0 12 0zm0 5.838a6.162 6.162 0 100 12.324 6.162 6.162 0 000-12.324zM12 16a4 4 0 110-8 4 4 0 010 8zm6.406-11.845a1.44 1.44 0 100 2.88 1.44 1.44 0 000-2.88z" /></svg></a>
            <a href="#"><!-- Twitter --><svg viewBox="0 0 24 24"><path d="M23.954 4.569c-.885.389-1.83.654-2.825.775 1.014-.611 1.794-1.574 2.163-2.723-.951.555-2.005.959-3.127 1.184-.896-.959-2.173-1.559-3.591-1.559-2.717 0-4.92 2.203-4.92 4.917 0 .39.045.765.127 1.124C7.691 8.094 4.066 6.13 1.64 3.161c-.427.722-.666 1.561-.666 2.475 0 1.71.87 3.213 2.188 4.096-.807-.026-1.566-.248-2.228-.616v.061c0 2.385 1.693 4.374 3.946 4.827-.413.111-.849.171-1.296.171-.314 0-.615-.03-.916-.086.631 1.953 2.445 3.377 4.604 3.417-1.68 1.319-3.809 2.105-6.102 2.105-.39 0-.779-.023-1.17-.067 2.189 1.394 4.768 2.209 7.557 2.209 9.054 0 14.01-7.496 14.01-13.986 0-.213-.005-.426-.015-.638.961-.689 1.79-1.56 2.455-2.549z" /></svg></a>
            <a href="#"><!-- Youtube --><svg viewBox="0 0 24 24"><path d="M23.495 6.205a3.007 3.007 0 00-2.088-2.088c-1.87-.502-9.407-.502-9.407-.502s-7.537 0-9.407.502a3.007 3.007 0 00-2.088 2.088C0 8.075 0 12 0 12s0 3.925.505 5.795a3.007 3.007 0 002.088 2.088c1.87.502 9.407.502 9.407.502s7.537 0 9.407-.502a3.007 3.007 0 002.088-2.088C24 15.925 24 12 24 12s0-3.925-.505-5.795zM9.545 15.568V8.432l6.591 3.568-6.591 3.568z" /></svg></a>
        </div>
        <div class="footer-grid">
            <a href="#">Audio Description</a>
            <a href="#">Help Center</a>
            <a href="#">Gift Cards</a>
            <a href="#">Media Center</a>
            <a href="#">Investor Relations</a>
            <a href="#">Jobs</a>
            <a href="#">Terms of Use</a>
            <a href="#">Privacy</a>
            <a href="#">Legal Notices</a>
            <a href="#">Cookie Preferences</a>
            <a href="#">Corporate Information</a>
            <a href="#">Contact Us</a>
        </div>
        <button class="service-code">Service Code</button>
        <div class="copyright">
            &copy; 1997-2024 Netflix, Inc.
        </div>
    </footer>

</body>
</html>
