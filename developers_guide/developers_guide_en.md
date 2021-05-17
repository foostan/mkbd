# Developer's Guide (EN)
## Introduction
This is a guide for those developing the Meishi Keyboard.
I will explain the process of creating a "business card type" keyboard such as `mkbd / pcb` in a tutorial format.

## PCB design
### Start a new project

Start KiCad and create a new project from "File> New> Project".

![new_project](./images/new_project/new_project.png)

Decide on a project name and click [Save]. 

![save_new_project](./images/new_project/save_new_project.png)

The KiCad project main menu screen is displayed.
Various tools are started from this screen, so if you have time, let's check what kind of tools are available.
I don't have time right now, so I'll move on.

![global_menu](./images/new_project/global_menu.png)

### Circuit design

First, we will design the keyboard circuit.
From the global menu, open the Schematic Layout Editor.

![open_eeschema](./images/eeschema/open_eeschema.png)


This is the work screen of the Schematic Layout Editor (hereafter Eeschema).

![start_eeschema](./images/eeschema/start_eeschema.png)

Right now, there are only the minimum explanations required for design, so if you want to learn systematically, please open "Help> KiCad Getting Started".
It is recommended because you can learn how to use KiCad in detail.

![kotohajime](./images/eeschema/kotohajime.png)

Before you start designing, first get the parts (library) you need for your keyboard.
Get the required library from GitHub as follows.

```
$ git clone https://github.com/foostan/kbd
```

Alternatively, go to https://github.com/foostan/kbd and download.

Next, load the obtained library from "Settings> Manage Symbol Library ...".

![manage_lib](./images/eeschema/manage_lib.png)

From [Browse Library ... `kbd / library / kbd.lib`], click [Open].

![open_lib](./images/eeschema/open_lib.png)

Now you are ready to design your circuit.
On the Eeschema work screen, press the shortcut `a` to open the symbol selection window.
Enter "promicro_r" in the search form, select "ProMicro_r" and click OK.

![add_promicro_r](./images/eeschema/add_promicro_r.png)

Place it appropriately.

![set_promicro_r](./images/eeschema/set_promicro_r.png)

If you have a real "Pro Micro" at hand, let's compare it.
You can see that the parts are facing the unmounted side.
In circuit design, it is not necessary to match the shape and orientation with the real thing, but for beginners, it is easier to understand if you can imagine the real thing.
If you use an external library, you may want to choose one that is easy to see.

![promicro](./images/eeschema/promicro.png)

Next, we will place the key switches.
This time I will design a keyboard with 6 keys.

On the Eeschema work screen, press the shortcut `a` to open the symbol selection window.

Type sw_push in the search form, select SW_PUSH and click OK.

![add_sw_push](./images/eeschema/add_sw_push.png)

Arrange them for easy understanding.
To move a symbol, place the cursor on the symbol and press the shortcut `m` to move it to the location you want to move it. 

![set_sw_push](./images/eeschema/set_sw_push.png)

Next, connect the key switch to the Pro Micro.
Press the shortcut `w` and connect while referring to the image below.
If you want to wire more neatly, there is also a method using "label", so please try to find a method with "KiCad Beginning" etc.

![wiring_sw_push](./images/eeschema/wiring_sw_push.png)

Then drop the other leg of the keyswitch to GND.
On the Eeschema work screen, press the shortcut `a` to open the symbol selection window.
Enter "gnd" in the search form, select "GND" and click OK.

![add_gnd](./images/eeschema/add_gnd.png)

Add GND referring to the image below.
If you are interested in the appearance, try adjusting with the shortcut `g` or the shortcut `r` (please actually check the behavior of each shortcut).

![set_gnd](./images/eeschema/set_gnd.png)

Next, I will attach a reset switch.
On the Eeschema work screen, press the shortcut `a` to open the symbol selection window.
Type "sw_push" in the search form, select SW_Push and click OK.
It doesn't matter if it's the same as the key switch, but I changed it because it's easier to understand if it looks different.

![add_sw_push_reset](./images/eeschema/add_sw_push_reset.png)

Wire and GND in the same way as a keyswitch.

![set_sw_push_reset](./images/eeschema/set_sw_push_reset.png)

Attach VCC and GND to ProMicro_r.
On the Eeschema work screen, press the shortcut `a` to open the symbol selection window.
Type "vcc" in the search form, select VCC and click OK.

![set_vcc_and_gnd](./images/eeschema/set_vcc_and_gnd.png)

Now that we've wired all the necessary parts, we'll flag the unused ports of ProMicro_r as "unconnected".


![set_quit_flag](./images/eeschema/set_quit_flag.png)


Finally, connect PWR_FLAG to VCC and GND.
On the Eeschema work screen, press the shortcut `a` to open the symbol selection window.
Type "pwr_flag" in the search form, select PWR_FLAG and click OK.

