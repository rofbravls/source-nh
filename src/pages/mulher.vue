<template>
  <main>
    <menuNH/>
    <menuMob/>
    <transition name="fade">
      <sklGeral v-if="loading.geral" />
    </transition>

    <transition name="fade">
      <div v-if="!loading.geral">
        <!-- <section class="bannerDefault" v-if="banner != null">
          <div class="">
            <img :src="banner.dimensoes[screenWidth < 770 ? 'mobile' : 'desktop'][tipoImg.banner]" alt="">
          </div>
        </section> -->
        <bannerProduto/>
        <pitchbar class="pitchbarHome" />
        <produtosEscolha/>


        <section id="infos">
          <div class="container">
            <div class="row">
              <div class="col-12">
                <div class="infos-nav">
                  <button type="button" name="button" @click="infos = 1" :class="{itemInfosActiver: infos == 1}">
                    DESCRIÇÃO
                  </button>
                  <button type="button" name="button" @click="infos = 2" :class="{itemInfosActiver: infos == 2}">
                    FORMAS DE ENTREGA
                  </button>
                  <button type="button" name="button" @click="infos = 3" :class="{itemInfosActiver: infos == 3}">
                    INFORMAÇÕES NUTRICIONAIS
                  </button>
                </div>
              </div>
              <div class="col-12">
                <div class="infos-texts">
                  <p v-if="infos == 1">
                    Oi, eu sou o New Hair! O blend de vitaminas número 1 do Brasil.<br v-if="screenWidth > 767">
                    Composto por minerais, aminoácidos, vitaminas e antioxidantes,  o New Hair combate a queda capilar em até 96%. Além disso promove um crescimento acelerado dos fios em até 6x mais rápido e o fortalecimento das unhas.
                    <br><br>
                    Com apenas uma cápsula ao dia, é possível notar resultados incríveis na hidratação, brilho e comprimento dos seus fios.
                  </p>
                  <p v-if="infos == 2">
                    Os pedidos são despachados no prazo de 24h úteis após a confirmação do pagamento e aprovação da compra.<br>
                    Os envios são feitos através do Correios, empresa parceira da NEW HAIR.<br><br>
                    O prazo é calculado no momento da compra e é contado após a postagem do pedido, que varia de acordo com o local de entrega.<br>
                    O prazo médio de entrega é de 03 a 05 dias úteis.
                  </p>
                  <div v-if="infos == 3" class="">
                    <img class="tabelaNutriImg" id="tabelaNutri" style="opacity:0.8;" src="//d2az8otjr0j19j.cloudfront.net/templates/001/174/785/twig/static/images/tabela-new-hair-caps.jpg?1142248343">
                    <br><br>
                    <p>
                      (*) Valores Diários com base em uma dieta de 2.000 kcal ou 8.400 kJ. Seus valores diários podem ser maiores ou menores<br v-if="screenWidth > 767"> dependendo de suas necessidades energéticas.<br v-if="screenWidth <= 767">
                      1(uma) cápsula equivale a 500mg de vitaminas e minerais.<br v-if="screenWidth <= 767">
                      <br><br>
                      Valores Diários não estabelecidos. Produto dispensado de registro no Ministério da Saúde de acordo com RDC 240/2018.
                    </p>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </section>

        <blend/>
        <gummies/>
        <!-- <section id="bannerMeio" class="bannerDefault" v-if="bannerMeio">
          <router-link to="/produtos" class="bannerMeio-link">
            <img :src="bannerMeio[0].dimensoes[screenWidth < 770 ? 'mobile' : 'desktop'][tipoImg.banner]" alt="">
          </router-link> -->
        <bannerDesafio/>
        <comentarios/>
        <compraSegura/>

      </div>
    </transition>
  </main>
</template>

<script>
import $ from 'jquery';
import Comunicacao from '@/comunicacao.js'
import {mapState, mapGetters} from 'vuex'

