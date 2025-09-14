<!-- AppSidebar.vue -->
<script setup>
import { ref } from 'vue'
import { addIcons } from 'oh-vue-icons'
import {
  HiSolidHome,
  FaUser,
  MdKeyboarddoublearrowleftOutlined,
  MdDashboardRound,
  MdCreatenewfolder,
  FaAngleDown,
  MdChecklistRound,
  BiCalendarFill,
  FaUserAlt,
} from 'oh-vue-icons/icons'

addIcons(
  HiSolidHome,
  FaUser,
  MdKeyboarddoublearrowleftOutlined,
  MdDashboardRound,
  MdCreatenewfolder,
  FaAngleDown,
  MdChecklistRound,
  BiCalendarFill,
  FaUserAlt,
) // 註冊到全域 icon 庫（此檔執行一次即可）

/**
 * 是否展開側邊欄：true=展開（預設），false=收合（套用 .close）
 */
const isSidebarOpen = ref(true)

/**
 * 目前展開中的子選單索引：0/1/…；null = 全部關閉
 * 此例有兩個下拉（新增、待辦），分別用 0 與 1
 */
const openSubmenuIndex = ref(null)

/** 切換整個側欄的收合狀態，同時關閉所有子選單 */
function toggleSidebar() {
  isSidebarOpen.value = !isSidebarOpen.value
  if (!isSidebarOpen.value) openSubmenuIndex.value = null
}

/**
 * 切換某個子選單
 * - 若點到別的子選單：互斥打開
 * - 若點到同一個：關閉
 * - 側欄若目前是收合，會自動展開
 */
function toggleSubMenu(index) {
  openSubmenuIndex.value = openSubmenuIndex.value === index ? null : index
  if (!isSidebarOpen.value) isSidebarOpen.value = true
}
</script>

<template>
  <nav id="sidebar" :class="{ close: !isSidebarOpen }">
    <ul>
      <li>
        <span class="logo">myLogo</span>
        <button id="toggle-btn" :class="{ rotate: !isSidebarOpen }" @click="toggleSidebar">
          <!-- <i class="fa-solid fa-angles-left"></i> -->
          <v-icon name="md-keyboarddoublearrowleft-outlined"></v-icon>
        </button>
      </li>

      <li class="active">
        <a href="index.html">
          <v-icon name="hi-solid-home"></v-icon>
          <span>首頁</span>
        </a>
      </li>

      <li>
        <a href="dashboard.html">
          <v-icon name="md-dashboard-round"></v-icon>
          <span>儀表板</span>
        </a>
      </li>

      <!-- 下拉式選單 #1：新增 -->
      <li>
        <button
          class="dropdown-btn"
          :class="{ rotate: openSubmenuIndex === 0 }"
          @click="toggleSubMenu(0)"
        >
          <v-icon name="md-createnewfolder"></v-icon>
          <span>新增</span>
          <v-icon name="fa-angle-down"></v-icon>
        </button>
        <ul class="sub-menu" :class="{ show: openSubmenuIndex === 0 }">
          <div>
            <li><a href="#">資料夾</a></li>
            <li><a href="#">文件</a></li>
            <li><a href="#">專案</a></li>
          </div>
        </ul>
      </li>

      <!-- 下拉式選單 #2：待辦清單 -->
      <li>
        <button
          class="dropdown-btn"
          :class="{ rotate: openSubmenuIndex === 1 }"
          @click="toggleSubMenu(1)"
        >
          <v-icon name="md-checklist-round"></v-icon>
          <span>待辦清單</span>
          <v-icon name="fa-angle-down"></v-icon>
        </button>
        <ul class="sub-menu" :class="{ show: openSubmenuIndex === 1 }">
          <div>
            <li><a href="#">工作</a></li>
            <li><a href="#">個人</a></li>
            <li><a href="#">Coding</a></li>
          </div>
        </ul>
      </li>

      <li>
        <a href="calendar.html">
          <v-icon name="bi-calendar-fill"></v-icon>
          <span>行事曆</span>
        </a>
      </li>

      <li>
        <a href="profile.html">
          <v-icon name="fa-user-alt"></v-icon>
          <span>帳戶</span>
        </a>
      </li>
    </ul>
  </nav>
</template>

<style>
/* === 版面與色彩：請在全域定義以下變數 ===
   :root {
     --base-clr: #0b0b0c;
     --text-clr: #e6e6e6;
     --secondary-text-clr: #a1a1a1;
     --hover-clr: #1a1a1d;
     --accent-clr: #6aa4ff;
     --line-clr: #242428;
   }
*/

