<script>
  import { onMount } from 'svelte';
  
  let conclusionContainer = $state();
  let isVisible = $state(false);
  let currentInsight = $state(0);
  
  // Key insights mejorados con estadísticas
  const keyInsights = [
    {
      icon: "🌍",
      title: "Alcance Global",
      stat: "3.2B",
      description: "jugadores activos conectados en una comunidad global sin precedentes",
      color: "#3b82f6"
    },
    {
      icon: "💰",
      title: "Impacto Económico", 
      stat: "$184B",
      description: "en ingresos anuales, superando al cine y la música combinados",
      color: "#22c55e"
    },
    {
      icon: "🚀",
      title: "Innovación Tecnológica",
      stat: "50+",
      description: "años de evolución constante impulsando nuevas tecnologías",
      color: "#8b5cf6"
    },
    {
      icon: "🎯",
      title: "Diversidad de Géneros",
      stat: "15+",
      description: "géneros únicos que ofrecen experiencias para todos los gustos",
      color: "#ec4899"
    }
  ];

  // Auto-rotate insights
  let insightInterval;
  
  onMount(() => {
    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            isVisible = true;
            // Start auto-rotation when visible
            insightInterval = setInterval(() => {
              currentInsight = (currentInsight + 1) % keyInsights.length;
            }, 3000);
          } else {
            // Stop auto-rotation when not visible
            if (insightInterval) {
              clearInterval(insightInterval);
            }
          }
        });
      },
      { threshold: 0.1 }
    );
    
    if (conclusionContainer) {
      observer.observe(conclusionContainer);
    }
    
    return () => {
      observer.disconnect();
      if (insightInterval) {
        clearInterval(insightInterval);
      }
    };
  });
</script>

