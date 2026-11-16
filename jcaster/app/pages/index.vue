<template>
  <v-container fluid class="pa-0 home-page">
    <!-- HERO -->
    <section class="hero-section">
      <v-container class="hero-inner py-14 py-md-16">
        <v-row align="center" class="ga-0">
          <v-col cols="12" md="7">
            <v-chip color="white" variant="outlined" class="mb-5">
              <v-icon start icon="mdi-star-four-points" />
              YOUR EVERYDAY MARKETPLACE
            </v-chip>
            <h1 class="hero-title mb-5">
              Find your next<br class="d-none d-md-block" />
              <span>everyday favorite.</span>
            </h1>
            <p class="hero-copy mb-8">
              Discover useful essentials, fresh finds, and products you'll love
              — all in one simple place.
            </p>
            <div class="d-flex flex-wrap ga-3">
              <v-btn
                href="#featured"
                color="white"
                size="large"
                rounded="lg"
                class="text-teal-darken-4 font-weight-bold"
                append-icon="mdi-arrow-down"
              >
                Explore products
              </v-btn>
              <v-btn
                to="/products"
                color="white"
                variant="outlined"
                size="large"
                rounded="lg"
              >
                Manage store
              </v-btn>
            </div>
            <div class="hero-points d-flex flex-wrap ga-6 mt-10">
              <div><strong>Easy</strong><span>to explore</span></div>
              <div><strong>Fresh</strong><span>product picks</span></div>
              <div><strong>Simple</strong><span>shopping</span></div>
            </div>
          </v-col>
          <v-col cols="12" md="5" class="d-none d-md-flex justify-center">
            <div class="hero-art">
              <div class="hero-orbit orbit-one"></div>
              <div class="hero-orbit orbit-two"></div>
              <v-icon icon="mdi-shopping-outline" size="150" color="white" />
              <div class="floating-tag tag-top">
                <v-icon icon="mdi-heart" color="pink" />
                <span>Made for you</span>
              </div>
              <div class="floating-tag tag-bottom">
                <v-icon icon="mdi-package-variant-closed-check" color="teal" />
                <span>Great finds</span>
              </div>
            </div>
          </v-col>
        </v-row>
      </v-container>
    </section>

    <!-- QUICK BENEFITS -->
    <v-container class="benefits-wrap">
      <v-row>
        <v-col cols="12" sm="4">
          <div class="benefit-item">
            <v-avatar color="teal-lighten-5" size="48"
              ><v-icon color="teal-darken-2" icon="mdi-magnify"
            /></v-avatar>
            <div>
              <strong>Easy discovery</strong><span>Find what you need</span>
            </div>
          </div>
        </v-col>
        <v-col cols="12" sm="4">
          <div class="benefit-item">
            <v-avatar color="orange-lighten-5" size="48"
              ><v-icon color="deep-orange" icon="mdi-heart-outline"
            /></v-avatar>
            <div>
              <strong>Personal picks</strong><span>Save your favorites</span>
            </div>
          </div>
        </v-col>
        <v-col cols="12" sm="4">
          <div class="benefit-item">
            <v-avatar color="blue-lighten-5" size="48"
              ><v-icon color="blue-darken-2" icon="mdi-cart-outline"
            /></v-avatar>
            <div>
              <strong>Simple cart</strong><span>Keep items together</span>
            </div>
          </div>
        </v-col>
      </v-row>
    </v-container>

    <!-- SEARCH + CART -->
    <v-container class="pt-8 pb-2">
      <div class="section-topline">
        <div>
          <p class="eyebrow mb-1">START EXPLORING</p>
          <h2 class="section-title">What are you looking for?</h2>
        </div>
        <v-btn
          variant="tonal"
          color="teal-darken-3"
          rounded="lg"
          @click="cartDrawer = true"
        >
          <v-icon start icon="mdi-cart-outline" />
          My cart
          <v-badge
            v-if="cartItemCount"
            :content="cartItemCount"
            color="deep-orange"
            inline
          />
        </v-btn>
      </div>
      <v-text-field
        v-model="searchQuery"
        class="search-field mt-5"
        placeholder="Search products, categories..."
        prepend-inner-icon="mdi-magnify"
        variant="solo"
        rounded="lg"
        clearable
        hide-details
      />
    </v-container>

    <!-- CATEGORIES -->
    <v-container class="py-8">
      <div class="d-flex align-center justify-space-between mb-4">
        <div>
          <p class="eyebrow mb-1">SHOP YOUR WAY</p>
          <h2 class="section-title">Browse categories</h2>
        </div>
        <v-btn
          variant="text"
          color="teal-darken-3"
          @click="selectedCategory = 'All'"
        >
          View all
        </v-btn>
      </div>
      <div class="category-list">
        <v-chip
          :color="selectedCategory === 'All' ? 'teal-darken-3' : undefined"
          :variant="selectedCategory === 'All' ? 'flat' : 'outlined'"
          size="large"
          @click="selectedCategory = 'All'"
          >All products</v-chip
        >
        <v-chip
          v-for="category in categories"
          :key="category"
          :color="selectedCategory === category ? 'teal-darken-3' : undefined"
          :variant="selectedCategory === category ? 'flat' : 'outlined'"
          size="large"
          @click= "selectedCategory = category"
          >{{ category }}</v-chip
        >
      </div>
    </v-container>

    <!-- FEATURED PRODUCTS -->
    <v-container id="featured" class="pb-14">
      <div class="d-flex align-end justify-space-between mb-5">
        <div>
          <p class="eyebrow mb-1">THE LATEST FINDS</p>
          <h2 class="section-title">Featured products</h2>
          <p class="text-medium-emphasis mt-1 mb-0">
            A few good things worth checking out.
          </p>
        </div>
        <v-chip color="teal-darken-3" variant="tonal"
          >{{ filteredProducts.length }} items</v-chip
        >
      </div>

      <div v-if="pending" class="py-16 text-center">
        <v-progress-circular indeterminate color="teal-darken-3" size="48" />
        <p class="text-medium-emphasis mt-4">Loading the collection...</p>
      </div>
      <v-alert v-else-if="error" type="warning" variant="tonal" rounded="lg">
        We couldn't load products right now. Please check your Strapi connection
        and try again.
      </v-alert>
      <v-card
        v-else-if="filteredProducts.length === 0"
        variant="tonal"
        color="grey-lighten-4"
        rounded="xl"
        class="pa-10 text-center"
      >
        <v-icon icon="mdi-package-variant" size="54" color="grey" />
        <h3 class="text-h6 mt-3">No products found</h3>
        <p class="text-medium-emphasis mb-4">
          Try another keyword or choose a different category.
        </p>
        <v-btn color="teal-darken-3" variant="outlined" @click="resetFilters"
          >Clear filters</v-btn
        >
      </v-card>
      <v-row v-else>
        <v-col
          v-for="product in filteredProducts"
          :key="product.documentId"
          cols="12"
          sm="6"
          md="4"
          lg="3"
        >
          <v-card class="product-card" rounded="xl" elevation="0" height="100%">
            <div class="product-image-wrap">
              <v-img
                v-if="getImageUrl(product.image)"
                :src="getImageUrl(product.image)"
                height="220"
                cover
              >
                <template #placeholder>
                  <div class="image-placeholder">
                    <v-progress-circular indeterminate color="teal" />
                  </div>
                </template>
              </v-img>
              <div v-else class="image-placeholder">
                <v-icon icon="mdi-image-outline" size="58" color="grey" />
              </div>
              <v-btn
                icon
                size="small"
                elevation="2"
                class="wishlist-button"
                :color="isWishlisted(product) ? 'red' : 'white'"
                @click.stop="toggleWishlist(product)"
              >
                <v-icon :color="isWishlisted(product) ? 'white' : 'red'">
                  {{
                    isWishlisted(product) ? "mdi-heart" : "mdi-heart-outline"
                  }}
                </v-icon>
              </v-btn>
              <v-chip
                v-if="product.stock <= 5 && product.stock > 0"
                color="orange-darken-2"
                size="small"
                class="stock-label"
                >Low stock</v-chip
              >
              <v-chip
                v-else-if="product.stock <= 0"
                color="grey-darken-2"
                size="small"
                class="stock-label"
                >Out of stock</v-chip
              >
            </div>
            <v-card-text class="pa-4">
              <div
                class="text-caption text-teal-darken-2 font-weight-bold text-uppercase mb-1"
              >
                {{ product.category || "General" }}
              </div>
              <h3 class="product-name">{{ product.name }}</h3>
              <p class="product-description">
                {{
                  product.description ||
                  "A great addition to your everyday essentials."
                }}
              </p>
              <div class="d-flex align-center justify-space-between mt-4">
                <span class="product-price">{{
                  formatPrice(product.price)
                }}</span>
                <span class="text-caption text-medium-emphasis"
                  >{{ product.stock }} available</span
                >
              </div>
            </v-card-text>
            <v-card-actions class="px-4 pb-4 pt-0">
              <v-btn
                block
                color="teal-darken-3"
                rounded="lg"
                :disabled="product.stock <= 0"
                @click="addToCart(product)"
              >
                <v-icon start icon="mdi-cart-plus" /> Add to cart
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
    </v-container>

    <!-- PROMO BANNER -->
    <v-container class="pb-14">
      <section class="promo-banner">
        <div>
          <p class="eyebrow text-teal-lighten-3 mb-2">A BETTER WAY TO BROWSE</p>
          <h2>Good finds for<br />everyday living.</h2>
          <p class="mb-5">
            Take a look around and discover something useful today.
          </p>
          <v-btn
            href="#featured"
            color="white"
            class="text-teal-darken-4"
            rounded="lg"
            append-icon="mdi-arrow-right"
          >
            Explore the collection
          </v-btn>
        </div>
        <v-icon class="promo-icon" icon="mdi-shopping" />
      </section>
    </v-container>

    <!-- NEWSLETTER -->
    <v-container class="pb-14">
      <v-card class="newsletter-card pa-6 pa-md-10" rounded="xl" elevation="0">
        <v-row align="center">
          <v-col cols="12" md="6">
            <p class="eyebrow mb-2">STAY IN THE LOOP</p>
            <h2 class="section-title mb-2">
              A little inspiration in your inbox.
            </h2>
            <p class="text-medium-emphasis mb-0">
              Get updates about new arrivals and featured finds.
            </p>
          </v-col>
          <v-col cols="12" md="6">
            <v-form @submit.prevent="subscribeNewsletter">
              <v-text-field
                v-model="newsletterEmail"
                label="Email address"
                type="email"
                variant="outlined"
                rounded="lg"
                bg-color="white"
                prepend-inner-icon="mdi-email-outline"
                :rules="emailRules"
                required
              />
              <v-btn
                type="submit"
                block
                color="teal-darken-3"
                size="large"
                rounded="lg"
                >Subscribe</v-btn
              >
            </v-form>
            <p class="text-caption text-medium-emphasis mt-2 mb-0">
              Demo subscription form — no email is sent.
            </p>
          </v-col>
        </v-row>
      </v-card>
    </v-container>

    <!-- CART DRAWER -->
    <v-navigation-drawer
      v-model="cartDrawer"
      location="right"
      temporary
      width="400"
      class="cart-drawer"
    >
      <div class="d-flex align-center justify-space-between pa-4">
        <div>
          <p class="eyebrow mb-1">YOUR SELECTION</p>
          <h2 class="text-h5 font-weight-bold">Shopping cart</h2>
        </div>
        <v-btn icon="mdi-close" variant="text" @click="cartDrawer = false" />
      </div>
      <v-divider />
      <div v-if="cart.length === 0" class="text-center pa-8">
        <v-icon icon="mdi-cart-outline" size="68" color="grey-lighten-1" />
        <h3 class="mt-4">Your cart is empty</h3>
        <p class="text-medium-emphasis mt-2">
          Add a product and it will appear here.
        </p>
        <v-btn class="mt-3" color="teal-darken-3" @click="cartDrawer = false"
          >Continue browsing</v-btn
        >
      </div>
      <div v-else class="pa-4">
        <v-card
          v-for="item in cart"
          :key="item.documentId"
          variant="outlined"
          rounded="lg"
          class="mb-3"
        >
          <v-card-text>
            <v-row align="center">
              <v-col cols="4">
                <v-img
                  :src="getImageUrl(item.image) || undefined"
                  height="82"
                  cover
                  rounded="lg"
                >
                  <template #placeholder
                    ><div class="image-placeholder">
                      <v-icon icon="mdi-image-outline" /></div
                  ></template>
                </v-img>
              </v-col>
              <v-col cols="8">
                <div class="d-flex justify-space-between align-start">
                  <strong class="cart-item-name">{{ item.name }}</strong>
                  <v-btn
                    icon="mdi-delete-outline"
                    color="error"
                    size="small"
                    variant="text"
                    @click="removeFromCart(item.documentId)"
                  />
                </div>
                <div class="font-weight-bold text-teal-darken-3 mt-1">
                  {{ formatPrice(item.price * item.quantity) }}
                </div>
                <div class="d-flex align-center mt-2">
                  <v-btn
                    icon="mdi-minus"
                    size="x-small"
                    variant="outlined"
                    :disabled="item.quantity <= 1"
                    @click="decreaseQuantity(item.documentId)"
                  />
                  <span class="mx-3 font-weight-bold">{{ item.quantity }}</span>
                  <v-btn
                    icon="mdi-plus"
                    size="x-small"
                    variant="outlined"
                    :disabled="item.quantity >= item.stock"
                    @click="increaseQuantity(item.documentId)"
                  />
                </div>
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
        <v-divider class="my-4" />
        <div class="d-flex justify-space-between mb-2">
          <span>Items</span><strong>{{ cartItemCount }}</strong>
        </div>
        <div class="d-flex justify-space-between mb-4">
          <span class="text-h6 font-weight-bold">Subtotal</span
          ><strong class="text-h6 text-teal-darken-3">{{
            formatPrice(cartTotal)
          }}</strong>
        </div>
        <v-btn
          block
          variant="outlined"
          color="error"
          class="mb-3"
          @click="clearCart"
          >Clear cart</v-btn
        >
        <v-btn block color="teal-darken-3" disabled>Checkout coming soon</v-btn>
      </div>
    </v-navigation-drawer>

    <v-snackbar v-model="showMessage" :color="messageColor" timeout="2500">{{
      message
    }}</v-snackbar>
  </v-container>
