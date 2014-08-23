---
layout: post
title: !binary |-
  TWFrZSBhIGR5bmFtaWMgYmFja2dyb3VuZCBzaWRlYmFyIGluIERydXBhbCA2
  IHVzaW5nIGpRdWVyeQ==
created: 1242851617
comments: true
categories: !binary |-
  ZW5fdXMgc29mdHdhcmU=
---
Sometimes you need some ugly behavior in a web-site for making customer happy, like changing some piece of the template when changing web-site section.

With the standard tools, you can create different content-types, and after theming each content-type with a custom template file (creating a new copy of node.tpl.php to node-foo.tpl.php, where foo is the name of the content-type).
The problem with this approch is that is not easy to change the content-type of an already saved node: you can use a module for doing that, but you will lose every custom cck, and is not what i want.

So i will do the work client-side, with JavaScript.
<!--break-->
I've created a script.js inside my template dir (it will be autoloaded):
<pre lang="javascript">window.onload = function setSidebarImage() {
// Check that I can find the cck where the background color is stored
  if ($(".field-field-sidebar").size()>0) {
// Get him
    var my_div = $(".field-field-sidebar").get()[0];
// Digg down to get the color
    color = jQuery.trim($(".field-item",my_div).get()[0].innerHTML);
    if (color=="") color="blue";
// Get the sidebar
    var barra = $("#sidebar-left");
// Change the background image with a custom one
    barra.get()[0].style.backgroundImage = "url('"+Drupal.settings.basePath+"sites/all/themes/foo/images/sidebar/bg_"+color+".jpg')";
  }
}</pre>

.field-field-sidebar is the class assigned to the div that contain a cck field (with display: none, for not showing it to browser): you can configure it thru cck with a drop-down list with all the custom background available.

As homework, you can implement a check of existance of #sidebar-left, or it will return an error when there isn't =P
