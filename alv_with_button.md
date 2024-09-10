# The REUSE_ALV_GRID_DISPLAY function.

The REUSE_ALV_GRID_DISPLAY is commonly used in ABAP to display data on the output.

In order for the reuse_alv_grid_display to work we need the followings:

1. A local/global (not DB) table that contains the retrieved data.

2. The layout structure.

3. The field catalog table. Is a table that contains all the fields we want to display on the output, the order of the field, the column title (if we don't need the standard one) and much more options.

_Only for adding or changing a button from the GUI_STATUS. Else the default/standard is used_

4. The PF_STATUS (GUI_STATUS). Is responsible for the button of the screen on ALV.

![image](https://github.com/user-attachments/assets/55fb1622-75f6-449e-ba75-f12dac63349d)

In order to use a custom PF_STATUS, to avoid creating a new one, we can copy a standard to our custom report.
* Program --> SAPLKKBL
* GUI_STATUS --> STANDARD

![image](https://github.com/user-attachments/assets/6d7a601f-665d-478c-aa8b-6a16da0b84c4)

![image](https://github.com/user-attachments/assets/ac4573f9-173d-4f8f-9f07-6b87f031927a)

![image](https://github.com/user-attachments/assets/eda8d16d-05c1-4776-a566-06395305c664)

Lets say that we want a button to delete a row from the ALV. We have to add on the GUI_STATUS the name of the "event".

![image](https://github.com/user-attachments/assets/291d8704-5f22-4f97-a83d-3b86ee2eff89)

By pressing enter we have the following popup, we press Enter.

![image](https://github.com/user-attachments/assets/047590b1-c69c-4f07-af77-aca1d8b07ce5)

Next we insert the text for the button + the icon.

![image](https://github.com/user-attachments/assets/51c56d54-bd40-4525-930b-2c5336cdec60)

We assign a shortcut and then continue.

![image](https://github.com/user-attachments/assets/ceb2ab34-29ff-4001-a427-6c68bd8f3812)

Finally we have the button in the GUI_STATUS.

![image](https://github.com/user-attachments/assets/86dab9bc-636c-4d77-94f4-4719e4519e12)


5. The USER_COMMAND. Is responsible to handle the events/click of the buttons.

In the below example we will see the order to 

> [!IMPORTANT]
> Keep in mind that in FM of ALV, the names should be the same as the FORMS!
> 
>       i_callback_pf_status_set = 'PF_STATUS'
>       i_callback_user_command  = 'USER_COMMAND'
> 
>       FORM user_command USING pi_ucomm LIKE sy-ucomm
>                               pi_selfield TYPE slis_selfield.
> 
>       FORM pf_status USING pi_extab TYPE slis_t_extab.

### Below an example code.

```abap

REPORT z_alv_grid_report.

"Data Declaration
TYPES: BEGIN OF st_qmel,
         qmnum TYPE qmel-qmnum,
         objnr TYPE qmel-objnr,
       END OF st_qmel.

DATA: lt_qmel TYPE STANDARD TABLE OF st_qmel,
      wa_qmel TYPE st_qmel,
      lt_ihpa TYPE STANDARD TABLE OF ihpa,
      wa_ihpa TYPE ihpa.

TYPES: BEGIN OF st_delete,
         mandt   TYPE mandt,
         objnr   TYPE ihpa-objnr,
         parvw   TYPE ihpa-parvw,
         counter TYPE ihpa-counter,
       END OF st_delete.

TYPES: BEGIN OF st_alv,
         checked TYPE c,
         qmnum   TYPE qmel-qmnum,
         parvw   TYPE ihpa-parvw,
         parnr   TYPE ihpa-parnr,
       END OF st_alv.

"ALV
DATA: gv_repid    TYPE sy-repid,
      gt_out_alv  TYPE STANDARD TABLE OF st_alv,
      gt_fieldcat TYPE slis_t_fieldcat_alv,
      wa_fieldcat LIKE LINE OF gt_fieldcat,
      gs_layout   TYPE slis_layout_alv,
      lt_delete   TYPE STANDARD TABLE OF st_delete,
      wa_delete   TYPE st_delete.

"Selection Screen
SELECTION-SCREEN: BEGIN OF BLOCK b1 WITH FRAME TITLE TEXT-000.

PARAMETERS: p_qmnum TYPE qmel-qmnum,
            p_parvw TYPE ihpa-parvw DEFAULT 'KU',
            p_parnr TYPE ihpa-parnr OBLIGATORY.

SELECTION-SCREEN: END OF BLOCK b1.

START-OF-SELECTION.

  SELECT qmnum objnr
    FROM qmel
    INTO TABLE lt_qmel
    WHERE qmnum = p_qmnum.

  IF lt_qmel IS NOT INITIAL.
    SELECT *
      FROM ihpa
      INTO TABLE lt_ihpa
      FOR ALL ENTRIES IN lt_qmel
      WHERE objnr = lt_qmel-objnr
      AND   parvw = p_parvw
      AND   parnr = p_parnr.
  ENDIF.

  IF lt_ihpa IS INITIAL.
    MESSAGE TEXT-e00 TYPE 'E'. "Display no values found
  ENDIF.

  MOVE-CORRESPONDING lt_ihpa TO gt_out_alv.
  PERFORM build_layout.
  PERFORM fieldcatalog.
  PERFORM display_alv.


FORM display_alv.

  gv_repid = sy-repid.

  CALL FUNCTION 'REUSE_ALV_GRID_DISPLAY'
    EXPORTING
*     I_INTERFACE_CHECK        = ' '
*     I_BYPASSING_BUFFER       = ' '
*     I_BUFFER_ACTIVE          = ' '
      i_callback_program       = gv_repid
      i_callback_pf_status_set = 'PF_STATUS'
      i_callback_user_command  = 'USER_COMMAND'
*     I_CALLBACK_TOP_OF_PAGE   = ' '
*     I_CALLBACK_HTML_TOP_OF_PAGE       = ' '
*     I_CALLBACK_HTML_END_OF_LIST       = ' '
*     I_STRUCTURE_NAME         =
*     I_BACKGROUND_ID          = ' '
*     I_GRID_TITLE             =
*     I_GRID_SETTINGS          =
      is_layout                = gs_layout
      it_fieldcat              = gt_fieldcat
*     IT_EXCLUDING             =
*     IT_SPECIAL_GROUPS        =
*     IT_SORT                  =
*     IT_FILTER                =
*     IS_SEL_HIDE              =
*     I_DEFAULT                = 'X'
*     I_SAVE                   = ' '
*     IS_VARIANT               =
*     IT_EVENTS                =
*     IT_EVENT_EXIT            =
*     IS_PRINT                 =
*     IS_REPREP_ID             =
*     I_SCREEN_START_COLUMN    = 0
*     I_SCREEN_START_LINE      = 0
*     I_SCREEN_END_COLUMN      = 0
*     I_SCREEN_END_LINE        = 0
*     I_HTML_HEIGHT_TOP        = 0
*     I_HTML_HEIGHT_END        = 0
*     IT_ALV_GRAPHICS          =
*     IT_HYPERLINK             =
*     IT_ADD_FIELDCAT          =
*     IT_EXCEPT_QINFO          =
*     IR_SALV_FULLSCREEN_ADAPTER        =
* IMPORTING
*     E_EXIT_CAUSED_BY_CALLER  =
*     ES_EXIT_CAUSED_BY_USER   =
    TABLES
      t_outtab                 = gt_out_alv
    EXCEPTIONS
      program_error            = 1
      OTHERS                   = 2.
  IF sy-subrc <> 0.
* Implement suitable error handling here
  ENDIF.

ENDFORM.

FORM fieldcatalog.

  CLEAR: wa_fieldcat.
  wa_fieldcat-fieldname = 'QMNUM'.
  wa_fieldcat-ref_fieldname = 'QMNUM'.
  wa_fieldcat-ref_tabname = 'QMEL'.
  APPEND wa_fieldcat TO gt_fieldcat.

  CLEAR: wa_fieldcat.
  wa_fieldcat-fieldname = 'PARVW'.
  wa_fieldcat-ref_fieldname = 'PARVW'.
  wa_fieldcat-ref_tabname = 'IHPA'.
  APPEND wa_fieldcat TO gt_fieldcat.

  CLEAR: wa_fieldcat.
  wa_fieldcat-fieldname = 'PARNR'.
  wa_fieldcat-ref_fieldname = 'PARNR'.
  wa_fieldcat-ref_tabname = 'IHPA'.
  APPEND wa_fieldcat TO gt_fieldcat.

ENDFORM.

FORM build_layout.

  gs_layout-colwidth_optimize = 'X'.
  gs_layout-zebra = 'X'.
  gs_layout-box_fieldname = 'CHECKED'.

ENDFORM.

FORM user_command USING pi_ucomm LIKE sy-ucomm
                        pi_selfield TYPE slis_selfield.

  CASE pi_ucomm.
    WHEN 'DEL_ROW'.

      LOOP AT gt_out_alv INTO wa_out_alv WHERE checked = 'X'.
        MOVE-CORRESPONDING: wa_out_alv TO wa_delete.
        wa_delete-mandt = sy-mandt.
        APPEND wa_delete TO lt_delete.
      ENDLOOP.

      DELETE ihpa FROM TABLE lt_delete.
      IF sy-subrc EQ 0.
        COMMIT WORK AND WAIT.
      ENDIF.
  ENDCASE.

ENDFORM.

FORM pf_status USING pi_extab TYPE slis_t_extab.

  SET PF-STATUS 'ZSTANDARD' EXCLUDING pi_extab.

ENDFORM.

```
