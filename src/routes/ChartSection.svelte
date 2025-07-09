<script lang="ts">
  import { onMount } from 'svelte';
  
  interface RegionData {
    region: string;
    count: number;
    color: string;
    shortName: string;
  }
  
  const regionalData: RegionData[] = [
    { region: "Norte América", count: 44, color: "#3b82f6", shortName: "NA" },
    { region: "Asia", count: 40, color: "#ef4444", shortName: "Asia" },
    { region: "Europa", count: 15, color: "#22c55e", shortName: "EU" },
    { region: "Oceania", count: 1, color: "#f59e0b", shortName: "OCE" },
  ];

  let hoveredRegion: string | null = $state(null);

  // Datos reactivos para el radar usando $derived
  const sortedRegions = $derived([...regionalData].sort((a, b) => b.count - a.count));
  const maxCount = $derived(sortedRegions[0]?.count || 1);
  
  // Función para calcular posición y tamaño de cada punto
  function getCircleData(region: RegionData, index: number) {
    // Distribuir los puntos uniformemente en 360 grados
    const angle = (index * 360 / sortedRegions.length) - 90; // -90 para empezar arriba
    const maxDistance = 280; // Distancia máxima desde el centro (aumentada)
    const minDistance = 100;  // Distancia mínima desde el centro (aumentada)
    
    // Calcular distancia basada en el valor
    const normalizedValue = region.count / maxCount;
    const distance = minDistance + (normalizedValue * (maxDistance - minDistance));
    
    // Calcular coordenadas
    const x = Math.cos((angle * Math.PI) / 180) * distance;
    const y = Math.sin((angle * Math.PI) / 180) * distance;
    
    // Calcular tamaño del círculo
    const minRadius = 30; // Aumentado
    const maxRadius = 70; // Aumentado
    const radius = minRadius + (normalizedValue * (maxRadius - minRadius));
    const size = radius * 2;
    
    return { x, y, radius, size, angle };
  }
</script>

