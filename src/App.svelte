<script>
  let inputURL;

  function processRequest(e) {
    const re = /commonwealth:\w*/
    let commonwealthId = inputURL.match(re);

    if (!commonwealthId || commonwealthId.length < 1) {
      window.alert("That doesn't seem to be a valid digital collections URL")
    } else {
      fetch(`https://collections.leventhalmap.org/search/${commonwealthId[0]}?format=json`)
        .then(d=>d.json())
        .then((r)=>{
          let document = r.response.document;
          let title = document.title_info_primary_tsi;
          let author = document.name_tsim.join("; ");
          // let pubisher = document.publisher_tsi ? document.publisher_tsi : "";
          let callNumber = document.identifier_local_call_tsim.join(" || ");
          let date = document.date_tsim.join(" || ")
          let aeonOpenUrl = encodeURI(`https://readingroom.bpl.org/aeon/aeon.dll?Action=10&Form=30&title=${title}&author=${author}&date=${date}&callNumber=${callNumber}`);
          window.open(aeonOpenUrl);
        })
        .catch(()=>{
          window.alert("Something went wrong trying to load the metadata for this item")
        })
    }

    
  }

</script>

<main class="p-8">

  <form>   
    <label for="search" class="mb-2 block text-lg font-medium text-gray-900 dark:text-gray-300">Enter digital collections URL</label>
    <div class="relative">
        <input type="search" bind:value={inputURL} id="search" class="block p-4 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="http://collections.leventhalmap.org/..." required>
        <button type="submit" on:click|preventDefault={processRequest} class="text-white absolute right-2.5 bottom-2.5 bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-4 py-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">Create Aeon Request</button>
    </div>
</form>

 
</main>

<style>

</style>
