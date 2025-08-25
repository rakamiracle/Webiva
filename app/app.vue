<template>
  <div class="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100">
    <!-- Navigation Header -->
    <nav class="bg-white shadow-lg sticky top-0 z-50">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex justify-between h-16">
          <div class="flex items-center">
            <div class="flex-shrink-0 flex items-center">
              <div class="bg-gradient-to-r from-blue-600 to-purple-600 w-10 h-10 rounded-lg flex items-center justify-center mr-3">
                <i class="fas fa-store text-white text-xl"></i>
              </div>
              <h1 class="text-2xl font-bold bg-gradient-to-r from-blue-600 to-purple-600 bg-clip-text text-transparent">
                WEBIVA
              </h1>
            </div>
            <div class="hidden md:ml-10 md:flex space-x-8">
              <a @click="currentView = 'home'" class="cursor-pointer text-gray-700 hover:text-blue-600 px-3 py-2 font-medium transition-colors">Beranda</a>
              <a @click="currentView = 'products'" class="cursor-pointer text-gray-700 hover:text-blue-600 px-3 py-2 font-medium transition-colors">Produk</a>
              <a @click="currentView = 'categories'" class="cursor-pointer text-gray-700 hover:text-blue-600 px-3 py-2 font-medium transition-colors">Kategori</a>
              <a @click="currentView = 'about'" class="cursor-pointer text-gray-700 hover:text-blue-600 px-3 py-2 font-medium transition-colors">Tentang</a>
            </div>
          </div>
          <div class="flex items-center space-x-4">
            <div class="relative">
              <input v-model="searchQuery" type="text" placeholder="Cari produk..." 
                     class="w-64 px-4 py-2 rounded-lg border border-gray-300 focus:ring-2 focus:ring-blue-500 focus:border-transparent outline-none">
              <i class="fas fa-search absolute right-3 top-3 text-gray-400"></i>
            </div>
            <button @click="showCart = true" class="relative p-2 text-gray-700 hover:text-blue-600 transition-colors">
              <i class="fas fa-shopping-cart text-xl"></i>
              <span v-if="cartCount > 0" class="absolute -top-2 -right-2 bg-red-500 text-white text-xs rounded-full w-5 h-5 flex items-center justify-center">
                {{ cartCount }}
              </span>
            </button>
            <button @click="showLoginModal = true" class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded-lg transition-all hover:scale-105">
              <i class="fas fa-user mr-2"></i>Login
            </button>
            <button @click="currentView = 'admin'" class="bg-purple-600 hover:bg-purple-700 text-white px-4 py-2 rounded-lg transition-all hover:scale-105">
              <i class="fas fa-cog mr-2"></i>Admin
            </button>
          </div>
        </div>
      </div>
    </nav>

    <!-- Main Content -->
    <main class="transition-all duration-500 ease-in-out">
      
      <!-- Home Section -->
      <section v-if="currentView === 'home'" class="animate-fadeIn">
        <!-- Hero Banner -->
        <div class="relative bg-gradient-to-r from-blue-600 to-purple-700 text-white">
          <div class="max-w-7xl mx-auto px-4 py-20 sm:px-6 lg:px-8">
            <div class="text-center">
              <h1 class="text-5xl font-bold mb-6 animate-pulse">
                Selamat Datang di WEBIVA
              </h1>
              <p class="text-xl mb-8 opacity-90">
                Platform E-Commerce Dinamis dengan CMS Terintegrasi
              </p>
              <button @click="currentView = 'products'" class="bg-white text-blue-600 px-8 py-3 rounded-lg font-semibold text-lg hover:bg-gray-100 transition-all hover:scale-105">
                Jelajahi Produk
              </button>
            </div>
          </div>
          <div class="absolute bottom-0 left-0 right-0 h-20 bg-gradient-to-t from-white to-transparent"></div>
        </div>

        <!-- Featured Products -->
        <div class="max-w-7xl mx-auto px-4 py-16 sm:px-6 lg:px-8">
          <h2 class="text-3xl font-bold text-center mb-12 text-gray-800">Produk Unggulan</h2>
          <div class="grid grid-cols-1 md:grid-cols-3 lg:grid-cols-4 gap-6">
            <div v-for="product in featuredProducts" :key="product.id" 
                 class="bg-white rounded-xl shadow-lg hover:shadow-2xl transition-all duration-300 hover:-translate-y-2 overflow-hidden">
              <div class="h-48 bg-gradient-to-br from-gray-200 to-gray-300 flex items-center justify-center">
                <i class="fas fa-image text-4xl text-gray-400"></i>
              </div>
              <div class="p-6">
                <h3 class="font-semibold text-lg mb-2 text-gray-800">{{ product.name }}</h3>
                <p class="text-gray-600 mb-3 text-sm">{{ product.description }}</p>
                <div class="flex justify-between items-center">
                  <span class="text-2xl font-bold text-blue-600">Rp {{ formatPrice(product.price) }}</span>
                  <button @click="addToCart(product)" class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 transition-colors">
                    <i class="fas fa-cart-plus"></i>
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Categories -->
        <div class="bg-gray-100 py-16">
          <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <h2 class="text-3xl font-bold text-center mb-12 text-gray-800">Kategori Populer</h2>
            <div class="grid grid-cols-2 md:grid-cols-4 lg:grid-cols-6 gap-6">
              <div v-for="category in categories" :key="category.id" 
                   @click="filterByCategory(category.id)"
                   class="bg-white p-6 rounded-xl text-center hover:shadow-lg transition-all cursor-pointer hover:scale-105">
                <div class="w-16 h-16 bg-gradient-to-br from-blue-500 to-purple-600 rounded-full flex items-center justify-center mx-auto mb-4">
                  <i :class="category.icon" class="text-2xl text-white"></i>
                </div>
                <h3 class="font-medium text-gray-800">{{ category.name }}</h3>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Products Section -->
      <section v-if="currentView === 'products'" class="animate-fadeIn">
        <div class="max-w-7xl mx-auto px-4 py-8 sm:px-6 lg:px-8">
          <div class="flex justify-between items-center mb-8">
            <h1 class="text-3xl font-bold text-gray-800">Semua Produk</h1>
            <select v-model="sortBy" class="px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500">
              <option value="name">Urutkan: Nama</option>
              <option value="price_low">Harga: Terendah</option>
              <option value="price_high">Harga: Tertinggi</option>
              <option value="newest">Terbaru</option>
            </select>
          </div>
          
          <div class="grid grid-cols-1 md:grid-cols-3 lg:grid-cols-4 gap-6">
            <div v-for="product in filteredProducts" :key="product.id" 
                 class="bg-white rounded-xl shadow-lg hover:shadow-2xl transition-all duration-300 hover:-translate-y-2 overflow-hidden">
              <div class="h-48 bg-gradient-to-br from-gray-200 to-gray-300 flex items-center justify-center">
                <i class="fas fa-image text-4xl text-gray-400"></i>
              </div>
              <div class="p-6">
                <h3 class="font-semibold text-lg mb-2 text-gray-800">{{ product.name }}</h3>
                <p class="text-gray-600 mb-3 text-sm">{{ product.description }}</p>
                <div class="flex justify-between items-center mb-3">
                  <span class="text-2xl font-bold text-blue-600">Rp {{ formatPrice(product.price) }}</span>
                  <span class="text-sm text-gray-500">Stok: {{ product.stock }}</span>
                </div>
                <button @click="addToCart(product)" 
                        :disabled="product.stock === 0"
                        class="w-full bg-blue-600 text-white py-2 rounded-lg hover:bg-blue-700 transition-colors disabled:bg-gray-400">
                  <i class="fas fa-cart-plus mr-2"></i>
                  {{ product.stock > 0 ? 'Tambah ke Keranjang' : 'Stok Habis' }}
                </button>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Categories Section -->
      <section v-if="currentView === 'categories'" class="animate-fadeIn">
        <div class="max-w-7xl mx-auto px-4 py-8 sm:px-6 lg:px-8">
          <h1 class="text-3xl font-bold text-gray-800 mb-8">Kategori Produk</h1>
          <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            <div v-for="category in categories" :key="category.id" 
                 class="bg-white rounded-xl shadow-lg hover:shadow-xl transition-all duration-300 hover:scale-105 overflow-hidden">
              <div class="p-8 text-center">
                <div class="w-20 h-20 bg-gradient-to-br from-blue-500 to-purple-600 rounded-full flex items-center justify-center mx-auto mb-6">
                  <i :class="category.icon" class="text-3xl text-white"></i>
                </div>
                <h3 class="text-xl font-semibold mb-2 text-gray-800">{{ category.name }}</h3>
                <p class="text-gray-600 mb-4">{{ category.description }}</p>
                <button @click="filterByCategory(category.id)" 
                        class="bg-blue-600 text-white px-6 py-2 rounded-lg hover:bg-blue-700 transition-colors">
                  Lihat Produk
                </button>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- About Section -->
      <section v-if="currentView === 'about'" class="animate-fadeIn">
        <div class="max-w-4xl mx-auto px-4 py-16 sm:px-6 lg:px-8">
          <div class="text-center mb-12">
            <h1 class="text-4xl font-bold text-gray-800 mb-6">Tentang WEBIVA</h1>
            <p class="text-xl text-gray-600 leading-relaxed">
              WEBIVA adalah platform e-commerce dinamis yang dirancang dengan teknologi modern untuk memberikan pengalaman berbelanja online terbaik.
            </p>
          </div>
          
          <div class="grid grid-cols-1 md:grid-cols-2 gap-12 mb-12">
            <div class="bg-white p-8 rounded-xl shadow-lg">
              <div class="w-16 h-16 bg-blue-600 rounded-full flex items-center justify-center mb-6">
                <i class="fas fa-rocket text-2xl text-white"></i>
              </div>
              <h3 class="text-2xl font-semibold mb-4 text-gray-800">Teknologi Modern</h3>
              <p class="text-gray-600">
                Dibangun dengan Nuxt.js untuk frontend yang responsif dan Golang untuk backend yang cepat dan handal.
              </p>
            </div>
            
            <div class="bg-white p-8 rounded-xl shadow-lg">
              <div class="w-16 h-16 bg-purple-600 rounded-full flex items-center justify-center mb-6">
                <i class="fas fa-shield-alt text-2xl text-white"></i>
              </div>
              <h3 class="text-2xl font-semibold mb-4 text-gray-800">Keamanan Terjamin</h3>
              <p class="text-gray-600">
                Sistem keamanan berlapis dengan enkripsi data dan otentikasi JWT untuk melindungi informasi pengguna.
              </p>
            </div>
          </div>
          
          <div class="bg-gradient-to-r from-blue-600 to-purple-600 p-8 rounded-xl text-white text-center">
            <h3 class="text-2xl font-semibold mb-4">Hubungi Kami</h3>
            <p class="mb-6">Ada pertanyaan atau butuh bantuan? Tim support kami siap membantu Anda 24/7.</p>
            <div class="flex justify-center space-x-6">
              <a href="mailto:info@webiva.com" class="bg-white text-blue-600 px-6 py-3 rounded-lg hover:bg-gray-100 transition-colors">
                <i class="fas fa-envelope mr-2"></i>Email
              </a>
              <a href="tel:+62123456789" class="bg-white text-blue-600 px-6 py-3 rounded-lg hover:bg-gray-100 transition-colors">
                <i class="fas fa-phone mr-2"></i>Telepon
              </a>
            </div>
          </div>
        </div>
      </section>

      <!-- Admin Section -->
      <section v-if="currentView === 'admin'" class="animate-fadeIn bg-gray-100 min-h-screen">
        <div class="flex">
          <!-- Admin Sidebar -->
          <div class="w-64 bg-gray-800 min-h-screen text-white">
            <div class="p-6 border-b border-gray-700">
              <h2 class="text-xl font-semibold">Admin Panel</h2>
            </div>
            <nav class="mt-6">
              <a @click="adminView = 'dashboard'" class="block px-6 py-3 hover:bg-gray-700 transition-colors cursor-pointer">
                <i class="fas fa-tachometer-alt mr-3"></i>Dashboard
              </a>
              <a @click="adminView = 'products'" class="block px-6 py-3 hover:bg-gray-700 transition-colors cursor-pointer">
                <i class="fas fa-box mr-3"></i>Kelola Produk
              </a>
              <a @click="adminView = 'categories'" class="block px-6 py-3 hover:bg-gray-700 transition-colors cursor-pointer">
                <i class="fas fa-tags mr-3"></i>Kelola Kategori
              </a>
              <a @click="adminView = 'orders'" class="block px-6 py-3 hover:bg-gray-700 transition-colors cursor-pointer">
                <i class="fas fa-shopping-bag mr-3"></i>Pesanan
              </a>
              <a @click="adminView = 'settings'" class="block px-6 py-3 hover:bg-gray-700 transition-colors cursor-pointer">
                <i class="fas fa-cog mr-3"></i>Pengaturan
              </a>
            </nav>
          </div>

          <!-- Admin Content -->
          <div class="flex-1 p-8">
            <!-- Admin Dashboard -->
            <div v-if="adminView === 'dashboard'">
              <h2 class="text-3xl font-bold mb-8 text-gray-800">Dashboard Admin</h2>
              <div class="grid grid-cols-1 md:grid-cols-4 gap-6 mb-8">
                <div class="bg-white p-6 rounded-xl shadow-lg">
                  <div class="flex items-center">
                    <div class="w-12 h-12 bg-blue-600 rounded-full flex items-center justify-center">
                      <i class="fas fa-box text-white"></i>
                    </div>
                    <div class="ml-4">
                      <p class="text-gray-600">Total Produk</p>
                      <p class="text-2xl font-bold text-gray-800">{{ products.length }}</p>
                    </div>
                  </div>
                </div>
                <div class="bg-white p-6 rounded-xl shadow-lg">
                  <div class="flex items-center">
                    <div class="w-12 h-12 bg-green-600 rounded-full flex items-center justify-center">
                      <i class="fas fa-shopping-cart text-white"></i>
                    </div>
                    <div class="ml-4">
                      <p class="text-gray-600">Total Pesanan</p>
                      <p class="text-2xl font-bold text-gray-800">{{ orders.length }}</p>
                    </div>
                  </div>
                </div>
                <div class="bg-white p-6 rounded-xl shadow-lg">
                  <div class="flex items-center">
                    <div class="w-12 h-12 bg-purple-600 rounded-full flex items-center justify-center">
                      <i class="fas fa-tags text-white"></i>
                    </div>
                    <div class="ml-4">
                      <p class="text-gray-600">Total Kategori</p>
                      <p class="text-2xl font-bold text-gray-800">{{ categories.length }}</p>
                    </div>
                  </div>
                </div>
                <div class="bg-white p-6 rounded-xl shadow-lg">
                  <div class="flex items-center">
                    <div class="w-12 h-12 bg-orange-600 rounded-full flex items-center justify-center">
                      <i class="fas fa-dollar-sign text-white"></i>
                    </div>
                    <div class="ml-4">
                      <p class="text-gray-600">Total Penjualan</p>
                      <p class="text-2xl font-bold text-gray-800">Rp {{ formatPrice(totalSales) }}</p>
                    </div>
                  </div>
                </div>
              </div>
            </div>

            <!-- Admin Products -->
            <div v-if="adminView === 'products'">
              <div class="flex justify-between items-center mb-6">
                <h2 class="text-3xl font-bold text-gray-800">Kelola Produk</h2>
                <button @click="showAddProduct = true" class="bg-blue-600 text-white px-6 py-2 rounded-lg hover:bg-blue-700 transition-colors">
                  <i class="fas fa-plus mr-2"></i>Tambah Produk
                </button>
              </div>
              
              <div class="bg-white rounded-xl shadow-lg overflow-hidden">
                <table class="w-full">
                  <thead class="bg-gray-50">
                    <tr>
                      <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Nama</th>
                      <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Kategori</th>
                      <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Harga</th>
                      <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Stok</th>
                      <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Aksi</th>
                    </tr>
                  </thead>
                  <tbody class="divide-y divide-gray-200">
                    <tr v-for="product in products" :key="product.id" class="hover:bg-gray-50">
                      <td class="px-6 py-4">{{ product.name }}</td>
                      <td class="px-6 py-4">{{ getCategoryName(product.category_id) }}</td>
                      <td class="px-6 py-4">Rp {{ formatPrice(product.price) }}</td>
                      <td class="px-6 py-4">{{ product.stock }}</td>
                      <td class="px-6 py-4">
                        <button class="text-blue-600 hover:text-blue-800 mr-3">
                          <i class="fas fa-edit"></i>
                        </button>
                        <button class="text-red-600 hover:text-red-800">
                          <i class="fas fa-trash"></i>
                        </button>
                      </td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        </div>
      </section>
    </main>

    <!-- Cart Modal -->
    <div v-if="showCart" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
      <div class="bg-white rounded-xl p-8 max-w-2xl w-full mx-4 max-h-[80vh] overflow-y-auto">
        <div class="flex justify-between items-center mb-6">
          <h3 class="text-2xl font-bold text-gray-800">Keranjang Belanja</h3>
          <button @click="showCart = false" class="text-gray-500 hover:text-gray-700">
            <i class="fas fa-times text-2xl"></i>
          </button>
        </div>
        
        <div v-if="cartItems.length === 0" class="text-center py-8">
          <i class="fas fa-shopping-cart text-6xl text-gray-300 mb-4"></i>
          <p class="text-gray-500">Keranjang Anda kosong</p>
        </div>
        
        <div v-else>
          <div v-for="item in cartItems" :key="item.id" class="flex items-center justify-between border-b py-4">
            <div class="flex items-center">
              <div class="w-16 h-16 bg-gray-200 rounded-lg mr-4 flex items-center justify-center">
                <i class="fas fa-image text-gray-400"></i>
              </div>
              <div>
                <h4 class="font-semibold text-gray-800">{{ item.name }}</h4>
                <p class="text-blue-600 font-semibold">Rp {{ formatPrice(item.price) }}</p>
              </div>
            </div>
            <div class="flex items-center space-x-3">
              <button @click="updateQuantity(item.id, item.quantity - 1)" class="w-8 h-8 bg-gray-200 rounded-full flex items-center justify-center hover:bg-gray-300">
                <i class="fas fa-minus text-sm"></i>
              </button>
              <span class="font-semibold">{{ item.quantity }}</span>
              <button @click="updateQuantity(item.id, item.quantity + 1)" class="w-8 h-8 bg-gray-200 rounded-full flex items-center justify-center hover:bg-gray-300">
                <i class="fas fa-plus text-sm"></i>
              </button>
              <button @click="removeFromCart(item.id)" class="text-red-600 hover:text-red-800 ml-4">
                <i class="fas fa-trash"></i>
              </button>
            </div>
          </div>
          
          <div class="mt-6 pt-4 border-t">
            <div class="flex justify-between items-center mb-4">
              <span class="text-xl font-bold">Total: Rp {{ formatPrice(cartTotal) }}</span>
            </div>
            <button class="w-full bg-blue-600 text-white py-3 rounded-lg hover:bg-blue-700 transition-colors">
              <i class="fas fa-credit-card mr-2"></i>Checkout
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Login Modal -->
    <div v-if="showLoginModal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
      <div class="bg-white rounded-xl p-8 max-w-md w-full mx-4">
        <div class="flex justify-between items-center mb-6">
          <h3 class="text-2xl font-bold text-gray-800">Login</h3>
          <button @click="showLoginModal = false" class="text-gray-500 hover:text-gray-700">
            <i class="fas fa-times text-2xl"></i>
          </button>
        </div>
        
        <form @submit.prevent="login">
          <div class="mb-4">
            <label class="block text-gray-700 text-sm font-bold mb-2">Email</label>
            <input v-model="loginForm.email" type="email" required 
                   class="w-full px-3 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent outline-none">
          </div>
          <div class="mb-6">
            <label class="block text-gray-700 text-sm font-bold mb-2">Password</label>
            <input v-model="loginForm.password" type="password" required 
                   class="w-full px-3 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent outline-none">
          </div>
          <button type="submit" class="w-full bg-blue-600 text-white py-2 rounded-lg hover:bg-blue-700 transition-colors">
            Login
          </button>
        </form>
        
        <div class="mt-4 text-center">
          <p class="text-gray-600">Belum punya akun? 
            <a href="#" class="text-blue-600 hover:text-blue-800">Daftar di sini</a>
          </p>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      currentView: 'home',
      adminView: 'dashboard',
      searchQuery: '',
      sortBy: 'name',
      showCart: false,
      showLoginModal: false,
      showAddProduct: false,
      
      // User & Auth
      user: null,
      isLoggedIn: false,
      loginForm: {
        email: '',
        password: ''
      },
      
      // Cart
      cartItems: [],
      
      // Sample Data
      featuredProducts: [
        { id: 1, name: 'Smartphone Premium', description: 'Smartphone terbaru dengan fitur canggih', price: 8500000, category_id: 1, stock: 15 },
        { id: 2, name: 'Laptop Gaming', description: 'Laptop gaming performa tinggi', price: 15000000, category_id: 1, stock: 8 },
        { id: 3, name: 'Sepatu Olahraga', description: 'Sepatu olahraga premium dan nyaman', price: 1200000, category_id: 2, stock: 25 },
        { id: 4, name: 'Jaket Casual', description: 'Jaket casual untuk aktivitas sehari-hari', price: 450000, category_id: 2, stock: 18 }
      ],
      
      products: [
        { id: 1, name: 'Smartphone Premium', description: 'Smartphone terbaru dengan fitur canggih', price: 8500000, category_id: 1, stock: 15 },
        { id: 2, name: 'Laptop Gaming', description: 'Laptop gaming performa tinggi', price: 15000000, category_id: 1, stock: 8 },
        { id: 3, name: 'Tablet Android', description: 'Tablet dengan layar HD', price: 3500000, category_id: 1, stock: 12 },
        { id: 4, name: 'Sepatu Olahraga', description: 'Sepatu olahraga premium dan nyaman', price: 1200000, category_id: 2, stock: 25 },
        { id: 5, name: 'Jaket Casual', description: 'Jaket casual untuk aktivitas sehari-hari', price: 450000, category_id: 2, stock: 18 },
        { id: 6, name: 'Celana Jeans', description: 'Celana jeans berkualitas tinggi', price: 350000, category_id: 2, stock: 30 },
        { id: 7, name: 'Sofa Minimalis', description: 'Sofa minimalis untuk ruang tamu', price: 4500000, category_id: 3, stock: 5 },
        { id: 8, name: 'Meja Kerja', description: 'Meja kerja ergonomis', price: 1800000, category_id: 3, stock: 10 }
      ],
      
      categories: [
        { id: 1, name: 'Elektronik', description: 'Perangkat elektronik dan gadget terbaru', icon: 'fas fa-laptop' },
        { id: 2, name: 'Fashion', description: 'Pakaian dan aksesoris terkini', icon: 'fas fa-tshirt' },
        { id: 3, name: 'Furniture', description: 'Perabotan rumah tangga berkualitas', icon: 'fas fa-couch' },
        { id: 4, name: 'Olahraga', description: 'Perlengkapan dan peralatan olahraga', icon: 'fas fa-dumbbell' },
        { id: 5, name: 'Kecantikan', description: 'Produk perawatan dan kecantikan', icon: 'fas fa-spa' },
        { id: 6, name: 'Makanan', description: 'Makanan dan minuman segar', icon: 'fas fa-utensils' }
      ],
      
      orders: [
        { id: 1, user_id: 1, total: 8500000, status: 'Pending', date: '2025-08-20' },
        { id: 2, user_id: 2, total: 1650000, status: 'Dibayar', date: '2025-08-21' },
        { id: 3, user_id: 1, total: 4500000, status: 'Dikirim', date: '2025-08-22' }
      ]
    }
  },
  
  computed: {
    cartCount() {
      return this.cartItems.reduce((total, item) => total + item.quantity, 0)
    },
    
    cartTotal() {
      return this.cartItems.reduce((total, item) => total + (item.price * item.quantity), 0)
    },
    
    filteredProducts() {
      let filtered = [...this.products]
      
      // Filter by search query
      if (this.searchQuery) {
        filtered = filtered.filter(product => 
          product.name.toLowerCase().includes(this.searchQuery.toLowerCase()) ||
          product.description.toLowerCase().includes(this.searchQuery.toLowerCase())
        )
      }
      
      // Sort products
      switch (this.sortBy) {
        case 'price_low':
          filtered.sort((a, b) => a.price - b.price)
          break
        case 'price_high':
          filtered.sort((a, b) => b.price - a.price)
          break
        case 'newest':
          filtered.sort((a, b) => b.id - a.id)
          break
        default:
          filtered.sort((a, b) => a.name.localeCompare(b.name))
      }
      
      return filtered
    },
    
    totalSales() {
      return this.orders.reduce((total, order) => total + order.total, 0)
    }
  },
  
  methods: {
    formatPrice(price) {
      return new Intl.NumberFormat('id-ID').format(price)
    },
    
    addToCart(product) {
      const existingItem = this.cartItems.find(item => item.id === product.id)
      
      if (existingItem) {
        if (existingItem.quantity < product.stock) {
          existingItem.quantity++
        } else {
          alert('Stok tidak mencukupi!')
        }
      } else {
        this.cartItems.push({
          id: product.id,
          name: product.name,
          price: product.price,
          quantity: 1,
          maxStock: product.stock
        })
      }
      
      // Show success animation or notification
      this.showNotification('Produk berhasil ditambahkan ke keranjang!')
    },
    
    removeFromCart(productId) {
      this.cartItems = this.cartItems.filter(item => item.id !== productId)
    },
    
    updateQuantity(productId, newQuantity) {
      const item = this.cartItems.find(item => item.id === productId)
      if (item) {
        if (newQuantity <= 0) {
          this.removeFromCart(productId)
        } else if (newQuantity <= item.maxStock) {
          item.quantity = newQuantity
        } else {
          alert('Stok tidak mencukupi!')
        }
      }
    },
    
    filterByCategory(categoryId) {
      this.currentView = 'products'
      // In real implementation, you would filter products by category
      console.log('Filter by category:', categoryId)
    },
    
    getCategoryName(categoryId) {
      const category = this.categories.find(cat => cat.id === categoryId)
      return category ? category.name : 'Unknown'
    },
    
    login() {
      // Simulate login process
      if (this.loginForm.email && this.loginForm.password) {
        this.user = {
          id: 1,
          email: this.loginForm.email,
          name: 'User Test'
        }
        this.isLoggedIn = true
        this.showLoginModal = false
        this.showNotification('Login berhasil!')
        
        // Reset form
        this.loginForm = { email: '', password: '' }
      }
    },
    
    logout() {
      this.user = null
      this.isLoggedIn = false
      this.cartItems = []
      this.showNotification('Logout berhasil!')
    },
    
    showNotification(message) {
      // Simple notification system
      const notification = document.createElement('div')
      notification.className = 'fixed top-4 right-4 bg-green-500 text-white px-6 py-3 rounded-lg shadow-lg z-50 animate-pulse'
      notification.textContent = message
      document.body.appendChild(notification)
      
      setTimeout(() => {
        if (notification.parentNode) {
          notification.parentNode.removeChild(notification)
        }
      }, 3000)
    }
  },
  
  mounted() {
    // Initialize app
    console.log('WEBIVA E-Commerce CMS Loaded')
  }
}
</script>