</template>

<script setup lang="ts">
interface Product {
  id: number;
  documentId: string;
  name: string;
  description?: string;
  price: number;
  stock: number;
  category?: string;
  is_active: boolean;
  image?: any;
}
interface CartItem extends Product {
  quantity: number;
}

const config = useRuntimeConfig();
const STRAPI_URL = config.public.strapiUrl;

useHead({ title: "NovaMart | Everyday Finds" });

const searchQuery = ref("");
const selectedCategory = ref("All");
const cartDrawer = ref(false);
const cart = ref<CartItem[]>([]);
const wishlist = ref<string[]>([]);
const newsletterEmail = ref("");
const showMessage = ref(false);
const message = ref("");
const messageColor = ref("success");

const {
  data: response,
  pending,
  error,
} = await useFetch<{ data: Product[] }>(`${STRAPI_URL}/api/shop-karts`, {
  lazy: true,
  query: {
    populate: "image",
    sort: "createdAt:desc",
    "pagination[pageSize]": 100,
    "filters[is_active][$eq]": true,
  },
});

const products = computed(() => response.value?.data ?? []);
const categories = computed<string[]>(() => {
  return [
    ...new Set(
      products.value
        .map(product => product.category)
        .filter((category): category is string => Boolean(category))
    )
  ]
})
const filteredProducts = computed(() => {
  const keyword = searchQuery.value.toLowerCase().trim();
  return products.value
    .filter((product) => product.is_active)
    .filter((product) => {
      const matchesSearch =
        !keyword ||
        product.name?.toLowerCase().includes(keyword) ||
        product.description?.toLowerCase().includes(keyword) ||
        product.category?.toLowerCase().includes(keyword);
      const matchesCategory =
        selectedCategory.value === "All" ||
        product.category === selectedCategory.value;
      return matchesSearch && matchesCategory;
    })
    .slice(0, 8);
});
const cartItemCount = computed(() =>
  cart.value.reduce((sum, item) => sum + item.quantity, 0),
);
const cartTotal = computed(() =>
  cart.value.reduce((sum, item) => sum + item.price * item.quantity, 0),
);

