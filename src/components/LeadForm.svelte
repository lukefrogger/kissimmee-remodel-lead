<script >
    import { fly, fade } from 'svelte/transition';
    import querystring from "query-string";
    let showModal = false;

   async function submitForm(formData) {

        var postData = querystring.stringify({
            'email': formData.target.elements.email.value,
            'firstname': formData.target.elements.firstName.value,
            'lastname': formData.target.elements.lastName.value,
            'phone':  formData.target.elements.phone.value
        });
        
        let resp = await fetch('https://forms.hubspot.com/uploads/form/v2/6498274/f429f3c6-1f8d-4805-a928-2e715b898717', {
            method: 'POST',
            mode: 'no-cors',
            headers: {
                'Content-Type': 'application/x-www-form-urlencoded'
            },
            body: postData
        });

        if(resp.status == 0 || resp.status == 204) {
            showModal = true;
        }
        setTimeout(() => document.location.reload(), 2000)
   }
</script>

<div class="form ">
    <div class="has-text-centered">
        <h3 class="is-size-4 field">Franklin Remodeling and Construction</h3>
        <p class="has-text-grey	">Fill out the form below to recieve your $250 coupon. Someone will be in contact with you to quote your project. Only 6 coupons left!</p>
        <br>
    </div>
    <form on:submit|preventDefault={submitForm}>
        <div class="field">
            <label>First Name</label>
            <input class="input" name="firstName" type="text"  required/>
        </div>
        <div class="field">
            <label>Last Name</label>
            <input class="input" name="lastName" type="text" required/>
        </div>
        <div class="field">
            <label>Email</label>
            <input class="input" name="email" type="email" required/>
        </div>
        <div class="field">
            <label>Phone Number</label>
            <input class="input" name="phone" type="text" required/>
        </div>
        <button class="button is-primary"  type="submit">Get A Quote!</button>
    </form>
</div>


{#if showModal}
<div class="modal is-active" transition:fade>
  <div class="modal-background" transition:fade></div>
  <div class="modal-content has-text-centered" transition:fade>
    <div class="box">
        <h4 class="is-size-4">Thank you for submitting the form!</h4>
    </div>
  </div>
</div>
{/if}