<style scoped>
.animate-fadeIn {
  animation: fadeIn 0.5s ease-in-out;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.transition-all {
  transition: all 0.3s ease;
}

/* Custom scrollbar */
::-webkit-scrollbar {
  width: 6px;
}

::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 10px;
}

::-webkit-scrollbar-thumb {
  background: #888;
  border-radius: 10px;
}

::-webkit-scrollbar-thumb:hover {
  background: #555;
}

/* Responsive improvements */
@media (max-width: 768px) {
  .product-grid {
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  }
  
  .category-grid {
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  }
}

/* Hover effects */
.card-hover {
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.card-hover:hover {
  transform: translateY(-5px);
  box-shadow: 0 20px 40px rgba(0,0,0,0.1);
}

/* Button animations */
button {
  transition: all 0.2s ease;
}

button:hover {
  transform: translateY(-1px);
}

button:active {
  transform: translateY(0);
}

/* Loading states */
.loading {
  position: relative;
  pointer-events: none;
}

.loading::after {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 20px;
  height: 20px;
  margin: -10px 0 0 -10px;
  border: 2px solid #f3f3f3;
  border-top: 2px solid #3498db;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

/* Glass morphism effect */
.glassmorphism {
  background: rgba(255, 255, 255, 0.25);
  box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
  backdrop-filter: blur(4px);
  -webkit-backdrop-filter: blur(4px);
  border-radius: 10px;
  border: 1px solid rgba(255, 255, 255, 0.18);
}

/* Custom animations */
.bounce-in {
  animation: bounceIn 0.6s cubic-bezier(0.68, -0.55, 0.265, 1.55);
}

@keyframes bounceIn {
  0% {
    transform: scale(0.3);
    opacity: 0;
  }
  50% {
    transform: scale(1.05);
  }
  70% {
    transform: scale(0.9);
  }
  100% {
    transform: scale(1);
    opacity: 1;
  }
}

/* Pulse animation for notifications */
.pulse {
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0% {
    box-shadow: 0 0 0 0 rgba(59, 130, 246, 0.7);
  }
  70% {
    box-shadow: 0 0 0 10px rgba(59, 130, 246, 0);
  }
  100% {
    box-shadow: 0 0 0 0 rgba(59, 130, 246, 0);
  }
}

/* Focus states for accessibility */
input:focus,
select:focus,
button:focus {
  outline: none;
  box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.3);
}

/* Dark mode support (future implementation) */
@media (prefers-color-scheme: dark) {
  /* Dark mode styles can be added here */
}
</style>