{% assign id =  include.os | downcase -%}
{% assign channels =  'stable beta' | split: ' ' -%}

<div id="{{id}}" class="tab-pane
  {%- if id == 'windows' %} active {% endif %}"
  role="tabpanel" aria-labelledby="{{id}}-tab" markdown="1">

{% for channel in channels -%}
## {{channel | capitalize }} channel ({{include.os}})

Select from the following scrollable list:

請從下列列表中選擇：

<div class="scrollable-table">
  <table id="downloads-{{id}}-{{channel}}" class="table table-striped">
  <thead><tr><th>Flutter 版本</th><th>架構</th><th>Ref</th><th class="date">釋出日期</th><th>Dart 版本</th><th>出處/來源</th></tr></thead>
  <tr class="loading"><td colspan="6">Loading...</td></tr>
  </table>
</div>
{% endfor -%}

</div>
