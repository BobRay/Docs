---
title: "FormitFastPack Tutorial"
_old_id: "1709"
_old_uri: "revo/formitfastpack/formitfastpack.tutorial"
---

 In this tutorial, we will be creating a simple FormIt contact form. FFP will help reduce repetition of HTML between forms and fields.

### 1. Call FormIt as usual, but use **field** snippets to manage the form HTML.

 ``` php
[[!FormIt?
    &hooks=`math,spam,email,redirect`
    &emailTpl=`ContactFormReport`
    &emailTo=`[[++emailsender]]`
    &emailSubject=`New message from [[++site_name]] [[*pagetitle]] page.`
    &redirectTo=`[[++site_start]]`
    &submitVar=`submitForm` &errTpl=`[[+error]] `
    &validate=`nospam:blank,full_name:required,email:email:required,message:required`
]]
[[!fieldSetDefaults? &prefix=`fi.` &outer_type=`default` &tpl=`fieldTypesTpl` &outer_tpl=`fieldWrapTpl`]]
<div>[[!+fi.error.error_message]] [[!+fi.validation_error_message]] [[!+fi.error.recaptcha]]</div>

<form id="ContactForm" action="[[~[[*id]]]]#ContactForm" method="post"><div>
<input name="nospam" type="hidden" />
[[!field? &type=`text` &name=`full_name` &req=`1`]]
[[!field? &type=`text` &name=`email` &req=`1`]]
[[!field? &type=`text` &name=`phone`]]
[[!field? &type=`textarea` &name=`message` &class=`cleardefault` &req=`1`]]
[[!field? &type=`text` &req=`1` &name=`math` &label=`What is [[!+fi.op1]] [[!+fi.operator]] [[!+fi.op2]]?`]]
    <input type="hidden" name="op1" value="[[!+fi.op1]]" />
    <input type="hidden" name="op2" value="[[!+fi.op2]]" />
    <input type="hidden" name="operator" value="[[!+fi.operator]]" />
[[!field? &type=`submit` &name=`submitForm` &label=` ` &message=`Send this Message!`]]
</div>
</form>
```

#### Explanation:

 The `[[!fieldSetDefaults]]` snippet call sets the default values for the subsequent field calls. The values used here are unnecessary, as they are the default values.

 Each `[[!field]]` snippet call manages the HTML for a single form field. This includes handling the latest value and error placeholders generated by FormIt.

### 2. OPTIONAL: Use the fiGenerateReport hook to simplify your email templates.

 A. Add the fiGenerateReport right before the "email" hook.

 ``` php
[[!FormIt?
  &hooks=`math,spam,fiGenerateReport,email,redirect`
  ...
]]<br>
```

 B. Add the &figrExcludedFields parameter to exclude the special fields used by the math and spam fields from the email report.

 ``` php
[[!FormIt?
  &hooks=`math,spam,fiGenerateReport,email,redirect`
  &figrExcludedFields=`op1,op2,operator,math`
]]
```

 C. In your emailTpl template chunk (called "ContactFormReport" in the above example, use the figr\_values placeholder to output a dynamic list of fields:

 ``` php
<p>A <strong>[[++site_name]]</strong> contact form submission was sent from the <strong>[[*pagetitle]]</strong> page:</p>
[[+figr_values]]
```

### 3. OPTIONAL: customize the &outer\_tpl chunk

 Create a chunk called **fieldWrapTpl**, where we will template the HTML for the "default" outer type.

 Important: the tpl and outer\_tpl chunks use special comments to separate the HTML for different field types. Make sure each type is surrounded above and below by an HTML comment of the type name as shown below, or you might get more output than you expected!

 ``` php
<!-- default -->
<div class="[[+outer_class:default=`field_wrap`]] [[+type]]_wrap" id="[[+name]]_wrap">
<label for="[[+name]]" title="[[+name:replace=`_== `:ucwords]]">[[+label:default=`[[+name:replace=`_== `:ucwords]]`]][[+req:notempty=` *`]]</label>
[[+inner_html]]
[[+note:notempty=`<span class="[[+note_class:default=`note`]]"><em>[[+note]]</em></span>`]]
[[+error:notempty=`<span class="[[+error_class]]">[[+error]]</span>`]]
</div>
<!-- default -->
```

### 4. OPTIONAL: customize the &tpl chunk

 Create a chunk called **fieldTypesTpl**, where we will template the HTML for all of the field types at once.

 Important: the tpl and outer\_tpl chunks use special comments to separate the HTML for different field types. Make sure each type is surrounded above and below by an HTML comment of the type name as shown below, or you might get more output than you expected!

``` html
<!-- default -->
  <input type="[[+type]]" name="[[+name]]" id="[[+key]]" value="[[+current_value]]" class="[[+type]] [[+class]][[+error_class]]" size="[[+size:default=`40`]]" />
<!-- default -->
<!-- file -->
  <input type="[[+type]]" name="[[+name]][[+array:notempty=`[]`]]" id="[[+key]]" class="[[+type]] [[+class]][[+error_class]]" />
<!-- file -->
<!-- hidden -->
  <input type="[[+type]]" name="[[+name]]" value="[[+current_value]]" />
<!-- hidden -->
<!-- textarea -->
  <textarea id="[[+key]]" class="[[+type]] [[+class]][[+error_class]]" name="[[+name]]">[[+current_value]]</textarea>
<!-- textarea -->
<!-- checkbox -->
<span class="boolWrap[[+error_class]]">
<input name="[[+name]][[+array:notempty=`[]`]]" type="hidden" value="" />
[[+options_html]]
</span>
<!-- checkbox -->
<!-- radio -->
<span class="boolWrap[[+error_class]]">
<input type="hidden" name="[[+name]]" value="" />
[[+options_html]]
</span>
<!-- radio -->
<!-- select -->
<span class="[[+class]][[+error_class]]">
<input type="hidden" name="[[+name]][[+array:notempty=`[]`]]" value="" />
<select name="[[+name]][[+array:notempty=`[]`]]" id="[[+key]]" class="[[+class]]"[[+multiple:notempty=` multiple="multiple"`]][[+title:notempty=` title="[[+title]]"`]]>
  [[+header:notempty=`<option value="[[+default_value]]">[[+header]]</option>`]]
  [[+options_html]]
</select>
</span>
<!-- select -->
<!-- static -->
<span class="static_field[[+error_class]]">[[!+[[+name]]]]</span>
<!-- static -->
<!-- submit -->
<input id="[[+key]]" class="button [[+type]] [[+class]]" name="[[+name]]" type="[[+type]]" value="[[+message:default=`Submit`]]" />
<input id="[[+name]]-clear" class="button [[+type]] [[+class]]" type="reset" value="[[+clear_message:default=`Clear Form`]]" />
<!-- submit -->
<!-- option --><option value="[[+value]]">[[+label]]</option><!-- option -->
<!-- bool -->
<span class="boolDiv [[+class]]">
<input type="[[+type]]" class="[[+type]]" value="[[+value]]" name="[[+name]][[+array:notempty=`[]`]]" id="[[+key]]"  />
<label for="[[+key]]" class="[[+type]]" id="label[[+key]]">[[+label]]</label></span><!-- bool -->
```
