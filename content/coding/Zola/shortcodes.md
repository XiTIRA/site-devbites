+++
title = "Shortcodes"
description = ""
date = 2021-05-01T08:00:00+00:00
updated = 2021-05-01T08:00:00+00:00
draft = false
weight = 10
sort_by = "weight"
template = "docs/page.html"

[extra]
lead = ''
toc = true
top = false
+++

# Tabs

```md
{%/* tabs(tabs=["Tab 1","Tab 2"],group="firstTabs") */%}
{% tab() %}
Tab 1 content
{% end %}
{% tab() %}
Tab 2 content
{% end %}
{%/* end */%}
```

{% tabs(tabs=["tabs.html","tab.html","_tabs.scss"],group="tabs") %}
{% tab() %}
```html
<div class="tab-wrap">

    {% for tab in tabs %}

    <input type="radio" id="tab-{{tab}}{{group}}" name="{{group}}" class="tab" checked>
    <label for="tab-{{tab}}{{group}}">{{tab}}</label>

    {% endfor %}

    {{ body  | safe}}
</div>
```
{% end %}
{% tab() %}
```html
<div class="tab__content">
    {{ body | markdown | safe }}
</div>
```
{% end %}
{% tab() %}
```scss
$max-tab-count: 5;
$tab-wrap-border-radius: 6px;
.tab-wrap {
  transition: 0.3s box-shadow ease;
  border-radius: $tab-wrap-border-radius;
  max-width: 100%;
  display: flex;
  flex-wrap: wrap;
  position: relative;
  list-style: none;
  margin: 20px 0;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
}

.tab {
  display: none;
  @for $i from 1 through $max-tab-count {
    &:checked:nth-of-type(#{$i}) ~ .tab__content:nth-of-type(#{$i}) {
      opacity: 1;
      transition: 0.5s opacity ease-in, 0.8s transform ease;
      position: relative;
      top: 0;
      z-index: 100;
      transform: translateY(0px);
      text-shadow: 0 0 0;
    }
  }
  &:first-of-type:not(:last-of-type) + label {
    border-top-right-radius: 0;
    border-bottom-right-radius: 0;
  }
  &:not(:first-of-type):not(:last-of-type) + label {
    border-radius: 0;
  }
  &:last-of-type:not(:first-of-type) + label {
    border-top-left-radius: 0;
    border-bottom-left-radius: 0;
  }
  &:checked + label {
    background-color: rgba(204, 194, 194, 0.16);
    opacity: 1;
    box-shadow: 0 -1px 0 #eee inset;
    cursor: default;
    &:hover {
      //   box-shadow: 0 -1px 0 #fff inset;
      //  background-color: #fff;
      background-color: rgba(134, 133, 133, 0.26);
    }
  }
  + label {
    box-shadow: 0 -1px 0 #eee inset;
    border-radius: $tab-wrap-border-radius $tab-wrap-border-radius 0 0;
    cursor: pointer;
    display: block;
    text-decoration: none;
    //  color: #333;
    flex-grow: 3;
    text-align: center;
    //  background-color: #f2f2f2;
    user-select: none;
    text-align: center;
    transition: 0.3s background-color ease, 0.3s box-shadow ease;
    height: 40px;
    box-sizing: border-box;
    padding: 6px;
    &:hover {
      //   background-color: #f9f9f9;
      //   box-shadow: 0 1px 0 #f4f4f4 inset;
      background-color: rgba(134, 133, 133, 0.12);

    }
  }
  &__content {
    padding: 10px 25px;
    background-color: transparent;
    position: absolute;
    width: 100%;
    z-index: -1;
    opacity: 0;
    left: 0;
    transform: translateY(-3px);
    border-radius: $tab-wrap-border-radius;
  }
}
```
{% end %}
{% end %}