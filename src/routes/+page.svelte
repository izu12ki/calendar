<script>
  // @ts-nocheck
  import Calendar from "@event-calendar/core";
  import "@event-calendar/core/index.css";
  import DayGrid from "@event-calendar/day-grid";
  import Interaction from "@event-calendar/interaction";
  import List from "@event-calendar/list";
  import TimeGrid from "@event-calendar/time-grid";
  import { onMount } from "svelte";

  // import "../app.css";

  /**
   * @type {{ setOption: (arg0: string, arg1: Date) => void; }}
   */
  onMount(() => {
    let ec = new Calendar({
      target: document.getElementById("ec"),
      props: {
        plugins: [TimeGrid, Interaction, List, DayGrid],
        options: {
          view: "timeGridWeek",
          locale: "ja",
          headerToolbar: {
            start: "prev,next today",
            center: "title",
            end: "dayGridMonth,timeGridWeek,timeGridDay,listWeek resourceTimeGridWeek",
          },
          editable: true,
          buttonText: function (texts) {
            texts.resourceTimeGridWeek = "resources";
            return texts;
          },
          resources: [
            { id: 1, title: "Resource A" },
            { id: 2, title: "Resource B" },
          ],
          scrollTime: "09:00:00",
          events: createEvents(),
          views: {
            timeGridWeek: { pointer: true },
            resourceTimeGridWeek: { pointer: true },
          },
          dayMaxEvents: true,
          nowIndicator: true,
          // selectable: true,
          dateClick: function (/** @type {{ view: any; }} */ info) {
            console.log("aaa");
            console.log(info.dayEl.innerHTML);
            console.log("bbb");
            console.log(info.view);

            if (
              info.view.type == "timeGridWeek" ||
              info.view.type == "timeGridDay"
            ) {
              const days = getDays();
              console.log("days", days);
              ec.addEvent([
                {
                  start: days[0] + " 15:00",
                  end: days[0] + " 16:00",
                  resourceId: 1,
                  title: "test",
                  // display: "background",
                },
              ]);
            }
            changeview(info.view.type);
          },
        },
      },
    });
  });

  // let plugins = [TimeGrid, Interaction, List, DayGrid];
  // let options = {
  //   view: "timeGridWeek",
  //   locale: "ja",
  //   headerToolbar: {
  //     start: "prev,next today",
  //     center: "title",
  //     end: "dayGridMonth,timeGridWeek,timeGridDay,listWeek resourceTimeGridWeek",
  //   },
  //   editable: true,
  //   buttonText: function (texts) {
  //     texts.resourceTimeGridWeek = "resources";
  //     return texts;
  //   },
  //   resources: [
  //     { id: 1, title: "Resource A" },
  //     { id: 2, title: "Resource B" },
  //   ],
  //   scrollTime: "09:00:00",
  //   events: createEvents(),
  //   views: {
  //     timeGridWeek: { pointer: true },
  //     resourceTimeGridWeek: { pointer: true },
  //   },
  //   dayMaxEvents: true,
  //   nowIndicator: true,
  //   // selectable: true,
  //   dateClick: function (/** @type {{ view: any; }} */ info) {
  //     console.log("aaa");
  //     console.log(info.dayEl.innerHTML);
  //     console.log("bbb");
  //     console.log(info.view);

  //     if (info.view.type == "timeGridWeek" || info.view.type == "timeGridDay") {
  //       const days = getDays();
  //       console.log("days", days);
  //       ec.addEvent([
  //         {
  //           start: days[0] + " 15:00",
  //           end: days[0] + " 16:00",
  //           resourceId: 1,
  //           title: "test",
  //           // display: "background",
  //         },
  //       ]);
  //     }
  //     changeview(info.view.type);
  //   },
  // };

  function changeview(currentview) {
    if (currentview == "dayGridMonth") {
      ec.setOption("view", "timeGridWeek");
    }
  }

  function createEvents() {
    let days = [];
    for (let i = 0; i < 7; ++i) {
      let day = new Date();
      let diff = i - day.getDay();
      day.setDate(day.getDate() + diff);
      days[i] =
        day.getFullYear() +
        "-" +
        _pad(day.getMonth() + 1) +
        "-" +
        _pad(day.getDate());
    }

    return [
      {
        start: days[0] + " 00:00",
        end: days[0] + " 09:00",
        resourceId: 1,
        display: "background",
      },
      {
        start: days[1] + " 12:00",
        end: days[1] + " 14:00",
        resourceId: 2,
        display: "background",
      },
      {
        start: days[2] + " 17:00",
        end: days[2] + " 24:00",
        resourceId: 1,
        display: "background",
      },
      {
        start: days[0] + " 10:00",
        end: days[0] + " 14:00",
        resourceId: 1,
        title: "バックグラウンドイベンはグレーアウトできる",
        color: "#FE6B64",
      },
      {
        start: days[1] + " 16:00",
        end: days[2] + " 08:00",
        resourceId: 2,
        title: "日をまたぐイベントも可能",
        color: "#B29DD9",
      },
      {
        start: days[2] + " 09:00",
        end: days[2] + " 13:00",
        resourceId: 2,
        title: "イベントとリソースを紐付けることも出来る",
        color: "#779ECB",
      },
      {
        start: days[3] + " 14:00",
        end: days[3] + " 20:00",
        resourceId: 1,
        title: "",
        color: "#FE6B64",
      },
      {
        start: days[3] + " 15:00",
        end: days[3] + " 18:00",
        resourceId: 1,
        title: "同じ時間帯に複数のイベントを表示することも可能",
        color: "#779ECB",
      },
      {
        start: days[5] + " 10:00",
        end: days[5] + " 16:00",
        resourceId: 2,
        title: {
          html: "イベントの <i><b>スタイリング</b></i>は完全にコントロール可能",
        },
        color: "#779ECB",
      },
      {
        start: days[5] + " 14:00",
        end: days[5] + " 19:00",
        resourceId: 2,
        title: "ドラッグ＆ドロップも出来る",
        color: "#FE6B64",
      },
      {
        start: days[5] + " 18:00",
        end: days[5] + " 21:00",
        resourceId: 2,
        title: "",
        color: "#B29DD9",
      },
      {
        start: days[1],
        end: days[3],
        resourceId: 1,
        title: "終日イベントも表示可能",
        color: "#B29DD9",
        allDay: true,
      },
    ];
  }

  function _pad(num) {
    let norm = Math.floor(Math.abs(num));
    return (norm < 10 ? "0" : "") + norm;
  }

  function getDays() {
    let days = [];
    for (let i = 0; i < 1; ++i) {
      let day = new Date();
      let diff = i - day.getDay();
      day.setDate(day.getDate() + diff);
      days[i] =
        day.getFullYear() +
        "-" +
        _pad(day.getMonth() + 1) +
        "-" +
        _pad(day.getDate());
    }
    return days;
  }
</script>

<!-- <Calendar bind:this={ec} {plugins} {options} /> -->

<div id="ec"></div>
