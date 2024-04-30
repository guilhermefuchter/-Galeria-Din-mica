# -Galeria-Din-mica

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Galeria de Imagens</title>

    <style>
    body {
        font-family: Arial, sans-serif;
        margin: 0;
        padding: 20px;
        background-color: #f4f4f4;
    }
    
    h1 {
        text-align: center;
        margin-bottom: 20px;
    }
    
    input[type="file"] {
        display: block;
        margin-bottom: 10px;
    }
    
    #addBtn {
        padding: 10px 20px;
        background-color: #007bff;
        color: #fff;
        border: none;
        border-radius: 4px;
        cursor: pointer;
    }
    
    #addBtn:hover {
        background-color: #0056b3;
    }
    
    #gallery {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
        grid-gap: 10px;
    }
    
    img {
        width: 100%;
        height: auto;
        border-radius: 4px;
        cursor: pointer;
        transition: transform 0.3s;
    }
    
    img:hover {
        transform: scale(1.05);
    }
</style>
    


</head>
<body>
    <h1>Galeria de Imagens</h1>
    <input type="file" id="fileInput">
    <button id="addBtn">Adicionar Imagem</button>
    <div id="gallery"></div>

    <script>
        const gallery = document.getElementById('gallery');
        const fileInput = document.getElementById('fileInput');
        const addBtn = document.getElementById('addBtn');

      
        window.onload = function() {
            const images = JSON.parse(localStorage.getItem('images')) || [];
            images.forEach(image => {
                displayImage(image);
            });
        };

    
        addBtn.addEventListener('click', function() {
            const file = fileInput.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = function(event) {
                    const imageData = event.target.result;
                    const image = { id: Date.now(), data: imageData };
                    displayImage(image);
                    saveImage(image);
                };
                reader.readAsDataURL(file);
            }
        });

        gallery.addEventListener('click', function(event) {
            if (event.target.tagName === 'IMG') {
                const imageId = parseInt(event.target.dataset.id);
                removeImage(imageId);
            }
        });

      
        function displayImage(image) {
            const imgElement = document.createElement('img');
            imgElement.src = image.data;
            imgElement.setAttribute('data-id', image.id);
            gallery.appendChild(imgElement);
        }

        function saveImage(image) {
            let images = JSON.parse(localStorage.getItem('images')) || [];
            images.push(image);
            localStorage.setItem('images', JSON.stringify(images));
        }
        function removeImage(id) {
            let images = JSON.parse(localStorage.getItem('images')) || [];
            images = images.filter(image => image.id !== id);
            localStorage.setItem('images', JSON.stringify(images));
            refreshGallery();
        }

       
        function refreshGallery() {
            gallery.innerHTML = '';
            const images = JSON.parse(localStorage.getItem('images')) || [];
            images.forEach(image => {
                displayImage(image);
            });
        }
    </script>
</body>
</html>