function notify(text: string, color = "success") {
  message.value = text;
  messageColor.value = color;
  showMessage.value = true;
}
function resetFilters() {
  searchQuery.value = "";
  selectedCategory.value = "All";
}
function getImageUrl(image: any): string {
  const imageData = Array.isArray(image) ? image[0] : image;
  if (!imageData?.url) return "";
  const imageUrl =
    imageData.formats?.medium?.url ||
    imageData.formats?.small?.url ||
    imageData.url;
  return imageUrl.startsWith("http://") || imageUrl.startsWith("https://")
    ? imageUrl
    : `${STRAPI_URL}${imageUrl}`;
}
function formatPrice(price: number): string {
  return new Intl.NumberFormat("en-PH", {
    style: "currency",
    currency: "PHP",
  }).format(Number(price || 0));
}
function addToCart(product: Product) {
  if (!product.is_active || product.stock <= 0)
    return notify("This product is unavailable.", "error");
  const existing = cart.value.find(
    (item) => item.documentId === product.documentId,
  );
  if (existing) {
    if (existing.quantity >= product.stock)
      return notify("Maximum available stock reached.", "warning");
    existing.quantity++;
  } else {
    cart.value.push({ ...product, quantity: 1 });
  }
  notify(`${product.name} added to cart!`);
}
function increaseQuantity(id: string) {
  const item = cart.value.find((entry) => entry.documentId === id);
  if (item && item.quantity < item.stock) item.quantity++;
}
function decreaseQuantity(id: string) {
  const item = cart.value.find((entry) => entry.documentId === id);
  if (item && item.quantity > 1) item.quantity--;
}
function removeFromCart(id: string) {
  cart.value = cart.value.filter((item) => item.documentId !== id);
}
function clearCart() {
  cart.value = [];
}
function toggleWishlist(product: Product) {
  const id = product.documentId;
  if (wishlist.value.includes(id)) {
    wishlist.value = wishlist.value.filter((item) => item !== id);
    notify(`${product.name} removed from wishlist.`);
  } else {
    wishlist.value.push(id);
    notify(`${product.name} added to wishlist!`);
  }
}
function isWishlisted(product: Product) {
  return wishlist.value.includes(product.documentId);
}

