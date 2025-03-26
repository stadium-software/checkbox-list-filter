# CheckBox List Filter

When checkbox lists contain many items, finding an item can be cumbersome and frustrating. Here is a simple example of how to add a filter to a checkbox list. 

https://github.com/user-attachments/assets/408df2a2-d9cc-4313-b1fe-4e4fda4c503e

# Version 
1.0 - initial

1.0.1 Added max-height variable (CSS only)

1.0.2 Added input background color variable (CSS only)

## Application Setup
1. Check the *Enable Style Sheet* checkbox in the application properties

## Global Script Setup
1. Create a Global Script called "CheckBoxListFilter"
2. Drag a JavaScript action into the script
3. Add the Javascript below unchanged into the JavaScript code property
```javascript
/* Stadium Script v1.0 https://github.com/stadium-software/checkbox-list-filter */
let checkboxList = document.querySelectorAll(".filterable-checkbox-list");
for (let i = 0; i < checkboxList.length; i++) {
    let filterField = document.createElement("input");
    filterField.classList.add("form-control", "error-border", "text-box-input", "checkbox-list-filter-input");
    filterField.setAttribute("placeholder", "Filter");
    filterField.addEventListener("keyup", filterCheckBoxList);
    let filterFieldContainer = document.createElement("div");
    filterFieldContainer.classList.add("control-container", "text-box-container", "checkbox-list-filter");
    filterFieldContainer.appendChild(filterField);
    let filterClear = document.createElement("div");
    filterClear.classList.add("clear-list-filter");
    filterClear.addEventListener("click", resetFilter);
    filterFieldContainer.appendChild(filterClear);
    checkboxList[i].prepend(filterFieldContainer);
}
function filterCheckBoxList(e) {
    let hasResults = false;
    let input = e.target;
    let checkboxListFilter = input.closest(".checkbox-list-filter");
    if (input.value) checkboxListFilter.querySelector(".clear-list-filter").style.display = "block";
    let container = input.closest(".check-box-list-container");
    removeMsg(container);
    let checkboxes = container.querySelectorAll(".checkbox");
    for (let i = 0; i < checkboxes.length; i++) {
        checkboxes[i].style.display = "block";
        let txt = checkboxes[i].querySelector("label").textContent;
        if (txt.indexOf(input.value) == -1) {
            checkboxes[i].style.display = "none";
        } else {
            hasResults = true;
        }
    }
    if (!hasResults) {
        let msg = document.createElement("div");
        msg.innerHTML = "No matching items";
        msg.classList.add("checkbox-list-message");
        insertAfter(checkboxListFilter, msg);
    }
}
function resetFilter(e) {
    let container = e.target.closest(".check-box-list-container");
    removeMsg(container);
    container.querySelector(".checkbox-list-filter-input").value = "";
    let checkboxes = e.target.closest(".check-box-list-container").querySelectorAll(".checkbox");
    for (let i = 0; i < checkboxes.length; i++) {
        checkboxes[i].style.display = "block";
    }
    e.target.style.display = "none";
}
function removeMsg(cont) {
    if (cont.querySelector(".checkbox-list-message")) cont.querySelector(".checkbox-list-message").remove();
}
function insertAfter(referenceNode, newNode) {
    referenceNode.parentNode.insertBefore(newNode, referenceNode.nextSibling);
}
```

## Checkbox List Setup
1. Drag a CheckBoxList Control to the page
2. Add the classname 'filterable-checkbox-list' to the control classes property
3. Populate the CheckBoxList with data

## Page.Load Event Setup
1. Drag the "CheckBoxListFilter" script into the Page.Load event
2. The filter matches strings using *Contains* and is case sensitive

## CSS
The CSS below is required for the correct functioning of the module. Variables exposed in the [*checkbox-list-filter-variables.css*](checkbox-list-filter-variables.css) file can be [customised](#customising-css).

### Before v6.12
1. Create a folder called "CSS" inside of your Embedded Files in your application
2. Drag the two CSS files from this repo [*checkbox-list-filter-variables.css*](checkbox-list-filter-variables.css) and [*checkbox-list-filter.css*](checkbox-list-filter.css) into that folder
3. Paste the link tags below into the *head* property of your application
```html
<link rel="stylesheet" href="{EmbeddedFiles}/CSS/checkbox-list-filter.css">
<link rel="stylesheet" href="{EmbeddedFiles}/CSS/checkbox-list-filter-variables.css">
``` 

### v6.12+
1. Create a folder called "CSS" inside of your Embedded Files in your application
2. Drag the CSS files from this repo [*checkbox-list-filter.css*](checkbox-list-filter.css) into that folder
3. Paste the link tag below into the *head* property of your application
```html
<link rel="stylesheet" href="{EmbeddedFiles}/CSS/checkbox-list-filter.css">
``` 

### Customising CSS
1. Open the CSS file called [*checkbox-list-filter-variables.css*](checkbox-list-filter-variables.css) from this repo
2. Adjust the variables in the *:root* element as you see fit
3. Stadium 6.12+ users can comment out any variable they do **not** want to customise
4. Add the [*checkbox-list-filter-variables.css*](checkbox-list-filter-variables.css) to the "CSS" folder in the EmbeddedFiles (overwrite)
5. Paste the link tag below into the *head* property of your application (if you don't already have it there)
```html
<link rel="stylesheet" href="{EmbeddedFiles}/CSS/checkbox-list-filter-variables.css">
``` 
6. Add the file to the "CSS" inside of your Embedded Files in your application

**NOTE: Do not change any of the CSS in the 'checkbox-list-filter.css' file**

## Upgrading Stadium Repos
Stadium Repos are not static. They change as additional features are added and bugs are fixed. Using the right method to work with Stadium Repos allows for upgrading them in a controlled manner. 

How to use and update application repos is described here: [Working with Stadium Repos](https://github.com/stadium-software/samples-upgrading)