<script lang="ts">
  // export let icon: string;
  export let id: string;
  export let title: string;
  export let description: string;
  export let link: string | undefined;
  export let type: "suites" | "snippets" | "usecases";
  export let installCmd: string | undefined = undefined;
  export let featured: boolean | undefined = undefined;

  import copy from "copy-text-to-clipboard";

  import suiteIcon from "@/assets/icon/suite.svg";
  import snippetIcon from "@/assets/icon/snippet.svg";
  import usecaseIcon from "@/assets/icon/usecase.svg";

  function onUsecaseShow() {
    const usecase = document.getElementById(id);
    const usecaseTitle = document.getElementById("popover-title");
    const usecaseContainer = document.getElementById("popover-content-container");
    if (usecaseTitle && usecaseContainer && usecase) {
      usecaseTitle.innerHTML = `${title} <span class="bg-blue-500 text-white h-full text-sm px-2 py-1 rounded-md">${type}</span>`;
      usecaseContainer.innerHTML = "";
      const content = usecase.cloneNode(true).childNodes;
      usecaseContainer.append(...content);
    }
  }
</script>

<div class="relative p-4 bg-gray-100 rounded-md shadow-md shadow-gray-200 hover:scale-105 hover:shadow-violet-200 hover:shadow-lg transition-transform duration-300 ease-in-out">
  {#if featured}
    <div class="absolute top-0 left-0 bg-blue-500 text-white text-sm px-2 py-1 rounded-md">
      {type}
    </div>
  {/if}

  <img
    class="mx-auto max-w-20 type-icon"
    src={type === "suites" ? suiteIcon.src
       : type === "snippets" ? snippetIcon.src
       : type === "usecases"? usecaseIcon.src
       : suiteIcon.src
    }
    alt="logo"
    width={64}
    height={64}
  />
  <h2 class="text-lg font-bold text-left">{title}</h2>
  <p class="text-sm">{description}</p>
  {#if installCmd}
    <div class="buttons mt-4 flex flex-grow justify-center">
      <button
        class="icon copy-icon bg-gray-500 rounded-md cursor-pointer flex"
        data-tip={installCmd}
        on:click={() => {
          copy(installCmd);
          alert("复制成功，请在终端（Terminal）中粘贴。");
        }}
      >
        终端执行
        <svg
          width="24"
          height="24"
          viewBox="0 0 24 24"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M2 11L2 18.9671C2 21.2763 4.13419 22.736 5.87083 21.6145L16.6598 14.6474C18.4467 13.4935 18.4467 10.5065 16.6598 9.35258L5.87084 2.38548C4.13419 1.26402 2 2.72368 2 5.03289V7"
            stroke="#1C274C"
            stroke-width="1.5"
            stroke-linecap="round"
          />
          <path
            d="M22 5V12M22 16V19"
            stroke="#1C274C"
            stroke-width="1.5"
            stroke-linecap="round"
          />
        </svg>
      </button>
      {#if link}
        <a
          class="icon download-icon bg-green-600 rounded-md cursor-pointer flex"
          href={link}
        >
          直接下载
          <svg
            width="24"
            height="24"
            viewBox="0 0 24 24"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M12 4L12 14M12 14L15 11M12 14L9 11"
              stroke="#1C274C"
              stroke-width="1.5"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
            <path
              d="M12 20C7.58172 20 4 16.4183 4 12M20 12C20 14.5264 18.8289 16.7792 17 18.2454"
              stroke="#1C274C"
              stroke-width="1.5"
              stroke-linecap="round"
            />
          </svg>
        </a>
      {/if}
    </div>
  {/if}

  {#if type === "usecases" || type === "snippets"}
    <div class="h-12"></div>
    <button on:click={onUsecaseShow}
            class="absolute bottom-2 left-2 right-2 h-10 bg-blue-500 text-white rounded-md p-2 hover:bg-blue-600 transition-colors"
            popovertarget="popover-container">
      查看
    </button>

  {/if}
</div>

<style>
 
  .buttons {
    font-size: 0.7rem;
    height: 40px;
    line-height: 40px;
    justify-content: space-between;
  }

  .type-icon {
    margin: 2.5rem auto 1rem;
  }

  .icon {
    padding: 0 1rem;
    color: #fff;
  }

  .icon svg {
    width: 16px;
    height: 16px;
    margin-left: 0.5rem;
    margin-top: 12px;
  }
</style>
