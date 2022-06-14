<template>
  <div class="service-catalog">
    <div class="catalog-actions">
      <h1>Services</h1>

      <KButton
        appearance="primary"
        class="create-service"
        :is-rounded="false"
        @click="serviceModalIsOpen = true"
      >
        Add New Service
      </KButton>
      <CreateServiceModal
        :service-modal-is-open="serviceModalIsOpen"
        @toggle-modal="serviceModalIsOpen = !serviceModalIsOpen"
      />
      <KInput
        v-model="searchQuery"
        class="search-input"
        placeholder="Search services"
        type="search"
      />
    </div>
    <ul
      class="catalog"
    >
      <KCard
        v-for="service in visibleServices"
        :key="service.id"
        class="service"
        :has-hover="true"
      >
        <template #body>
          <h2 class="truncate">
            <a href="#">{{ service.name }}</a>
          </h2>
          <p
            class="multi-line-truncation"
            :title="service.description"
          >
            {{ service.description }}
          </p>
          <div
            v-if="service.versions.length"
            class="versions"
          >
            <div class="badge">
              {{ service.versions.length }}
            </div>
            <span>Versions</span>
          </div>
        </template>
      </KCard>
    </ul>
    <KPagination
      :key="componentKey"
      :items="services"
      :neighbors="100"
      :page-sizes="[12]"
      :total-count="services.length"
      @page-changed="({visibleItems}) => visibleServices = visibleItems"
    />
  </div>
</template>

<script lang="ts" setup>
import { ref, watch } from 'vue'
import { KPagination, KCard, KButton, KInput } from '@kong/kongponents'
import useServices from '@/composables/useServices'
import CreateServiceModal from './CreateServiceModal.vue'

// Import services from the composable
const { services, loading } = useServices()
// Set the search string to a Vue ref
const searchQuery = ref('')
const visibleServices = ref()
const componentKey = ref(0)
const serviceModalIsOpen = ref(false)
let originalServices: any[]

watch(loading, () => {
  originalServices = services.value
})
watch(services, () => {
  visibleServices.value = services.value.slice(0, 12)
  // hack to force rerender of pagination
  componentKey.value++
})

watch(searchQuery, (searchQuery) => {
  services.value = originalServices.filter((service) => {
    return service.name.toLowerCase().indexOf(searchQuery.toLowerCase()) !== -1
  })
})

</script>
<style>
@import "../../node_modules/@kong/kongponents/dist/style.css";
</style>
<style lang="scss">
.service-catalog {
  max-width: 908px;
  margin: 0 auto;
  padding: 0 20px;
  .card-pagination-bar {
    .pagination-button-container {
      padding: 0;
    }
    .page-size-select, .pagination-text {
      display: none;
    }
    justify-content: center;
    @media (max-width: 500px){
      .pagination-button {
        display: none;
      }
      .pagination-button:first-child, .pagination-button:last-child, .pagination-text {
        display: inline-block;
      }
    }
  }
  .catalog-actions {
     h1 {
      display: inline-block;
      color: var(--blue-700);
      font-size: 24px;
      margin: 0 0 24px 0;
    }
    margin: 0 6px;
      .create-service {
      float: right;
      top: 0;
    }
     @media (max-width: 500px){
      .search-input {
         width: 100%;
      }
     }
  }
  .catalog {
    display: flex;
    flex-wrap: wrap;
    margin: 20px 0 0 0;
    padding: 0;
    list-style: none;
    .service {
      width: 215px;
      height: 181px;
      padding: 0 14px;
      margin: 6px;
      box-sizing: border-box;
      position: relative;
      @media (max-width: 500px){
        width: 100%;
      }
      &:hover {
        border-color: var(--blue-300);
        box-shadow: none;
        cursor: pointer;
      }
      a, h2 {
        color: var(--blue-500);
        font-weight: 500;
        font-size: 16px;
        text-decoration: none;
      }

      p {
        color: var(--black-45);
        font-weight: 400;
        font-size: 14px;
        // TODO fix .multi-line-truncation
        display: -webkit-box;
        -webkit-line-clamp: var(--TMaxLineLimit, 3);
        -webkit-box-orient: vertical;
        overflow: hidden;
      }

      .versions {
        position: absolute;
        bottom: 10px;
        .badge {
          display: inline-block;
          border: solid 1px var(--blue-300);
          font-weight: 500;
          font-size: 13px;
          border-radius: 40px;
          padding: 0 10px;
          line-height: 16px;
        }
        span {
          font-weight: 500;
          font-size: 14px;
          padding-left: 10px;
        }
      }
    }
  }
}
</style>
