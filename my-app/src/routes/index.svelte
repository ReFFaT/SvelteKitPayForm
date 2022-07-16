
<script>
import "../assets/css/index.css"
import Usercard from "../components/pay/paycomponents/Usercard.svelte";
import { cards } from "../components/pay/Cards";
import AddCard from "../components/pay/paycomponents/AddCard.svelte";
import Verifed from "../components/pay/paycomponents/Verifed.svelte";
import { t, locale, locales } from "../internationalization/i18n";
	
	// Create a locale specific timestamp
  $: time = new Date().toLocaleDateString($locale, {
    weekday: "long",
    year: "numeric",
    month: "long",
    day: "numeric",
  });



let cardnumber="";
let ccmonth="";
let ccyear="";
let CVVCVC="";
let credit_mindValue= false;
let yearerr;
let montherr;
let errValue='';
function saveCard(){
        let error=CCchek();
        if(error== false){
            return false;
        }
        
            if (credit_mindValue){
                let cardDate=
                {
                    id:$cards.length+1,
                    cardnumber: String(cardnumber.replace(/\s/g, '')),
                    ccmonth:ccmonth,
                    ccyear:ccyear,
                    CVVCVC:CVVCVC,
                }
                    console.log(cardDate);
                cards.update(card => {
                return [...card, cardDate ]
            })
        }
        else{
            window.location.href='https://www.youtube.com/';
        } 
    
}

function prettify() {
        cardnumber =cardnumber.replace(/[^\dA-Z]/g, '').replace(/(.{4})/g, '$1 ').trim();
        }

function CCchek (){
    let numyear=Number(ccyear);
    let nummoth=Number(ccmonth);
    const date2 = new Date;
    if(ccmonth.length< 2){
        ccmonth= '0'+ccmonth;
    }
    console.log(yearerr, montherr, numyear,nummoth, date2.getMonth()+1,date2.getFullYear()%100);
    console.log(date2.getMonth()+1)
    if(nummoth>12 ){
        errValue='Невозможное значение месяца!!!'
        return false;
    }
    if(numyear< date2.getFullYear()%100 ){
        errValue='Карта устарела!!!'
        return false;
    }
    if(numyear<= (date2.getFullYear()%100) && nummoth< (date2.getMonth()+1) ){
        errValue='Карта устарела!!!'
        return false;
    }
    return true;
}




</script>
    <svelte:head>
        <head>
            <link href="http://fonts.cdnfonts.com/css/bambino-2" rel="stylesheet">
        </head>
    </svelte:head>

    <p>
        <select bind:value={$locale}>
        {#each locales as l}
            <option value={l}>{l}</option>
        {/each}
        </select>
    </p>

<div class="wrapper">
    <h2>{$t('homepage.title')}</h2>
    <h3>{$t('homepage.price')}</h3>
    <section class="cards">
        {#each $cards as cardBind (cardBind.id)}
        <AddCard  cardBind={cardBind}/>
        {/each}
        
        <Usercard {t}/>
    </section>
        
    <section>
        <form id="form"  on:submit|preventDefault={saveCard}>
            <div class="card">
                <div class="card__wrapper">
                    <div class="card-front">
                        <div class="card-front__banks">
                            <img class="card-front__bank-1" src="img/pay/mir.png" alt="">
                            <img class="card-front__bank-2" src="img/pay/visa1.png" alt="">
                            <img class="card-front__bank-3" src="img/pay/Mastercard.png" alt="">
                            <img class="card-front__bank-4" src="img/pay/maestro.png" alt="">
                            <img class="card-front__bank-5" src="img/pay/american_express.png" alt="">
                            <img class="card-front__bank-6" src="img/pay/union.jpg" alt="">
                        </div>
                        <div class="card-front__num">
                            <label class="" for="">{$t('homepage.cardnum-1')}</label>
                            <input maxlength="19" id="cardNum" pattern="[0-9]*\s[0-9]*\s[0-9]*\s[0-9]*"  class="card-front__digit inputmode"
                            required autocomplete="cc-number" type="text" bind:value={cardnumber} on:input={prettify}>
                            <label  class=""  for="">{$t('homepage.cardnum-2')}</label>
                        </div>
                        <div class="card-front__date">
                            <label class=" card-front__text"  for=""> {$t('homepage.cardnum-3')}</label>
                            <div class="card-front__date-wrap">
                                <input minlength="1" maxlength="2"  bind:this={montherr} bind:value={ccmonth} class="card-front__month inputmode card-front__date-st "
                                pattern="[0-9]*" required type="text" placeholder="MM" >
                                
                                <label class=""  for=""> / </label>
                                <input maxlength="2" id="ccyear" bind:value={ccyear} bind:this={yearerr} class="card-front__year inputmode card-front__date-st" pattern="[0-9]{'{2}'}"
                                required  autocomplete="cc-exp-year" type="text" placeholder="ГГ">
                                <p> {errValue}</p>
                            </div>
                            
                        </div>
                    </div>
                    <div class="card-back">
                        <div class="card-back__line"></div>
                        <div class="card-back__cvv">
                            <label  for="">CVV/CVC</label>
                            <input required maxlength="3" bind:value={CVVCVC}  class="card-back__digital inputmode "
                            pattern="[0-9]{'{3}'}" type="password" placeholder="000">
                            <label  for="">{$t('homepage.cardback')}</label>
                        </div>
                    </div>
                </div>
                <div class="card__save">
                    <div class="card__save-check">
                        <div><input type="checkbox" bind:checked={credit_mindValue}></div><div><p>{$t('homepage.cardcheck-1')}</p><img src="img/pay/icon.png" alt="">. <p>{$t('homepage.cardcheck-2')} <a href=" ">{$t('homepage.cardcheck-3')}</a>.</p></div>
                    </div>
                </div>
                <div class="card__button">
                    <button id="btn" type="submit">{ $t('homepage.button')}</button>
                </div>
            </div>
        </form>
    </section>
    <Verifed />
</div>
    
    
    
    <style>
    .card-front__date-wrap p{
        margin: 0;
        text-align: center;
        font-family: "Bambino-Light", sans-serif;
        font-size: 16px;
        line-height: 18px;
        padding: 4px 0 0 4px;
        color:rgba(245, 12, 12, 0.767);
    }
    </style>