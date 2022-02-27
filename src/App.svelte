<script>
  import { onMount } from 'svelte';
  import { mediumWords, longWords } from './words';

  let password = '';
  let hidePassword = false;
  let timeout;

  let numberOfWords = 4;
  let lengthOfWords = 'medium';

  function generatePassword() {
    const words = { medium: mediumWords, long: longWords }[lengthOfWords];

    hidePassword = true;

    let randomWords = [];

    for (let i = 0; i < numberOfWords; i++) {
      randomWords.push(words[Math.floor(Math.random() * words.length)]);
    }

    clearTimeout(timeout);

    timeout = setTimeout(() => {
      hidePassword = false;
      password = randomWords.join(' ');
    }, 300);
  }

  function copyPassword() {
    navigator.clipboard.writeText(password);
  }

  onMount(generatePassword);
</script>

<h1>Password Generator, but with actual words</h1>

<div class="button">
  <button on:click={generatePassword}>Generate</button>
</div>

<label>
  {numberOfWords}
  {numberOfWords === 1 ? 'word' : 'words'}
  <input type="range" bind:value={numberOfWords} on:input={generatePassword} min="1" max="10" />
</label>

<label>
  Length of words
  <select bind:value={lengthOfWords} on:input={() => setTimeout(generatePassword)}>
    <option value="medium">Medium (5-8 letters)</option>
    <option value="long">Long (9+ letters)</option>
  </select>
</label>

<div class="password">
  <span class:hidden={hidePassword} on:click={copyPassword} on:focus={copyPassword} tabindex="0" title="Copy">{password}</span>
</div>

<footer>
  <small>
    &copy; {new Date().getFullYear()} Vojta | Colors mostly from
    <a href="https://tailwindcss.com/docs/customizing-colors#color-palette-reference">Tailwind</a> | Why?
    <a href="https://xkcd.com/936/">xkcd 936</a>
  </small>
</footer>

<style>
  :root {
    --purple: #9333ea;
    --purple-gradient: linear-gradient(to right, #5b21b6, #9333ea);
    --green: #10b981;
    --blue: #60a5fa;
  }

  ::selection {
    background-color: var(--purple);
  }

  :global(body) {
    height: 100vh;
    margin: 0;
    display: flex;
    justify-content: center;
    text-align: center;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
    background-color: black;
    color: white;
  }

  :global(#app) {
    width: 100vw;
  }

  h1 {
    font-size: 3rem;
  }

  .button {
    margin: 6rem auto 0;
    max-width: fit-content;
    position: relative;
    background: var(--purple-gradient);
    padding: 0.5rem;
    border-radius: 2rem;
  }

  button {
    font-size: 1.5rem;
    padding: 1.5rem 2rem;
    border-radius: 1.5rem;
    background-color: black;
    border: none;
    text-transform: uppercase;
    cursor: pointer;
    color: inherit;
    user-select: none;
  }

  button:hover {
    background-image: var(--purple-gradient);
  }

  label {
    display: block;
  }

  input {
    margin-top: 2rem;
  }

  .password {
    margin-top: 8rem;
    position: relative;
  }

  .password span {
    display: inline-block;
    font-size: 2rem;
    font-weight: bold;
    border: 0.5rem solid var(--green);
    border-radius: 2rem;
    padding: 1.5rem 2rem;
    transition: background-color 200ms;
    user-select: all;
    line-height: 3rem;
    cursor: text;
    min-width: 30rem;
    height: 3rem;
  }

  .password span.hidden {
    color: transparent;
  }

  .password span:hover,
  .password span:focus,
  .password span.hidden {
    background-color: var(--green);
    transition: background-color 50ms;
  }

  footer {
    position: absolute;
    bottom: 1rem;
    width: 100%;
  }

  footer a {
    color: var(--blue);
    text-decoration: none;
  }
</style>
