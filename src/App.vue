<script setup>
import SignIn from './components/SignIn.vue'
import { createClient } from '@supabase/supabase-js'
import { SupabaseAuthClient } from '@supabase/supabase-js/dist/module/lib/SupabaseAuthClient'

const SUPABASE_URL = 'https://bzciqcbnmsgtsuflbcjy.supabase.co'
const SUPABASE_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImJ6Y2lxY2JubXNndHN1ZmxiY2p5Iiwicm9sZSI6ImFub24iLCJpYXQiOjE2NjMzMzcyNDEsImV4cCI6MTk3ODkxMzI0MX0.b52EyXMgTnXNPGjWmEKp5z34Ezx31LVHgbDLTh8kvb8'
const supabase = createClient(SUPABASE_URL, SUPABASE_KEY)

supabase.auth.onAuthStateChange((event, session) => { 
  if(session==null){ 
    document.getElementById('status').innerHTML='You are not logged !!!'; 
  } else{ 
    //alert('session value: ' + JSON.stringify(session)) 
    document.getElementById('status').innerHTML='You are logged with the email: ' + session.user.email; 
  } 
})

//this method allows to release the connexion with the Google account 
async function logout(){ 
      try { 
        const { user, session, error } = await supabase.auth.signOut(); 
        if (error) throw error; 
        document.getElementById('status').innerHTML='You are disconnected !' 
      } catch (error) { 
        alert(error.error_description || error.message); 
      }  
    }
    //this method allows to log in the system using Google provider 
     
  async function login(){ 
      try { 
        const { user, session, error } = await supabase.auth.signIn({ 
          provider: 'github', 
        }); 
        if (error) throw error; 
      } catch (error) { 
        alert(error.error_description || error.message); 
      }  
    }

</script>

<template>    
  <h1>{{ msg }}</h1> 
    <p> 
      Please login if you have an account or register : 
    </p> 
    <button @click="login()">Sign In</button><br> 
    <button @click="logout()">Sign Out</button><br> 
    <label id="status">You are not yet logged !  </label> 
</template>



<style>
@import './assets/base.css';

header .hidden {
    visibility: hidden;
    overflow: hidden;
    display: flex;
    display:inline-block;
    place-items: flex-start;
    flex-wrap: wrap;
}

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;

  font-weight: normal;
}

header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

a,
.green {
  text-decoration: none;
  color: hsla(160, 100%, 37%, 1);
  transition: 0.4s;
}

@media (hover: hover) {
  a:hover {
    background-color: hsla(160, 100%, 37%, 0.2);
  }
}

@media (min-width: 1024px) {
  body {
    display: flex;
    place-items: center;
  }

  #app {
    display: grid;
    grid-template-columns: 1fr 1fr;
    padding: 0 2rem;
  }

  header {
    display: inline-block;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  header .wrapper {
    display: flex;
    display:inline-block;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  .logo {
    margin: 0 2rem 0 0;
  }
}
</style>
