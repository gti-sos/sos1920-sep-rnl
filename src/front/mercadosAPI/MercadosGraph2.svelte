<script>

    import Button from "sveltestrap/src/Button.svelte";
    import  {pop} from "svelte-spa-router";
    import bb from "billboard.js/dist/billboard.pkgd";
  
    const URL_BASE = "api/v1/mercados";
    
    async function loadGraph(){
    
      const resData = await fetch(URL_BASE);
      let MyData = await resData.json();
      
      let regions = Array.from(MyData.map((d) => {return d.Region+" / "+d.Country;}));
      let population = Array.from(MyData.map((d) => {return parseFloat(d.Population);}));
      let internet_pop = Array.from(MyData.map((d) => {return parseFloat(d.Internet_pop);}));
      let revenues = Array.from(MyData.map((d) => {return parseFloat(d.Revenues);}));
  
      let data_regions = [];
      let data_pop = [];
      let data_internet = [];
      let data_revenues = [];
  
      data_regions[0]="x";
      data_revenues[0]="Ingresos";
      population.forEach(aux => data_pop.push(aux));
      internet_pop.forEach(aux => data_internet.push(aux));
      revenues.forEach(aux => data_revenues.push(aux));
      regions.forEach(aux => data_regions.push(aux));
  
  
      var chart = bb.generate({
      data: {
        x: "x",
        columns: [
        data_regions,
        data_revenues
        ],
        type: "bubble",
        labels: true
      },
      bubble: {
        maxR: 60
      },
      axis: {
        x: {
          tick: {
            rotate: 0
          },
          type: "category",
          max: data_revenues.length-2
        },
        y: {
          min: -15,
          max: 60
        }
      },
      bindto: "#bubbleChart"
      });
    }
  
    loadGraph();
  </script>

  <head>
  
    <script src="https://d3js.org/d3.v5.min.js"></script>
    <script src="libraries/billboard.js"></script>
    <link rel="stylesheet" href="css/billboard.css">
    
  </head>
  
  <main>
    <h2>Gráfica 2 Billboard</h2>
        <div id= "bubbleChart"></div>

        <b><p>
          El gráfico de burbujas muestra los ingresos en Millones de euros (€).
      </p></b>  
      <p></p>
      <button type="button"  onclick="window.location.href='#/'"> ATRAS</button>
  </main>
  

  <style>
    main{
        font-weight: bold;
        text-align:center;
    }

    a.button {
    -webkit-appearance: button;
    -moz-appearance: button;
    appearance: button;
    font-weight: bold;
    text-decoration: none;
    color: initial;
}
  </style>