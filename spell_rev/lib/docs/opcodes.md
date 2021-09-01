# The Opcodes library

A utilities library to handle the processing of opcodes.

## A. Constants.

`OPCODE_SIZE`

The size of an opcode.

### A. 1. Arrays.

`OPCODE_OFFSETS`

Array of opcode offsets. The field names are the same as in WeiDU macros like ADD_EFFECT.

note(s):
* no more `WRITE_BYTE (offset + 0x02) value`; instead write `WRITE_BYTE (offset + $SPELL_OFFSETS("target")) value`.

## B. Functions.

In general, the WeiDU suite of functions `ALTER_EFFECT`, `ADD_EFFECT` and `DELETE_EFFECT` are all that is needed for patching opcodes. However, when doing extensive patching surgery, there is a need to gather information on what is already there and act accordingly. This is the main job of the suite of functions in this library.

All functions in this section are patch functions.

`GET_OPCODE_FIELD INT_VAR offset STR_VAR field RET value`

Generic read function. Return the value of field of opcode at offset. If the field is not an opcode field, function PATCH_FAIL's.

note(s):
* the array of offsets `OPCODE_OFFSETS` allows us to do away with explicitly writing offset locations. The patch function `GET_OPCODE_FIELD` allows us to not have to remember exactly what type is the field (is it a byte? an unsigned 32 bit integer?) and which read function to use. It also allows us to start writing some generic gathering code like stuffing an entire opcode in an associative array as in:

```
DEFINE_ASSOCIATIVE_ARRAY opcode_fields BEGIN
END
PHP_EACH OPCODE_OFFSETS AS field => field_offset BEGIN
    LPF GET_OPCODE_FIELD INT_VAR offset = <some offset> STR_VAR field = "%field%" RET value END
    TEXT_SPRINT $opcode_fields("%field%") "%value%"
END
```

`SET_OPCODE_FIELD INT_VAR offset STR_VAR field value`

Generic write function. Write value, passed as a string, to field of opcode at offset. If the field is not an opcode field, function PATCH_FAIL's.

note(s):
* as mentioned above, when patching opcodes the standard WeiDU functions are, almost always, all that is needed. The exception is when the input data is stuffed inside an associative array and we want to use a loop as with `GET_OPCODE_FIELD` to alter the opcode.

`MATCH_OPCODE_WITH_ARRAY INT_VAR offset STR_VAR array RET bool`

Matches opcode field by field against the array fields. PATCH_FAIL's if any of the array fields is not an opcode field.

note(s):
* one of the reasons this is not as useful as it sounds, is that there is no simple syntax expression to construct associative arrays (unless using something like SSL or some very black magic hacks). This is the reason for the variant `MATCH_OPCODE` -- see below. Another reason is that without `AUTO_EVAL_STRINGS` you have to interpolate an `EVAL` every time you pass an array by name to a function, which quickly becomes tedious and is error-prone.

```
MATCH_OPCODE INT_VAR
    offset
    opcode = -1
    target = -1
    power = -1
    parameter1 = -1
    parameter2 = -1
    timing = -1
    resist_dispel = -1
    duration = -1
    probability1 = -1
    probability2 = -1
    dicenumber = -1
    dicesize = -1
    savingthrow = -1
    savebonus = -11
    special = -1
STR_VAR
    resource = "*"
RET
    bool
END
```

Matches opcode at offset against the given field values. This follows the WeiDU conventions for the various `match_*` fields (now renamed and stripped of the leading "match_" prefix) of the function `ALTER_EFFECT`.