---
layout: default
---

<h2> How this site is made </h2>

<ul>

  <li> This site is made with Jekyll and Ruby</li>
  <li> I use neovim for my editor </li>
  <li> Workstation: MacBook Air</li>
  <li> Personal Server: System 76 Laptop Running Debian
  <li> This site is hosted on GitHub Pages </li>
</ul>

<h2> Stuff I am working on </h2>
<ul>
<li> Conflict! A set of tools to manage TTRPG's </li>
<li> Rails Development Environment with Docker Compose Watch</li>
</ul>
<h2> Books I am currently Reading</h2>
<ul>
<li> Ruby on Rails Background Jobs with Sidkiq </li>
<li> Active Web Development with Rails 7 </li>
<li> Kubernetes Best Practices </li>
<li> PHP 8 Objects, Patterns, and Practice</li>
</ul>

<h2> Where in The World Am I?</h2>

{% leaflet_map {"zoom" : 10 } %}

    {% leaflet_marker { "latitude" : 34.0885,
                       "longitude" : -81.1804,
                       "popupContent" : "Irmo, South Carolina"} %}
{% endleaflet_map %}
