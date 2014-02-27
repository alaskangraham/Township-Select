Township-Select
===============

arcpy code to select and zoom to township
# ---------------------------------------------------------------------------
# Township Selector
# Created on: 2014-02-24 15:28:23.00000
# Create by Jason Graham
# Usage: Select2 <NS> <EW> <Rng> <Twp> <M> <MTR>
# Description:Add-in tool to easily select a township then zoom to the selection 
# ---------------------------------------------------------------------------

# Import arcpy module

import arcpy
from arcpy import env

#set workspace
mxd = arcpy.mapping.MapDocument("CURRENT")

#location of Township layer
env.workspace = r"O:\DSF\R5\cartography\graham\LandStatus\Landstatus10.gdb\Grid"


# Collect user input for Meridian, township and range
M = arcpy.GetParameterAsText(0)
if M == '#' or not M:
    M = "S" # provide a default value if unspecified
    
Twp = arcpy.GetParameterAsText(1)
if Twp == '#' or not Twp:
    Twp = "010" # provide a default value if unspecified
    
NS = arcpy.GetParameterAsText(2)
if NS == '#' or not NS:
    NS = "N" # provide a default value if unspecified

Rng = arcpy.GetParameterAsText(3)
if Rng == '#' or not Rng:
    Rng = "010" # provide a default value if unspecified

EW = arcpy.GetParameterAsText(4)
if EW == '#' or not EW:
    EW = "E" # provide a default value if unspecified

# Concatenate user input
MTRsel = M + Twp + NS + Rng + EW

# Select user input value
arcpy.SelectLayerByAttribute_management("Townships","NEW_SELECTION",'"MTR" =' + "'%s'" %MTRsel)

#Zoom to selection
arcpy.mapping.ListDataFrames(mxd)[0].zoomToSelectedFeatures()
