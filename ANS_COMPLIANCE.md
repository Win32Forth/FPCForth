# ANS Forth 2012 Compliance Status for TZForth (based on Leif Bruder's lbForth / LBForth model)

This document summarizes the implementation status of the Core and Core Extensions word sets from the 2012 ANS Forth Standard in the current codebase (as of the latest commits).

## Implemented Words (Core + Extensions, non-exhaustive)
The engine implements a substantial and practical subset of the standard, focused on usability for classic Forth sources (e.g., F-PC style, Forthing.fth). This includes:

- Core arithmetic (incl. new / +! */MOD etc), double-cell stack ops, memory (FILL etc), literals/immediate, pictured numeric (<# etc), S", etc. (see added list in missing section notes).
- Many Core Ext: 2>R 2R@ 2R>, NIP, PICK, ROLL, TUCK, U.R, WITHIN, ?DO, etc.
- App-specific but useful extensions: FLOAD, EDIT, CHDIR, DIR, FILE-ECHO, DEBUG-ON/OFF, >HEADER, >NFA, ID., FORGET-WORD, etc.
- High-level conveniences: HERE as DP @, >LFA, >NFA, ID., etc.
- Full test coverage expanded in TestTZForth.swift (FTEST; originally TestLBForth.swift) for many words per standard stack effects.

See `TZForth/TZForth.swift` (register calls + primitiveHelpData + bootstrap high-level defs; originally LBForth.swift) and `TestTZForth.swift` for details. `WORDS` in the REPL shows current dictionary.

## Missing from Core Word Set (6.1 - required for conformance)
(Compiled via comparison of standard list vs. current `primitiveHelpData` + live WORDS + registrations. Reduced; many Core words now implemented with tests: double stack, arith, memory, compile/immed, pictured, S", EXECUTE/J/RECURSE, >IN/>NUMBER/ABORT etc, EVALUATE/FIND etc.)

*/
<#
>BODY
HOLD
POSTPONE
QUIT
S"
S>D
SIGN
SOURCE
[']
[CHAR]

Notes on some:
- Pictured numeric output (<# # #S #> HOLD SIGN) + S" + EXECUTE/J/RECURSE + >IN >NUMBER ABORT ABORT" ACCEPT ENVIRONMENT? EVALUATE FIND now implemented (with tests).
- Still missing: >IN >NUMBER ABORT ABORT" ACCEPT ENVIRONMENT? EVALUATE FIND POSTPONE QUIT S>D SOURCE etc. (recent batches implemented many).
- ABORT/ABORT", QUIT, RECURSE, full FIND/EVALUATE etc. for complete control and meta-programming.
- The added words (double stack, extra arith, memory ops, LITERAL etc. + pictured/S" + meta/input batch) have tests and pass in the harness.

## Missing from Core Extensions Word Set (6.2)
(~29 items.)

.R
0<>
:NONAME
<>
ACTION-OF
BUFFER:
C"
CASE
COMPILE,
DEFER
DEFER!
DEFER@
ENDCASE
ENDOF
ERASE
HOLDS
IS
MARKER
OF
PAD
PARSE
PARSE-NAME
RESTORE-INPUT
SAVE-INPUT
SOURCE-ID
U>
UNUSED
VALUE
[COMPILE]

Notes:
- The engine has good coverage of practical extensions (PICK/ROLL/TUCK/NIP/U.R/WITHIN/?DO etc.).
- Missing many for full "programming tools" like DEFER, CASE, VALUE, PARSE family, etc.
- Some like AGAIN, 2>R etc. *are* present.

## Recommendations / Status
- The system is **highly functional** for the user's needs (loading classic sources, REPL, FLOAD/EDIT/CHDIR in sandbox, FILE-ECHO, \S, ." , WORD, etc.).
- Recent work (FLOAD reliability, compact .", DP/HERE, header tools, expanded ANS tests in FTEST) has made it much closer to usable classic Forth.
- Full ANS conformance would require implementing the above missing items + tests + documentation.
- Current tests (TestTZForth.swift FTEST, originally TestLBForth.swift) cover a lot of the *implemented* core words per standard + special behaviors.
- To continue under credit limits: prioritize user-requested words from Forthing.fth or specific missing ones that block porting.

Generated from codebase inspection (TZForth.swift / originally LBForth.swift, TestTZForth.swift / originally TestLBForth.swift, live runs).
Last update: after ANS test expansion commit.

For full standard details, refer to the official 2012 ANS Forth document (sections 6.1 and 6.2).
