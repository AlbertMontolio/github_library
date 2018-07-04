create an update form with html, without rails

check a checkbox without the button submit
with a javascript request, addEventListener

or use that

$(".edit").click(function(){  
  if ($(this).hasClass("update")){     
    $.ajax({
      type: "PUT",
      url: "/sections/<%= section.id %>"
    });
  } else {
    //do something else 
  }; 
})

no this, with the code in the task-manager exercise

the response is missing. it's not a json response
you want to somehow redirect



https://github.com/SortableJS

https://lukasoppermann.github.io/html5sortable/index.html

https://github.com/RubaXa/Sortable

patch in html form

method: (:get|:post|:patch|:put|:delete)
from the documentation:

"in the options hash. If the verb is not GET or POST, which are natively supported by HTML forms, the form will be set to POST and a hidden input called _method will carry the intended verb for the server to interpret."

source: http://api.rubyonrails.org/classes/ActionView/Helpers/FormHelper.html