<div class="min-h-screen bg-gradient-to-br from-slate-900 via-purple-900 to-slate-900 p-6 flex items-center justify-center mt">
  <div class="max-w-6xl mx-auto w-full mb-12">
    <!-- Encabezado con el título -->
    <div class="text-center mt-12 mb-5">
      <h1 class="text-6xl font-bold bg-gradient-to-r from-purple-400 via-pink-400 to-cyan-400 bg-clip-text text-transparent mb-6">
        Conquista Digital Global
      </h1>
      <p class="text-2xl text-gray-300 max-w-4xl mx-auto">
        El dominio territorial de los videojuegos por continente
      </p>
    </div>

    <!-- Gráfico Regional -->
    <div class="bg-transparent max-w-5xl mx-auto">
      <div class="p-8 pb-20">
        <!-- Gráfico Radar -->
        <div class="relative w-full h-[800px] flex items-center justify-center rounded-xl overflow-visible">
          <!-- Background glow effect -->
          <div class="absolute inset-0 bg-gradient-radial from-cyan-900/10 via-transparent to-transparent"></div>
          
          <!-- Contenedor circular del radar -->
          <div 
            class="absolute rounded-full overflow-visible"
            style="width: 750px; height: 750px; top: 50%; left: 50%; transform: translate(-50%, -50%);"
          >
            <!-- Círculos concéntricos del radar -->
            <div class="absolute inset-0 flex items-center justify-center">
              {#each [1, 2, 3, 4, 5, 6] as ring}
                <div
                  class="absolute border rounded-full border-gray-500/60"
                  style="
                    width: {ring * 120}px;
                    height: {ring * 120}px;
                    opacity: {ring === 6 ? 0.8 : 0.5};
                  "
                ></div>
              {/each}
              <!-- Círculo exterior principal -->
              <div
                class="bg-gray-900/40 absolute border-2 rounded-full border-cyan-400/80"
                style="width: 750px; height: 750px;"
              ></div>
            </div>

            <!-- Líneas del radar -->
            <div class="absolute inset-0 flex items-center justify-center">
              {#each Array(sortedRegions.length) as _, index}
                {@const angle = (index * 360 / sortedRegions.length) - 90}
                <div
                  class="absolute bg-gradient-to-t from-cyan-500/50 to-transparent"
                  style="
                    width: 2px;
                    height: 375px;
                    transform: rotate({angle}deg);
                    transform-origin: center bottom;
                    bottom: 50%;
                    left: 50%;
                    margin-left: -1px;
                    opacity: 0.7;
                  "
                ></div>
              {/each}
            </div>

            <!-- Barrido del radar -->
            <div
              class="absolute w-full h-full pointer-events-none radar-sweep"
              style="
                background: conic-gradient(from 0deg, transparent 350deg, rgba(6, 182, 212, 0.2) 355deg, rgba(6, 182, 212, 0.4) 360deg, transparent 365deg);
                border-radius: 50%;
                filter: blur(1px);
              "
            ></div>

            <!-- Centro del radar -->
            <div class="absolute w-4 h-4 bg-cyan-400 rounded-full shadow-lg shadow-cyan-400/50 z-10"
                 style="top: 50%; left: 50%; transform: translate(-50%, -50%);"></div>

            <!-- Puntos de datos en el radar -->
            {#each sortedRegions as region, index}
              {@const { x, y, radius, size } = getCircleData(region, index)}
              {@const isHovered = hoveredRegion === region.region}
              
              <div
                class="absolute cursor-pointer group z-20"
                style="
                  top: 50%; 
                  left: 50%; 
                  transform: translate(calc(-50% + {x}px), calc(-50% + {y}px));
                "
                onmouseenter={() => hoveredRegion = region.region}
                onmouseleave={() => hoveredRegion = null}
                role="button"
                tabindex="0"
                aria-label="Ver detalles de {region.region}"
              >
                <!-- Punto principal -->
                <div
                  class="rounded-full flex items-center justify-center text-white font-bold transition-all duration-500 relative border-2"
                  style="
                    width: {size}px;
                    height: {size}px;
                    font-size: {Math.max(size / 3, 16)}px;
                    background: radial-gradient(circle at 30% 30%, {region.color}, {region.color}cc);
                    border-color: {region.color};
                    box-shadow: {isHovered
                      ? `0 0 50px ${region.color}, 0 0 100px ${region.color}40`
                      : `0 0 25px ${region.color}60`};
                    transform: {isHovered ? 'scale(1.2)' : 'scale(1)'};
                    filter: {isHovered ? 'brightness(1.3)' : 'brightness(1)'};
                  "
                >
                  <span class="relative z-10 font-bold text-xl">{region.shortName}</span>
                  
                  <!-- Efectos de hover -->
                  {#if isHovered}
                    <div
                      class="absolute inset-0 rounded-full border-2 animate-ping"
                      style="border-color: {region.color}; animation-duration: 1s;"
                    ></div>
                    <div
                      class="absolute inset-0 rounded-full border opacity-60"
                      style="border-color: {region.color}; transform: scale(1.4);"
                    ></div>
                  {/if}
                </div>

                <!-- Tooltip -->
                {#if isHovered}
                  <div class="absolute -top-20 left-1/2 transform -translate-x-1/2 bg-gray-900/95 text-white px-4 py-3 rounded-lg text-sm whitespace-nowrap border border-cyan-500/50 shadow-xl z-50 backdrop-blur-sm">
                    <div class="font-bold text-base mb-1" style="color: {region.color}">{region.region}</div>
                    <div class="text-gray-200">{region.count} juegos</div>
                    
                    <!-- Flecha del tooltip -->
                    <div class="absolute top-full left-1/2 transform -translate-x-1/2 w-0 h-0 border-l-4 border-r-4 border-t-4 border-l-transparent border-r-transparent border-t-gray-900"></div>
                  </div>
                {/if}
              </div>
            {/each}
          </div>
        </div>

        <!-- Estadísticas mejoradas -->
        <div class="grid grid-cols-2 lg:grid-cols-4 xl:grid-cols-4 gap-8 mt-10 max-w-5xl mx-auto">
          {#each regionalData as region}
            <div class="relative p-6 bg-gradient-to-br from-gray-800/70 to-gray-900/90 rounded-xl border border-gray-600/50 hover:border-cyan-500/50 transition-all duration-300 group backdrop-blur-sm">
              <!-- Glow effect on hover -->
              <div class="absolute inset-0 bg-gradient-to-br from-transparent to-cyan-900/10 rounded-xl opacity-0 group-hover:opacity-100 transition-opacity duration-300"></div>
              
              <div class="text-center relative z-10">
                <div class="text-4xl font-bold mb-3 transition-all duration-300" style="color: {region.color}; text-shadow: 0 0 20px {region.color}60;">
                  {region.count}
                </div>
                <div class="text-white text-lg font-semibold mb-1">{region.shortName}</div>
                <div class="text-gray-400 text-sm">{region.region}</div>
              </div>
            </div>
          {/each}
        </div>
      </div>
    </div>
  </div>
</div>

<style>
  @keyframes radar-sweep {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }
  
  .radar-sweep {
    animation: radar-sweep 6s linear infinite;
  }

  @keyframes pulse {
    0%, 100% { opacity: 0.4; transform: scale(1); }
    50% { opacity: 0.8; transform: scale(1.02); }
  }

  .bg-gradient-radial {
    background: radial-gradient(circle, var(--tw-gradient-stops));
  }
</style>