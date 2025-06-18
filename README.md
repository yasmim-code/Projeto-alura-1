# Projeto-alura-1
<!DOCTYPE html><html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Flash Cards - Filmes de Terror</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Creepster&family=Roboto&display=swap');body {
  margin: 0;
  font-family: 'Roboto', sans-serif;
  background-color: #0b0c10;
  color: #ffffff;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
}

h1 {
  font-family: 'Creepster', cursive;
  font-size: 3rem;
  color: #e50914;
  margin-bottom: 1rem;
}

.card-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 2rem;
  width: 100%;
  max-width: 1000px;
}

.card {
  background-color: #1f1f1f;
  border: 2px solid #e50914;
  border-radius: 10px;
  padding: 1rem;
  cursor: pointer;
  perspective: 1000px;
}

.card-inner {
  transition: transform 0.6s;
  transform-style: preserve-3d;
  position: relative;
}

.card:hover .card-inner {
  transform: rotateY(180deg);
}

.card-front, .card-back {
  position: absolute;
  width: 100%;
  backface-visibility: hidden;
  padding: 1rem;
}

.card-front {
  background-color: #1f1f

