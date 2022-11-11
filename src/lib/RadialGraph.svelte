<script>
  import { arc } from "d3";
  import { rawData } from "./audioSourceStore.js";

  export let color, scale;
  let intensity, paths;

  const arcBuilder = arc();

  $: {
    const data = $rawData,
      total = data.reduce((acc, cur) => acc + cur, 0),
      highCount = data.filter((d) => d > 32).length;

    intensity = highCount / data.length;

    const range = 1.0 + intensity,
      rangeInRadians = range * Math.PI;

    let currentAngle = -(rangeInRadians / 2);

    paths = [];
    for (const d of data) {
      const angle = rangeInRadians * (d / total),
        path = arcBuilder({
          innerRadius: 50 - ((d + 10) / 255) * 35,
          outerRadius: 50 + ((d + 10) / 255) * 35,
          startAngle: currentAngle,
          endAngle: currentAngle + angle,
        });

      paths.push({ path, color: color(d / 255) });

      currentAngle += angle;
    }
  }
</script>

<g transform={`scale(${intensity * scale + 1})`}>
    {#each paths as p}
        <path d={p.path} fill={p.color}></path>
    {/each}
</g>