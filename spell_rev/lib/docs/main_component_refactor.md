# A. Main component refactoring.

This readme file details the progress in refactoring the main component.

## A. 1. Justification.

The main component of SR does a number of basic things. For what interests us here, its main job is copying the resources needed and setting them up (e.g. add new spells). The code is just a large list of COPY's with ad-hoc patching and copying. This is just unmaintainable; much better to make the code data-driven by sticking all the needed data on a table and then looping over it. The exact details will be discussed below; the first step is to introduce some auxiliary libraries.

## A. 2. Auxiliary libraries.

WeiDU is the de-facto tool used to mod the game, but it has several problems as a programming language. One of them is the very poor standard library: this can be seen by any cursory look at any standard mod that does any significant patching: there are magical numbers, in the form of offsets, everywhere. This is a sad state of affairs. In the hopes of mitigating the problem three new libraries are introduced: opcodes.tpa for dealing with opcodes, spl.tpa for dealing with spells and eff.tpa for dealing with effects. The libraries are located for now in lib/macros and are fully documented in the relevant files in lib/docs.