<script>
  let lang = $state("en");
  let q = $state("");
  let promise = $state(undefined);

  let level = {
    fr: "Niveau",
    en: "Level",
    de: "Stufe",
    es: "Nivel",
    pt: "Nível",
  };

  let not_affiliated = {
    fr: "non affilié à ankama games",
    en: "not affiliated with ankama games",
    de: "nicht mit ankama games verbunden",
    es: "no afiliado a ankama games",
    pt: "não afiliado à ankama games",
  };

  let uses = {
    fr: "utilise",
    en: "uses",
    de: "verwendet",
    es: "usa",
    pt: "usa",
  };

  async function search() {
    try {
      const res = await fetch(
        `https://api.dofusdu.de/dofus3/v1/${lang}/search?query=${q}&fields%5Bitem%5D=level,image_urls,type`
      );
      if (!res.ok) throw new Error("Network response was not ok");
      return await res.json();
    } catch {
      promise = undefined;
    }
  }
</script>

<main>
  <input
    type="search"
    id="search"
    class:center={promise === undefined}
    bind:value={q}
    onkeyup={() => (promise = search())}
    placeholder="ankama"
  />

  {#await promise then res}
    <div class="container">
      {#each res as item}
        <div class="card">
          <img alt={item.name} src={item.item_fields.image_urls.sd} />
          <div class="card-body">
            <h4><b>{item.name}</b></h4>
            <p>
              {item.item_fields.type.name} - {level[lang]}
              {item.item_fields.level}
            </p>
          </div>
        </div>
      {/each}
    </div>
  {/await}

  <select
    bind:value={lang}
    onchange={() => (promise = search())}
    name="lang"
    id="lang"
  >
    <option value="en">en</option>
    <option value="fr">fr</option>
    <option value="de">de</option>
    <option value="es">es</option>
    <option value="pt">pt</option>
  </select>

  <footer id="footer">
    <small>
      {not_affiliated[lang]} -
      <a
        href="https://github.com/your-repo"
        target="_blank"
        rel="noopener noreferrer">github</a
      >
      - {uses[lang]}
      <a
        href="https://github.com/dofusdude/doduapi"
        target="_blank"
        rel="noopener noreferrer">doduapi</a
      >
    </small>
  </footer>
</main>

<style>
  #search {
    position: absolute;
    top: 1rem;
    left: 1rem;
    transition: all 0.5s ease;
  }

  #search.center {
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }

  #lang {
    position: absolute;
    top: 1rem;
    right: 1rem;
  }

  .container {
    display: grid;
    grid-template-columns: repeat(4, 200px);
    column-gap: 2rem;
    row-gap: 2rem;
    margin: 3rem auto;
  }

  @media (max-width: 1100px) {
    .container {
      grid-template-columns: repeat(3, 200px);
    }
  }

  @media (max-width: 750px) {
    .container {
      grid-template-columns: 200px;
    }
  }

  .card {
    background-color: black;
    border: 1px solid greenyellow;
    transition: 0.3s;
  }

  .card:hover {
    border: 1px solid green;
  }

  .card-body {
    padding: 2px 16px;
  }

  #footer {
    position: fixed;
    bottom: 1rem;
    right: 1rem;
  }
</style>
