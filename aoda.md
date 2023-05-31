---
layout: default
title: AODA Compliance
nav_order: 10
---

# AODA Compliance 

The Information and Communications Standard is part of the AODA Integrated Standards Regulation released in June 2011 by the Ontario legislative body. It requires McMaster University to provide information in an accessible format, which includes web sites and information published on them. In this section, we will learn how to evaluate a website for AODA compliance and how to address some common AODA errors related to MUI components.

### Installing the Wave Browser Extension
Open the [WAVE Extensions ](https://wave.webaim.org/extension/) webpage and install the Wave extension for the browser that you are currently using. 

### Using the Wave Browser Extension
To evaluate a page, simply click on the extension button in your browser's toolbar.
![wave-button](assets/img/wave-button.png)
The Wave sidebar will be shown on the left side of your browser. The sidebar includes a summary of all the errors, alerts and features that are present on the page you have open.

In our case, the Wave tool identifies 1 regular error and 1 contrast error as shown in the image below. 
![wave-sidebar](assets/img/wave-sidebar.png)

Open the "Details" tab in the Wave sidebar to find more details regarding the errors and alerts. We can now see that the error is caused by an empty button, i.e., a button without any text or tooltip message. Screen readers cannot properly process empty buttons, so we will need to address this issue to ensure that our website meets the accessibility standards.

Pull the website's HTML code by clicking on the turquoise "Code" button at the bottom of the screen. With the code view pulled up, click on the red button under "1 X Empty button" and you will redirected to the HTML code section that corresponds to the empty button. 

![empty-button](assets/img/empty-button.png)

### Empty Button Error
After inspecting the code, we can find that the empty button in the "Menu" button that appears on the navigation bar on smaller screens. To fix the issue, we will add a tooltip message to the "Menu" button.

Open the `Navbar.tsx` file located under `Components/Navbar`, and wrap the `IconButton` component containing a `MenuIcon` with a `Tooltip` component as shown below:
```
<Tooltip title={"Menu"}>
	<MacIconNavButton
		size="large"
		aria-controls="menu-appbar"
		aria-haspopup="true"
		onClick={handleOpenNavMenu}
		color="inherit"
>
		<MenuIcon />
	</MacIconNavButton>
</Tooltip>
```

Save the file and go back to your browser. Reload the webpage and re-evaluate it using the Wave tool. The "Empty Button" error will no longer be present. 

![empty-button-fixed](assets/img/empty-button-fixed.png)

Navigate to "Page 1" and re-evaluate the page using the Wave tool. The summary page shows another empty button error. Examining the code shows that the error is caused by the back button.

![page-1-wave](assets/img/page-1-wave.png)

While we can use a tooltip message to resolve this issue, we will show another way of addressing the problem.

Open the `components/BreadCrumbs/BreadCrumbs.tsx` file and locate the `MacButton` component containing an `ArrowBackIcon`. Add a `title` prop to the `MacButton` component:
```
<MacButton variant="contained" mainColor="primary" onClick={() => router.back()} title={"Back"}>
    <ArrowBackIcon />
</MacButton>
```

Go back to your browser and try hovering over the back button. You will see a the title message appear.
Re-run the Wave tool on "Page 1". The empty button error will no longer be shown.

![page-1-wave-fixed](assets/img/page-1-wave-fixed.png)

### Contrast Error
TODO

### Missing Form Label Error

Navigate to the "Settings" page by clicking on the gear icon and evaluate the page using the Wave tool. The "Details" tab of the Wave tool shows a missing form label error.

![settings-wave](assets/img/settings-wave.png)

This error is caused by the dropdown menu component (i.e., the `Select` component) not having an `id` that matches that of its corresponding `InputLabel`. To fix this issue, simply open the `components/TabPanel/VerticalTabs.tsx` file and locate the `InputLabel` and `Select` components.

Add an `htmlFor` prop to the `InputLabel` component as shown below:
```
<InputLabel id="demo-simple-select-label" htmlFor="demo-simple-select">
	Demo Dropdown Menu
</InputLabel>
```

Next, we will add an `id` to the `Select` component using `inputProps`. The `id` of the `Select` component has to match the one enter in the `htmlFor` field of the `InputLabel`:
```
<Select
	labelId="demo-simple-select-label"
	id="demo-simple-select"
	label="Demo Dropdown Menu"
	inputProps={{
		id:'demo-simple-select',
	}}
>
	<MenuItem value={1}>Option 1</MenuItem>
	<MenuItem value={2}>Option 2</MenuItem>
	<MenuItem value={3}>Option 3</MenuItem>
</Select>
```

Reload the "Settings" page in your browser and re-run the Wave tool. The missing form label error should now be fixed.

