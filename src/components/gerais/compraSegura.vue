<template>
    <section class="compraSegura">
      <div class="container">
        <div class="head_text" style="text-align:center;">
            <h1 style="margin-bottom:0px;color:#404040;">AQUI SUA COMPRA É<br class="mob"> 100% SEGURA</h1>
            <p style="color:#666666; font-size:15px;">Não se preocupe, comprar em nosso site é seguro <br class="mob">
            e  totalmente confiável</p>
        </div>
        <div class="row" style="justify-content:center !important;">
            <div class="col-md-5" style="background-color:#eff6e9; border:1px solid #afd493; padding: 12px 0;">
                <div class="row" style="align-items: center; justify-content:center;">
                    <div class="col-2">
                        <img src="@/assets/img/cofre1.png" alt="">
                    </div>
                    <div class="col-8">
                        <h6>Dados Pessoais</h6>
                        <p>São totalmente sigilosos e não são <br>
                        compartilhados.</p>
                    </div>
                </div><!--row-->
            </div>

            <div class="col-md-5" style="background-color:#eff6e9; border:1px solid #afd493; padding: 12px 0;">
                <div class="row" style="align-items: center; justify-content:center;">
                    <div class="col-2">
                        <img src="@/assets/img/safe1.png" alt="">
                    </div>
                    <div class="col-8">
                        <h6>Site Confiável</h6>
                        <p>Monitorado todo o tempo por empresas
                        de segurança digital.</p>
                    </div>
                </div><!--row-->
            </div>

        </div><!--row-->

        <div class="row" style="justify-content:center !important;">
            <div class="col-md-5" style="background-color:#eff6e9; border:1px solid #afd493; padding: 12px 0;">
                <div class="row" style="align-items: center; justify-content:center;">
                <div class="col-2">
                    <img src="@/assets/img/locker1.png" alt="">
                </div>
                <div class="col-8">
                    <h6>Dados Financeiros</h6>
                    <p>Gravados em ambiente 100% seguro.</p>
                </div>
                </div>
            </div>
            <div class="col-md-5" style="background-color:#eff6e9; border:1px solid #afd493; padding: 12px 0;">
                <div class="row" style="align-items: center; justify-content:center;">
                <div class="col-2">
                    <img src="@/assets/img/caderno1.png" alt="">
                </div>
                <div class="col-8">
                    <h6>Entrega Garantida</h6>
                    <p>Logistica eficaz entregas feitas no prazo
                    combinado.</p>
                </div>
                </div>
            </div>
        </div><!--row-->
    </div>
    </section>
</template>


<style lang="scss" scoped>
.compraSegura{
padding-top:40px;
padding-bottom:30px;
h6{
    font-weight:bold;
}
}
.compraSegura h6{
margin: 10px 0px 5px 0px;
color:#60aa28;
font-size:20px;
}
.compraSegura p{
color:#666666;
font-size:14px;
}
.compraSegura .col-8{
margin-left:30px;
}
.compraSegura .col-md-5{
margin: 25px 25px;
}
@media screen and (max-width: 820px){
.compraSegura .col-md-5{
margin: 12px 25px;
}
.head_text h1{
font-size:32px !important;
}
.compraSegura .col-8 p{
font-size:12px !important;
}
.compraSegura .col-8{
margin-left:40px;
}
}
@media screen and (max-width: 768px){
    .compraSegura{
        img{
            max-width: 89px !important;
        }
        .col-md-5{
            .row{
                max-width:100%;
                margin:0 auto;
                overflow: hidden;
            }
        }
    }
}
</style>