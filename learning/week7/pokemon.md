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
     // TODO: show in the myviz that the data is loaded
     pokemonData = data  
     $('.myviz').html('number of records load:' + pokemonData.length + ' has successfully collected.')        
 })

// for each of the TODOs below, you will write a "callback function" similar
// to vizAsHorizontalBars and a $('something').click to add this callback
// function to respond to the click event of a particular button
function vizAsHorizontalBars(){

    // TODO: modify this function to visualize the data as horizontal
    // bars to compare attack points

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
    function computeX(d, i) {return 0}
    function computeWidth(d, i) {return d.Attack}
    function computeY(d, i) {return i * 20}
    function computeColor(d, i) {return 'red'}
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

// TODO: add code to visualize the attack points as a series
// of vertical bars (without labels)
function vizAsVerticalBars(){
    // TODO: modify this function to visualize the data as horizontal
    // bars to compare attack points
    // define a template string
    var tplString = '<g transform="translate(${d.x} ${d.y})"> \
                    <rect   \
                         width="20" \
                         height="${d.height}"    \
                         style="fill:${d.color};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    </g>'
    // compile the string to get a template function
    var template = _.template(tplString)
    function computeX(d, i) {return i*20}
    function computeWidth(d, i) {return 20}
    function computeY(d, i) {return 0}
    function computeColor(d, i) {return 'red'}
    function computeHeight(d,i){return d.Attack}
    var viz = _.map(pokemonData, function(d, i){
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    color: computeColor(d, i),
                    height: computeHeight(d,i)
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

// TODO: add code visualize the attack points vs. defense
// points as side-by-side horizontal bar charts (with labels)
function vizAsAttackDefenseBars(){
    // TODO: modify this function to visualize the data as horizontal
    // bars to compare attack points
    // define a template string
    var tplString = '<g transform="translate(120 ${d.y})"> \
                        <rect  \
                            x="-${d.width_a}"  \
                            width="${d.width_a}"  \
                            height="20"  \
                            style="fill:${d.color};  \
                                   stroke-width:1;  \
                                   stroke:rgb(0,0,0)" />  \
                        <rect  \
                            x="0"  \
                            width="${d.width_d}"  \
                            height="20"  \
                            style="fill:${d.dcolor};  \
                                   stroke-width:1;  \
                                   stroke:rgb(0,0,0)" />  \
                        <text transform="translate(0 15)">${d.label}</text>  \
                    </g>'
    var template = _.template(tplString)
    // compile the string to get a template function
    function computeX(d, i) {return 0}
    function computeWidthA(d, i) {return d.Attack}
    function computeY(d, i) {return i * 20}
    function computeColor(d, i) {return 'red'}
    function computeDColor(d,i) {return 'blue'}
    function computeLabel(d, i) {return d.Name}
    function computeWidthD(d, i){return d.Defense}


    var viz = _.map(pokemonData, function(d, i){
            return {
                x: computeX(d, i),
                y: computeY(d, i),
                width_d: computeWidthD(d, i),
                width_a: computeWidthA(d, i),
                dcolor:computeDColor(d,i),
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
$('button#viz-attack-defense').click(vizAsAttackDefenseBars)

// TODO: add code visualize the speed points vs. defense
// points as side-by-side horizontal bar charts (with labels)
function vizAsDefenseSpeedBars(){
    // TODO: modify this function to visualize the data as horizontal
    // bars to compare attack points
    // define a template string
    var tplString = '<g transform="translate(120 ${d.y})"> \
                        <rect  \
                            x="-${d.width_s}"  \
                            width="${d.width_s}"  \
                            height="20"  \
                            style="fill:${d.color};  \
                                   stroke-width:1;  \
                                   stroke:rgb(0,0,0)" />  \
                        <rect  \
                            x="0"  \
                            width="${d.width_d}"  \
                            height="20"  \
                            style="fill:${d.scolor};  \
                                   stroke-width:1;  \
                                   stroke:rgb(0,0,0)" />  \
                        <text transform="translate(0 15)">${d.label}</text>  \
                    </g>'
    var template = _.template(tplString)
    // compile the string to get a template function
    function computeX(d, i) {return 0}
    function computeWidthD(d, i) {return d.Defense}
    function computeY(d, i) {return i * 20}
    function computeColor(d, i) {return 'red'}
    function computeSColor(d,i) {return 'green'}
    function computeLabel(d, i) {return d.Name}
    function computeWidthS(d, i){return d.Speed}


    var viz = _.map(pokemonData, function(d, i){
            return {
                x: computeX(d, i),
                y: computeY(d, i),
                width_s: computeWidthS(d, i),
                width_d: computeWidthD(d, i),
                scolor:computeSColor(d,i),
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
$('button#viz-speed-defense').click(vizAsDefenseSpeedBars)


// TODO: add code to visualize the attack points in ascending order as a
// series of horizontal bar charts (with labels)
function vizAsHorizontalSortBars(){

    // TODO: modify this function to visualize the data as horizontal
    // bars to compare attack points

    // define a template string
    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <rect   \
                         width="${d.width}" \
                         height="20"    \
                         style="fill:${d.color};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    <text transform="translate(0 15)">${d.label}</text>  \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)
    function computeX(d, i) {return 0}
    function computeWidth(d, i) {return d.Attack}
    function computeY(d, i) {return i * 20}
    function computeColor(d, i) {return 'red'}
    function computeLabel(d, i) {return d.Name}
    var viz = _.map(_.sortBy(pokemonData,'Attack'), function(d, i){
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
$('button#viz-horizontal-sorted').click(vizAsHorizontalSortBars)


// TODO: add code to visualize the attack points in descending order as a
// series of horizontal bar charts (with labels)
function vizAsHorizontalSortDescBars(){

    // TODO: modify this function to visualize the data as horizontal
    // bars to compare attack points

    // define a template string
    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <rect   \
                         width="${d.width}" \
                         height="20"    \
                         style="fill:${d.color};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    <text transform="translate(0 15)">${d.label}</text>  \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)
    function computeX(d, i) {return 0}
    function computeWidth(d, i) {return d.Attack}
    function computeY(d, i) {return i * 20}
    function computeColor(d, i) {return 'red'}
    function computeLabel(d, i) {return d.Name}
    var viz = _.map((_.sortBy(pokemonData,'Attack')).reverse(), function(d, i){
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
$('button#viz-horizontal-sorted-desc').click(vizAsHorizontalSortDescBars)

// TODO: add code to visualize the attack points as a series of horizontal bar
// charts (with labels), and using the brightness of red to represent defense
// points
function vizAsAttackSpeedBars(){

    // TODO: modify this function to visualize the data as horizontal
    // bars to compare attack points

    // define a template string
    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <rect   \
                         width="${d.width}" \
                         height="20"    \
                         style="fill:rgb(${d.color},0,0);    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    <text transform="translate(0 15)">${d.label}</text>  \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)
    function computeX(d, i) {return 0}
    function computeWidth(d, i) {return d.Attack}
    function computeY(d, i) {return i * 20}
    function computeColor(d, i) {return d.Speed}
    function computeLabel(d, i) {return d.Name}
    var viz = _.map((_.sortBy(pokemonData,'Attack')).reverse(), function(d, i){
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
$('button#viz-attack-speed').click(vizAsAttackSpeedBars)

{% endscript %}