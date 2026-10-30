<script setup>
import { ref, computed } from 'vue'

// State Daftar Tugas & Jadwal
const tasks = ref([
  { id: 1, title: 'Minum 1 gelas air', time: '08:00 - 08:30', category: 'Hidrasi', status: false, icon: '💧' },
  { id: 2, title: 'Olahraga aerobik', time: '16:00 - 17:00', category: 'Kebugaran', status: false, icon: '🏃' },
  { id: 3, title: 'Lari pagi', time: '06:00 - 06:30', category: 'Kebugaran', status: true, icon: '⚡' }
])

// Input State
const inputTask = ref('')
const inputTime = ref('')
const activeFilter = ref('all') // 'all', 'active', 'completed'

// Hitung persentase progres selesai secara otomatis
const progressPercentage = computed(() => {
  if (tasks.value.length === 0) return 0
  const completed = tasks.value.filter(t => t.status).length
  return Math.round((completed / tasks.value.length) * 100)
})

// Filter tugas berdasarkan tab yang aktif
const filteredTasks = computed(() => {
  if (activeFilter.value === 'active') {
    return tasks.value.filter(t => !t.status)
  } else if (activeFilter.value === 'completed') {
    return tasks.value.filter(t => t.status)
  }
  return tasks.value
})

// Fungsi Tambah Tugas
const addTask = () => {
  if (!inputTask.value.trim()) {
    alert('Nama tugas tidak boleh kosong!')
    return
  }

  const newTask = {
    id: new Date().getTime(),
    title: inputTask.value,
    time: inputTime.value.trim() || 'Waktu fleksibel',
    category: 'Jadwal Harian',
    status: false,
    icon: '📌'
  }

  tasks.value.push(newTask)
  inputTask.value = ''
  inputTime.value = ''
}

// Fungsi Hapus Tugas
const deleteTask = (id) => {
  const confirmation = confirm('Apakah Anda yakin ingin menghapus jadwal ini?')
  if (confirmation) {
    tasks.value = tasks.value.filter(item => item.id !== id)
  }
}
</script>

