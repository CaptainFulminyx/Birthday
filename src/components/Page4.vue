<template>
  <div class="calendar-wrapper">
    <div class="calendar">
      <!-- Header -->
      <div class="calendar-header">
        <button @click="prevMonth" class="nav-btn" aria-label="Previous month">
          <svg
            width="24"
            height="24"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
          >
            <path d="M15 18l-6-6 6-6" />
          </svg>
        </button>
        <div class="month-year">
          <span class="month">{{ monthName }}</span>
          <span class="year">{{ year }}</span>
        </div>
        <button @click="nextMonth" class="nav-btn" aria-label="Next month">
          <svg
            width="24"
            height="24"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
          >
            <path d="M9 18l6-6-6-6" />
          </svg>
        </button>
      </div>

      <!-- Weekdays -->
      <div class="weekdays">
        <div v-for="day in WEEKDAYS" :key="day" class="weekday">
          {{ day }}
        </div>
      </div>

      <!-- Days -->
      <div class="days-grid">
        <div
          v-for="day in calendarDays"
          :key="day.date"
          class="day-cell"
          :class="{
            'other-month': !day.isCurrentMonth,
            today: day.isToday,
            highlighted: day.isHighlighted,
            weekend: day.isWeekend,
          }"
        >
          <span class="day-number">{{ day.day }}</span>
          <div v-if="day.isHighlighted" class="highlight-badge">🎯</div>
        </div>
      </div>

      <!-- Legend -->
      <div class="legend">
        <div class="legend-item">
          <span class="legend-dot highlighted-dot"></span>
          <span>Highlighted (31st)</span>
        </div>
        <div class="legend-item">
          <span class="legend-dot today-dot"></span>
          <span>Today</span>
        </div>
      </div>
    </div>
  </div>
</template>
