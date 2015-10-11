# Input

<button id="load-pokemon">Load Pokemon Small</button>
<button id="load-fcq">Load FCQ</button>

## Viz

<div class="myviz" style="width:100%; height:100px; border: 1px black solid;">
</div>

{% script %}

$('button#load-pokemon').click(function(){ 
    $.get('/data/pokemon-small.json')
     .success(function(data){
         $('.myviz').html('number of records load:' + data.length)
     })
})

$('button#load-fcq').click(function(){ 
    $.get('/data/fcq.clean.json')
     .success(function(data){
         $('.myviz').html('number of records load:' + data.length)
     })
})

{% endscript %}