![set_pwr_flag](./images/eeschema/set_pwr_flag.png)

Next, number the installed symbols.
Select "Annotate schematic symbol" from the above menu

![annotation](./images/eeschema/annotation.png)

Leave the settings as they are and click [Annotate].

![set_annotation](./images/eeschema/set_annotation.png)

Next, check the designed circuit for defects.
Select "Run Electrical Rule Check" from the menu above.

![erc](./images/eeschema/erc.png)

I will do it. It is OK if no warning is displayed in "Message" and only "End" is displayed.

![run_erc](./images/eeschema/run_erc.png)

Next, link the symbol with the actual board parts (footprint).
Select "Associate PCB footprint with schematic symbol" from the menu above

![run_erc](./images/eeschema/relate_to_footprint.png)

Select Edit Footprint Library Table and select

![run_erc](./images/eeschema/footprint_lib.png)

From [Browse Library ...], select `kbd / kbd.pretty` and click [OK].

![run_erc](./images/eeschema/select_kbd_pretty.png)


You can now use the keyboard footprint.

![start_relation](./images/eeschema/start_relation.png)

Allocate from the read kbd as follows.
Note that SW1 is the reset switch in this example.
It may be easier to understand if you change the annotation to "RSW" etc. so that it is easy to understand in advance.
(You can change the annotation by hovering over the symbol and using the shortcut `e`).

![finish_relate](./images/eeschema/finish_relate.png)

If you want to check the footprint, select the footprint from the right frame and click "View selected footprint".

![show_footprint](./images/eeschema/show_footprint.png)
![show_selected_footprint](./images/eeschema/show_selected_footprint.png)


Finally, select "Generate Netlist" from the above menu and select
![output_netlist](./images/eeschema/output_netlist.png)

Output the circuit information called netlist from [Generate netlist] as a file.
![output_netlist_to_file](./images/eeschema/output_netlist_to_file.png)

The circuit design is now complete.


### Creating a PCB

Go back to the global menu and select Board Layout Editor.
![open_pcbnew](./images/pcbnew/open_pcbnew.png)

This is the work screen of the Board Layout Editor (Pcbnew).
![pcbnew](./images/pcbnew/pcbnew.png)

First, read the circuit data created earlier here.
Select "Load Netlist" from the above menu
![open_netlist](./images/pcbnew/open_netlist.png)

Select Load current netlist.
![load_netlist](./images/pcbnew/load_netlist.png)

It is OK if each footprint is loaded as shown below.
![loaded_netlist](./images/pcbnew/loaded_netlist.png)

First of all, I will make a frame for a business card.
By the way, the size of the business card is 91mm x 55mm.

Change the layer you work on from the above menu to "Edge.Cuts" and
![select_edge_cut](./images/pcbnew/select_edge_cut.png)

Select "Add Shape Line" from the right menu.
![add_line](./images/pcbnew/add_line.png)

First, make a rough rectangle.
![draw_lines](./images/pcbnew/draw_lines.png)


Then hover over the line and use the shortcut `e` to display the Wiring Segment Properties.
The position of the line is determined by the start point X, start point Y, end point X, and end point Y. 
![edit_xy](./images/pcbnew/edit_xy.png)

I think there are 4 main lines, so each one

1st
- Starting point X: 0
- Starting point Y: 0
- End point X: 91
- End point Y: 0

2nd
- Starting point X: 91
- Starting point Y: 0
- End point X: 91
- End point Y: 55

3rd
- Starting point X: 91
- Starting point Y: 55
- End point X: 0
- End point Y: 55

4th
- Starting point X: 0
- Starting point Y: 55
- End point X: 0
- End point Y: 0

is specified.

![edge_cut](./images/pcbnew/edge_cut.png)

This has sharp corners and hurts, so round it.

First, set the grid to make it easier to work.
I think 0.1mm is fine.
![select_0.1mm](./images/pcbnew/select_0.1mm.png)

Select Add Arc Next to round the corners.

![add_enko](./images/pcbnew/add_enko.png)

![set_enko](./images/pcbnew/set_enko.png)

Next, we will arrange the footprints of the key switches.
The key switch pitch is generally 19.05mm for Cherry MX series.

To start with custom grid settings, select View> Grid Settings and set size X and size Y to 19.05.

![grid_setting](./images/pcbnew/grid_setting.png)

After that, if you set the grid setting in the above menu to "Custom User Grid", the grid in 19.05mm units will be displayed.
After that, arrange the key switches along this grid.
You can move the footprint by hovering over the footprint and using the shortcut key `m`.

![set_key_sw](./images/pcbnew/set_key_sw.png)


