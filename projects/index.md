---
layout: default
location:
    latitude: 51.5285582
    longitude: -0.2416807
---

<h2> How this site is made </h2>

<ul>

  <li> This site is made with Jekyll and Ruby</li>
  <li> I use neovim for my editor </li>
  <li> I use Mac for my OS </li>
  <li> This site is hosted on GitHub Pages </li>
</ul>

<h2> Stuff I am working on </h2>
<ul>
<li> Conflict! A set of tools to manage TTRPG's </li>
</ul>
<h2> Books I am currently Reading</h2>
<ul>
<li> Ruby on Rails Background Jobs with Sidkiq </li>
<li> Active Web Development with Rails 7 </li>
<li> Kubernetes Best Practices </li>
</ul>


{% leaflet_map {"zoom" : 10 } %}

    {% leaflet_marker { "latitude" : 34.0885,
                       "longitude" : -81.1804,
                       "popupContent" : "Irmo, South Carolina"} %}
{% endleaflet_map %}
