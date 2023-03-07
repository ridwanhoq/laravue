Sure! Here's an example of a Laravel Vue.js CRUD (Create, Read, Update, Delete) application.

Assuming that you have already installed Laravel and Vue.js, here are the steps to create a CRUD application:

Step 1: Create a Laravel Project

javascript
Copy code
laravel new laravel-vue-crud
Step 2: Setup the Database
Open .env file and add your database credentials. Then run the following command to create the database tables:

Copy code
php artisan migrate
Step 3: Install Required Packages
Install the following packages using NPM:

Copy code
npm install vue vue-router axios vue-axios bootstrap jquery popper.js
Step 4: Create a Model and Migration

go
Copy code
php artisan make:model Product -m
Step 5: Define the Table Schema
Open the create_products_table.php file in the database/migrations folder and define the table schema:

lua
Copy code
public function up()
{
    Schema::create('products', function (Blueprint $table) {
        $table->id();
        $table->string('name');
        $table->text('description');
        $table->decimal('price', 8, 2);
        $table->timestamps();
    });
}
Step 6: Run the Migration

Copy code
php artisan migrate
Step 7: Create a Controller

go
Copy code
php artisan make:controller ProductController --resource
Step 8: Define the Routes
Open the web.php file in the routes folder and define the routes:

sql
Copy code
Route::get('/', function () {
    return view('welcome');
});

Route::resource('products', 'ProductController');
Step 9: Create Views
Create the following views in the resources/views folder:

products/index.blade.php
products/create.blade.php
products/edit.blade.php
Step 10: Create a Vue Component
Create a Vue component in the resources/js/components folder:

php
Copy code
<template>
    <div>
        <h1>Products</h1>
        <table class="table table-striped">
            <thead>
                <tr>
                    <th>Name</th>
                    <th>Description</th>
                    <th>Price</th>
                    <th></th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(product, index) in products" :key="product.id">
                    <td>{{ product.name }}</td>
                    <td>{{ product.description }}</td>
                    <td>{{ product.price }}</td>
                    <td>
                        <button @click="editProduct(product.id)" class="btn btn-primary">Edit</button>
                        <button @click="deleteProduct(product.id)" class="btn btn-danger">Delete</button>
                    </td>
                </tr>
            </tbody>
        </table>
        <button @click="addProduct" class="btn btn-success">Add Product</button>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                products: []
            }
        },
        mounted() {
            this.fetchProducts();
        },
        methods: {
            fetchProducts() {
                axios.get('/products')
                    .then(response => {
                        this.products = response.data;
                    });
            },
            addProduct() {
                this.$router.push('/products/create');
            },
            editProduct(id) {
                this.$router.push('/products/' + id + '/edit');
            },
            deleteProduct(id) {
                if (confirm('Are you sure you want to delete this product?')) {
                    axios.delete('/products/' + id)
                        .