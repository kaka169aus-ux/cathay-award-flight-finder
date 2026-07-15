# Modern UI changes

Apply the following changes to the script in Tampermonkey. The date picker shows a calendar, while searches still send dates as `YYYYMMDD`.

## 1. Add date conversion helpers

Directly below the existing `toDashedDate` function, add:

```js
function normaliseDate(value) { return value.replace(/-/g, ""); }
function toDateInputValue(value) { return toDashedDate(normaliseDate(value)); }
```

## 2. Replace the date input

Find the existing input with `id='uef_date'` and replace it with:

```js
<input tabindex="3" class="uef_date" type="date" id="uef_date" name="uef_date"
    min="` + toDashedDate(dateAdd(-1)) + `"
    max="` + toDashedDate(dateAdd(365)) + `"
    value="` + toDateInputValue(uef_date) + `">
```

## 3. Preserve the date used for searches

Make these substitutions throughout the script:

```js
value_set("uef_date", input_date.value)
```

becomes:

```js
value_set("uef_date", normaliseDate(input_date.value))
```

```js
uef_date = input_date.value;
```

becomes:

```js
uef_date = normaliseDate(input_date.value);
```

```js
bulk_date = bulk_date ? bulk_date : input_date.value;
```

becomes:

```js
bulk_date = bulk_date ? bulk_date : normaliseDate(input_date.value);
```

```js
bulk_date = input_date.value;
```

becomes:

```js
bulk_date = normaliseDate(input_date.value);
```

Replace the date change handler with:

```js
input_date.addEventListener("change", function() {
    if (!isValidDate(normaliseDate(this.value))) {
        alert(lang.invalid_date);
        this.value = toDateInputValue(uef_date);
    } else {
        route_changed = true;
    }
});
```

## 4. Rename the heading

Replace:

```html
<div class='unelevated_title'><a href="...">Unelevated Award Search (Unlocked)</a></div>
```

with:

```html
<div class='unelevated_title'>Award flight finder</div>
```

## 5. Append this CSS before the final backtick of `styleCss`

```css
/* Minimal modern interface */
.unelevated_form, .bulk_box {
  font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  border: 1px solid #e3e9e7;
  background: #fff;
  box-shadow: 0 12px 32px rgb(20 54 48 / 10%);
}
.unelevated_form {
  position: relative;
  overflow: hidden;
  border-top: 0;
  border-radius: 18px;
  margin: 20px 0 12px;
  padding: 24px;
}
.unelevated_form::before {
  content: "";
  position: absolute;
  inset: 0 0 auto;
  height: 5px;
  background: linear-gradient(90deg, #005f57, #2b7a71);
}
.unelevated_title {
  color: #183b35;
  font-size: 21px;
  font-weight: 700;
  height: auto;
  letter-spacing: -.02em;
  margin: 8px 0 22px;
}
.unelevated_form .unelevated_saved {
  right: 20px;
  top: 18px;
  padding: 5px 11px;
  background: #fdf1f2;
  border-radius: 999px;
}
.unelevated_form .unelevated_saved a,
.unelevated_form .unelevated_saved a:hover,
.unelevated_form .unelevated_saved a:active,
.unelevated_form .unelevated_saved a:focus { color: #b84b59; }
.unelevated_form .unelevated_saved svg.heart_save path { fill: #d35b69; }
.unelevated_form .labels { gap: 12px; }
.unelevated_form .labels label.labels_left,
.unelevated_form .labels label.labels_right {
  width: calc(50% - 6px);
  padding: 0;
}
.unelevated_form .labels input {
  height: 58px;
  margin: 0;
  padding: 24px 13px 8px;
  border: 1px solid #d9e4e1;
  border-radius: 10px;
  background: #fbfcfc;
  color: #183b35;
  font-size: 16px;
  transition: border-color .2s, box-shadow .2s;
}
.unelevated_form .labels input:focus {
  border-color: #14776d;
  box-shadow: 0 0 0 4px rgb(20 119 109 / 12%);
}
.unelevated_form .labels label > span {
  top: 6px;
  left: 13px;
  color: #687b76;
  font-size: 11px;
  font-weight: 600;
  letter-spacing: .03em;
  text-transform: uppercase;
}
svg.clear_from, svg.clear_to { right: 13px; top: 21px; }
a.switch {
  background: #fff;
  border-color: #d9e4e1;
  box-shadow: 0 2px 8px rgb(20 54 48 / 12%);
}
.unelevated_form button.uef_search,
button.bulk_submit {
  width: calc(50% - 6px);
  height: 58px;
  border-radius: 10px;
  background: #006b61;
  font-size: 15px;
  font-weight: 700;
  transition: background .2s, transform .2s;
}
.unelevated_form button.uef_search:hover,
button.bulk_submit:hover { background: #00564f; }
.unelevated_form button.uef_search:active,
button.bulk_submit:active { transform: translateY(1px); }
.unelevated_sub { margin-top: 16px; color: #71827e; font-size: 12px; }
.unelevated_sub a { color: #50716a !important; font-size: 12px !important; font-weight: 500; }
.bulk_box { margin-top: 0 !important; overflow: hidden; border-radius: 18px; }
.bulk_results { margin: 0 18px 12px; }
.filters { padding: 16px 0; background: #fff; border-bottom-color: #edf1f0; }
.bulk_table { overflow: hidden; border-color: #e3e9e7; border-radius: 10px; }
.bulk_table th { background: #f3f7f6; color: #36514b; }
.bulk_table th, .bulk_table td { border-color: #e3e9e7; padding: 9px; }
.bulk_footer .bulk_footer_container { padding: 18px; background: #fff; }
button.bulk_submit { width: 100%; }
@media screen and (max-width: 600px) {
  .unelevated_form { padding: 18px; border-radius: 14px; }
  .unelevated_form .labels label.labels_left,
  .unelevated_form .labels label.labels_right,
  .unelevated_form button.uef_search { width: 100%; }
  a.switch { left: calc(100% - 29px); margin-top: 38px; }
}
```