export default {
  components: {
    menuNH: () => import('@/components/gerais/menuNH.vue'),
    menuMob: () => import('@/components/gerais/menuMob.vue'),
    sklGeral: () => import('@/components/skeletons/home.vue'),
     produtosEscolha: () => import("@/components/gerais/produtosEscolha.vue"),
     blend: () => import("@/components/gerais/blend.vue"),
     bannerDesafio: () => import("@/components/gerais/bannerDesafio.vue"),
     comentarios: () => import("@/components/gerais/comentarios.vue"),
     compraSegura: () => import("@/components/gerais/compraSegura.vue"),
     gummies: () => import("@/components/gerais/gummies.vue"),
     bannerProduto: () => import("@/components/gerais/bannerProduto.vue"),
    pitchbar: () => import('@/components/gerais/pitchbar.vue'),
  },
  data(){
    return {
      banner: null,
      bannerMeio: null,
      vitrines: [],
      loading: {
        geral: true
      },
      answer: [],
      listagemData: null,
      menu: null,
      infos: 1,
      boxCompraCarrinho: {
        tipo: "produto",
        itens: [
          {
            idProduto: "37",
            idAtributoSimples: "0",
            idUnidadeVenda: "0",
            idArmazem: "0",
            quantidade: "1",
            adicional: "",
            parametros: ""
          }
        ]
      }
    }
  },
  computed: {
    ...mapState({
      screenWidth: state => state.screenWidth,
      depoimentos: state => state.depoimentos.data
    }),
    ...mapGetters(['tipoImg'])
  },
  watch: {
    listagemData(newVal) {
      this.$requestSend('get', `/v2/front/showcase/banners/banner-categoria?idNivel=${newVal.categoria.id}&limit=1`, {}, (success, response) => {
        if (success) {
          this.banner = response.data.banners[0];
        }
      });
    }
  },
  methods: {
    addCartBoxCompra() {
      Comunicacao.$emit('toggleLoader', true);
      let {tipo, itens} = this.boxCompraCarrinho;
      let itemAdc       = false
      if (tipo && itens) {
        //ADICIONA O PRODUTO AO CARRINHO
        this.$carrinho.add(tipo,itens,(success, response) => {

          response.data.itens.forEach(prod => {
            itemAdc = itemAdc ? itemAdc : prod.hash.idProduto == this.boxCompraCarrinho.itens[0].idProduto;
          });
          //PRODUTO ADICIONADO COM SUCESSO
          if(success) return Comunicacao.$emit('toggleCarrinho', {show: true, data: response.data });
          //PROBLEMAS AO ADICIONAR PRODUTO
          alert('Um problema impediu a adição do produto ao carrinho. Tente novamente mais tarde.');
        });
        return;
      }
    },
    openAnswer(index) {
      this.$set(this.answer, index, !this.answer[index])
    },
    genPath(path, onlyQuery){
      if (onlyQuery) {
        path = path.replace(/\/.*(\?.*)/g, '$1');
        return path;
      }
      path = path.replace('?', '&');
      return path;
    },
  },
  created(){
    this.$requestSend('get', `/v2/front/url/product/listing/category?url=${this.genPath(this.$route.fullPath)}&offset=0&limit=1`, {}, (success, response) => {
      if (success) {
        this.$aplicaSEO(response.data.estrutura);
        this.listagemData = response.data.conteudo;

        this.loading.geral = false;

        //VARIÁVEIS PÚBLICAS
        Comunicacao.$emit('paginaCarregada', response.data);
      }
    });

    this.$requestSend('get', '/v2/front/struct/menus/categorias', {}, (success, response) => {
      this.menu = success ? response.data.menu : false;
    });

    this.$requestSend('get', '/v2/front/showcase/products/escolha-tratamento', {}, (success, response) => {
      this.vitrines[0] = success ? response.data : false;
    });
    this.$requestSend('get', '/v2/front/showcase/products/mulher', {}, (success, response) => {
      this.vitrines[1] = success ? response.data : false;
    });
    this.$requestSend('get', '/v2/front/showcase/banners/meio-home', {}, (success, response) => {
      this.bannerMeio = success ? response.data.banners : false;
    });
  },
  mounted(){
    $(document).ready(function(){
      function clickaCaps(){
        var clickando = setTimeout(() =>{
            $('#btn2').click();
            clearTimeout(clickando);
        },2000);
        
      }
      clickaCaps();
    })
  }
}

