<script>






export let cardBind;




</script>




    
    <a class="card-bind "  href="/card/{cardBind.id}">
        <div class="card-bind__wrap sber">
            <div class="card-bind__img">
                <img class="card-bind__logo" src="img/pay/sbermobilelogo.png" alt="">
                <img class="card-bind__name" src="img/pay/visa.png" alt="">
            </div>
            <div class="card-bind__text">
                <p>**** {cardBind.cardnumber.slice(12)} </p>
                <p>{cardBind.ccmonth} / {cardBind.ccyear}</p>
            </div>
        </div>
    </a>
    



<style>
a{
    text-decoration: none;
}


</style>