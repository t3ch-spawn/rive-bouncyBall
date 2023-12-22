<script>
    import SplitType from "split-type";
  import gsap from "gsap"
  import { onMount } from "svelte";
  import {Rive} from "@rive-app/canvas"
  let heading
  let ballClick
  let ballClick2
  let isBtnPressed = false
  

  onMount(()=>{

    const r = new Rive({
        src: "/eye_ball.riv",
        // Or the path to a public Rive asset
        // src: '/public/example.riv',
        // @ts-ignore
        canvas: document.getElementById("canvas"),
        autoplay: true,
        stateMachines: "eye ball Bounce",
        onLoad: () => {
          r.resizeDrawingSurfaceToCanvas();
          const inputs = r.stateMachineInputs('eye ball Bounce')
          console.log(inputs)
          // @ts-ignore
          const startBounce = inputs.find((input)=> {input.runtimeInput.name == "start"})
          // @ts-ignore
          const endBounce = inputs.find((input)=> {input.runtimeInput.name == "end"})
          ballClick.addEventListener('click', (e)=>{
            // console.log('hey')
            // startBounce.fire()
            inputs[1].fire()
          })
          ballClick2.addEventListener('click', (e)=>{
            // console.log('hey')
            // endBounce.fire()
            inputs[0].fire()
          })

        },
        // @ts-ignore
        stateMachines: 'eye ball Bounce'
    });
    // const heading = document.querySelector('.heading')
    let headingChars = new SplitType('.heading', {types: "words"})

    gsap.timeline().from(heading.querySelectorAll('.word'), { y: 100, opacity: 0, stagger: 0.05, ease: "power1.inOut"})
    .from(document.querySelectorAll('.bounce-btns'), {
        y: 200,
        opacity: 0,
        duration: 0.8,
        stagger: 0.25,
        ease: 'power1.inOut'
    })

  })

  function btnPressed(e){
      const btn = e.target
      btn.classList.toggle('active')
      console.log(btn)
  }

  function cursorAway(e){
      e.target.classList.remove('active')
  }


</script>

<main>

     <div class="w-full flex justify-center overflow-hidden">
      <h1 bind:this={heading} class="font-grotesk heading text-4xl text-center mt-[100px] max-w-[700px] font-semibold"> Click to Bounce the Ball! </h1>
     </div>
<div class="flex gap-6 mx-auto w-fit text-center -500:flex-col mt-8 overflow-hidden">
  <!-- svelte-ignore a11y-no-static-element-interactions -->
  <div  class={`w-fit bounce-btns border-2 rounded-xl scale-100 border-black p-5 cursor-pointer`} on:mouseleave={cursorAway} on:mouseup={btnPressed} on:mousedown={btnPressed} bind:this={ballClick}>Start Bouncing</div>
  <!-- svelte-ignore a11y-no-static-element-interactions -->
  <div class={`w-fit bounce-btns border-2 rounded-xl scale-100 border-black p-5 cursor-pointer`} on:mouseleave={cursorAway} on:mouseup={btnPressed} on:mousedown={btnPressed} bind:this={ballClick2}>Stop Bouncing</div>
</div>
     <!-- <div id="canvas"></div> -->
     <canvas class="mx-auto max-w-[600px] w-[110%] max-h-[600px] mt-8 flex" id="canvas"></canvas>


</main>

<style>


  .bounce-btns.active{
    transform: scale(0.9) !important;
    transition: 0.2s;
  }

</style>
