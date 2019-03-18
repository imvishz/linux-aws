<template>
  <div id="app">
    <h2>A d3 Barchart using Vue </h2>
    
    <label> Sort By
      <select v-model="filterType" @change="onMonthlyFilterClick(filterType)">
          <option v-for="filter in filters"> {{filter.value}}</option>
      </select>
    </label>

    <label v-if="filterType == 'WEEKLY'"> Select Month
      <select v-model="filterMonth"  @change="onWeeklyFilterClick(filterType,filterMonth)">
          <option value=""  v-if="filterMonth == ''">Select Month</option>
          <option v-for="month in monthFilter"> {{month.value}}</option>
      </select>
    </label>

  </div>
</template>

<script>
import axios from 'axios'
import * as d3 from 'd3'

export default {
    name: 'app',
    data () {
        return {
            filterType : "MONTHLY",
            filters    : [{"value":"WEEKLY"},{"value":"MONTHLY"}],
            filterMonth : "",
            monthFilter : [{"value":"Jan"},{"value":"Feb"},{"value":"Mar"},{"value":"Apr"},{"value":"May"},{"value":"Jun"},
                          {"value":"Jul"},{"value":"Aug"},{"value":"Sep"},{"value":"Oct"},{"value":"Nov"},{"value":"Dec"}],
        }
    },
    
    methods: {
      createGraph: function(filterType) {
        //Margin for Svg
        var margin = { left:80, right:20, top:50, bottom:100 };
        //Width & height for svg
        var width = 600 - margin.left - margin.right,
            height = 400 - margin.top - margin.bottom;
        
        var flag ;
        //Clearing Svg for update
        d3.select("svg")
            .remove()
            .exit()
        //appending svg to body 
        var g = d3.select("body")
                  .append("svg")
                  .attr("width", width + margin.left + margin.right)
                  .attr("height", height + margin.top + margin.bottom)
                  .append("g")
                  .attr("transform", "translate(" + margin.left + ", " + margin.top + ")");
        //XAxis Group
        var xAxisGroup = g.append("g")
                          .attr("class", "x axis")
                          .attr("transform", "translate(0," + height +")");
        //Y axis Group
        var yAxisGroup = g.append("g")
                          .attr("class", "y axis");

        // X Scale
        var x = d3.scaleBand()
                  .range([0, width])
                  .padding(0.2);
                                        
        // Y Scale
        var y = d3.scaleLinear()
                  .range([height, 0]);

        // X Label
        g.append("text")
         .attr("y", height + 50)
         .attr("x", width / 2)
         .attr("font-size", "20px")
         .attr("text-anchor", "middle")
         .attr("fill","#3c52bf")
         .text("MONTH / WEEK");

        // Y Label
        g.append("text")
         .attr("y", -60)
         .attr("x", -(height / 2))
         .attr("font-size", "20px")
         .attr("text-anchor", "middle")
         .attr("transform", "rotate(-90)")
         .attr("fill", "#3c52bf")
         .text("SUM");
         
        //Setting the URL For Filter

        var url = "";
        var reqObj={};
        
        if(this.filterType == "MONTHLY"){
          url = 'http://13.234.38.162:3000/monthly';
          reqObj['filterType'] = filterType;
          flag = true;
        }else if(this.filterType == "WEEKLY"){
          url = 'http://13.234.38.162:3000/weekly';
          reqObj['filterType'] = filterType;
          reqObj['month'] = this.filterMonth;
          flag = false;  
        }
      
      //Fetching the data from backend
        axios.post(url,reqObj)
             .then(response => {
        
        var data = response['data']
      
        data.forEach(function(d) {   
            d.sum = +d.sum;
            d.month = d.month;
            d.week = d.week;
        });
        
        this.update(data,flag,x,y,xAxisGroup,yAxisGroup,g,margin,width,height);
      });
      },
    
    update :function(data,flag,x,y,xAxisGroup,yAxisGroup,g,margin,width,height) {
            
            var value = flag ? "month" : "week";

            //Mapping the data on axes
            x.domain(data.map(function(d){ return d[value] }));
            y.domain([0, d3.max(data, function(d) { return d.sum })])

            // X Axis
            var xAxisCall = d3.axisBottom(x);
            xAxisGroup.call(xAxisCall);;

            // Y Axis
            var yAxisCall = d3.axisLeft(y)
                              .tickFormat(function(d){ return "$" + d; });
            yAxisGroup.call(yAxisCall);

            // JOIN new data with old elements.
            var rects = g.selectAll("rect")
                         .data(data);

            // EXIT old elements not present in new data.
            rects.exit()
                 .remove();

            // UPDATE old elements present in new data.
            rects.attr("y", function(d){ return y(d.sum); })
                 .attr("x", function(d){ return x(d[value]) })
                 .attr("height", function(d){ return height - y(d.sum); })
                 .attr("width", x.bandwidth);

            // ENTER new elements present in new data.
            rects.enter()
                 .append("rect")
                 .attr("y", function(d){ return y(d.sum); })
                 .attr("x", function(d){ return x(d[value]) })
                 .attr("height", function(d){ return height - y(d.sum); })
                 .attr("width", x.bandwidth)
                 .attr("fill", "#4682b4");
        },
        //Function if monthly filter is clicked
        onMonthlyFilterClick:function(filterType){
            this.filterMonth="";
            if(filterType == "MONTHLY"){
            this.filterType = filterType;
            this.createGraph(filterType);
        }       
      },
      //Function if weekly filter is clicked
        onWeeklyFilterClick:function(filterType,month){
            this.filterType = filterType;
            this.createGraph(filterType); 
      }
    },
    
    created () {
        this.createGraph(this.filterType)
      }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
  color:#4682b4;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

label{
  color:#4682b4;
}
</style>
