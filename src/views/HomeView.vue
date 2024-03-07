<template>
  <div class="card">
    <DataTable :value="products" tableStyle="min-width: 50rem">
      <template #header>
        <div class="flex flex-wrap align-items-center justify-content-between gap-2">
          <span class="text-xl text-900 font-bold">Products</span>
          <Button label="add product" rounded raised @click="showAddDialog()" />
        </div>
      </template>
      <Column field="name" header="Name"></Column>
      <Column header="Image">
        <template #body="slotProps">
          <img :src="`https://primefaces.org/cdn/primevue/images/product/${slotProps.data.image}`"
            :alt="slotProps.data.image" class="w-6rem border-round" />
        </template>
      </Column>
      <!-- <Column field="price" header="Price">
        <template #body="slotProps">
          {{ formatCurrency(slotProps.data.price) }}
        </template>
      </Column>
      <Column field="category" header="Category"></Column>
      <Column field="rating" header="Reviews">
        <template #body="slotProps">
          <Rating :modelValue="slotProps.data.rating" readonly :cancel="false" />
        </template>
      </Column> -->
      <!-- <Column header="Status">
        <template #body="slotProps">
          <Tag :value="slotProps.data.inventoryStatus" :severity="getSeverity(slotProps.data)" />
        </template>
      </Column> -->
      <Column header="actions">
        <template #body="slotProps">
          <Button icon="pi pi-pencil" @click="editProduct(slotProps.data)" />
          <Button icon="pi pi-trash" class="p-button-rounded p-button-danger" @click="deleteProduct(slotProps.data)" />
        </template>
      </Column>
      <template #footer> In total there are {{ products ? products.length : 0 }} products. </template>
    </DataTable>
    <Dialog v-model="displayAddDialog" :visible="displayAddDialog" header="Add Product" modal>
      <div>
        <div class="mb-3">
          <span class="p-float-label">
            <InputText v-model="selectedProduct.name" id="name" />
            <label for="name">Name</label>
          </span>
        </div>
        <div class="mb-3">
          <span class="p-float-label">
            <InputText v-model="selectedProduct.price" id="price" />
            <label for="price">Price</label>
          </span>
        </div>
        <div class="mb-3">
          <span class="p-float-label">
            <InputText v-model="selectedProduct.category" id="category" />
            <label for="category">Category</label>
          </span>
        </div>
        <div>
          <Button icon="pi pi-check" @click="saveProduct" />
          <Button icon="pi pi-times" class="p-button-secondary" @click="cancelAddDialog" />
        </div>
      </div>
    </Dialog>
  </div>
</template>

<script>

import DataTable from 'primevue/datatable';
import Column from 'primevue/column';
import Button from 'primevue/button';
import Rating from 'primevue/rating';
import Tag from 'primevue/tag';
import Dialog from 'primevue/dialog';
import InputText from 'primevue/inputtext';

export default {
  data() {
    return {
      products: [
        { id: 1, name: 'Laptop', price: '$999', category: 'Electronics' },
        { id: 2, name: 'Smartphone', price: '$699', category: 'Electronics' },
        { id: 3, name: 'Headphones', price: '$199', category: 'Electronics' },
        { id: 4, name: 'Book', price: '$19', category: 'Books' },
        { id: 5, name: 'Chair', price: '$149', category: 'Furniture' },
        { id: 6, name: 'Desk', price: '$299', category: 'Furniture' }
      ],
      displayAddDialog: false,
      selectedProduct: { id: 0, name: '', price: '', category: '' },
    };
  },
  components: {
    DataTable,
    Column,
    Button,
    Rating,
    Tag,
    Dialog,
    InputText
  },
  methods: {
    // formatCurrency(value) {
    //   return value.toLocaleString('en-US', { style: 'currency', currency: 'USD' });
    // },
    // getSeverity(product) {
    //   switch (product.inventoryStatus) {
    //     case 'INSTOCK':
    //       return 'success';

    //     case 'LOWSTOCK':
    //       return 'warning';

    //     case 'OUTOFSTOCK':
    //       return 'danger';

    //     default:
    //       return null;
    //   }
    // },
    showAddDialog() {
      this.displayAddDialog = true;
    },

    cancelAddDialog() {
      this.displayAddDialog = false;
      this.newProduct = { id: 0, name: '', price: '', category: '' };
    },
    saveProduct() {
      if (this.selectedProduct.id != 0) {
        const index = this.products.findIndex(p => p.id === this.selectedProduct.id);
        this.products[index] = { ...this.selectedProduct };
      } else {
        const newId = this.products.length > 0 ? this.products[this.products.length - 1].id + 1 : 1;
        this.selectedProduct.id = newId;
        this.products.push({ ...this.selectedProduct });
      }
      this.cancelAddDialog();
    },

    editProduct(product) {
      this.selectedProduct = { ...product };
      this.displayAddDialog = true;
    },

    deleteProduct(product) {
      const index = this.products.findIndex(p => p.id === product.id);
      if (index !== -1) {
        this.products.splice(index, 1);
      }
    },

  }
};
</script>
<style scoped>
.p-field {
  margin-bottom: 1rem;
}
</style>