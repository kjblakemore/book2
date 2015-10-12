# Pokemon Form

In the previous page, the selection of which attributes to visualize were
hard coded. Let's make our viz more interactive and flexible by providing
forms for users to select which attributes to visualize dynamically.

## Menu

### One attribute

<div style="border:1px grey solid; padding:5px;">
Attribute (e.g., Attack): <input id="pokemon-attribute-name" type="text" value="Attack"/>
<button id="viz-horizontal">Horizontal Bars</button>
</div>

### Comparing two attributes

<div style="border:1px grey solid; padding:5px;">
Attribute 1: <input id="pokemon-attribute-name-1" type="text" value="Attack"/>
Attribute 2: <input id="pokemon-attribute-name-2" type="text" value="Attack"/>
<button id="viz-compare">Compare Side-by-Side</button>
</div>

### One attribute with sorting

<div style="border:1px grey solid; padding:5px;">
Attribute: <input id="pokemon-sorted-attribute-name" type="text" value="Attack"/>
<button id="viz-horizontal-sorted-asc">Ascending</button>
<button id="viz-horizontal-sorted-desc">Descending</button>
</div>

### Three attributes

<div style="border:1px grey solid; padding:5px;">
Width 1: <input id="pokemon-three-width1" type="text" value="Attack"/>
Width 2: <input id="pokemon-three-width2" type="text" value="Defense"/>
Color: <input id="pokemon-three-color" type="text" value="Speed"/>
<button id="viz-three">Viz Three Attributes</button>
</div>

## Viz

<div class="myviz" style="width:100%; height:500px; border: 1px black solid;">
Data is not loaded yet
</div>

{% script %}
// pokemonData is a global variable
pokemonData = 'not loaded yet'

$.get('/data/pokemon-small.json')
 .success(function(data){
     console.log('data loaded', data)
     $('.myviz').html('number of records loaded:' + data.length)
     pokemonData = data          
 })

// Visualize Attribute as horizontal bars
function vizAsHorizontalBars(attributeName){
    // define a template string
    var tplString = '<rect   \
                        x="0" \
                        y=${d.y} \
                         width="${d.width}" \
                         height="20"    \
                         style="fill:${d.color};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {        
        return d[attributeName]
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
        return 'red'
    }

    var viz = _.map(pokemonData, function(d, i){
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    color: computeColor(d, i)
                }
             })
    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg width="100%">' + result + '</svg>')
}

$('button#viz-horizontal').click(function(){
    var attributeName = $('input#pokemon-attribute-name').val()
    vizAsHorizontalBars(attributeName)
})  

// Visualize two attributes side-by-side
function vizSideBySide(attributeName1, attributeName2){
    // define a template string
    var tplString = '<g transform="translate(180 ${d.y})"> \
                        <rect \
                             x="-${d.width.Attribute1}" \
                             width="${d.width.Attribute1}" \
                             height="20" \
                             style="fill:${d.color.Attribute1}; \
                                    stroke-width:1; \
                                    stroke:rgb(0,0,0)" /> \
                        <rect \
                             x=0 \
                             width="${d.width.Attribute2}" \
                             height="20" \
                             style="fill:${d.color.Attribute2}; \
                                    stroke-width:1; \
                                    stroke:rgb(0,0,0)" /> \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {        
        return {'Attribute1': d[attributeName1], 'Attribute2': d[attributeName2]}
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
        return {'Attribute1': 'red', 'Attribute2': 'blue'}
    }

    var viz = _.map(pokemonData, function(d, i){
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    color: computeColor(d, i)
                }
             })
    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg>' + result + '</svg>')
}

$('button#viz-compare').click(function(){    
    var attributeName1 = $('input#pokemon-attribute-name-1').val()
    var attributeName2 = $('input#pokemon-attribute-name-2').val()
    vizSideBySide(attributeName1, attributeName2)
})


// Visualize one attribute sorted in ascending or descending order
function vizAsSortedHorizontalBars(attributeName, sortDirection){

    console.log('sort direction', sortDirection)

    // define a template string
    var tplString = '<rect   \
                        x="0" \
                        y=${d.y} \
                         width="${d.width}" \
                         height="20"    \
                         style="fill:${d.color};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {        
        return d[attributeName]
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
        return 'red'
    }

    var sortedData = 
    	sortDirection == 
    		'asc'? _.sortBy(pokemonData, attributeName) : _.sortBy(pokemonData, attributeName).reverse()

    console.log('sorted data', sortedData)

    var viz = _.map(sortedData, function(d, i){
    			return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    color: computeColor(d, i)
                }
             })
    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg width="100%">' + result + '</svg>')
}

$('button#viz-horizontal-sorted-asc').click(function(){    
    var attributeName = $('input#pokemon-sorted-attribute-name').val()
    var sortDirection = 'asc'
    vizAsSortedHorizontalBars(attributeName, sortDirection)
})  

$('button#viz-horizontal-sorted-desc').click(function(){    
    var attributeName = $('input#pokemon-sorted-attribute-name').val()
    var sortDirection = 'desc'
    vizAsSortedHorizontalBars(attributeName, sortDirection)
}) 

// TODO: complete the code below
// visualize three attributes, the first two attributes as side-by-side bar charts
// using bar widths to represent attribute values, and the third attribute's value
// is represented using the red color brightness
function vizThreeAttributes(attributeName1, attributeName2, attributeName3){
    // define a template string
    var tplString = '<g transform="translate(180 ${d.y})"> \
                        <rect \
                             x="-${d.width.Attribute1}" \
                             width="${d.width.Attribute1}" \
                             height="20" \
                             style="fill:${d.color.Attribute1}; \
                                    stroke-width:1; \
                                    stroke:rgb(0,0,0)" /> \
                        <rect \
                             x=0 \
                             width="${d.width.Attribute2}" \
                             height="20" \
                             style="fill:${d.color.Attribute2}; \
                                    stroke-width:1; \
                                    stroke:rgb(0,0,0)" /> \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {        
        return {'Attribute1': d[attributeName1], 'Attribute2': d[attributeName2]}
    }

    function computeY(d, i) {
        return i * 20
    }

    console.log('attributes: ', attributeName1, attributeName2, attributeName3)

    var maxValue = _.max(_.pluck(pokemonData, attributeName3))

    // Scale the color by the third attribute, with highest value = brightest
    function computeColor(d, i) {
    	var r1 = Math.round(d[attributeName3] * (255/maxValue))
    	var r2 = Math.round(d[attributeName3] * (255/maxValue))

    	console.log('attribute 1 color', r1)
    	console.log('attribute 2 color', r2)

        return {'Attribute1': 'rgb(' + r1 + ',0,0)', 'Attribute2': 'rgb(0,0,' + r2 + ')'}
    }

    var viz = _.map(pokemonData, function(d, i){
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    color: computeColor(d, i)
                }
             })
    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg>' + result + '</svg>')
}

$('button#viz-three').click(function(){    
    var attributeName1 = $('input#pokemon-three-width1').val()
    var attributeName2 = $('input#pokemon-three-width2').val()
    var attributeName3 = $('input#pokemon-three-color').val()  
    vizThreeAttributes(attributeName1, attributeName2, attributeName3)
})  


{% endscript %}
