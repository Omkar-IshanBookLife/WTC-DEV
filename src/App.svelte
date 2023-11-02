<script lang="ts">
  type Dish = {
    id: string;
    title: string;
  };

  let dishes: Dish[];
  let dishText: string = "";

  let breaskfast_dish = "none";
  let lunch_dish = "none";
  let dinner_dish = "none";

  import { init } from "@paralleldrive/cuid2";
  import { onMount } from "svelte";

  const createId = init({
    random: Math.random,
    length: 10,
    fingerprint: "a-custom-host-fingerprint",
  });

  const addDish = () => {
    let newDish: Dish = {
      id: createId(),
      title: dishText,
    };
    let newDishes = [...dishes, newDish];
    window.localStorage.setItem("dishes", JSON.stringify(newDishes));
    dishText = "";
    dishes = newDishes;
  };

  const deleteDish = (id: string) => {
    dishes = dishes.filter((dish) => dish.id !== id);
    window.localStorage.setItem("dishes", JSON.stringify(dishes));
  };

  const randomize = () => {
    breaskfast_dish = dishes[Math.floor(Math.random() * dishes.length)].title;
    lunch_dish = dishes[Math.floor(Math.random() * dishes.length)].title;
    dinner_dish = dishes[Math.floor(Math.random() * dishes.length)].title;
  };

  const clear = () => {
    breaskfast_dish = "none";
    lunch_dish = "none";
    dinner_dish = "none";
  };

  onMount(() => {
    dishes = JSON.parse(window.localStorage.getItem("dishes") || "[]");
    randomize();
  });
</script>

<main>
  <h2>What to Cook.com</h2>
  <p>Inspired by rupali mam and Ishita. By Omkar Kumbhar</p>
  <hr />
  <h4>Step 1: Add dishes you are patient to cook today!</h4>
  <br />
  <div class="input-group mb-4">
    <input
      type="text"
      placeholder="Enter the Dishes you Low"
      bind:value={dishText}
      class="form-control"
    />
    <input
      type="submit"
      value="Add"
      on:click={addDish}
      class="btn btn-secondary"
    />
  </div>
  <ul>
    {#if dishes}
      {#each dishes as dish, i}
        <p class="text-start d-flex align-items-center justify-content-between">
          <b>{i + 1}. {dish.title}</b>
          <button on:click={() => deleteDish(dish.id)} class="btn btn-danger"
            >Remove</button
          >
        </p>
      {:else}
        <p><b>No Dishes. Please add some</b></p>
      {/each}
    {/if}
  </ul>
  <hr />
  <h4>Step 2: Randomly generate dishes to cook!</h4>
  <br />
  <p class="d-flex align-items-center justify-content-between">
    <b>For Breakfast: </b>
    {breaskfast_dish}
  </p>
  <p class="d-flex align-items-center justify-content-between">
    <b>For Lunch: </b>
    {lunch_dish}
  </p>
  <p class="d-flex align-items-center justify-content-between">
    <b>For Dinner: </b>
    {dinner_dish}
  </p>
  <button on:click={randomize} class="btn btn-info me-2">Randomize</button>
  <button on:click={clear} class="btn btn-warning">Clear</button>
  <hr />
  <h4>Step 3: Its done, Time to cook!</h4>
  <br />
  <button
    on:click={() => alert("You can do it! 👍")}
    class="btn btn-success btn-lg">Motivation to cook 👍</button
  >
  <hr />
  <h4>
    Step 4: Still Stuck or Lack the motivation to cook! order the food instead
  </h4>
  <br />
  <button
    class="btn btn-success"
    on:click={() =>
      window.open(
        `https://www.swiggy.com/search?query=${breaskfast_dish.replaceAll(
          " ",
          "+"
        )}`,
        "_blank"
      )}>Order Breakfast</button
  >
  <button
    class="btn btn-success"
    on:click={() =>
      window.open(
        `https://www.swiggy.com/search?query=${lunch_dish.replaceAll(
          " ",
          "+"
        )}`,
        "_blank"
      )}>Order Lunch</button
  >
  <button
    class="btn btn-success"
    on:click={() =>
      window.open(
        `https://www.swiggy.com/search?query=${dinner_dish.replaceAll(
          " ",
          "+"
        )}`,
        "_blank"
      )}>Order Dinner</button
  >
  <hr />
  <h4>Well Thats the end of the app.</h4>
  <p>
    Hope you find it useful, I hope Rupali Mam Does. This is just a small
    prototype built within an hour so be sure to find couple of bugs, that
    you'll report to me (I know you won't but still its fun to aniticipate)
    which I wont fix anyway. This is just a silly randomizer for some and a
    useful tool in reducing decision fatigue for many.
  </p>
  <p><b>Made with 💖 by Omkar Kumbhar</b></p>
  <hr>
  <p class="text-start d-flex align-items-center justify-content-between"><span>@ogamethorns</span><span>© Omkar & Swiggy 2023</span></p>
</main>
