<template>
  <div class="product-item">
    <!-- Add click event to call onFavoriteClicked -->
     <!-- Add the dinamic class to active the favorite icon -->
    <span class="icono favorite" :class="{ selected: product.favorite }" @click="onFavoriteClicked"></span>
    <img :src="product.image" :alt="product.title" class="product-image" />
    <h3 class="product-title">{{ product.title }}</h3>
    <p class="product-description">{{ product.description }}</p>
    <p><strong>Price:</strong> ${{ product.price }}</p>
  </div>
</template>

<script>
  import { PRODUCT_FAVORITE_CLICKED_EVENT_NAME } from '@/helpers/constants';

  export default {
    name: 'ProductCard',
    props: ['product'],
    data () {
      return {}
    },
    methods: {
      onFavoriteClicked () {
        // use the constant for the name of the event
        this.$emit(PRODUCT_FAVORITE_CLICKED_EVENT_NAME, this.product.id)
      }
    }
  }
</script>

<style scoped>
.product-item {
  position: relative;
  border: 1px solid #ddd;
  padding: 15px;
  border-radius: 5px;
  text-align: center;
}

.product-image {
  width: 150px;
  height: 150px;
  object-fit: scale-down;
  margin-bottom: 10px;
}

.product-title {
  display: block;
  text-overflow: ellipsis;
  word-wrap: break-word;
  overflow: hidden;
  height: 2em;
  line-height: 1.8em;
}

.product-description {
  display: block;
  text-overflow: ellipsis;
  word-wrap: break-word;
  overflow: hidden;
  height: 3.6em;
  line-height: 1.8em;
}

.favorite {
  position: absolute;
  right: 20px;
  width: 30px;
  height: 30px;
}

span.icono.favorite.selected::before {
  background-image: url("../assets/favorite-filled-red.svg");
}

span.icono.favorite::before {
  background-image: url("../assets/favorite-filled-muted.svg");
}

.favorite:hover {
  filter: drop-shadow(3px 5px 2px rgb(0 0 0 / 0.4));
}

.favorite-icon {
  background-image: url('../assets/favorite-filled-muted.svg');
}

.favorite-icon.selected {
  background-image: url('../assets/favorite-filled-red.svg');
}

span.icono {
  display: inline-block;
}

span.icono::before {
  content: "";
  width: 2.4rem;
  height: 2.4rem;
  display: inline-block;
  background-repeat: no-repeat;
  background-position: center center;
  background-size: contain;
  vertical-align: text-bottom;
}
</style>
