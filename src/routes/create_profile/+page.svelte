<script lang="ts">
    import { enhance, applyAction } from "$app/forms";
    import type { SubmitFunction } from "@sveltejs/kit";
    import { supabase } from '$lib/supabase/subabaseClient';
  
    let loading = false;
    let errorMessage = '';
  
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
  
    const handleSubmit: SubmitFunction = async ({ update, result }) => {
      loading = true;
      errorMessage = '';
  
      try {
        const { data, error } = await supabase.from('registrations').insert([form]);
  
        if (error) {
          console.error('Error saving to Supabase:', error);
          errorMessage = 'An error occurred while saving data.';
        } else {
          console.log('Saved to Supabase:', data);
        }
      } catch (error) {
        console.error('Error saving to Supabase:', error);
        errorMessage = 'An error occurred while saving data.';
      } finally {
        loading = false;
        await update({ reset: false });
        await applyAction(result);
      }
    };
  </script>
  
  <svelte:head>
    <title>Registration Form</title>
  </svelte:head>
  
  <div class="text-center content-center max-w-lg mx-auto min-h-[100vh] pb-12 flex items-center place-content-center">
    <div class="flex flex-col w-64 lg:w-80">
      <div>
        <h1 class="text-2xl font-bold mb-6">Registration Form</h1>
        <form class="form-widget" use:enhance={handleSubmit}>
          <div class="mt-4">
            {#each [
              { label: 'Name', name: 'name', type: 'text' },
              { label: 'Age', name: 'age', type: 'number' },
              { label: 'Location', name: 'location', type: 'text' },
              { label: 'Height (in cm)', name: 'height', type: 'number' },
              { label: 'Intro', name: 'intro', type: 'text' },
              { label: 'Job', name: 'job', type: 'text' },
              { label: 'Education', name: 'education', type: 'text' },
              { label: 'LinkedIn verified', name: 'linkedInVerified', type: 'checkbox' },
              { label: 'Hobbies', name: 'hobbies', type: 'text' }
            ] as { label: string, name: string, type: string }, index}
              <label for="{name}">
                <span class="text-l text-center">{label}</span>
              </label>
              {#if type === 'checkbox'}
                <input type="checkbox" id="{name}" bind:checked={form[name]} class="mt-1 input input-bordered w-full max-w-xs" />
              {:else}
                <input type="{type}" id="{name}" bind:value={form[name]} class="{fieldError(form, name) ? 'input-error' : ''} mt-1 input input-bordered w-full max-w-xs" />
              {/if}
            {/each}
          </div>
          {#if errorMessage}
            <p class="text-red-700 text-sm font-bold text-center mt-3">{errorMessage}</p>
          {/if}
          <div class="mt-4">
            <input type="submit" class="btn btn-primary mt-3 btn-wide" value={loading ? "..." : "Submit"} disabled={loading} />
          </div>
        </form>
  
        <div class="text-sm text-slate-800 mt-14">
          <a href="/">Go back to homepage</a>
        </div>
      </div>
    </div>
  </div>
  