body {
  min-height: 100vh;
  min-height: 100dvh;
  background-color: var(--base-clr);
  color: var(--text-clr);
  display: grid;
  grid-template-columns: auto 1fr;
}

#sidebar {
  box-sizing: border-box;
  height: 100vh;
  width: 250px;
  padding: 5px 1em;
  background-color: var(--base-clr);
  border-right: 1px solid var(--line-clr);
  position: sticky;
  top: 0;
  align-self: start;
  transition: 300ms ease-in-out;
  overflow: hidden;
  text-wrap: nowrap;
}
#sidebar.close {
  padding: 5px;
  width: 55px;
}
#sidebar ul {
  list-style: none;
}
#sidebar > ul > li:first-child {
  display: flex;
  justify-content: flex-end;
  margin-bottom: 16px;
}
#sidebar > ul > li:first-child .logo {
  font-weight: 600;
}
#sidebar ul li.active a,
#sidebar ul li.active svg {
  color: var(--accent-clr);
}
#sidebar a,
#sidebar .dropdown-btn,
#sidebar .logo {
  border-radius: 0.5em;
  padding: 0.85em;
  text-decoration: none;
  color: var(--text-clr);
  display: flex;
  align-items: center;
  gap: 1em;
}
#sidebar svg {
  color: var(--text-clr);
  flex-shrink: 0; /* 防止變形 */
}
#sidebar a span,
#sidebar .dropdown-btn span {
  flex-grow: 1;
}
#sidebar a:hover,
#sidebar .dropdown-btn:hover {
  background-color: var(--hover-clr);
}

/* 下拉選單用 grid，避免 height:auto 無法動畫的問題 */
#sidebar .sub-menu {
  display: grid;
  grid-template-rows: 0fr;
  transition: 300ms ease-in-out;
}
#sidebar .sub-menu > div {
  overflow: hidden;
}
#sidebar .sub-menu.show {
  grid-template-rows: 1fr;
}
.rotate svg:last-child {
  rotate: 180deg;
}
#sidebar .sub-menu a {
  padding-left: 2em;
}

/* 側欄收合按鈕 */
#toggle-btn {
  margin-left: auto;
  padding: 1em;
  border: none;
  border-radius: 0.5em;
  background: none;
  cursor: pointer;
}
#toggle-btn i {
  transition: rotate 150ms ease;
}
#toggle-btn:hover {
  background-color: var(--hover-clr);
}

/* 下拉按鈕外觀（全寬、無邊框） */
.dropdown-btn {
  width: 100%;
  text-align: left;
  background: none;
  border: none;
  font: inherit;
  cursor: pointer;
}

main {
  padding: min(30px, 7%);
}
main p {
  color: var(--secondary-text-clr);
  margin-top: 5px;
  margin-bottom: 15px;
}
.container {
  border: 1px solid var(--line-clr);
  border-radius: 1em;
  margin-bottom: 20px;
  padding: min(3em, 15%);
}
.container h2,
.container p {
  margin-top: 1em;
}

/* 手機版：底部工具列樣式 */
@media (max-width: 800px) {
  body {
    grid-template-columns: 1fr;
  }
  main {
    padding: 2em 1em 60px 1em;
  }
  .container {
    border: none;
    padding: 0;
  }
  #sidebar {
    height: 50px;
    width: 100%;
    border-right: none;
    border-top: 1px solid var(--line-clr);
    padding: 0;
    position: fixed;
    top: unset;
    bottom: 0;
  }
  #sidebar > ul {
    padding: 0;
    display: grid;
    grid-auto-columns: 60px;
    grid-auto-flow: column;
    align-items: center;
    overflow-x: scroll;
  }
  #sidebar ul li {
    height: 100%;
  }
  #sidebar ul a,
  #sidebar ul .dropdown-btn {
    width: 50px;
    height: 50px;
    padding: 0;
    border-radius: 0;
    justify-content: center;
  }
  #sidebar ul li .sub-menu.show {
    position: fixed;
    bottom: 50px;
    left: 0;
    box-sizing: border-box;
    height: 60px;
    width: 100%;
    background-color: var(--hover-clr);
    border-top: 1px solid var(--line-clr);
    display: flex;
    justify-content: center;
  }
  #sidebar ul li .sub-menu.show > div {
    overflow-x: auto;
  }
  #sidebar ul li .sub-menu.show li {
    display: inline-flex;
  }
  #sidebar ul li .sub-menu.show a {
    box-sizing: border-box;
    padding: 1em;
    width: auto;
    justify-content: center;
  }
  #sidebar ul li span,
  #sidebar ul li:first-child,
  #sidebar .dropdown-btn i:last-child {
    display: none;
  }
}
</style>
