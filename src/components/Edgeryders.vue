<template>
  <div class="section md:section-md" id="edgeryders">
    <div class="w-full flex flex-col items-start justify-between bg-white">
      <div
        class="w-full flex flex-col md:flex-row md:px-0 md:mt-4 leading-normal"
      >
        <div
          class="w-full pt-0 md:w-1/2 md:px-6 md:px-0 md:pr-8 md:pt-2 text-left md:text-right md:border-r border-gray-200"
        >
          <h3
            class="hidden font-display text-2xl font-heading text-center md:inline-block md:text-right md:mb-2 mt-2"
          >
            {{custom.header1}}
          </h3>
          <p class="text-lg">
            {{custom.text1}}
          </p>
          <a :href="custom.action1url" 
            ><button
              class="text-black bg-transparent border border-black text-xm font-heading px-4 py-3 mt-6 leading-normal"
            >
              {{custom.action1}}
            </button></a
          >
        </div>
        <div class="md:w-1/2 flex flex-col md:pl-8 mt-4">
          <div class="community_photo">
            <div class="photo">
            </div>
          </div>
        </div>
      </div>
      <div class="w-full md:flex md:px-0 text-left leading-normal">
        <div class="md:w-1/2 mt-0 md:pt-8 pr-8 md:border-r border-gray-200">
          <div class="house_photo">
            <div class="photo">
            </div>
          </div>
        </div>
        <div class="md:w-1/2 mt-2 px-0 pt-0 mb-12 md:pl-8 pt-4">
          <h3
            class="font-display leading-tight text-2xl font-heading text-left md:leading-normal mb-2"
          >
            {{custom.header2}}
          </h3>
          <p class="text-lg m-0 pr-0">
            {{custom.text2}}
          </p>
          <a :href="custom.action2url" target="_blank"
            ><button
              class="text-black bg-transparent border border-black text-xm font-heading px-4 py-3 mt-6 leading-normal"
            >
              {{custom.action2}}
            </button></a
          >
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["custom"]
};
</script>

<style type="text/css" lang="scss">
@import url("https://fonts.googleapis.com/css?family=Permanent+Marker&display=swap");

.community_photo {
  transform: rotate(0.1deg);
  .photo {
    background: url("https://forum.blivande.com/uploads/blivande/original/1X/f041776ff4502c8ae41651469d15846c5cb9ea0f.jpeg") no-repeat center 15px
      #f7f8f8;
    background-size: 95%;
    height: 300px !important;
    padding-bottom: 10px;
    position: relative;
    border-radius: 10px;
    width: 90%;
    left: 5%;
    margin-top: 2%;
    &:before,
    &:after {
      z-index: -1;
      position: absolute;
      content: "";
      bottom: 15px;
      left: 10px;
      width: 50%;
      top: 80%;
      max-width: 300px;
      background: #777;
      box-shadow: 0 15px 10px #777;
      transform: rotate(-3deg);
    }
    &:after {
      transform: rotate(3deg);
      right: 10px;
      left: auto;
    }
    h3 {
      position: absolute;
      bottom: 0%;
      left: 0%;
      width: 100%;
      text-align: center;
      color: black;
      font-size: 1.1em;
      font-family: "Trattatello", cursive;
    }
  }
}

.house_photo {
  transform: rotate(-1.2deg);
  .photo {
    background: url("https://forum.blivande.com/uploads/blivande/original/1X/46b8e0f9d376e5446378e898a106938da3dd549d.jpeg") no-repeat center 15px
      #f7f8f8;
    background-size: 95%;
    height: 300px !important;
    padding-bottom: 10px;
    position: relative;
    border-radius: 10px;
    width: 90%;
    left: 5%;
    margin-top: 2%;
    &:before,
    &:after {
      z-index: -1;
      position: absolute;
      content: "";
      bottom: 15px;
      left: 10px;
      width: 50%;
      top: 80%;
      max-width: 300px;
      background: #777;
      box-shadow: 0 15px 10px #777;
      transform: rotate(-3deg);
    }
    &:after {
      transform: rotate(3deg);
      right: 10px;
      left: auto;
    }
    h3 {
      position: absolute;
      bottom: 0%;
      left: 0%;
      width: 100%;
      text-align: center;
      color: black;
      font-size: 1.1em;
      font-family: "Trattatello", cursive;
    }
  }
}

@media (max-width: 640px) {
  .icon {
    height: 70px !important;
    width: 21%;
  }
  .er_logo {
    width: 50px;
  }
  .er_photo {
    .photo {
      height: 185px !important;
      width: 100%;
      padding-bottom: 30px;
      left: 0;
      h3 {
        bottom: 4%;
      }
    }
  }
}

@media (min-width: 640px) {
  .icon {
    height: 8.5vw !important;
  }
}
</style>
