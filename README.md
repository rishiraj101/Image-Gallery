<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Image Gallery</title>
    <style>

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f4f7f6;
            color: #333;
            line-height: 1.6;
            padding: 40px 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
        }

        h1 {
            text-align: center;
            margin-bottom: 30px;
            color: #2c3e50;
        }

       
        .filter-wrapper {
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
            gap: 10px;
            margin-bottom: 30px;
        }

        .filter-btn {
            background-color: #fff;
            border: 2px solid #3498db;
            color: #3498db;
            padding: 8px 18px;
            font-size: 1rem;
            font-weight: 600;
            border-radius: 25px;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .filter-btn:hover, 
        .filter-btn.active {
            background-color: #3498db;
            color: #fff;
            box-shadow: 0 4px 12px rgba(52, 152, 219, 0.3);
        }

        
        .gallery-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
            gap: 20px;
        }

        .gallery-item {
            position: relative;
            background-color: #fff;
            border-radius: 8px;
            overflow: hidden;
            box-shadow: 0 4px 8px rgba(0,0,0,0.05);
            cursor: pointer;
            aspect-ratio: 4 / 3;
            transition: transform 0.4s ease, opacity 0.4s ease, visibility 0.4s;
        }

     
        .gallery-item.hide {
            transform: scale(0.8);
            opacity: 0;
            visibility: hidden;
            position: absolute;
            width: 0;
            height: 0;
            overflow: hidden;
            margin: 0;
            padding: 0;
        }

        .gallery-item img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            display: block;
            transition: transform 0.5s ease;
        }

       
        .gallery-item:hover img {
            transform: scale(1.1);
        }

        .image-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(to top, rgba(0,0,0,0.7) 0%, rgba(0,0,0,0) 60%);
            display: flex;
            align-items: flex-end;
            padding: 15px;
            opacity: 0;
            transition: opacity 0.3s ease;
        }

        .gallery-item:hover .image-overlay {
            opacity: 1;
        }

        .image-title {
            color: #fff;
            font-size: 1.1rem;
            transform: translateY(10px);
            transition: transform 0.3s ease;
        }

        .gallery-item:hover .image-title {
            transform: translateY(0);
        }

       
        .lightbox {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.9);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.3s ease;
        }

        .lightbox.active {
            opacity: 1;
            pointer-events: auto;
        }

        .lightbox-content {
            position: relative;
            max-width: 85%;
            max-height: 80%;
        }

        .lightbox-img {
            max-width: 100%;
            max-height: 80vh;
            border-radius: 4px;
            box-shadow: 0 5px 25px rgba(0,0,0,0.5);
            display: block;
        }

        .lightbox-caption {
            color: #fff;
            text-align: center;
            margin-top: 15px;
            font-size: 1.2rem;
        }

        /* Lightbox UI Controls */
        .close-btn {
            position: absolute;
            top: 20px;
            right: 25px;
            color: #fff;
            font-size: 40px;
            font-weight: bold;
            cursor: pointer;
            transition: color 0.2s;
        }

        .close-btn:hover {
            color: #ff4757;
        }

        .nav-btn {
            position: absolute;
            top: 50%;
            transform: translateY(-50%);
            background: rgba(255,255,255,0.1);
            color: white;
            padding: 15px 20px;
            border: none;
            font-size: 24px;
            font-weight: bold;
            cursor: pointer;
            border-radius: 5px;
            user-select: none;
            transition: background 0.2s;
        }

        .nav-btn:hover {
            background: rgba(255,255,255,0.3);
        }

        .prev-btn { left: 20px; }
        .next-btn { right: 20px; }

        /* Responsive UI tweaks */
        @media (max-width: 600px) {
            .nav-btn {
                padding: 10px 14px;
                font-size: 18px;
            }
            .prev-btn { left: 5px; }
            .next-btn { right: 5px; }
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>Interactive Project Gallery</h1>

        <div class="filter-wrapper">
            <button class="filter-btn active" data-filter="all">All</button>
            <button class="filter-btn" data-filter="nature">Nature</button>
            <button class="filter-btn" data-filter="architecture">Architecture</button>
            <button class="filter-btn" data-filter="animals">Animals</button>
        </div>

        <div class="gallery-grid" id="gallery">
            <div class="gallery-item" data-category="nature">
                <img src="https://picsum.photos/800/600?random=1" alt="Mountain Lake">
                <div class="image-overlay">
                    <div class="image-title">Mountain Lake</div>
                </div>
            </div>
            <div class="gallery-item" data-category="architecture">
                <img src="https://picsum.photos/800/600?random=2" alt="Modern Skyscraper">
                <div class="image-overlay">
                    <div class="image-title">Modern Skyscraper</div>
                </div>
            </div>
            <div class="gallery-item" data-category="animals">
                <img src="https://picsum.photos/800/600?random=3" alt="Majestic Lion">
                <div class="image-overlay">
                    <div class="image-title">Majestic Lion</div>
                </div>
            </div>
            <div class="gallery-item" data-category="nature">
                <img src="https://picsum.photos/800/600?random=4" alt="Autumn Forest">
                <div class="image-overlay">
                    <div class="image-title">Autumn Forest</div>
                </div>
            </div>
            <div class="gallery-item" data-category="architecture">
                <img src="https://picsum.photos/800/600?random=5" alt="Classic Cathedral">
                <div class="image-overlay">
                    <div class="image-title">Classic Cathedral</div>
                </div>
            </div>
            <div class="gallery-item" data-category="animals">
                <img src="https://picsum.photos/800/600?random=6" alt="Golden Retriever">
                <div class="image-overlay">
                    <div class="image-title">Golden Retriever</div>
                </div>
            </div>
        </div>
    </div>

    <div class="lightbox" id="lightbox">
        <span class="close-btn" id="closeLightbox">&times;</span>
        <button class="nav-btn prev-btn" id="prevBtn">&#10094;</button>
        
        <div class="lightbox-content">
            <img class="lightbox-img" id="lightboxImg" src="" alt="">
            <div class="lightbox-caption" id="lightboxCaption"></div>
        </div>

        <button class="nav-btn next-btn" id="nextBtn">&#10095;</button>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            // DOM Elements
            const filterButtons = document.querySelectorAll('.filter-btn');
            const galleryItems = document.querySelectorAll('.gallery-item');
            const lightbox = document.getElementById('lightbox');
            const lightboxImg = document.getElementById('lightboxImg');
            const lightboxCaption = document.getElementById('lightboxCaption');
            const closeLightbox = document.getElementById('closeLightbox');
            const prevBtn = document.getElementById('prevBtn');
            const nextBtn = document.getElementById('nextBtn');

           
            let visibleItems = Array.from(galleryItems);
            let currentIndex = 0;

            
            filterButtons.forEach(button => {
                button.addEventListener('click', () => {
                   
                    document.querySelector('.filter-btn.active').classList.remove('active');
                    button.classList.add('active');

                    const filterValue = button.getAttribute('data-filter');

                    galleryItems.forEach(item => {
                        const itemCategory = item.getAttribute('data-category');
                        
                        if (filterValue === 'all' || itemCategory === filterValue) {
                            item.classList.remove('hide');
                        } else {
                            item.classList.add('hide');
                        }
                    });

                    
                    visibleItems = Array.from(galleryItems).filter(item => !item.classList.contains('hide'));
                });
            });

        
            galleryItems.forEach(item => {
                item.addEventListener('click', () => {
                
                    currentIndex = visibleItems.indexOf(item);
                    showImage(visibleItems[currentIndex]);
                    lightbox.classList.add('active');
                });
            });

            function showImage(item) {
                const imgElement = item.querySelector('img');
                const titleElement = item.querySelector('.image-title');
                
                lightboxImg.src = imgElement.src;
                lightboxImg.alt = imgElement.alt;
                lightboxCaption.textContent = titleElement.textContent;
            }

     
            nextBtn.addEventListener('click', (e) => {
                e.stopPropagation(); 
                currentIndex = (currentIndex + 1) % visibleItems.length;
                showImage(visibleItems[currentIndex]);
            });

            
            prevBtn.addEventListener('click', (e) => {
                e.stopPropagation();
                currentIndex = (currentIndex - 1 + visibleItems.length) % visibleItems.length;
                showImage(visibleItems[currentIndex]);
            });

         
            closeLightbox.addEventListener('click', () => {
                lightbox.classList.remove('active');
            });

         
            lightbox.addEventListener('click', (e) => {
                if (e.target === lightbox) {
                    lightbox.classList.remove('active');
                }
            });

           
            document.addEventListener('keydown', (e) => {
                if (!lightbox.classList.contains('active')) return;
                
                if (e.key === 'Escape') {
                    lightbox.classList.remove('active');
                } else if (e.key === 'ArrowRight') {
                    nextBtn.click();
                } else if (e.key === 'ArrowLeft') {
                    prevBtn.click();
                }
            });
        });
    </script>
</body>
</html>
