# A BUTTON CLICCABLE #

Create a button with an immage and a text in lvgl grid layout.
If you press the button the system will call a script or a home assistant automation, depends what you want to do.

```
widgets:
  - button:
      id: button_id
      align: CENTER
      grid_cell_column_pos: 0          # place the widget in the column pos
      grid_cell_row_pos: 0             # place the widget in the column row
      grid_cell_column_span: 1         # expand the widget in columns, default to 1   
      grid_cell_row_span: 1            # expand the widget in row, default to 1
      grid_cell_x_align: STRETCH       # use STRETCH or CENTER
      grid_cell_y_align: STRETCH       # use STRETCH or CENTER
      bg_color: 0x8fce00               # Define color when the button IS NOT pressed
      checked:
        bg_color: 0xFF0000            # Define color when the button IS pressed
      widgets:                            # Add widgets inside the widget 
        - image:                          # Add an image (you need to define it)
            src: transmissiontower        
            pad_all: 2                    
        - label:                          # Add a text
            text: "ENERGIA"
            text_font: montserrat_28
            align: CENTER
            text_align: CENTER
            text_color: 0xFFFFFF
      on_press:                        # Define what happen if you press the button
        then:
          lvgl.page.show: page_xxx    # For example, show a page

      Or you can call a script or automation in home assistant directly

      on_press:
      - homeassistant.action:
          action: script.toggle   /   automation.trigger
          data:
            entity_id: script.xxxx   /   automation.xxxx

image:
  - file: mdi:transmission-tower   # Define the image
    id: transmissiontower
    type: binary
    resize: 70x70


```

In addition, if you whant that the botton will follow hte stats of home assistant you need to add the lambda role.
(For example:
if you have a button that start / stop an automation and you click start in home assistant WebUi, the button on the LVGL display will follow the status.)

```
binary_sensor:
  - platform: homeassistant
    id: status_button_id
    entity_id: automation.allarme_perimetrale
    publish_initial_state: true
    on_state:
      then:
        lvgl.widget.update:
          id: button_id
          state:
            checked: !lambda return x;
```

Another userfull case is a button not cliccable that follow some binary sensor; as a "led".
You need to remove 'on_press' and add 'clickable: false.

```
widgets:
  - button:
      id: button_id
      align: CENTER
      grid_cell_column_pos: 0          # place the widget in the column pos
      grid_cell_row_pos: 0             # place the widget in the column row
      grid_cell_column_span: 1         # expand the widget in columns, default to 1   
      grid_cell_row_span: 1            # expand the widget in row, default to 1
      grid_cell_x_align: STRETCH       # use STRETCH or CENTER
      grid_cell_y_align: STRETCH       # use STRETCH or CENTER
      bg_color: 0x8fce00               # Define color when the button IS NOT pressed
      clickable: false
      checked:
        bg_color: 0xFF0000            # Define color when the button IS pressed
      widgets:                            # Add widgets inside the widget 
        - image:                          # Add an image (you need to define it)
            src: transmissiontower        
            pad_all: 2                    
        - label:                          # Add a text
            text: "ENERGIA"
            text_font: montserrat_28
            align: CENTER
            text_align: CENTER
            text_color: 0xFFFFFF

```