Next, return the grid setting to 0.1 mm etc. and place the Pro Micro and reset switch as shown below.
Basically, it doesn't matter what you like, but for Pro Micro, arrange it so that the USB side overlaps the outer shape.
(If you install it further inside, the USB cable will not interfere).

![set_promicro_and_reset](./images/pcbnew/set_promicro_and_reset.png)

Next, we will wire. Rats nests (elongated white lines connecting holes and pads) indicate where to wire.
In order to wire cleanly, I think that it will work if you make a plan on how to wire by referring to this Rats Nest.

First, switch to the "F.Cu" layer from the above menu for wiring. 
![select_fcu](./images/pcbnew/select_fcu.png)

Select "Wiring" from the right menu.

![line](./images/pcbnew/line.png)


Note that GND is processed separately, so other wiring is required.
Place the cursor on the wire and use the shortcut key `d` or
Move the cursor to the footprint and perform operations such as the shortcut key `r`.
Try to keep the wiring as simple as possible.

Here is an example of wiring.

![finished_line](./images/pcbnew/finished_line.png)

Next, we will process GND. Select "Add Fill Zone" from the right menu and place it so that it covers the outline.

![beta](./images/pcbnew/beta.png)

The settings are as follows. Set the layer to "F.Cu", set the net to "GND", and leave the defaults.

![set_zone](./images/pcbnew/set_zone.png)

If the zone can be set correctly, it will be as follows.
All unconnected GNDs should be connected.

![finished_zone](./images/pcbnew/finished_zone.png)

In this case, the zone is only the F.Cu layer and there is no problem in operation,
Since there will be a difference in the finish on the front and back, place a zone on the B.Cu layer as well.

![finished_line_and_zone](./images/pcbnew/finished_line_and_zone.png)

Next, enter the name in silk so that it functions as a "business card".
Create a business card image using your favorite drawing software or painting software.

This time I will use PhotoShop.
Create a new one with a width of 91mm x height of 55mm and 300 ppi. 

![new_meishi](./images/pcbnew/new_meishi.png)

Here is what I created this time.
Create with two colors with a black background.

![mkbd](./images/pcbnew/mkbd.png)

Go back to the global menu and select "Import Bitmap".

![import_bitmap](./images/pcbnew/import_bitmap.png)

Now load the image you just created.
I think it's okay to leave the default settings, but let's check the size and resolution for the time being.

![import_meishi](./images/pcbnew/import_meishi.png)


Save as silk data that can be read by Pcbnew with [Export].
Please be careful to create a new folder called "* .pretty" and save it in it.

In this case, it is saved as "mkbd.pretty/meishi.kicad_mod".

![export](./images/pcbnew/export.png)

Return to the Pcbnew screen and select Settings> Manage Footprint Library.
Select "mkbd.pretty" created earlier from [Browse Library].

![load_lib](./images/pcbnew/load_lib.png)

The created image can now be used as silk.
Try to place it from "Placement> Footprint".
You should be able to select as below.

![set_meishi_footprint](./images/pcbnew/set_meishi_footprint.png)

The rest is completed by arranging it as you like.
This time, I set it on the back side where the parts are not mounted.
If you want to flip the footprint, place the cursor on the footprint and use the shortcut `e` to open the edit window.
Change "Board side :: Front side" to "Back side".

![finished](./images/pcbnew/finished.png)

That's it.
Finally, check for mistakes.

From "Run Design Rule Check" in the above menu

![drc](./images/pcbnew/drc.png)

Press Start DRC to check. If there are no problems or unwiring, it is OK.

![run_drc](./images/pcbnew/run_drc.png)

Check the completed PCB from "View> 3D Viewer".

![3d](./images/pcbnew/3d.png)

![3d_back](./images/pcbnew/3d_back.png)

### PCB ordering

I will explain the process of ordering the completed PCB data from a vendor.

Generate a manufacturing file from "File> Plot".

In "Included Layers"

- F.Cu
- B.Cu
- F.SilkS
- B.SilkS
- F.Mask
- B. Mask
- Edge.Cuts

Choose.
The rest is the default.
In the output directory, do [Production File Output].

![plot](./images/order/plot.png)

We also need drill data, so from [Generate Drill File ...]

![drill](./images/order/drill.png)

Click Generate Drill File.
Also, compress the generated manufacturing file (Gerber data) with zip or the like.


We will use Seeed's FusionPCB for this order.
https://www.fusionpcb.jp/prototype-pcb-sale.html

There are optional restrictions here, but there is a plan that allows you to order PCBs of 100mm x 100mm or less for $ 7.9.
Even if you choose OCS, it's as cheap as $ 12.90 (until a while ago, it was even cheaper and less than $ 10).

I asked Elecrow for the same configuration and it cost $ 18.13.

![fusionpcb](./images/order/fusionpcb.png)

Then [Add to Cart], place an order and you're done.

