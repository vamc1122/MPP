PROGRAM ZSDR_MDP_EX1_VALIDATE.

DATA A TYPE T001-BUKRS.
DATA B TYPE KNB1-KUNNR.

DATA V1 TYPE KNB1-KUNNR.
DATA V2 TYPE KNB1-BUKRS.
      TYPES : BEGIN OF TY_KNB1,
              KUNNR TYPE KNB1-KUNNR,
              BUKRS TYPE KNB1-BUKRS,
              PERNR TYPE KNB1-PERNR,
            END OF TY_KNB1.
*
    DATA :   IT_KNB1 TYPE STANDARD TABLE OF TY_KNB1,
      WA_KNB1 TYPE TY_KNB1.


*&---------------------------------------------------------------------*
*&      Module  XYZ  INPUT
*&---------------------------------------------------------------------*
*       text
*----------------------------------------------------------------------*
MODULE XYZ INPUT.


  SELECT SINGLE KUNNR BUKRS FROM KNB1
    INTO (V1,V2)
    WHERE KUNNR = B
    AND BUKRS = A .
    IF SY-SUBRC NE 0.
  "  //  MESSAGE ZMESS
      MESSAGE 'PLEASE ENTER VALID CUSTOMER UNDER COMPANY CODE' TYPE 'E'.
  ENDIF.


ENDMODULE.
*&---------------------------------------------------------------------*
*&      Module  USER_COMMAND_1000  INPUT
*&---------------------------------------------------------------------*
*       text
*----------------------------------------------------------------------*
MODULE USER_COMMAND_1000 INPUT.

  IF SY-UCOMM = 'DIS'.
      SELECT KUNNR BUKRS  PERNR FROM KNB1
         INTO TABLE IT_KNB1
        WHERE  bukrs = A AND KUNNR =  B.

        LEAVE TO LIST-PROCESSING.
  IF IT_KNB1 IS NOT INITIAL.
        LOOP AT IT_KNB1 INTO WA_KNB1.
          WRITE :/ WA_KNB1-KUNNR ,
          WA_KNB1-BUKRS,
          WA_KNB1-PERNR.
          ENDLOOP.
          ENDIF.
    ELSEIF SY-UCOMM = 'BACK'.
    LEAVE PROGRAM .
    ENDIF.

ENDMODULE.
