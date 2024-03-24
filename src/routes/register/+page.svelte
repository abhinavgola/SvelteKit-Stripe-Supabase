<script>
  import { supabase } from '$lib/supabase/subabaseClient';
  
  let form = {
    name: '',
    age: '',
    location: '',
    height: '',
    intro: '',
    job: '',
    education: '',
    linkedInVerified: false,
    hobbies: ''
  };
  
  async function handleSubmit() {
    const { data, error } = await supabase.from('registrations').insert([form]);
    if (error) console.error('Error saving to Supabase:', error);
    else console.log('Saved to Supabase:', data);
  }
</script>

<style>
  form {
    max-width: 400px;
    margin: 0 auto;
    padding: 20px;
    border-radius: 8px;
    background-color: #f9f9f9;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  }

  input[type="text"],
  input[type="number"],
  textarea,
  button {
    width: 100%;
    margin-bottom: 10px;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    box-sizing: border-box;
    font-size: 16px;
    color: #333; /* Text color */
  }

  input[type="checkbox"] {
    margin-right: 5px;
    vertical-align: middle;
  }

  label {
    display: block;
    margin-bottom: 10px;
    color: #333; /* Label text color */
  }

  button[type="submit"] {
    background-color: #007bff; /* Button color */
    color: #fff; /* Button text color */
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }

  button[type="submit"]:hover {
    background-color: #0056b3; /* Button color on hover */
  }
</style>

<form on:submit|preventDefault={handleSubmit}>
  <input type="text" bind:value={form.name} placeholder="Name">
  <input type="number" bind:value={form.age} placeholder="Age">
  <input type="text" bind:value={form.location} placeholder="Location">
  <input type="number" bind:value={form.height} placeholder="Height (in cm)">
  <textarea bind:value={form.intro} placeholder="Intro"></textarea>
  <input type="text" bind:value={form.job} placeholder="Job">
  <input type="text" bind:value={form.education} placeholder="Education">
  <label>
    <input type="checkbox" bind:checked={form.linkedInVerified}>
    <span>Is LinkedIn verified?</span>
  </label>
  <input type="text" bind:value={form.hobbies} placeholder="Hobbies">
  <button type="submit">Register</button>
</form>