</script>

<style>
#app > div:nth-child(5) > main > div > section.gummies > div > div > div.col-sm-5 > div > img{
    position: absolute;
    top: -120px;
    left: 150px;
    max-width: 95%;
}
#acertaBlend{
  position:static !important; 
}
#acertaBlend > div:nth-child(3) > img{
  top:0 !important;
}
</style>
<style lang="scss" scoped>
section.blend{
  margin:0px auto !important;
}
  /* MENU DE CATEGORIAS */
  #menuCategorias{
    margin-top: 45px
  }
  .menuCategorias-container{
    display: flex;
    justify-content: space-between;
  }
  .menuCategorias-item{
    width: 48%;
  }
  .menuCategorias-item a{
    width: 100%;
    text-align: center;
    font-size: 14px;
    font-weight: 700;
    color: #666666;
    border: 1px solid #ccc;
    height: 40px;
    border-radius: 7px;
    display: flex;
    justify-content: center;
    align-items: center;
    text-transform: uppercase;
  }
  .menuCategorias-item .router-link-exact-active{
    background-color: #EEE;
  }

  @media (max-width: 767px) {
    #menuCategorias {
      margin-top: 0;
    }
  }

  /* BOX DE COMPRA */
  .boxCompra-container{
    box-shadow: 0px 10px 30px rgb(171 171 171 / 60%);
    border: 1px solid #ddd;
    display: flex;
  }
  .boxCompra-container > div{
    width: 33.333%;
  }
  .boxCompra-container-left{
    padding-left: 30px;
    background-color: #f8f8f8;
    padding-right: 30px;
  }
  .boxCompra-container-left-top{
    margin: 0 -30px;
    display: flex;
    align-items: center;
    margin-bottom: 25px;
  }
  .boxCompra-container-left-top-desconto{
    display: inline-block;
    padding: 15px 15px 15px 15px;
    font-weight: bold;
    font-size: 17px;
    line-height: 17px;
    color: #fff;
    background-color: #42C7A1;
  }
  .boxCompra-container-left-top-text{
    margin-left: 10px;
  }
  .boxCompra-container-left-top-text-t1{
    font-size: 16px;
    font-weight: 900;
    font-family: sans-serif;
    line-height: 1.1
  }
  .boxCompra-container-left-top-text-t2{
    font-size: 13px;
    color: #888888;
  }
  .boxCompra-container-left-middle{
    text-align: center;
    margin-bottom: 25px;
  }
  .boxCompra-container-left-bottom-text{
    font-size: 13px;
    margin-bottom: 13px;
    color: #444;
    line-height: 1.5;
  }
  .boxCompra-container-center{
    position: relative;
    padding: 0 15px;
    left: 3%;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }
  .boxCompra-container-center-selo{
    position: absolute;
    left: 0;
    top: 20px;
  }
  .boxCompra-container-center-img2{
    margin-top: -86px;
  }
  .boxCompra-container-right{
    padding: 0 15px;
  }
  .vitrineDefault-container-item-bottom{
    flex: 1 1 auto;
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    align-items: center;
    padding-top: 20px;
    text-align: center;
  }
  .vitrineDefault-container-item-bottom-precoDe{
    font-size: 20px;
    text-decoration: line-through;
    color: #888;
    line-height: 1;
    margin-bottom: 5px;
  }
  .vitrineDefault-container-item-bottom-precoPor{
    font-size: 20px;
    color: #888;
    line-height: 1;
  }
  .vitrineDefault-container-item-bottom-precoPor b{
    color: #000;
  }
  .vitrineDefault-container-item-bottom-ou{
    margin-top: 10px;
    margin-bottom: 5px;
  }
  .vitrineDefault-container-item-bottom-condicao{
    font-size: 20px;
  }
  .vitrineDefault-container-item-bottom-bt{
    background: #04b900;
    color: #fff;
    font-size: 15px;
    font-weight: bold;
    padding: 20px 50px 20px 50px;
    border-radius: 5px;
    letter-spacing: 0px;
    transition: all 0.3s ease;
    margin-top: 10px;
    font-family: 'Open sans', sans-serif;
  }
  .vitrineDefault-container-item-bottom-bt div{
    min-height: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .vitrineDefault-container-item-bottom-frete{
    margin: 20px 0 14px;
  }


@media (max-width: 767px) {
  .boxCompra-container {
    flex-direction: column;
  }
  .boxCompra-container > div {
    width: 100%;
  }
  .boxCompra-container-center{
    left: 0;
  }
  .boxCompra-container-center-selo{
    left: 20px;
    max-width: 70px;
  }
  .tabelaNutriImg{
    max-width: 100% !important;
  }
}

  /* INFORMAÇÕES */
  .infos-nav{
    display: flex;
  }
  .infos-nav button {
    width: 33.333%;
    text-align: center;
    display: flex;
    height: 50px;
    justify-content: center;
    align-items: center;
    font-size: 15px;
    color: #eb57c0;
    font-weight: 700;
    border: 1px solid #eee;
  }
  .infos-nav button.itemInfosActiver,
  .infos-nav button:hover{
    background-color: #ff25ad;
    color: #fff;
  }

  .infos-texts{
    font-size: 14px;
    padding: 40px 30px 25px;
    color: #656565;
  }
  @media (max-width: 767px) {
    .infos-nav{
      flex-direction: column;
    }
    .infos-nav button {
      width: 100%;
    }
  }

  /* PITCHBAR */
  .pitchbarHome{
    margin-top: 10px;
  }

  /* BANNER DO MEIO DA PÁGINA */
  @media (max-width: 767px) {
    .bannerMeio-link{
      width: 100%;
    }
  }

  /* VITRINE */
  .vitrineDefault{
    margin-top: 45px;
  }
  /* ===================================================== */

  /* DEPOIMENTOS DOS CONSUMIDORES */
  #depoimentos{
    margin-top: 45px;
    margin-bottom: 70px;
  }
  .depoimentos-item{
    margin-bottom: 10px;
  }
  .depoimentos-item-top{
    display: flex;
    align-items: center;
  }
  .depoimentos-item-top-user{
    margin-right: 20px;
    flex: 0 0 auto;
  }
  .depoimentos-item-top-dados{
    flex: 1 1 auto;
  }
  .depoimentos-item-top-dados-nome{
    font-size: 16px;
    font-weight: 700;
    color: #828282;
    margin-bottom: 5px;
    line-height: 1;
  }
  .depoimentos-item-top-dados-nacionalidade{
    margin-bottom: 5px;
  }
  .depoimentos-item-top-dados-avaliacao img{
    margin-right: 5px;
  }
  .depoimentos-item-top-data{
    flex: 0 1 auto;
    color: #999;
    font-size: 12px;
    align-self: flex-start;
    font-weight: 700;
  }
  .depoimentos-item-depoimento{
    margin-top: 10px;
  }
  .depoimentos-item-depoimento-t1{
    font-size: 17px;
    font-weight: 700;
    color: #3e3e3e;
    margin-bottom: 17px;
    line-height: 1.17;
  }
  .depoimentos-item-depoimento-text{
    font-size: 15px;
    color: #828282;
    padding-bottom: 15px;
    line-height: 1.3;
  }
  .depoimentos-item:not(:last-child) .depoimentos-item-depoimento-text{
    border-bottom: 2px solid #e2e2e2;
  }

  .depoimentos-obs{
    font-size: 11px;
    margin-top: 30px;
    color: #999;
  }

  @media (max-width: 767px) {
    .depoimentos-item{
      padding: 0 10px;
    }
    .depoimentos-item-top-data{
      align-self: center;
    }
  }

  .bannerDefault {
    min-height: 450px;
    background-color: #DDE8EE;
    background-repeat: no-repeat;
    background-position: -600%;
    background-size: 80%;
    background-image: linear-gradient(90deg, #dee9ee 0%, #d3e0e8 50%, #dee9ee 100%);
    animation: loading infinite 1.5s;
    max-width: 1920px;
    margin: 0 auto;
  }
</style>
