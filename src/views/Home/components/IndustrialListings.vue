<template>
  <CCard class="mb-4">
    <CCardHeader>
      <strong>Industrial Listings</strong>
    </CCardHeader>
    <CCardBody>
      <div class="table-responsive">
        <CTable hover>
          <CTableHead>
            <CTableRow>
              <CTableHeaderCell>
                <input 
                  type="checkbox" 
                  :checked="allSelected"
                  @change="toggleAllSelection"
                />
              </CTableHeaderCell>
              <CTableHeaderCell>Hide</CTableHeaderCell>
              <CTableHeaderCell v-for="column in columns" :key="column.field">
                {{ column.label }}
              </CTableHeaderCell>
            </CTableRow>
          </CTableHead>
          <CTableBody>
            <CTableRow 
              v-for="building in buildingsData" 
              :key="building.id"
              :class="{ 'table-secondary': isRowHidden(building.id) }"
            >
              <CTableDataCell>
                <input 
                  type="checkbox"
                  :checked="isRowSelected(building.id)"
                  @change="() => toggleRowSelection(building.id)"
                />
              </CTableDataCell>
              <CTableDataCell>
                <input 
                  type="checkbox"
                  :checked="isRowHidden(building.id)"
                  @change="() => toggleRowHidden(building.id)"
                />
              </CTableDataCell>
              <CTableDataCell v-for="column in columns" :key="column.field">
                {{ formatValue(building[column.field], column.type) }}
              </CTableDataCell>
            </CTableRow>
          </CTableBody>
        </CTable>
      </div>
    </CCardBody>
  </CCard>
</template>

<script setup>
import { computed } from 'vue'
import { useIndustrialData } from '../composables/useIndustrialData'

const { 
  filteredData, 
  selectedRows, 
  hiddenRows,
  toggleRowSelection,
  toggleRowHidden 
} = useIndustrialData()

const columns = [
  { field: 'buildingName', label: 'BUILDING NAME', type: 'text' },
  { field: 'class', label: 'CLASS', type: 'text' },
  { field: 'buildingSizeSF', label: 'BUILDING SIZE (SF)', type: 'number' },
  { field: 'availableSF', label: 'AVAILABLE (SF)', type: 'number' },
  { field: 'minSpaceSF', label: 'MINIMUM SPACE (SF)', type: 'number' },
  { field: 'expansionSF', label: 'EXPANSION UP TO (SF)', type: 'number' },
  { field: 'industrialPark', label: 'INDUSTRIAL PARK', type: 'text' },
  { field: 'clearHeight', label: 'CLEAR HEIGHT', type: 'number' },
  { field: 'dockDoors', label: 'DOCK DOORS', type: 'number' },
  { field: 'driveInDoor', label: 'DRIVE IN DOOR', type: 'number' },
  { field: 'floorThickness', label: 'FLOOR THICKNESS', type: 'text' },
  { field: 'type', label: 'TYPE', type: 'text' },
  { field: 'owner', label: 'OWNER', type: 'text' },
  { field: 'developer', label: 'DEVELOPER', type: 'text' },
  { field: 'builder', label: 'BUILDER', type: 'text' },
  { field: 'status', label: 'STATUS', type: 'text' },
  { field: 'market', label: 'MARKET', type: 'text' },
  { field: 'subMarket', label: 'SUBMARKET', type: 'text' },
  { field: 'deal', label: 'DEAL', type: 'text' },
  { field: 'currency', label: 'CURRENCY', type: 'text' },
  { field: 'minLeaseSF', label: 'MIN. LEASE (SF/YR)', type: 'number' },
  { field: 'maxLeaseSF', label: 'MAX. LEASE (SF/YR)', type: 'number' },
  { field: 'salePriceSF', label: 'SALE PRICE (SF/YR)', type: 'number' },
  { field: 'availableSince', label: 'AVAILABLE SINCE', type: 'date' },
  { field: 'tenantType', label: 'SINGLE OR MULTITENANT', type: 'text' },
  { field: 'listingBroker', label: 'LISTING BROKER', type: 'text' },
  { field: 'comments', label: 'COMMENTS', type: 'text' },
  { field: 'contact', label: 'CONTACT', type: 'text' },
  { field: 'phone', label: 'PHONE', type: 'text' },
  { field: 'email', label: 'EMAIL', type: 'text' }
]

const buildingsData = computed(() => filteredData.value)

const allSelected = computed(() => 
  buildingsData.value.length > 0 && 
  buildingsData.value.every(row => selectedRows.value.includes(row.id))
)

const isRowSelected = (id) => selectedRows.value.includes(id)
const isRowHidden = (id) => hiddenRows.value.includes(id)

const toggleAllSelection = () => {
  if (allSelected.value) {
    selectedRows.value = []
  } else {
    selectedRows.value = buildingsData.value.map(row => row.id)
  }
}

const formatValue = (value, type) => {
  if (value == null) return ''
  
  switch (type) {
    case 'number':
      return new Intl.NumberFormat().format(value)
    case 'date':
      return new Date(value).toLocaleDateString()
    default:
      return value
  }
}
</script>