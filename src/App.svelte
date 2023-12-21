<script>
    import SplitType from "split-type";
  import gsap from "gsap"
  import { onMount } from "svelte";
  import {Rive} from "@rive-app/canvas"
  let heading
  let ballClick
  let ballClick2
  

  onMount(()=>{

    const r = new Rive({
        src: "../new_file.riv",
        // Or the path to a public Rive asset
        // src: '/public/example.riv',
        // @ts-ignore
        canvas: document.getElementById("canvas"),
        autoplay: true,
        stateMachines: "State Machine 1",
        onLoad: () => {
          r.resizeDrawingSurfaceToCanvas();
          const inputs = r.stateMachineInputs('State Machine 1')
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
        stateMachines: 'State Machine 1'
    });
    // const heading = document.querySelector('.heading')
    let headingChars = new SplitType('.heading', {types: "words"})

    gsap.from(heading.querySelectorAll('.word'), { y: 100, opacity: 0, stagger: 0.05, ease: "power1.inOut"})
  })
</script>

<main>

     <div class="w-full flex justify-center overflow-hidden">
      <h1 bind:this={heading} class="font-grotesk heading text-4xl text-center mt-[100px] max-w-[700px] font-semibold"> Click to Bounce the Ball! </h1>
     </div>
<div class="flex gap-6 mx-auto w-fit text-center -500:flex-col mt-8">
  <div class="w-fit  border-2 rounded-xl border-black p-5 cursor-pointer" bind:this={ballClick}>Start Bouncing</div>
  <div class="w-fit border-2 rounded-xl border-black p-5 cursor-pointer" bind:this={ballClick2}>Stop Bouncing</div>
</div>
     <!-- <div id="canvas"></div> -->
     <canvas class="mx-auto max-w-[600px] w-[110%] max-h-[600px] mt-8 flex" id="canvas"></canvas>


</main>

<style>



</style>
