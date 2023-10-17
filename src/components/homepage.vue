<!-- <script setup>

</script> -->

<template>

    <div class="bg-animation">
        <div id="stars"></div>
        <div id="stars2"></div>
        <div id="stars3"></div>
        <div id="stars4"></div>
    </div>

    <div class="form-wrapper">
        <h2>Shorten Url</h2>
        <div>
            <label for="url">Input Url</label>
            <input type="text" v-model="link" id="url" name="url" placeholder="enter url here">
            <button @click="getNewUrl">Minify</button>
        </div>
        <p>Shorten url here:</p>
        <p id="shortenedurl">{{ newLink }}</p>
    </div>
</template>



<style scoped>
    .form-wrapper h2,
    .form-wrapper p {
        color: #fff;
    }
    .form-wrapper {
        box-shadow: 0 0 0 #000;
        background: linear-gradient(to left top, rgba(0, 0, 0, .5), rgba(255, 255, 255, .4));
        padding: 25px 30px;
        border-radius: 24px;
        width: 25%;
        text-align: center;
        position: relative;
        z-index: 1;
    }

    @media (max-width: 1199px) {
        .form-wrapper {
            width: 50%;
        }
    }

    @media (max-width: 450px) {
        .form-wrapper {
            width: 80%;
        }
    }

    label {
        display: block;
        text-align: left;
        font-size: 16px;
    }

    input {
        display: block;
        width: 95%;
        border: none;
        height: 35px;
        margin: 10px 0;
        border-radius: 55px;
        padding: 0 0.5rem;
        box-shadow: rgb(0 0 0 / 16%) 0px 1px 4px;
        background: rgb(255 255 255 / 15%);
        color: #fff;
        border: 1px solid transparent;
        font-size: 14px;
    }
    input::placeholder {
        color: #fff;
    }
   label {
        color: #fff;
    }

    input:focus {
        outline: none;
        border: 1px solid #fff;
    }

   button {
        display: block;
        margin: 0 auto;
        border-radius: 25px;
        padding: 12px 40px;
        background-color: rgb(255 255 255 / 6%);
        color: #fff;
        border: 1px solid #fff;
      
        margin-top: 2rem;
        transition: 0.2s all;
        font-family: 'Poppins', sans-serif;
        cursor: pointer;
        transition: 0.5s all ease-in-out;
    }

    button:hover {
        background: #fff;
        color: #000;
    }
</style>

<script>
// import ClipboardJS from 'clipboard';
export default {
	data() {
		return {
			link: '',
            newLink: '',
            token: 'a40b9a55c8b16c3e833ff71be14a020388ad3f2f',
        }
    },
    methods: {
        getNewUrl() {
			if(this.link.length === 0) {
               return;
            }
            const options = {
				method: "POST",
                headers: { 
                    "Authorization": `Bearer ${this.token}`,
                    "Content-Type": "application/json" 
                },
                body: JSON.stringify({ "long_url": this.link, "domain": "bit.ly", "group_guid": this.groupID })
            }
            fetch('https://api-ssl.bitly.com/v4/shorten', options)
            .then(response => response.json())
            .then(data => {
                if(data.message === "INVALID_ARG_LONG_URL") {
					this.errorMessage = "Invalid URL to shorten !";
                    return;
                }
                
                const url = data;
                this.newLink = url.link;
                this.exist = true;
                this.link = '';
            })
            .catch(error => {
				console.error('There was an error!', error);
            });
        }
    }
	
}
</script>