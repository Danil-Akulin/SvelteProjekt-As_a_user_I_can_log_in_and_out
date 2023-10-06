<script>
    import { goto, stores } from '@sapper/app';
    import { post } from 'utils.js';
    const { session } = stores();
    let username = '';
    let name = '';
    let password = '';
    let error = null;
    async function submit(event) {
        const response = await post(`auth/register`, { username, name, password });
        // TODO handle network errors
        error = response.error;
        if (response.id) {
            goto('/');
        }
    }
</script>

<svelte:head>
    <title>Sign up • Conduit</title>
</svelte:head>

<div class="auth-page">
    <div class="container page">
        <div class="row">
            <div class="col-md-6 offset-md-3 col-xs-12">
                <h1 class="text-xs-center">Sign up</h1>
                <p class="text-xs-center">
                    <a href="/login">Have an account?</a>
                </p>


                   {#if error}
                       <div class="alert alert-danger" role="alert">{error}</div>
                   {/if}


                <form on:submit|preventDefault={submit}>
                    <fieldset class="form-group">
                        <input class="form-control form-control-lg" type="text" required placeholder="Your Name" bind:value={username}>
                    </fieldset>
                    <fieldset class="form-group">
                        <input class="form-control form-control-lg" type="name" required placeholder="Name" bind:value={name}>
                    </fieldset>
                    <fieldset class="form-group">
                        <input class="form-control form-control-lg" type="password" required placeholder="Password" bind:value={password}>
                        {#if password.length > 1 && password.length < 6}
                            <sup><div class="alert-danger" role="alert">Password too short</div></sup>
                        {/if}
                    </fieldset>
                    <button class="btn btn-lg btn-primary pull-xs-right" disabled="{password.length < 6}">
                        Sign up
                    </button>
                </form>
            </div>
        </div>
    </div>
</div>
