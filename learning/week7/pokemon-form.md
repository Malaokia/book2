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
     // TODO: show in the myviz that the data is loaded
     pokemonData = data          
      $('.myviz').html('number of records load:' + pokemonData.length + ' has successfully collected.')
 })


function vizAsHorizontalBars(attributeName){

    // TODO: modify this function to visualize the data as horizontal
    // bars to compare attack points

    // define a template string
    var tplString = '<g transform="translate(120 ${d.y})"> \
                        <rect  \
                            x="-${d.width}"  \
                            width="${d.width}"  \
                            height="20"  \
                            style="fill:${d.color};  \
                                   stroke-width:1;  \
                                   stroke:rgb(0,0,0)" />  \
                        <rect  \
                            x="0"  \
                            width="${d.width_a}"  \
                            height="20"  \
                            style="fill:${d.color_a};  \
                                   stroke-width:1;  \
                                   stroke:rgb(0,0,0)" />  \
                        <text transform="translate(0 15)">${d.label}</text>  \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }
    function computeWidth(d,i){
        return d.Attack
    }
    function computeWidth_A(d, i) {        
        return d[attributeName]
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
        return 'red'
    }
    function computeColor_A(d, i){
        return 'green'
    }
    function computeLabel(d,i){
        return d.Name
    }

    var viz = _.map(pokemonData, function(d, i){
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    width_a: computeWidth_A(d, i),
                    color: computeColor(d, i),
                    color_a:computeColor_A(d,i),   
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

$('button#viz-horizontal').click(function(){
    // TODO: replace this line with JQuery code to grab the attribute name
    var attribute = $('input#pokemon-attribute-name').val()
    vizAsHorizontalBars(attribute)
})  


// TODO: complete the code below

function vizSideBySide(attributeName1, attributeName2){

    // TODO: modify this function to visualize the data as horizontal
    // bars to compare attack points

    // define a template string
    var tplString = '<g transform="translate(120 ${d.y})"> \
                        <rect  \
                            x="-${d.width}"  \
                            width="${d.width}"  \
                            height="20"  \
                            style="fill:${d.color};  \
                                   stroke-width:1;  \
                                   stroke:rgb(0,0,0)" />  \
                        <rect  \
                            x="0"  \
                            width="${d.width_a}"  \
                            height="20"  \
                            style="fill:${d.color_a};  \
                                   stroke-width:1;  \
                                   stroke:rgb(0,0,0)" />  \
                        <text transform="translate(0 15)">${d.label}</text>  \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }
    function computeWidth(d,i){
        return d[attributeName1]
    }
    function computeWidth_A(d, i) {        
        return d[attributeName2]
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
        return 'red'
    }
    function computeColor_A(d, i){
        return 'green'
    }
    function computeLabel(d,i){
        return d.Name
    }

    var viz = _.map(pokemonData, function(d, i){
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    width_a: computeWidth_A(d, i),
                    color: computeColor(d, i),
                    color_a:computeColor_A(d,i),   
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
    var attribute1 = $('input#pokemon-attribute-name-1').val()
    var attribute2 = $('input#pokemon-attribute-name-2').val()
    vizSideBySide(attribute1, attribute2)
})  

// TODO: complete the code below

function vizAsSortedHorizontalBars(attributeName, sortDirection){
// TODO: modify this function to visualize the data as horizontal
    // bars to compare attack points

    // define a template string
    var tplString = '<g transform="translate(120 ${d.y})"> \
                        <rect  \
                            x="-${d.width}"  \
                            width="${d.width}"  \
                            height="20"  \
                            style="fill:${d.color};  \
                                   stroke-width:1;  \
                                   stroke:rgb(0,0,0)" />  \
                        <text transform="translate(0 15)">${d.label}</text>  \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)
    var data = 0
    function computeX(d, i) {
        return 0
    }
    function computeWidth(d,i){
        return d[attributeName]
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
        return 'red'
    }
    function computeLabel(d,i){
        return d.Name
    }
    if(sortDirection == "Ascending"){
        data = _.sortBy(pokemonData,attributeName)
    }
    else{
        data = (_.sortBy(pokemonData,attributeName)).reverse()
    }
    var viz = _.map(data, function(d, i){
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

$('button#viz-horizontal-sorted-asc').click(function(){    
    var attributeName = $('input#pokemon-attribute-name').val()
    var sortDirection = "Ascending"
    vizAsSortedHorizontalBars(attributeName, sortDirection)
}) 

$('button#viz-horizontal-sorted-desc').click(function(){    
    var attributeName = $('input#pokemon-attribute-name').val()
    var sortDirection = "Descending"
    vizAsSortedHorizontalBars(attributeName, sortDirection)
})   

// TODO: complete the code below
// visualize three attributes, the first two attributes as side-by-side bar charts
// using bar widths to represent attribute values, and the third attribute's value
// is represented using the red color brightness
function vizThreeAttributes(attributeName1, attributeName2, attributeColor){

    // TODO: modify this function to visualize the data as horizontal
    // bars to compare attack points

    // define a template string
    var tplString = '<g transform="translate(120 ${d.y})"> \
                        <rect  \
                            x="-${d.width}"  \
                            width="${d.width}"  \
                            height="20"  \
                            style="fill:${d.color};  \
                                   stroke-width:1;  \
                                   stroke:rgb(0,0,0)" />  \
                        <rect  \
                            x="0"  \
                            width="${d.width_a}"  \
                            height="20"  \
                            style="fill:${d.color};  \
                                   stroke-width:1;  \
                                   stroke:rgb(0,0,0)" />  \
                        <text transform="translate(0 15)">${d.label}</text>  \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }
    function computeWidth(d,i){
        return d[attributeName1]
    }
    function computeWidth_A(d, i) {        
        return d[attributeName2]
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
        return attributeColor
    }
    
    function computeLabel(d,i){
        return d.Name
    }

    var viz = _.map(pokemonData, function(d, i){
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    width_a: computeWidth_A(d, i),
                    color: computeColor(d, i),  
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
    var attribute1 = $('input#pokemon-three-width1').val()
    var attribute2 = $('input#pokemon-three-width2').val()
    var attributeColor = $('input#pokemon-three-color').val()   
    vizThreeAttributes(attribute1, attribute2, attributeColor)
})  


{% endscript %}
