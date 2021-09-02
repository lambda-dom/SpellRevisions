# The Effects library

A library to handle extended effect .eff files.

## A. Constants.

`EFFECT_SIZE`

Size (in bytes) of an extended effect.

### A. 1. Arrays.

`EFFECT_OFFSETS`

Array of extended effect offsets. The field names are the same as in WeiDU macros like ADD_SPELL_EFFECT.

## B. Basic functions.

All functions in this section are patch functions.

`GET_EFFECT_FIELD STR_VAR field RET value `

Generic read function. Return the value of effect field. If the field is not an effect field, function PATCH_FAIL's.

`SET_EFFECT_FIELD STR_VAR field value`

Generic write function. Write value, passed as a string, to field effect. If the field is not an effect field, function PATCH_FAIL's.
