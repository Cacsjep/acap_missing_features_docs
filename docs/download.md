#### Releases

You can download the ACAP files from: [ACAP Missing Features Github](https://github.com/Cacsjep/acap_missing_features_docs/releases)

!!! bug "Info"
    If you are unsure whether your camera model uses the Artpec (7, 8) or CV25 chip, please use the [AXIS Product Selector](https://www.axis.com/support/tools/product-selector) or refer to **Obtain Chip and Architecture Section** below.
    We are happy to help if you encounter any issues.

#### Obtain Chip and Architecture
- Open a browser.
- Use the following URL to list all camera properties:  
  `http://<your-ipaddress>/axis-cgi/param.cgi?action=list`
    - Example: `http://10.0.0.47/axis-cgi/param.cgi?action=list`
- Search for the `root.Properties.System.Architecture` and `root.Properties.System.Soc` properties as shown in the image below:

[![](images/soc.PNG)](images/soc.PNG)
