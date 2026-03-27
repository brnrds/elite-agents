---
title: "The MESS subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mess.html"
---

[me1](me1.md "Previous routine")[mes9](mes9.md "Next routine")
    
    
           Name: MESS                                                    [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Display an in-flight message
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-mess)
     Variations: See [code variations](../../related/compare/main/subroutine/mess.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [EXNO2](exno2.md) calls MESS
                 * [FR1](fr1.md) calls MESS
                 * [Ghy](ghy.md) calls MESS
                 * [KILLSHP](killshp.md) calls MESS
                 * [Main flight loop (Part 8 of 16)](main_flight_loop_part_8_of_16.md) calls MESS
                 * [Main flight loop (Part 12 of 16)](main_flight_loop_part_12_of_16.md) calls MESS
                 * [Main flight loop (Part 15 of 16)](main_flight_loop_part_15_of_16.md) calls MESS
                 * [me2](me2.md) calls MESS
                 * [ou2](ou2.md) calls MESS
                 * [ou3](ou3.md) calls MESS
                 * [OUCH](ouch.md) calls MESS
                 * [SFRMIS](sfrmis.md) calls MESS
    
    
    
    
    
    * * *
    
    
     Display an in-flight message in capitals at the bottom of the space view,
     erasing any existing in-flight message first.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The text token to be printed
    
    
    
    
    .MESS
    
     PHA                    \ Store A on the stack so we can restore it after the
                            \ following
    
     LDX QQ11               \ If this is the space view, skip the following
     BEQ infrontvw          \ instruction
    
     JSR CLYNS              \ Clear the bottom three text rows of the upper screen,
                            \ and move the text cursor to the first cleared row
    
    .infrontvw
    
     LDA #21                \ Move the text cursor to row 21
     STA YC
    
     LDA #YELLOW            \ Switch to colour 1, which is yellow
     STA COL
    
     LDX #0                 \ Set QQ17 = 0 to switch to ALL CAPS
     STX QQ17
    
     LDA messXC             \ Move the text cursor to column messXC, in case we
     STA XC                 \ jump to me1 below to erase the current in-flight
                            \ message (whose column we stored in messXC when we
                            \ called MESS to put it there in the first place)
    
     PLA                    \ Restore A from the stack
    
     LDY #20                \ Set Y = 20 for setting the message delay below
    
     CPX DLY                \ If the message delay in DLY is not zero, jump up to
     BNE me1                \ me1 to erase the current message first (whose token
                            \ number will be in MCH)
    
     STY DLY                \ Set the message delay in DLY to 20
    
     STA MCH                \ Set MCH to the token we are about to display
    
                            \ Before we fall through into mes9 to print the token,
                            \ we need to work out the starting column for the
                            \ message we want to print, so it's centred on-screen,
                            \ so the following doesn't print anything, it just uses
                            \ the justified text mechanism to work out the number of
                            \ characters in the message we are going to print
    
     LDA #%11000000         \ Set the DTW4 flag to %11000000 (justify text, buffer
     STA DTW4               \ entire token including carriage returns)
    
     LDA de                 \ Set the C flag to bit 1 of the destruction flag in de
     LSR A
    
     LDA #0                 \ Set A = 0
    
     BCC P%+4               \ If the destruction flag in de is not set, skip the
                            \ following instruction
    
     LDA #10                \ Set A = 10
    
     STA DTW5               \ Store A in DTW5, so DTW5 (which holds the size of the
                            \ justified text buffer at BUF) is set to 0 if the
                            \ destruction flag is not set, or 10 if it is (10 being
                            \ the number of characters in the " DESTROYED" token)
    
     LDA MCH                \ Call TT27 to print the token in MCH into the buffer
     JSR TT27               \ (this doesn't print it on-screen, it just puts it into
                            \ the buffer and moves the DTW5 pointer along, so DTW5
                            \ now contains the size of the message we want to print,
                            \ including the " DESTROYED" part if that's going to be
                            \ included)
    
     LDA #32                \ Set A = (32 - DTW5) / 2
     SEC                    \
     SBC DTW5               \ so A now contains the column number we need to print
     LSR A                  \ our message at for it to be centred on-screen (as
                            \ there are 32 columns)
    
     STA messXC             \ Store A in messXC, so when we erase the message via
                            \ the branch to me1 above, messXC will tell us where to
                            \ print it
    
     STA XC                 \ Move the text cursor to column messXC
    
     JSR MT15               \ Call MT15 to switch to left-aligned text when printing
                            \ extended tokens disabling the justify text setting we
                            \ set above
    
     LDA MCH                \ Set MCH to the token we are about to display
    
                            \ Fall through into mes9 to print the token in A
    

[X]

Subroutine [CLYNS](clyns.md) (category: Drawing the screen)

Clear the bottom three text rows of the space view

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Variable [DLY](../workspace/wp.md#dly) in workspace [WP](../workspace/wp.md)

In-flight message delay

[X]

Variable [DTW4](../variable/dtw4.md) (category: Text)

Flags that govern how justified extended text tokens are printed

[X]

Variable [DTW5](../variable/dtw5.md) (category: Text)

The size of the justified text buffer at BUF

[X]

Variable [MCH](../workspace/wp.md#mch) in workspace [WP](../workspace/wp.md)

The text token number of the in-flight message that is currently being shown, and which will be removed by the me2 routine when the counter in DLY reaches zero

[X]

Subroutine [MT15](mt15.md) (category: Text)

Switch to left-aligned text when printing extended tokens

[X]

Variable [QQ11](../workspace/zp.md#qq11) in workspace [ZP](../workspace/zp.md)

The type of the current view

[X]

Variable [QQ17](../workspace/zp.md#qq17) in workspace [ZP](../workspace/zp.md)

Contains a number of flags that affect how text tokens are printed, particularly capitalisation

[X]

Subroutine [TT27](tt27.md) (category: Text)

Print a text token

[X]

Variable [XC](../workspace/zp.md#xc) in workspace [ZP](../workspace/zp.md)

The x-coordinate of the text cursor (i.e. the text column), which can be from 0 to 32

[X]

Variable [YC](../workspace/zp.md#yc) in workspace [ZP](../workspace/zp.md)

The y-coordinate of the text cursor (i.e. the text row), which can be from 0 to 23

[X]

Configuration variable [YELLOW](../../all/workspaces.md#yellow) = %00001111

Four mode 1 pixels of colour 1 (yellow)

[X]

Variable [de](../workspace/wp.md#de) in workspace [WP](../workspace/wp.md)

Equipment destruction flag

[X]

Label [infrontvw](mess.md#infrontvw) is local to this routine

[X]

Subroutine [me1](me1.md) (category: Flight)

Erase an old in-flight message and display a new one

[X]

Variable [messXC](../workspace/zp.md#messxc) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store the text column of the in-flight message in MESS, so it can be erased from the screen at the correct time

[me1](me1.md "Previous routine")[mes9](mes9.md "Next routine")
