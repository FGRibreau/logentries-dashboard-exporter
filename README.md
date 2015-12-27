Logentries-dashboard exporter
=============================

This is an always work-in-progress and a quick & dirty hack **but** [it does the job](https://media.giphy.com/media/RgfW4ywPdhxzq/giphy.gif) and **that's what matters**.

```js
// first extend jQuery with a new filter
$.extend($.expr[':'],{
    isTableView: function(a) {
        return $(a).text() === 'Table view';
    }
});

// then prepare the output document
$output = $('<body/>')
  .append('<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css" integrity="sha384-1q8mTJOASx8j1Au+a5WDVnPi2lkFfwwEAa8hDDdjZlpLegxhjVME1fgjWPGmkzs7" crossorigin="anonymous"/>')
  .append('<style>'+`
  .table tr td:first-child{
    text-align:right;
  }
  .table tr td:last-child{
    width:100px;
  }
  `+'</style>')
  .append('<div class="container-fluid" />')

// loop through each widget
$('.widget-wrapper').each(function(){
  // extract widget title
  var widgetTitle = $('.widget-title', this).text();

  // select table view
  $('.v5-dropdown-menu > li:isTableView', this).click();

  // extract data and put them into the output
  var $widget = $('<div/>', {class: 'panel panel-default', style:'float:left;width:45%;margin:2.5%;'});
  $widget.append($('<div>', {class: 'panel-heading', text:$('.widget-title', this).text()}));
  $widget.append($('<div>', {class: 'panel-body'}).append($('.v5-table', this).clone().addClass('table')));

  $output.find('.container-fluid').append($widget);
});

// open new document inside a popup
var win = window.open('about:blank');
win.document.write($output.html());

// job done.
```