<section bind:this={conclusionContainer} class="py-20 px-4 bg-gradient-to-br from-gray-900 via-purple-900/30 to-black relative overflow-hidden">
  <!-- Background effects mejorados -->
  <div class="absolute inset-0 opacity-5">
    <div class="absolute top-1/4 left-1/3 w-96 h-96 bg-purple-500 rounded-full blur-3xl animate-pulse"></div>
    <div class="absolute bottom-1/4 right-1/3 w-96 h-96 bg-cyan-500 rounded-full blur-3xl animate-pulse" style="animation-delay: 1s;"></div>
    <div class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 w-64 h-64 bg-pink-500 rounded-full blur-3xl animate-pulse" style="animation-delay: 2s;"></div>
  </div>
  
  <div class="max-w-6xl mx-auto relative z-10">
    <!-- Main conclusion con mejor tipografía -->
    <div class="text-center mb-16">
      <h2 class="text-4xl md:text-6xl font-bold bg-gradient-to-r from-purple-400 via-pink-400 to-cyan-400 bg-clip-text text-transparent mb-8 leading-tight">
        El Futuro del Entretenimiento
      </h2>
      
      <div 
        class="max-w-4xl mx-auto space-y-6 text-lg md:text-xl text-gray-300 leading-relaxed transform transition-all duration-1000"
        class:translate-y-0={isVisible}
        class:opacity-100={isVisible}
        class:translate-y-10={!isVisible}
        class:opacity-0={!isVisible}
      >
        <p class="text-xl md:text-2xl font-medium text-gray-200">
          Los videojuegos han evolucionado desde simples experimentos hasta convertirse en 
          <span class="text-transparent bg-clip-text bg-gradient-to-r from-cyan-400 to-purple-400 font-bold">
            la forma de entretenimiento más influyente de nuestra era
          </span>.
        </p>
        
        <p>
          Con una industria que genera más ingresos que el cine y la música combinados, 
          hemos presenciado el nacimiento de un <em class="text-purple-400">nuevo lenguaje cultural</em> 
          que trasciende fronteras y conecta generaciones.
        </p>
        
        <p>
          Los <strong class="text-pink-400">gamers</strong> no son solo jugadores; son 
          <strong class="text-yellow-400">arquitectos de mundos digitales</strong> que han redefinido 
          lo que significa crear, colaborar y competir en el siglo XXI.
        </p>
      </div>
    </div>

    <!-- Key insights con estadísticas destacadas -->
    <div class="mb-16">
      <h3 class="text-2xl md:text-3xl font-bold text-center text-white mb-8">
        Datos que Definen una Era
      </h3>
      
      <!-- Insight destacado actual -->
      <div class="mb-8">
        <div class="bg-gradient-to-r from-gray-800/80 to-gray-900/80 backdrop-blur-sm rounded-2xl p-8 border border-gray-600 text-center">
          <div class="text-6xl mb-4">{keyInsights[currentInsight].icon}</div>
          <div 
            class="text-5xl md:text-6xl font-bold mb-2 transition-colors duration-500"
            style="color: {keyInsights[currentInsight].color};"
          >
            {keyInsights[currentInsight].stat}
          </div>
          <h4 class="text-xl font-bold text-white mb-3">{keyInsights[currentInsight].title}</h4>
          <p class="text-gray-300 text-lg max-w-2xl mx-auto">{keyInsights[currentInsight].description}</p>
        </div>
      </div>

      <!-- Indicadores de insights -->
      <div class="flex justify-center gap-3 mb-8">
        {#each keyInsights as insight, index}
          <button
            onclick={() => currentInsight = index}
            class="w-3 h-3 rounded-full transition-all duration-300 {index === currentInsight ? 'scale-125' : 'scale-100 opacity-50'}"
            style="background-color: {insight.color};"
          ></button>
        {/each}
      </div>

      <!-- Grid compacto de todos los insights -->
      <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
        {#each keyInsights as insight, index}
          <button
            onclick={() => currentInsight = index}
            class="p-4 bg-gray-800/30 backdrop-blur-sm rounded-lg border border-gray-700 hover:border-gray-600 transition-all duration-300 text-center group {index === currentInsight ? 'ring-2' : ''}"
            style="ring-color: {insight.color};"
          >
            <div class="text-2xl mb-2 group-hover:scale-110 transition-transform">{insight.icon}</div>
            <div class="text-lg font-bold" style="color: {insight.color};">{insight.stat}</div>
            <div class="text-xs text-gray-400">{insight.title}</div>
          </button>
        {/each}
      </div>
    </div>

    <!-- Declaración de impacto mejorada -->
    <div 
      class="bg-gradient-to-r from-purple-900/50 via-pink-900/50 to-cyan-900/50 backdrop-blur-sm rounded-2xl p-8 md:p-12 border border-purple-700 mb-16 transform transition-all duration-1000"
      class:scale-100={isVisible}
      class:opacity-100={isVisible}
      class:scale-95={!isVisible}
      class:opacity-0={!isVisible}
      style="transition-delay: 600ms;"
    >
      <div class="text-center">
        <div class="text-4xl mb-6">🎮</div>
        <blockquote class="text-xl md:text-2xl font-bold text-white mb-6 italic">
          "Los videojuegos no son solo entretenimiento; son el laboratorio donde experimentamos con nuevas formas de ser humanos."
        </blockquote>
        <div class="space-y-4 text-gray-300">
          <p class="text-lg">
            Han enseñado a generaciones sobre <strong class="text-cyan-400">resolución de problemas</strong>, 
            <strong class="text-purple-400">trabajo en equipo</strong> y <strong class="text-pink-400">perseverancia</strong>. 
            Han creado espacios para la <em class="text-yellow-400">experimentación de identidad</em> y 
            han demostrado que el <strong class="text-green-400">fracaso</strong> es una oportunidad de aprendizaje.
          </p>
        </div>
      </div>
    </div>

    <!-- Mensaje de agradecimiento renovado -->
    <div 
      class="text-center transform transition-all duration-1000"
      class:translate-y-0={isVisible}
      class:opacity-100={isVisible}
      class:translate-y-10={!isVisible}
      class:opacity-0={!isVisible}
      style="transition-delay: 800ms;"
    >
      <div class="bg-gray-800/30 backdrop-blur-sm rounded-2xl p-8 border border-gray-600">
        <h3 class="text-2xl md:text-3xl font-bold text-white mb-4">
          Gracias por Explorar con Nosotros
        </h3>
        <p class="text-lg text-gray-300 max-w-3xl mx-auto leading-relaxed mb-6">
          Esta visualización es solo el comienzo de una conversación más amplia sobre el impacto 
          de los videojuegos en nuestra cultura. Cada dato que exploraste forma parte de esta 
          historia que seguimos escribiendo juntos.
        </p>
        
        <!-- Estadísticas finales -->
        <div class="grid grid-cols-3 gap-6 max-w-2xl mx-auto mb-6">
          <div class="text-center">
            <div class="text-2xl font-bold text-purple-400">100</div>
            <div class="text-sm text-gray-400">Juegos Analizados</div>
          </div>
          <div class="text-center">
            <div class="text-2xl font-bold text-cyan-400">50+</div>
            <div class="text-sm text-gray-400">Años de Historia</div>
          </div>
          <div class="text-center">
            <div class="text-3xl font-bold text-pink-400">∞</div>
            <div class="text-sm text-gray-400">Posibilidades</div>
          </div>
        </div>
        
        <div class="text-4xl animate-bounce">🎮✨🚀</div>
      </div>
    </div>

    <!-- Call to action mejorado -->
    <div 
      class="text-center mt-12 transform transition-all duration-1000"
      class:translate-y-0={isVisible}
      class:opacity-100={isVisible}
      class:translate-y-10={!isVisible}
      class:opacity-0={!isVisible}
      style="transition-delay: 1000ms;"
    >
      <p class="text-gray-400 text-lg mb-6">
        ¿Listo para seguir explorando el futuro del gaming?
      </p>
      <div class="flex flex-col sm:flex-row gap-4 justify-center">
        <button 
          onclick={(event) => { event.preventDefault(); window.scrollTo({ top: 0, behavior: 'smooth' }); }}
          class="px-8 py-3 bg-gradient-to-r from-purple-600 to-pink-600 hover:from-purple-700 hover:to-pink-700 rounded-lg font-semibold text-lg transition-all duration-300 transform hover:scale-105 shadow-lg"
        >
          🔄 Volver al Inicio
        </button>
        <a 
          href="https://es.ign.com/portal-2/178635/feature/los-100-mejores-videojuegos-de-la-historia"
          target="_blank"
          rel="noopener noreferrer"
          class="px-8 py-3 bg-gradient-to-r from-cyan-600 to-blue-600 hover:from-cyan-700 hover:to-blue-700 rounded-lg font-semibold text-lg transition-all duration-300 transform hover:scale-105 shadow-lg inline-block text-center"
        >
          🎯 Explorar Más Datos
        </a>
      </div>
    </div>
  </div>
</section>