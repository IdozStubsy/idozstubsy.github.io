<div class="site-search">
  <input
    type="search"
    id="searchInput"
    placeholder="Search..."
    autocomplete="off"
  >

  <div id="searchResults"></div>
</div>

<style>
.site-search {
  max-width: 600px;
  margin: 20px auto;
}

#searchInput {
  width: 100%;
  padding: 12px;
  font-size: 16px;
  box-sizing: border-box;
  border: 1px solid #ccc;
  border-radius: 8px;
}

.search-result {
  padding: 10px 0;
  border-bottom: 1px solid #ddd;
}

.search-result a {
  font-weight: bold;
  text-decoration: none;
}

.search-result p {
  margin: 5px 0;
}
</style>

<script>
const input = document.getElementById("searchInput");
const results = document.getElementById("searchResults");

let pages = [];

fetch("/search.json")
  .then(response => response.json())
  .then(data => {
    pages = data;
  })
  .catch(error => {
    console.error("Could not load search index:", error);
  });

input.addEventListener("input", () => {
  const query = input.value.trim().toLowerCase();

  if (!query) {
    results.innerHTML = "";
    return;
  }

  const matches = pages.filter(page =>
    page.title.toLowerCase().includes(query) ||
    page.content.toLowerCase().includes(query)
  );

  if (matches.length === 0) {
    results.innerHTML = "<p>No results found.</p>";
    return;
  }

  results.innerHTML = matches.map(page => `
    <div class="search-result">
      <a href="${page.url}">${page.title}</a>
      <p>${page.content}</p>
    </div>
  `).join("");
});
</script>