<template>
  <div class="min-h-screen bg-gradient-to-br from-[#eef2f7] via-[#f4f7fa] to-[#e4ebf2] flex items-center justify-center p-4 sm:p-6 font-sans text-gray-800">
    
    <!-- Frame Simulasi Layar Smartphone -->
    <div class="relative w-full max-w-md bg-[#fdfdfd] rounded-[48px] p-6 shadow-2xl overflow-hidden border-8 border-white">
      
      <!-- Header Bar Simulasi -->
      <div class="flex items-center justify-between mb-6 pt-2 px-2">
        <span class="text-xs font-bold text-gray-800">9:41</span>
        <div class="flex items-center gap-1.5">
          <div class="w-3 h-3 rounded-full bg-gray-800"></div>
          <div class="w-4 h-3 rounded-sm border border-gray-800 flex items-center p-0.5">
            <div class="w-full h-full bg-gray-800 rounded-2xs"></div>
          </div>
        </div>
      </div>

      <!-- Navigasi Atas -->
      <div class="flex items-center justify-between mb-6 px-2">
        <button class="w-10 h-10 rounded-full bg-gray-100 flex items-center justify-center hover:bg-gray-200 transition-all text-gray-700 cursor-pointer">
          ←
        </button>
        <h2 class="text-base font-semibold text-gray-900 tracking-wide">Daily Schedule</h2>
        <button class="w-10 h-10 rounded-full bg-gray-100 flex items-center justify-center hover:bg-gray-200 transition-all text-gray-700 cursor-pointer" title="Bagikan">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8.684 13.342C8.886 12.938 9 12.482 9 12c0-.482-.114-.938-.316-1.342m0 2.684a3 3 0 110-2.684m0 2.684l6.632 3.316m-6.632-6l6.632-3.316m0 0a3 3 0 105.367-2.684 3 3 0 00-5.367 2.684zm0 9.316a3 3 0 105.368 2.684 3 3 0 00-5.368-2.684z" />
          </svg>
        </button>
      </div>

      <!-- Widget Progres Kartu -->
      <div class="bg-gradient-to-r from-gray-50 to-white border border-gray-100 rounded-3xl p-5 mb-5 shadow-sm">
        <div class="flex justify-between items-center mb-2">
          <div>
            <span class="text-[11px] uppercase tracking-wider text-gray-400 font-bold">Your Progress</span>
            <h3 class="text-sm font-bold text-gray-900">You are doing well!</h3>
          </div>
          <span class="text-2xl font-extrabold text-indigo-600">{{ progressPercentage }}%</span>
        </div>
        
        <!-- Bar Progres -->
        <div class="w-full bg-gray-200 h-2 rounded-full overflow-hidden">
          <div 
            class="bg-indigo-600 h-full rounded-full transition-all duration-500" 
            :style="{ width: progressPercentage + '%' }"
          ></div>
        </div>
      </div>

      <!-- Form Input Tambah Tugas & Atur Waktu -->
      <form @submit.prevent="addTask" class="bg-gray-50/80 border border-gray-200 rounded-3xl p-4 mb-5 shadow-xs space-y-3">
        <div class="text-xs font-bold text-gray-700">Tambah Jadwal Baru</div>
        <input 
          v-model="inputTask"
          type="text" 
          placeholder="Nama aktivitas (cth: Belajar Coding)..." 
          class="w-full bg-white border border-gray-200 rounded-2xl px-4 py-2.5 text-xs text-gray-800 focus:outline-none focus:ring-2 focus:ring-indigo-500"
        />
        <div class="flex gap-2">
          <input 
            v-model="inputTime"
            type="text" 
            placeholder="Atur jam (cth: 13:00 - 14:00)" 
            class="flex-1 bg-white border border-gray-200 rounded-2xl px-4 py-2 text-xs text-gray-800 focus:outline-none focus:ring-2 focus:ring-indigo-500"
          />
          <button 
            type="submit" 
            class="bg-indigo-600 hover:bg-indigo-700 text-white font-semibold px-5 py-2 rounded-2xl text-xs shadow-md transition-all active:scale-95 cursor-pointer"
          >
            Simpan
          </button>
        </div>
      </form>

      <!-- Tombol Filter Status (Semua, Belum Selesai, Selesai) -->
      <div class="flex gap-1.5 mb-4 bg-gray-100 p-1 rounded-2xl text-xs font-medium">
        <button 
          @click="activeFilter = 'all'" 
          :class="{'bg-white text-indigo-600 shadow-xs font-bold': activeFilter === 'all', 'text-gray-500': activeFilter !== 'all'}"
          class="flex-1 py-1.5 rounded-xl transition-all cursor-pointer"
        >
          Semua ({{ tasks.length }})
        </button>
        <button 
          @click="activeFilter = 'active'" 
          :class="{'bg-white text-indigo-600 shadow-xs font-bold': activeFilter === 'active', 'text-gray-500': activeFilter !== 'active'}"
          class="flex-1 py-1.5 rounded-xl transition-all cursor-pointer"
        >
          Proses
        </button>
        <button 
          @click="activeFilter = 'completed'" 
          :class="{'bg-white text-indigo-600 shadow-xs font-bold': activeFilter === 'completed', 'text-gray-500': activeFilter !== 'completed'}"
          class="flex-1 py-1.5 rounded-xl transition-all cursor-pointer"
        >
          Selesai
        </button>
      </div>

      <!-- Daftar Jadwal Harian (List) -->
      <div class="space-y-3 max-h-[220px] overflow-y-auto pr-1">
        <div 
          v-for="task in filteredTasks" 
          :key="task.id"
          class="bg-white border border-gray-100 rounded-2xl p-3.5 flex items-center justify-between shadow-xs hover:shadow-md transition-all"
        >
          <div class="flex items-center gap-3">
            <div class="w-10 h-10 rounded-2xl bg-gray-50 flex items-center justify-center text-lg shrink-0 border border-gray-100">
              {{ task.icon }}
            </div>
            <div>
              <h4 :class="{'line-through text-gray-400': task.status, 'text-gray-900 font-semibold': !task.status}" class="text-xs">
                {{ task.title }}
              </h4>
              <span class="text-[10px] text-indigo-500 font-semibold bg-indigo-50 px-2 py-0.5 rounded-md mt-0.5 inline-block">
                ⏰ {{ task.time }}
              </span>
            </div>
          </div>

          <div class="flex items-center gap-2">
            <!-- Checkbox Selesai/Belum -->
            <input 
              type="checkbox" 
              v-model="task.status" 
              class="w-4 h-4 accent-indigo-600 rounded cursor-pointer"
            />
            <!-- Tombol Hapus -->
            <button 
              @click="deleteTask(task.id)" 
              class="w-7 h-7 rounded-full bg-red-50 text-red-500 hover:bg-red-500 hover:text-white flex items-center justify-center transition-all cursor-pointer text-xs"
              title="Hapus Tugas"
            >
              ✕
            </button>
          </div>
        </div>

        <div v-if="filteredTasks.length === 0" class="text-center py-8 text-gray-400 text-xs">
          Belum ada jadwal pada kategori ini.
        </div>
      </div>

      <!-- Home Indicator Bar di Bawah -->
      <div class="w-32 h-1 bg-gray-300 rounded-full mx-auto mt-6"></div>

    </div>
  </div>
</template>