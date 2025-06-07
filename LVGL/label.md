Create a text label that display an entity value.

```
 widgets:
  - label:
      id: sensor_label_id
      text: "nd"                      # Default text, it will displayed until the display will connected to home assistant
      text_font: montserrat_46
      grid_cell_column_pos: 0          # place the widget in the column pos
      grid_cell_row_pos: 0             # place the widget in the column row
      grid_cell_column_span: 1         # expand the widget in columns, default to 1   
      grid_cell_row_span: 1            # expand the widget in row, default to 1
      grid_cell_x_align: STRETCH       # use STRETCH or CENTER
      grid_cell_y_align: STRETCH       # use STRETCH or CENTER
      align: center




sensor:
  - platform: homeassistant
    id: sensor_label_id
    entity_id: climate.termostato_caldaia     # Define the entity
    attribute: temperature                    # OPTIONAL!!! Define the attribute
    on_value:
      then:
        lvgl.label.update:
          id: lable_id
          text:
            format: "Temperatur Set %.1f °C"   # Define text before and after the value of entity
            args: [ 'x' ]

```


For update the label, use thi metod:

```
   - lvgl.label.update:
       id: sensor_label_id          
       text: "NEW_TEXT"
       text_color: 0x00FF00
```
