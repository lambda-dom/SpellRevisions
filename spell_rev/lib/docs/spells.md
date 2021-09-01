# The Spells library

A utilities library to handle the processing of spells.

## A. Constants.

`SPELL_SIZE`

Size in bytes of the (main) spell block.

`SPELL_HEADER_SIZE`

Size in bytes of a spell header.

### A. 1. Arrays.

`SPELL_OFFSETS`

Array of spell offsets.

`SPELL_HEADER_OFFSETS`

Array of spell header offsets. The field names are the same as in WeiDU macros like ALTER_SPELL_HEADER.

note(s):
* these two arrays do *not* contain fields, like the offset of the opcodes block, that should not be messed with directly. To get their values use the functions.

## B. Spell functions.

All functions in this section are patch functions.

### B. 1. Basic functions.

Basic read functions that return the values of fields that should not be read directly.

`GET_SPELL_HEADER_COUNT RET count`

Return number of spell headers in spell.

`GET_SPELL_HEADERS_OFFSET RET offset`

Return offset of the spell headers block.

`GET_SPELL_OPCODES_OFFSET RET offset`

Return offset of the spell opcodes block.

note(s):
* due to the way the opcode block is organized this is the same as the offset of the first casting opcode.

`GET_CASTING_OPCODE_COUNT RET count`

Return number of casting opcodes in spell.

### B. 2. Read-write functions.

`GET_SPELL_FIELD STR_VAR field RET value`

Generic read function. Return the value of spell field. If field is not a spell field, or is an internal field like the headers count, function PATCH_FAIL's.

`SET_SPELL_FIELD STR_VAR field value`

Generic write function. Write value, passed as a string, to spell field. If field is not a spell field, or is an internal field like the headers count, function PATCH_FAIL's.

note(s):
* for most purposes, the WeiDU function `ALTER_SPELL_HEADER` is enough. However, if you have the input data in an associative array then this function comes in handy.

### B. 3. Spell header functions.

`GET_SPELL_HEADER_OFFSET INT_VAR header RET offset`

Return offset of (0-indexed) header. Function PATCH_FAIL's if header out of bounds.

`GET_SPELL_HEADER_FIELD INT_VAR header STR_VAR field RET value`

Generic read function. Read the value of a spell header field. PATCH_FAIL's if header out of bounds or field is not a spell head field or is an internal, private field.

`SET_SPELL_HEADER_FIELD INT_VAR header STR_VAR field value`

Generic write function. Write value, passed as a string, to spell header field. If field is not a spell header field, or is an internal field like the header opcode count, function PATCH_FAIL's.

### B. 4. Spell header opcode functions.

`GET_SPELL_HEADER_OPCODE_COUNT INT_VAR header RET count`

Return the number of opcodes of header. Function PATCH_FAIL's if header out of bounds.

`GET_SPELL_HEADER_OPCODE_OFFSET INT_VAR header index RET offset`

Return offset of index opcode (0-indexed) of header. If any of index or header are out of bounds, function PATCH_FAIL's.

### B. 5. Search functions.
