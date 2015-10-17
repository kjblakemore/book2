# UI

Pick one question class and build an exploratory visualization interface for it.
The question class you pick must have at least three variables that can be changed.

## How many businesses are open on X day at Y hour?
Show the distribution over States.

<div style="border:1px grey solid; padding:5px;">
    <div><h5>X</h5> (Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday)
        <input id="day" type="text" value="Monday"/>
    </div>
    <div><h5>Y</h5> (using 24 hour clock, e.g., 19:00)
        <input id="hour" type="text" value="19:00"/>
    </div>
    <div><h5>Z</h5> (e.g., ascending, descending)
        <input id="direction" type="text" value="ascending"/>
    </div>    
    <div style="margin:20px;">
        <button id="viz">Visualize</button>
    </div>
</div>

<div class="myviz" style="width:100%; height:500px; border: 1px black solid; padding: 5px;">
Data is not loaded yet
</div>

{% script %}
items = 'not loaded yet'

$.get('http://bigdatahci2015.github.io/data/yelp/yelp_academic_dataset_business.5000.json.lines.txt')
    .success(function(data){        
        var lines = data.trim().split('\n')

        // convert text lines to json arrays and save them in `items`
        items = _.map(lines, JSON.parse)

        console.log('number of items loaded:', items.length)

        console.log('first item', items[0])
     })
     .error(function(e){
         console.error(e)
     })

function viz(day, hour, direction){    

    // define a template string
    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <text y="20">${d.label}</text> \
                    <rect x="30"   \
                         width="${d.width}" \
                         height="${d.height}"  \
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
        return d[1].length < 700 ? d[1].length : 700
    }

    // Scale the height by the maximum number of businesses in the result
    function computeHeight(d, i) {
        return Math.round(d[1].length <= 700 ? 20 : 20 * (d[1].length/700))
    }

    // Adjust y coordinate by width of previous bars
    function computeY(d, i) {
    	return y = i==0? 0: _.sum(_.map(first20.slice(0,i), 
    								function(d) {
    									return computeHeight(d, 0)
                                    	})
                                	)
    							}
    							

    function computeColor(d, i) {
        return 'red'
    }

    function computeLabel(d, i) {
        return d[0]     // The label is the state abbreviation
    }

    // Convert hour string to int
    function hourToInt(h) {
        var x = h.split(':')
        return x[0] * 3600 + x[1] * 60
    }

    // The input hour as an integer
    var hourAsInt = hourToInt(hour)
    console.log('hour', hour)

    // First filter all entries for the specified day and time
    itemsFiltered = _.filter(items, function(d) {
        return d.hours && d.hours[day] &&
            (hourToInt(d.hours[day].open) <= hourAsInt &&
             hourAsInt <= hourToInt(d.hours[day].close))
        })

    // Next group by state
    var groups = _.groupBy(itemsFiltered, 'state')
    console.log('groups', groups)

    // Convert to array of [state, record]
    var pairs = _.pairs(groups)
    console.log('pairs', pairs)

    // Sort in specified direction
    var sortorder = direction == 'ascending'? 1 : -1
    pairsSorted = _.sortBy(pairs, function(d, i) {
        return d[1].length * sortorder
        })

    console.log('pairs sorted', pairsSorted)

    // Just take the first 20 states
    first20 = _.take(pairsSorted, 20)

    var viz = _.map(first20, function(d, i){                
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    height: computeHeight(d, i),
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

    $('.myviz').html('<svg width="100%" height="100%">' + result + '</svg>')
}

$('button#viz').click(function(){    
    var day = $('input#day').val()
    var hour = $('input#hour').val()
    var direction = $('input#direction').val()  
    viz(day, hour, direction)
})  

{% endscript %}

# Authors

This UI is developed by
* [Karen Blakemore](https://github.com/kjblakemore)
* [Matt Schroeder](https://github.com/mattschroeder97)
* [Mingqi Lew](https://github.com/Malaokia)
* [Brian McKean](https://github.com/co-bri)
