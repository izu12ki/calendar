<script lang="ts">
  import Week from "./Week.svelte";

  let currentDay: Date;
  let sundays: Date[];

  const setCurrentDay = (currentDay_: Date) => {
    currentDay = currentDay_;

    // 今月1日
    const firstDayOfMonth = new Date(currentDay);
    firstDayOfMonth.setDate(1);

    // 1日が属する週の日曜日
    const firstDayOfFirstWeek = new Date(firstDayOfMonth);
    firstDayOfFirstWeek.setDate(1 - firstDayOfMonth.getDay());

    // 表示するすべての日曜日
    sundays = Array.from(Array(6).keys(), (i) => {
      const sunday = new Date(firstDayOfFirstWeek);
      sunday.setDate(sunday.getDate() + 7 * i);
      return sunday;
    }).filter(
      (date, i) => i === 0 || date.getMonth() === currentDay.getMonth()
    );
  };

  setCurrentDay(new Date());

  const goToPrevMonth = () => {
    currentDay.setMonth(currentDay.getMonth() - 1);
    setCurrentDay(currentDay);
  };

  const goToNextMonth = () => {
    currentDay.setMonth(currentDay.getMonth() + 1);
    setCurrentDay(currentDay);
  };
</script>

<div>
  <span class="month-control" role="button" on:click={goToPrevMonth}>&lt;</span>
  <h1>{currentDay.getFullYear()}年{currentDay.getMonth() + 1}月</h1>
  <span class="month-control" role="button" on:click={goToNextMonth}>&gt;</span>
</div>
<div class="calendar">
  <div class="week-header">
    {#each ["日", "月", "火", "水", "木", "金", "土"] as dow}
      <div class="dow">{dow}</div>
    {/each}
  </div>
  <div class="days">
    {#each sundays as sunday}
      <Week startDate={sunday} />
    {/each}
  </div>
</div>

<style>
  h1 {
    display: inline-block;
    min-width: 13rem;
    margin: 0;
  }

  .calendar {
    display: flex;
    flex-direction: column;
    flex: 1;
    padding-bottom: 18px;
  }

  .week-header {
    display: flex;
  }

  .dow {
    flex: 1;
    border: 1px solid #cccccc;
  }

  .dow:nth-child(n + 2) {
    border-left-width: 0;
  }

  .days {
    display: flex;
    flex-direction: column;
    flex: 1;
  }

  .month-control {
    display: inline-block;
    color: #666;
    font-size: 1.8rem;
    width: 1.8rem;
    height: 1.8rem;
    line-height: 0.9;
    border-radius: 50%;
    cursor: pointer;
  }

  .month-control:hover {
    background-color: #e6e6e6;
  }
</style>
