<script>
  import { enhance } from '$app/forms';

  let loading = false;
</script>

<section class="basis-full py-5">
  <form
    action=""
    method="POST"
    class="flex flex-col max-w-md mx-auto"
    use:enhance={() => {
      return async ({ formElement }) => {
        loading = true;
        // mimic sending the signup data
        await new Promise((resolve) => setTimeout(resolve, 1000));
        loading = false;
        formElement.reset();
      };
    }}
  >
    <h2 class="text-4xl font-semibold">Register</h2>
    <p class="mb-4 opacity-50">Enter your Info to signup for Pipeline</p>
    <div class="flex flex-col gap-2 font-medium">
      <label for="name" class="block">Name</label>
      <input
        type="text"
        id="name"
        name="name"
        class="w-full border border-black rounded-full py-3 px-4"
        required
      />
    </div>

    <div class="flex flex-col gap-2 mt-4 font-medium">
      <label for="email" class="block">Email</label>
      <input
        type="email"
        id="email"
        name="name"
        class="w-full border border-black py-3 rounded-full px-4"
        required
      />
    </div>

    <div class="flex flex-col gap-2 mt-4 font-medium">
      <label for="password" class="block">Password</label>
      <input
        type="password"
        id="password"
        name="password"
        class="w-full border border-black py-3 rounded-full px-4"
        required
      />
    </div>

    <button
      type="submit"
      class="py-4 mt-8 w-full font-light text-white bg-teal-900 rounded-full"
      disabled={loading}
    >
      {loading ? 'Signing up...' : 'Sign up'}
    </button>

    <label class="flex items-center gap-2 mt-6 text-sm font-light mx-auto">
      <input type="checkbox" name="rememberMe" />
      Remember me
    </label>
  </form>

  <div class="flex flex-col items-center w-full gap-6 mt-10">
    <p class="text-sm text-neutral-400">or continue with</p>

    <div class="flex gap-4 mb-6">
      <button class="p-3 bg-teal-900 rounded-full">
        <img
          src="https://cdn.builder.io/api/v1/image/assets/TEMP/879dd3080c7bd34490beea474587e94f0faf9dc63499622616e98b3997258731?placeholderIfAbsent=true&apiKey=567aaefef2da4f73a3149c6bc21f1ea8"
          alt="Social Icon"
          class="w-6 h-6"
        />
      </button>
      <button class="p-3 bg-teal-900 rounded-full">
        <img
          src="https://cdn.builder.io/api/v1/image/assets/TEMP/1eef60ce512ca366df9b9bbaf2534ef3d07f36cd1005177c9fcb115bc7726767?placeholderIfAbsent=true&apiKey=567aaefef2da4f73a3149c6bc21f1ea8"
          alt="Social Icon"
          class="w-6 h-6"
        />
      </button>
      <button class="p-3 bg-teal-900 rounded-full">
        <img
          src="https://cdn.builder.io/api/v1/image/assets/TEMP/879dd3080c7bd34490beea474587e94f0faf9dc63499622616e98b3997258731?placeholderIfAbsent=true&apiKey=567aaefef2da4f73a3149c6bc21f1ea8"
          alt="Social Icon"
          class="w-6 h-6"
        />
      </button>
      <button class="px-6 py-2 self-center text-sm text-teal-900 rounded-full bg-lime-400">
        Connect wallet
      </button>
    </div>
  </div>
</section>
