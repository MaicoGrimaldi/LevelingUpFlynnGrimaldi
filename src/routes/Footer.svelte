<script>
  import { onMount } from 'svelte';
  
  let footerContainer = $state();
  let isVisible = $state(false);
  
  onMount(() => {
    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            isVisible = true;
          }
        });
      },
      { threshold: 0.1 }
    );
    
    if (footerContainer) {
      observer.observe(footerContainer);
    }
    
    return () => observer.disconnect();
  });
</script>

<footer bind:this={footerContainer} class="bg-black py-12 px-4 relative overflow-hidden">
  <!-- Subtle background effects -->
  <div class="absolute inset-0 opacity-5">
    <div class="absolute top-1/2 left-1/4 w-64 h-64 bg-purple-500 rounded-full blur-3xl"></div>
    <div class="absolute top-1/2 right-1/4 w-64 h-64 bg-cyan-500 rounded-full blur-3xl"></div>
  </div>
  
  <div class="max-w-4xl mx-auto text-center relative z-10">
    <!-- Project title -->
    <div 
      class="transform transition-all duration-1000"
      class:translate-y-0={isVisible}
      class:opacity-100={isVisible}
      class:translate-y-10={!isVisible}
      class:opacity-0={!isVisible}
    >
      <h3 class="text-2xl md:text-3xl font-bold text-white mb-2" style="font-family: 'zrnic rg', sans-serif;">
        Proyecto final de Visualización de Datos
      </h3>
      
      <!-- Developers -->
      <div class="mb-8">
        <p class="text-lg text-gray-300 mb-4">Desarrollado por:</p>
        <div class="flex flex-col sm:flex-row justify-center items-center gap-4 sm:gap-8">
          <div 
            class="bg-gradient-to-r from-purple-900/50 to-purple-800/50 backdrop-blur-sm rounded-lg px-6 py-3 border border-purple-700 transform transition-all duration-300 hover:scale-105 flex items-center justify-center"
          >
            <div class="text-xl font-bold text-purple-400" style="font-family: 'zrnic rg', sans-serif;">
              Maico Taiel Grimaldi
            </div>
            </div>
          
          <div class="text-2xl text-gray-500 hidden sm:block">×</div>
          
          <div 
            class="bg-gradient-to-r from-cyan-900/50 to-cyan-800/50 backdrop-blur-sm rounded-lg px-6 py-3 border border-cyan-700 transform transition-all duration-300 hover:scale-105"
          >
            <div class="text-xl font-bold text-cyan-400" style="font-family: 'zrnic rg', sans-serif;">
              Mateo Flynn
            </div>
          </div>
        </div>
      </div>
    </div>

      
      <!-- Technology stack -->
      <div class="mt-8 pt-6 border-t border-gray-800">
        <p class="text-gray-500 text-sm mb-3">Construido con:</p>
        <div class="flex flex-wrap justify-center gap-4 text-xs text-gray-400">
          <span class="bg-gray-800 px-3 py-1 rounded-full">Svelte 5</span>
          <span class="bg-gray-800 px-3 py-1 rounded-full">TailwindCSS</span>
          <span class="bg-gray-800 px-3 py-1 rounded-full">JavaScript</span>
          <span class="bg-gray-800 px-3 py-1 rounded-full">SVG Animations</span>
          <span class="bg-gray-800 px-3 py-1 rounded-full">Interactive Data Viz</span>
        </div>
      </div>
      
      <!-- Copyright -->
      <div class="mt-8 pt-6 border-t border-gray-800">
        <p class="text-gray-500 text-xs">
          © 2025 - Creado con ❤️ para la comunidad gaming
        </p>
        <p class="text-gray-600 text-xs mt-2">
          Todos los datos de juegos son utilizados con fines educativos y de investigación
        </p>
      </div>
    </div>
</footer>
