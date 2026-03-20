html
<html lang="en">

  <style>
    body {
  font-family: Arial, sans-serif;
  padding: 20px;
  background: #fafafa;
}

h1 {
  text-align: center;
  margin-bottom: 30px;
}

.card {
  background: white;
  padding: 20px;
  margin-bottom: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

input, textarea {
  width: 100%;
  padding: 10px;
  margin: 8px 0;
  border: 1px solid #ccc;
  border-radius: 6px;
}

button {
  padding: 10px 15px;
  background: #4CAF50;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}

button:hover {
  background: #45a049;
}

ul li {
  margin: 10px 0;
  padding: 10px;
  background: #f3f3f3;
  border-radius: 6px;
}
  </style>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>My Recipe Book</title>
  <link rel="stylesheet" href="style.css">
</head>

<body>

  <h1>My Recipe Book</h1>

  <!-- Add Recipe Form -->
  <section class="card">
    <h2>Add a Recipe</h2>
    <input id="recipeName" type="text" placeholder="Recipe name">
    <textarea id="recipeIngredients" placeholder="Ingredients (one per line)"></textarea>
    <textarea id="recipeSteps" placeholder="Steps"></textarea>
    <button id="saveRecipe">Save Recipe</button>
  </section>

  <!-- Search -->
  <section class="card">
    <h2>Search Recipes</h2>
    <input id="searchInput" type="text" placeholder="Search by name or ingredient">
  </section>

  <!-- Results -->
  <section id="recipeList" class="card">
    <h2>Saved Recipes</h2>
    <ul id="recipes"></ul>
  </section>

  <script src="app.js">

    // Load existing recipes or create empty array
let recipes = JSON.parse(localStorage.getItem("recipes")) || [];

// DOM elements
const nameInput = document.getElementById("recipeName");
const ingredientsInput = document.getElementById("recipeIngredients");
const stepsInput = document.getElementById("recipeSteps");
const saveBtn = document.getElementById("saveRecipe");
const searchInput = document.getElementById("searchInput");
const recipeList = document.getElementById("recipes");

// Save recipe
saveBtn.addEventListener("click", () => {
  const recipe = {
    name: nameInput.value.trim(),
    ingredients: ingredientsInput.value.trim().split("\n"),
    steps: stepsInput.value.trim()
  };

  if (!recipe.name) {
    alert("Recipe must have a name");
    return;
  }

  recipes.push(recipe);
  localStorage.setItem("recipes", JSON.stringify(recipes));

  nameInput.value = "";
  ingredientsInput.value = "";
  stepsInput.value = "";

  renderRecipes(recipes);
});

// Search recipes
searchInput.addEventListener("input", () => {
  const query = searchInput.value.toLowerCase();
  const filtered = recipes.filter(r =>
    r.name.toLowerCase().includes(query) ||
    r.ingredients.some(i => i.toLowerCase().includes(query))
  );
  renderRecipes(filtered);
});

// Render recipe list
function renderRecipes(list) {
  recipeList.innerHTML = "";
  list.forEach(recipe => {
    const li = document.createElement("li");
    li.innerHTML = `
      <strong>${recipe.name}</strong><br>
      <em>${recipe.ingredients.length} ingredients</em>
    `;
    recipeList.appendChild(li);
  });
}

// Initial load
renderRecipes(recipes);
  </script>
</body>
</html>
