# Pokemon

Create interactive visualizations for the Pokemon data. Complete the script
so that when a user clicks on each menu button, there will be a different
bar chart shown in the Viz block.

You will be reusing the code you wrote for generating SVG bar charts a couple weeks
ago. The main challenge is to figure out how to connect your visualization code
with the front-end code (event handlers ... etc).

## Menu

<button id="viz-horizontal">Attack (Horizontal Bars)</button>
<button id="viz-vertical">Attack (Vertical Bars)</button>
<button id="viz-attack-defense">Attack vs. Defense</button>
<button id="viz-speed-defense">Speed vs. Defense</button>
<button id="viz-horizontal-sorted">Attack (sorted from low to high)</button>
<button id="viz-horizontal-sorted-desc">Attack (sorted from high to low)</button>
<button id="viz-attack-speed">Attack (width) vs. Speed (color)</button>

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

// Visuaize Attack as Horizontal Bars
function vizAsHorizontalBars(){
    // define a template string
    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <rect   \
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
        return d.Attack
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

    $('.myviz').html('<svg>' + result + '</svg>')
}

$('button#viz-horizontal').click(vizAsHorizontalBars)

// Visualize Attack as Vertical Bars
function vizAsVerticalBars(){
    // define a template string
    var tplString = '<rect x="${d.x}" \
    					 y="${d.y}" \
                         width="20" \
                         height="${d.height}"    \
                         style="fill:${d.color};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return i * 20
    }

    function computeHeight(d, i) {
        return d.Attack
    }

    function computeY(d, i) {
        return 0
    }

    function computeColor(d, i) {
        return 'red'
    }

    var viz = _.map(pokemonData, function(d, i){
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    height: computeHeight(d, i),
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

$('button#viz-vertical').click(vizAsVerticalBars)

// Visualize Attack vs Defense as horizontal bars
function vizAttackDefense(){
    // define a template string
    var tplString = '<g transform="translate(120 ${d.y})"> \
    				<rect \
    					x="-${d.width.Attack}" \
				         width="${d.width.Attack}" \
				         height="20" \
				         style="fill:${d.color.Attack}; \
				                stroke-width:1; \
				                stroke:rgb(0,0,0)" /> \
				    <rect \
				    	x=0 \
				         width="${d.width.Defend}" \
				         height="20" \
				         style="fill:${d.color.Defend}; \
				                stroke-width:1; \
				                stroke:rgb(0,0,0)" /> \
				    <text transform="translate(0 15)">${d.label}</text> \
				</g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {
    	return {'Attack': d.Attack, 'Defend': d.Defense}
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
    	return {'Attack': 'red', 'Defend': 'blue'}
    }

    function computeLabel(d, i) {
    	return d.Name
	}

    var viz = _.map(pokemonData, function(d, i){
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    color: computeColor(d, i),
                    label: computeLabel(d, i)
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

$('button#viz-attack-defense').click(vizAttackDefense)


// Visualize Speed vs. Defense as horizontal bars.
function vizSpeedDefense(){
    // define a template string
    var tplString = '<g transform="translate(120 ${d.y})"> \
                    <rect \
                        x="-${d.width.Speed}" \
                         width="${d.width.Speed}" \
                         height="20" \
                         style="fill:${d.color.Speed}; \
                                stroke-width:1; \
                                stroke:rgb(0,0,0)" /> \
                    <rect \
                        x=0 \
                         width="${d.width.Defend}" \
                         height="20" \
                         style="fill:${d.color.Defend}; \
                                stroke-width:1; \
                                stroke:rgb(0,0,0)" /> \
                    <text transform="translate(0 15)">${d.label}</text> \
                </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {
        return {'Speed': d.Speed, 'Defend': d.Defense}
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
        return {'Speed': 'red', 'Defend': 'blue'}
    }

    function computeLabel(d, i) {
        return d.Name
    }

    var viz = _.map(pokemonData, function(d, i){
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    color: computeColor(d, i),
                    label: computeLabel(d, i)
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

$('button#viz-speed-defense').click(vizSpeedDefense)

// Visualize Attacks, horizontally, ascending order, with labels.
function vizAsHorizontalSortedBars(){
    // define a template string
    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <rect   \
                         width="${d.width}" \
                         height="20"    \
                         style="fill:${d.color};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    <text transform="translate(110 15)"> ${d.label}</text> \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {
        return d.Attack
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
        return 'red'
    }

    function computeLabel(d, i) {
        return d.Name
    }

    var viz = 
    	_.map(
    		_.sortBy(pokemonData, 'Attack'), function(d, i){
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    color: computeColor(d, i),
                    label: computeLabel(d, i)
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

$('button#viz-horizontal-sorted').click(vizAsHorizontalSortedBars)

// Visualize Attacks, horizontally, descending order, with labels.
function vizAsHorizontalSortedDescBars(){
    // define a template string
    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <rect   \
                         width="${d.width}" \
                         height="20"    \
                         style="fill:${d.color};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    <text transform="translate(110 15)"> ${d.label}</text> \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {
        return d.Attack
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
        return 'red'
    }

    function computeLabel(d, i) {
        return d.Name
    }

    var viz = 
    	_.map(
    		_.sortBy(pokemonData, 'Attack').reverse(), function(d, i){
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    color: computeColor(d, i),
                    label: computeLabel(d, i)
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

$('button#viz-horizontal-sorted-desc').click(vizAsHorizontalSortedDescBars)

// Visuaize Attacks, horizontally, with labels & brightness representing speed
function vizAsHorizontalAttackSpeedBars(){
    // define a template string
    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <rect   \
                         width="${d.width}" \
                         height="20"    \
                         style="fill:${d.color};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    <text transform="translate(110 15)"> ${d.label}</text> \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {
        return d.Attack
    }

    function computeY(d, i) {
        return i * 20
    }

    var maxSpeed = _.max(_.pluck(pokemonData, 'Speed'))

	// Compute the color based on Speed value w/max Speed = brightest
	function computeColor(d, i) {
    	var r = Math.round(d.Speed * (255/maxSpeed))
    	return 'rgb(' + r + ',0,0)'            // rgb(r,0,0)
	}

    function computeLabel(d, i) {
    	return d.Name
    }

    var viz = _.map(pokemonData, function(d, i){
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    color: computeColor(d, i),
                    label: computeLabel(d, i)
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

$('button#viz-attack-speed').click(vizAsHorizontalAttackSpeedBars)

{% endscript %}
