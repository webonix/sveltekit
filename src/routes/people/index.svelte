<script>
	import SiteMenu from '$components/SiteMenu.svelte';

    // let people = [];

  
    async function getPeople() {
      const response = await fetch(`https://webonix.org/dk/roster-planner/api/people.cfm`); // const res = await fetch(`/api/people.json`);
      console.log('response',response);

      let people = await response.json();
      //console.log('people',people);

      if (response.ok) {
			return people;
	} else {
			throw new Error(people);
	}

  }

  console.log('getPeople()');
  let peoplePromise = getPeople();

  console.log('peoplePromise', peoplePromise);
  
</script>

<SiteMenu/>
<h1>People: API</h1>
<p>https://webonix.org/dk/roster-planner/api/people.cfm</p>

<!--
<h4>People</h4>
    {#each people as person }
    <div>
        <p> {person.name} </p>
    </div>
    {/each}
 -->

<hr>
{#await peoplePromise}
	<p>...loading</p>
{:then people}
    <h2>People Await</h2>
	{#each people as person }
    <div>
        <p> <strong>Name:</strong> {person['name']} <strong>Timezone:</strong> {person['timezone']} </p>
    </div>
    {/each}
{:catch error}
	<p style="color: red">{error.message}</p>
{/await}
<hr>