const emailRules = [
  (value: string) => !!value || "Email is required.",
  (value: string) =>
    /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value) || "Enter a valid email address.",
];
function subscribeNewsletter() {
  if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(newsletterEmail.value)) {
    return notify("Please enter a valid email address.", "error");
  }
  notify(
    "Thanks for your interest! Newsletter integration can be added later.",
  );
  newsletterEmail.value = "";
}
</script>

<style scoped>
.home-page {
  background: #fbfdfc;
  color: #172b29;
}
.hero-section {
  color: white;
  overflow: hidden;
  background:
    radial-gradient(
      circle at 82% 20%,
      rgba(94, 234, 212, 0.32),
      transparent 28%
    ),
    linear-gradient(118deg, #073b38 0%, #0f766e 58%, #159b8d 100%);
}
.hero-title {
  font-size: clamp(2.7rem, 5vw, 4.7rem);
  line-height: 1.04;
  letter-spacing: -2px;
  font-weight: 800;
}
.hero-title span {
  color: #a7f3d0;
}
.hero-copy {
  max-width: 570px;
  color: rgba(255, 255, 255, 0.84);
  font-size: 1.12rem;
  line-height: 1.8;
}
.hero-points div {
  display: flex;
  flex-direction: column;
  gap: 2px;
}
.hero-points strong {
  font-size: 1rem;
}
.hero-points span {
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.78rem;
}
.hero-art {
  width: 330px;
  height: 330px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.24);
  box-shadow: 0 25px 80px rgba(0, 0, 0, 0.15);
}
.hero-orbit {
  position: absolute;
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 50%;
}
.orbit-one {
  inset: 25px;
}
.orbit-two {
  inset: -22px;
}
.floating-tag {
  position: absolute;
  display: flex;
  align-items: center;
  gap: 8px;
  background: white;
  color: #173b37;
  padding: 12px 16px;
  border-radius: 14px;
  box-shadow: 0 12px 35px rgba(0, 0, 0, 0.16);
  font-size: 0.85rem;
  font-weight: 700;
}
.tag-top {
  top: 36px;
  right: -34px;
}
.tag-bottom {
  bottom: 38px;
  left: -42px;
}
.benefits-wrap {
  margin-top: -1px;
  padding-top: 26px;
  padding-bottom: 22px;
  border-bottom: 1px solid #e7efec;
}
.benefit-item {
  display: flex;
  align-items: center;
  gap: 13px;
}
.benefit-item div {
  display: flex;
  flex-direction: column;
}
.benefit-item strong {
  font-size: 0.92rem;
}
.benefit-item span {
  color: #71817d;
  font-size: 0.8rem;
}
.eyebrow {
  color: #0f766e;
  font-size: 0.7rem;
  letter-spacing: 1.5px;
  font-weight: 800;
}
.section-title {
  font-size: clamp(1.45rem, 2vw, 1.9rem);
  font-weight: 800;
  letter-spacing: -0.6px;
}
.section-topline {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 16px;
}
.search-field :deep(.v-field) {
  border: 1px solid #e2ece8;
  box-shadow: 0 8px 25px rgba(16, 70, 60, 0.05);
}
.category-list {
  display: flex;
  flex-wrap: wrap;
  gap: 9px;
}
.product-card {
  border: 1px solid #e7efec;
  overflow: hidden;
  transition:
    transform 0.2s ease,
    box-shadow 0.2s ease;
}
.product-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 16px 35px rgba(15, 75, 65, 0.1) !important;
}
.product-image-wrap {
  position: relative;
  background: #f0f5f3;
  overflow: hidden;
}
.image-placeholder {
  height: 220px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #f0f5f3;
}
.wishlist-button {
  position: absolute;
  top: 12px;
  right: 12px;
  z-index: 2;
}
.stock-label {
  position: absolute;
  left: 12px;
  top: 12px;
}
.product-name {
  font-size: 1rem;
  font-weight: 750;
  line-height: 1.35;
  min-height: 2.7em;
}
.product-description {
  color: #788681;
  font-size: 0.82rem;
  line-height: 1.5;
  min-height: 3em;
  margin-top: 8px;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
.product-price {
  color: #0f766e;
  font-size: 1.15rem;
  font-weight: 800;
}
.promo-banner {
  background: linear-gradient(115deg, #092f2d, #0f766e);
  color: white;
  border-radius: 24px;
  padding: clamp(28px, 5vw, 55px);
  position: relative;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: space-between;
  min-height: 260px;
}
.promo-banner h2 {
  font-size: clamp(2rem, 4vw, 3rem);
  line-height: 1.08;
  font-weight: 800;
  letter-spacing: -1px;
}
.promo-banner p:not(.eyebrow) {
  color: rgba(255, 255, 255, 0.78);
  max-width: 480px;
}
.promo-icon {
  font-size: 150px;
  color: rgba(167, 243, 208, 0.22);
  margin-right: 5%;
}
.newsletter-card {
  background: #eaf7f2;
  border: 1px solid #d8eee6;
}
.cart-drawer {
  max-width: 100vw;
}
.cart-item-name {
  max-width: 180px;
  overflow-wrap: anywhere;
}
@media (max-width: 600px) {
  .hero-title {
    letter-spacing: -1px;
  }
  .section-topline {
    align-items: flex-start;
  }
  .promo-icon {
    display: none;
  }
  .benefit-item {
    padding: 5px 0;
  }
}
</style>
