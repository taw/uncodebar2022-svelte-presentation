# Svelte

---

# Overview

## Better than React

---

# Satisfaction

![satisfaction](survey-2021-satisfaction.png)

---

# Popularity

![usage](survey-2021-usage.jpg)

---

# Don't be stuck in the past

- React in the new jQuery
- jQuery 2006
- React 2013
- many React decisions made sense back then, did not age well
- nobody uses just React
- one React stack is so different from another React stack that it's not that much more work to learn a new framework

---

# Svelte

- familiar design - components, child components, props, state
- more like plain HTML, CSS, and JavaScript
- a lot faster without virtual DOM
- more common features included like scoped CSS, stores, transitions

---

# Hello.svelte

    !html
    <div>Hello, World!</div>

---

# Scoped CSS built in

## You write:

    !html
    <div>Hello, World!</div>

    <style>
      div { text-align: center; }
    </style>

### Browser sees:

    !html
    <div class="svelte-bl172v">Hello, World!</div>

### And:

    !css
    div.svelte-bl172v { text-align:center; }

---

# State

    !html
    <script>
      let count = 0

      function handleClick() {
        count += 1
      }
    </script>

    <button on:click={handleClick}>
      Clicked {count} {count === 1 ? 'time' : 'times'}
    </button>

---

# Reactive properties

    !html
    <script>
      let count = 1

      // the `$:` means 're-run whenever these values change'
      $: doubled = count * 2

      function handleClick() {
        count += 1
      }
    </script>

    <button on:click={handleClick}>Count: {count}</button>
    <p>{count} * 2 = {doubled}</p>

---

# Reactive statements

    !html
    <script>
      let count = 0

      $: if (count >= 10) {
        alert("count is dangerously high!")
        count = 9
      }

      function handleClick() {
        count += 1
      }
    </script>

    <button on:click={handleClick}>
      Clicked {count} {count === 1 ? 'time' : 'times'}
    </button>

---

# Child Components

## Parent.svelte

    !html
    <script>
      import Child from './Child.svelte'
    </script>

    <Child answer={42}/>

## Child.svelte

    !html
    <script>
      export let answer
    </script>

    <p>The answer is {answer}</p>

---

# {#if}

    !html
    <script>
      let user = { loggedIn: false }

      function toggle() {
        user.loggedIn = !user.loggedIn
      }
    </script>

    {#if user.loggedIn}
      <button on:click={toggle}>
        Log out
      </button>
    {:else}
      <button on:click={toggle}>
        Log in
      </button>
    {/if}

---

# {#each}

    !html
    <script>
      let cats = [
        { id: 'J---aiyznGQ', name: 'Keyboard Cat' },
        { id: 'z_AbfPXTKms', name: 'Maru' },
        { id: 'OUtn3pvWmpg', name: 'Henri The Existential Cat' }
      ]
    </script>

    <h1>The Famous Cats of YouTube</h1>

    <ul>
      {#each cats as { id, name }, i}
        <li>
          <a target="_blank" href="https://www.youtube.com/watch?v={id}">
            {i + 1}: {name}
          </a>
        </li>
      {/each}
    </ul>

---

# bind:

    !html
    <script>
      let name = ''
    </script>

    <input bind:value={name} placeholder="enter your name">
    <p>Hello {name || 'stranger'}!</p>

---

# bind:

    !html
    <script>
      let a = 1
      let b = 2
    </script>

    <label>
      <input type=number bind:value={a} min=0 max=10>
      <input type=range bind:value={a} min=0 max=10>
    </label>

    <label>
      <input type=number bind:value={b} min=0 max=10>
      <input type=range bind:value={b} min=0 max=10>
    </label>

    <p>{a} + {b} = {a + b}</p>

---

# @html

    !html
    <script>
      import { marked } from 'marked'
      let text = 'Some words are *italic*, some are **bold**'
    </script>

    <textarea bind:value={text}></textarea>

    {@html marked(text)}

    <style>
      textarea { width: 100%; height: 200px; }
    </style>

---

# bind: parent

    !html
    <script>
      import Keypad from './Keypad.svelte'

      let pin
      $: view = pin ? pin.replace(/\d(?!$)/g, '•') : 'enter your pin'

      function handleSubmit() {
        alert(`submitted ${pin}`)
      }
    </script>

    <h1 style="color: {pin ? '#333' : '#ccc'}">{view}</h1>

    <Keypad bind:value={pin} on:submit={handleSubmit}/>

---

# bind: child

    !html
    <script>
      import { createEventDispatcher } from 'svelte'
      export let value = ''
      const dispatch = createEventDispatcher()
      const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9]
      const select = num => () => value += num
      const clear  = () => value = ''
      const submit = () => dispatch('submit')
    </script>

    <div class="keypad">
      {#each numbers as i}
        <button on:click={select(i)}>{i}</button>
      {/each}
      <button disabled={!value} on:click={clear}>clear</button>
      <button on:click={select(0)}>0</button>
      <button disabled={!value} on:click={submit}>submit</button>
    </div>

---

# Questions?

## https://svelte.dev/tutorial

## https://svelte.dev/repl

---

# Generated code - create

    !javascript
    c() {
      button = element("button")
      t0 = text("Clicked ")
      t1 = text(/*count*/ ctx[0])
      t2 = space()
      t3_value = (/*count*/ ctx[0] === 1 ? 'time' : 'times') + ""
      t3 = text(t3_value)
    }

---

# Generated code - mount

    !javascript
    m(target, anchor) {
      insert(target, button, anchor)
      append(button, t0)
      append(button, t1)
      append(button, t2)
      append(button, t3)

      if (!mounted) {
        dispose = listen(button, "click", /*handleClick*/ ctx[1])
        mounted = true
      }
    }

---

# Generated code - handleClick

    !javascript
    function instance($$self, $$props, $$invalidate) {
      let count = 0;

      function handleClick() {
        $$invalidate(0, count += 1);
      }

      return [count, handleClick];
    }

---

# Generated code - update

    !javascript
    p(ctx, [dirty]) {
      if (dirty & /*count*/ 1) set_data(t1, /*count*/ ctx[0])
      if (dirty & /*count*/ 1 && t3_value !== (t3_value = (/*count*/ ctx[0] === 1 ? 'time' : 'times') + "")) set_data(t3, t3_value)
    }

---

# Generated code - destroy

    !javascript
    d(detaching) {
      if (detaching) detach(button)
      mounted = false
      dispose()
    }

---

# Questions?

<style>
  img {
    max-width: 100%;
    max-height: 500px;
    /* max-height: 100%; */
  }
  pre {
    white-space: pre-wrap;
  }
</style>
