<template>
  <div id="q-app" style="min-height: 100vh">
    <div>
      <q-layout view="hHh Lpr lff" container style="height: 100vh">
        <q-drawer
          v-model="drawer"
          show-if-above
          :mini="!drawer || miniState"
          @click.capture="drawerClick"
          :width="300"
          :breakpoint="768"
          bordered
          :class="$q.dark.isActive ? 'bg-blue-13' : 'bg-blue-grey'"
        >
          <q-scroll-area class="fit" :horizontal-thumb-style="{ opacity: 0 }">
            <q-list padding>
              <br />
              <q-item class="q-ma-md text-white" clickable v-ripple>
                <q-item-section
                  avatar
                  class="self-start text-h5 text-weight-bold"
                  :class="{ iconHide: !miniState }"
                >
                  <q-icon name="E"></q-icon>
                  <!-- <img width="25" height="25"src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiIHN0YW5kYWxvbmU9Im5vIj8+CjwhLS0gQ3JlYXRlZCB3aXRoIElua3NjYXBlIChodHRwOi8vd3d3Lmlua3NjYXBlLm9yZy8pIC0tPgoKPHN2ZwogICB4bWxuczpkYz0iaHR0cDovL3B1cmwub3JnL2RjL2VsZW1lbnRzLzEuMS8iCiAgIHhtbG5zOmNjPSJodHRwOi8vY3JlYXRpdmVjb21tb25zLm9yZy9ucyMiCiAgIHhtbG5zOnJkZj0iaHR0cDovL3d3dy53My5vcmcvMTk5OS8wMi8yMi1yZGYtc3ludGF4LW5zIyIKICAgeG1sbnM6c3ZnPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIKICAgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIgogICB4bWxuczpzb2RpcG9kaT0iaHR0cDovL3NvZGlwb2RpLnNvdXJjZWZvcmdlLm5ldC9EVEQvc29kaXBvZGktMC5kdGQiCiAgIHhtbG5zOmlua3NjYXBlPSJodHRwOi8vd3d3Lmlua3NjYXBlLm9yZy9uYW1lc3BhY2VzL2lua3NjYXBlIgogICB3aWR0aD0iMTgwIgogICBoZWlnaHQ9IjE4MCIKICAgdmlld0JveD0iMCAwIDQ3LjYyNSA0Ny42MjUwMDEiCiAgIHZlcnNpb249IjEuMSIKICAgaWQ9InN2ZzgiCiAgIGlua3NjYXBlOnZlcnNpb249IjAuOTIuMyAoMjQwNTU0NiwgMjAxOC0wMy0xMSkiCiAgIHNvZGlwb2RpOmRvY25hbWU9ImV4YW1zLnN2ZyIKICAgaW5rc2NhcGU6ZXhwb3J0LWZpbGVuYW1lPSIvaG9tZS9kZXh0ZXIvUGljdHVyZXMvZWQtMzIweDEzMi5wbmciCiAgIGlua3NjYXBlOmV4cG9ydC14ZHBpPSIzMDAiCiAgIGlua3NjYXBlOmV4cG9ydC15ZHBpPSIzMDAiPgogIDxkZWZzCiAgICAgaWQ9ImRlZnMyIiAvPgogIDxzb2RpcG9kaTpuYW1lZHZpZXcKICAgICBpZD0iYmFzZSIKICAgICBwYWdlY29sb3I9IiNmZmZmZmYiCiAgICAgYm9yZGVyY29sb3I9IiM2NjY2NjYiCiAgICAgYm9yZGVyb3BhY2l0eT0iMS4wIgogICAgIGlua3NjYXBlOnBhZ2VvcGFjaXR5PSIwLjAiCiAgICAgaW5rc2NhcGU6cGFnZXNoYWRvdz0iMiIKICAgICBpbmtzY2FwZTp6b29tPSIxLjQiCiAgICAgaW5rc2NhcGU6Y3g9IjI1NC41NDAzMyIKICAgICBpbmtzY2FwZTpjeT0iLTE3LjIzMTYyMiIKICAgICBpbmtzY2FwZTpkb2N1bWVudC11bml0cz0ibW0iCiAgICAgaW5rc2NhcGU6Y3VycmVudC1sYXllcj0ibGF5ZXIxIgogICAgIHNob3dncmlkPSJmYWxzZSIKICAgICBmaXQtbWFyZ2luLXRvcD0iMCIKICAgICBmaXQtbWFyZ2luLWxlZnQ9IjAiCiAgICAgZml0LW1hcmdpbi1yaWdodD0iMCIKICAgICBmaXQtbWFyZ2luLWJvdHRvbT0iMCIKICAgICBpbmtzY2FwZTp3aW5kb3ctd2lkdGg9IjI1NjAiCiAgICAgaW5rc2NhcGU6d2luZG93LWhlaWdodD0iMTAyNSIKICAgICBpbmtzY2FwZTp3aW5kb3cteD0iMCIKICAgICBpbmtzY2FwZTp3aW5kb3cteT0iMjciCiAgICAgaW5rc2NhcGU6d2luZG93LW1heGltaXplZD0iMSIKICAgICBzaG93Z3VpZGVzPSJ0cnVlIgogICAgIGlua3NjYXBlOmd1aWRlLWJib3g9InRydWUiCiAgICAgdW5pdHM9InB4Ij4KICAgIDxzb2RpcG9kaTpndWlkZQogICAgICAgcG9zaXRpb249IjYzLjg3NzQxNiwtMjIwLjM2MzczIgogICAgICAgb3JpZW50YXRpb249IjEsMCIKICAgICAgIGlkPSJndWlkZTEwMjEiCiAgICAgICBpbmtzY2FwZTpsb2NrZWQ9ImZhbHNlIiAvPgogICAgPHNvZGlwb2RpOmd1aWRlCiAgICAgICBwb3NpdGlvbj0iODkuOTM2MTk1LC0yMjIuNzY5MTYiCiAgICAgICBvcmllbnRhdGlvbj0iMSwwIgogICAgICAgaWQ9Imd1aWRlMTAyMyIKICAgICAgIGlua3NjYXBlOmxvY2tlZD0iZmFsc2UiIC8+CiAgPC9zb2RpcG9kaTpuYW1lZHZpZXc+CiAgPG1ldGFkYXRhCiAgICAgaWQ9Im1ldGFkYXRhNSI+CiAgICA8cmRmOlJERj4KICAgICAgPGNjOldvcmsKICAgICAgICAgcmRmOmFib3V0PSIiPgogICAgICAgIDxkYzpmb3JtYXQ+aW1hZ2Uvc3ZnK3htbDwvZGM6Zm9ybWF0PgogICAgICAgIDxkYzp0eXBlCiAgICAgICAgICAgcmRmOnJlc291cmNlPSJodHRwOi8vcHVybC5vcmcvZGMvZGNtaXR5cGUvU3RpbGxJbWFnZSIgLz4KICAgICAgICA8ZGM6dGl0bGU+PC9kYzp0aXRsZT4KICAgICAgPC9jYzpXb3JrPgogICAgPC9yZGY6UkRGPgogIDwvbWV0YWRhdGE+CiAgPGcKICAgICBpbmtzY2FwZTpsYWJlbD0iTGF5ZXIgMSIKICAgICBpbmtzY2FwZTpncm91cG1vZGU9ImxheWVyIgogICAgIGlkPSJsYXllcjEiCiAgICAgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMTYyLjY3NzI3LC0xNTEuNTIwMikiPgogICAgPGcKICAgICAgIGlkPSJnMTA2NyIKICAgICAgIHRyYW5zZm9ybT0ibWF0cml4KDAuOTUyNDk5OTgsMCwwLDAuOTUyNDk5OTgsLTcuNzI3MTcyNiw2LjYxODc0MzEpIj4KICAgICAgPHBhdGgKICAgICAgICAgaWQ9InBhdGg5NjQiCiAgICAgICAgIGQ9Im0gLTEzMy42MDc4MywxOTguODY3MjUgYyAtMi4wODUzMiwtMC40NzY0MiAtNS4yNDA2NywtMS43NjAxNSAtNi41NjM1NCwtMi42NzAzMSAtMC41OTIwMiwtMC40MDczMiAtMS44MTgxOSwtMS41MzA0NSAtMi43MjQ4MywtMi40OTU4NiAtMS4yODg4MiwtMS4zNzIzNiAtMS44ODM1MiwtMi4yODc5NiAtMi43MjYwMiwtNC4xOTcgLTEuMTI3MDEsLTIuNTUzNzMgLTEuOTQwNDgsLTUuMjI1NDMgLTEuNzA2MzUsLTUuNjA0MjYgMC4wNzU1LC0wLjEyMjA2IDEuNTM1NzUsLTAuMjE4ODIgMy4yNDUxMiwtMC4yMTUwMSBsIDMuMTA3OTYsMC4wMDcgMC45NjI1NCwyLjAzMzU3IGMgMi4yMDU2NSw0LjY1OTkgNi44MTQxLDcuMjQ3OTMgMTEuNDgyMzIsNi40NDgzIDkuMTUyNTksLTEuNTY3NzcgMTIuNTYzNTgsLTEzLjM1ODM1IDUuNzcxNDYsLTE5Ljk0OTg3IC0xLjM3NTIzLC0xLjMzNDYzIC00LjIwMTUyLC0yLjczODgxIC01Ljk1MzAyLC0yLjk1NzY0IC0xLjMyMTEsLTAuMTY1MDcgLTEuNDQxMywtMC4yNDUzNSAtMi43MTE5MywtMS44MTE0NCAtMC43MzEyNiwtMC45MDEyOSAtMi4wMDY5MywtMi4xNzA4OCAtMi44MzQ4MywtMi44MjEzMiAtMC44Mjc5LC0wLjY1MDQzIC0xLjQwODM3LC0xLjI3OTUxIC0xLjI4OTk0LC0xLjM5Nzk1IDAuMTE4NDQsLTAuMTE4NDQgMS4wNjgyNywtMC4zOTcwNyAyLjExMDc0LC0wLjYxOTE5IDMuODI5MjgsLTAuODE1ODkgOC41NzY4MiwwLjAxMDIgMTEuOTk2MzksMi4wODc1MiAwLjc5MDg4LDAuNDgwNDIgMS41NTQwOCwwLjg3MzUgMS42OTYwMSwwLjg3MzUgMC40ODU3NywwIDAuNTY3NTQsLTAuODEyMiAwLjU2NzU0LC01LjYzNzEyIHYgLTQuODMwNjEgbCAzLjI1MDQ3LDAuMDcxMyAzLjI1MDQ3LDAuMDcxMyB2IDE0LjI3NjU3IGMgMCwxNC4xNDAzNyAtMC4wMDYsMTQuMjk1OTEgLTAuNTg2MzYsMTYuMzAxNjggLTIuMjYzMDEsNy44MTU2NCAtOC42MTE1NSwxMy4wMTgzIC0xNi4yMjY4OSwxMy4yOTgwMiAtMS42MTE4MywwLjA1OTIgLTMuMTM4OTMsLTAuMDM3NyAtNC4xMTczMSwtMC4yNjEyMiB6IgogICAgICAgICBzdHlsZT0iZmlsbDojNDA2YWIyO2ZpbGwtb3BhY2l0eToxO3N0cm9rZS13aWR0aDowLjI1NDkzODkzIgogICAgICAgICBpbmtzY2FwZTpjb25uZWN0b3ItY3VydmF0dXJlPSIwIiAvPgogICAgICA8cGF0aAogICAgICAgICBpZD0icGF0aDk0NCIKICAgICAgICAgZD0ibSAtMTQ5Ljc0ODQ1LDE5OC4xOTY0IGMgLTkuNDU2NzgsLTIuNDc2MzYgLTE1LjM1MDI2LC0xNC41Njk5NCAtMTEuOTY0MDcsLTI0LjU1MDYyIDIuMDgzNywtNi4xNDE1OSA2LjYzNTM2LC0xMC4zODc4MSAxMi44MDgzNywtMTEuOTQ4ODMgMi40MDUyOSwtMC42MDgyNSA2LjIxMjI4LC0wLjUyNzYgOC41MjA1OCwwLjE4MDUgNC42MTc4NSwxLjQxNjU3IDguNTAwOTgsNC41NDc2OCAxMC40Mzc1Miw4LjQxNjE0IDEuNjg5MDksMy4zNzQxMyAyLjE0MzksNS4xNjEwNiAyLjI3MTA3LDguOTIyODcgMC4wNjE3LDEuODIyODEgMC4wMTk2LDMuNDQwMzIgLTAuMDkzMywzLjU5NDQ3IC0wLjEzOTE2LDAuMTg5OTQgLTMuMzQ4MDYsMC4zMzUyIC05Ljk1Njc1LDAuNDUwNzEgbCAtOS43NTE0MSwwLjE3MDQ0IHYgLTMuNzAxMTkgLTMuNzAxMTcgaCA2LjEyNjUzIGMgNi45MjQxLDAgNi44NTI4MSwwLjAyMjUgNS45MjM1MiwtMS44NzUzNiAtMS44NDQsLTMuNzY2MDcgLTYuMjM5NTMsLTYuMjcwODYgLTEwLjQ0NjE1LC01Ljk1MjcxIC03LjEyMzEzLDAuNTM4NzIgLTExLjkxNzM2LDcuOTg3MjUgLTkuNzM4ODUsMTUuMTMwNzEgMC44NjYyNCwyLjg0MDQzIDIuNjMzNTUsNS4xNTE1IDUuMDc1NjgsNi42MzczNyAxLjA5NTksMC42NjY3OCAzLjM3NjU1LDEuMzU2MzMgNC40ODU5OCwxLjM1NjMzIDAuNjkwMjksMCAwLjk3MDksMC4xOTk2OCAxLjcyNTg3LDEuMjI4MTggMS4wNjM0OCwxLjQ0ODc1IDMuMDExNDksMy4zMTUyMiA0LjE3NjkxLDQuMDAyMDkgMC40NTU3LDAuMjY4NTcgMC44Mjg1NSwwLjU1NTY1IDAuODI4NTUsMC42Mzc5MyAwLDAuMDgyMyAtMC41NDQ5MywwLjMzNjgxIC0xLjIxMDk2LDAuNTY1NjEgLTMuMTQyNzEsMS4wNzk2IC02LjIwODg3LDEuMjI0NzkgLTkuMjE5MSwwLjQzNjUzIHoiCiAgICAgICAgIHN0eWxlPSJmaWxsOiNlYzMyMzc7ZmlsbC1vcGFjaXR5OjE7c3Ryb2tlLXdpZHRoOjAuMjU0OTM4OTMiCiAgICAgICAgIGlua3NjYXBlOmNvbm5lY3Rvci1jdXJ2YXR1cmU9IjAiIC8+CiAgICA8L2c+CiAgPC9nPgo8L3N2Zz4K" alt="Image"> -->
                </q-item-section>

                <q-item-section class="text-h5 text-center text-weight-bold">
                  Edex Tech
                </q-item-section>
              </q-item>
              <br />
              <br />
              <q-item
                class="text-white"
                clickable
                v-ripple
                :style="{
                  backgroundColor:
                    activeItem === 'IndexPage'
                      ? 'rgba(0, 0, 0, 0.2)'
                      : 'transparent',
                }"
                :class="{ active: activeItem === 'IndexPage' }"
                @click="activateItem('IndexPage')"
              >
                <q-item-section class="text-white" avatar>
                  <q-icon name="account_box"></q-icon>
                </q-item-section>

                <q-item-section class="text-grey-4 text-h6">
                  Generate Offer Letter
                </q-item-section>
              </q-item>
              <br />
              <q-item
                class="text-white"
                clickable
                v-ripple
                :style="{
                  backgroundColor:
                    activeItem === 'CreateLetter' ? 'rgba(0, 0, 0, 0.2)' : 'transparent',
                }"
                :class="{ active: activeItem === 'CreateLetter' }"
                @click="activateItem('CreateLetter')"
              >
                <q-item-section class="text-white" avatar>
                  <q-icon name="equalizer"></q-icon>
                </q-item-section>

                <q-item-section class="text-grey-4 text-h6">
                  Create Letter
                </q-item-section>
              </q-item>
              <br />
              <q-item
                class="text-white"
                clickable
                v-ripple
                :style="{
                  backgroundColor:
                    activeItem === 'LetterTemplates' ? 'rgba(0, 0, 0, 0.2)' : 'transparent',
                }"
                :class="{ active: activeItem === 'LetterTemplates' }"
                @click="activateItem('LetterTemplates')"
              >
                <q-item-section avatar>
                  <q-icon name="note"></q-icon>
                </q-item-section>

                <q-item-section class="text-grey-4 text-h6">
                  Letter Templates
                </q-item-section>
              </q-item>
              <br />
              <q-item
                class="text-white"
                clickable
                v-ripple
                :style="{
                  backgroundColor:
                    activeItem === 'GenerateAndSend' ? 'rgba(0, 0, 0, 0.2)' : 'transparent',
                }"
                :class="{ active: activeItem === 'GenerateAndSend' }"
                @click="activateItem('GenerateAndSend')"
              >
                <q-item-section avatar>
                  <q-icon name="mail"></q-icon>
                </q-item-section>

                <q-item-section class="text-grey-4 text-h6">
                  Generate and Send
                </q-item-section>
              </q-item>
            </q-list>
          </q-scroll-area>

          <!--
            in this case, we use a button (can be anything)
            so that user can switch back
            to mini-mode
          -->
          <div class="q-mini-drawer absolute" style="top: 50px; right: -19px">
            <q-btn
              dense
              round
              unelevated
              class="text-light"
              color="blue-grey"
              :icon="miniState ? 'chevron_right' : 'chevron_left'"
              @click="toggleMiniState"
            ></q-btn>
          </div>
        </q-drawer>
        <div class="lt-md">
          <div class="row q-pa-md bg-blue justify-center items-center">
            <div
              class="col-4 text-h5 text-weight-bold text-center text-blue-grey-9"
            >
              Edex Tech
            </div>
          </div>
        </div>

        <q-page-container>
          <router-view />
        </q-page-container>
      </q-layout>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      drawer: false,
      miniState: false,
      activeItem: "IndexPage",
    };
  },
  methods: {
    toggleMiniState() {
      this.miniState = !this.miniState;
    },
    drawerClick(e) {
      if (this.miniState) {
        this.miniState = false;
        e.stopPropagation();
      }
    },
    activateItem(item) {
      this.activeItem = item;
      if (this.activeItem === "GenerateAndSend") {
        console.log("GenerateAndSend");
        // Assuming `$router` is available, otherwise, you need to pass it as an argument or use inject
        this.$router.push({ path: "/GenerateAndSend" });
      } else if (this.activeItem === "CreateLetter") {
        console.log("CreateLetter");
        this.$router.push({ path: "/CreateLetter" });
      } else if (this.activeItem === "LetterTemplates") {
        console.log("LetterTemplates");
        this.$router.push({ path: "/LetterTemplates" });
      } else if (this.activeItem === "IndexPage") {
        console.log("IndexPage");
        this.$router.push({ path: "/" });
      }
    },
  },
  computed: {
    miniDrawerIcon() {
      return miniState.value ? "chevron_right" : "chevron_left";
    },
  },
};
</script>

<style>
* {
  /* color: white; */
  font-family: "Gill Sans", "Gill Sans MT", Calibri, "Trebuchet MS", sans-serif;
}
.iconHide {
  display: none;
}
</style>
