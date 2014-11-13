## react-filtered-multiselect

A `<FilteredMultiSelect/>` React component, for making and adding to selections
using a filtered multi-select.

[Live example with Bootstrap styles applied](http://insin.github.io/react-filtered-multiselect/)

![FilteredMultiSelect with Bootstrap styles screenshot](https://github.com/insin/react-filtered-multiselect/raw/master/bootstrap-example.png "A FilteredMultiSelect with Bootstrap styles applied")

### About

This component manages an `<input>`, a `<select multiple>` and a `<button>`.

You provide a list of objects to be used to populate the select and an `onChange`
callback function.

Typing in the the input will filter the select to items with matching option text.

When some of the select's options are selected, the button will become enabled.
Clicking it will select the objects backing the currently selected options.

If only one option is displayed after filtering against the input, pressing Enter
in the input will select the object backing it.

When backing objects are selected, the `onChange` callback is executed, passing
a list of all backing objects selected so far.

To deselect items, remove them from the list passed back via the `onChange`
callback and re-render the `FilteredMultiSelect` with the new list passed as its
`selectedOptions` prop.

### Required props

Minimal usage:

```javascript
var options = [
  {value: 1, text: 'Item One'}
, {value: 2, text: 'Item Two'}
]

<FilteredMultiSelect
  onChange={this.onChange}
  options={options}
/>
```

`options` - list of objects providing `<option>` data for the multi-select. By
default, these should have ``text`` and ``value`` properties, but this is
configurable via props.

`onChange(selectedOptions)` - callback which will be called with selected option
objects each time the selection is added to.

### Optional props

`buttonText` - text to be displayed in the `<button>` for adding selected `<option>`s.

`className` - class name for the component's `<div>` container.

`classNames` - class names for each of the component's child elements.

`defaultFilter` - default filter text to be applied to the `<select>`

`disabled` - disables each child element if `true`.

`placeholder` - placeholder text to be displayed in the filter `<input>`.

`selectedOptions` - array of option objects which are selected, so should no
longer be displayed in the `<select>`.

`size` - `size` attribute for the `<select>`

`textProp` - name of the property in each object in `options` which provides
the displayed text for its `<option>`.

`valueProp` - name of the property in each object in `options` which provides
the `value` for its `<option>`.

#### Default props

```javascript
{
  buttonText: 'Select',
  className: 'FilteredMultiSelect',
  classNames: {
    button: 'FilteredMultiSelect__button',
    filter: 'FilteredMultiSelect__filter',
    select: 'FilteredMultiSelect__select'
  }
  defaultFilter: '',
  disabled: false,
  placeholder: 'type to filter',
  size: 6,
  selectedOptions: [],
  textProp: 'text',
  valueProp: 'value'
}
```

### Example

Example which implements display of selected items and de-selection.

```javascript
var CULTURE_SHIPS = [
  {id: 1, name: "5*Gelish-Oplule"}
, {id: 2, name: "7*Uagren"}
// ...
, {id: 249, name: "Zero Gravitas"}
, {id: 250, name: "Zoologist"}
]

var Example = React.createClass({
  getInitialState: function() {
    return {selectedShips: []}
  },

  onMultiSelectChanged: function(selectedShips) {
    this.setState({selectedShips: selectedShips.slice()})
  },

  deselectShip: function(index) {
    var selectedShips = this.state.selectedShips.slice()
    selectedShips.splice(index, 1)
    this.setState({selectedShips: selectedShips})
  },

  render: function() {
    return <div>
      <FilteredMultiSelect
        onChange={this.onMultiSelectChanged}
        options={CULTURE_SHIPS}
        selectedOptions={this.state.selectedShips}
        textProp="name"
        valueProp="id"
      />
      {this.state.selectedShips.length === 0 && <p>(nothing selected yet)</p>}
      {this.state.selectedShips.length > 0 && <ul>
        {this.state.selectedShips.map(function(ship, i) {
          return <li>{ship.name}{' '}
            <button type="button" onClick={this.deselectShip.bind(null, i)}>
              &times;
            </button>
          </li>
        }.bind(this))}
      </ul>}
    </div>
  }
})
```

### MIT Licensed