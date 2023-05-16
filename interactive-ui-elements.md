---
layout: default
title: Interactive UI Elements
nav_order: 8
---

# Interative UI Elements

Modern websites requires the use of UI elements that not only allow the user to perform a wide range of functions, but also react to the user's actions in an intuitive manner. MUI offers a wide range of customizable built-in components that adhere to the Material Design Language. In this section, we will learn how to style certain MUI components to conform to the stylistic guidelines set by McMaster Digital Brand Standards Manual. The aim of this section is not to provide a comprehensive tutorial on how to use these different MUI components (this information is covered in the [Material UI Documentation](https://mui.com/material-ui/getting-started/overview/) ), but to modify the components' behavior and appearance to fit the McMaster Digital Brand Standards. 

### Buttons
We have already create a stylized `MuiButton` component called `MacButton` in the "Breadcrumbs With a Universal Back Button" section. We will now show a couple more examples of how we can use the stylized `MacButton` component in different ways.

Open the `pages/index.tsx` file and add the following import statements:
```
import React from "react";
import Stack from "@mui/material/Stack";  
import Snackbar from '@mui/material/Snackbar';  
import IconButton from '@mui/material/IconButton';  
import CloseIcon from '@mui/icons-material/Close';
```
Declare and export these two interfaces above the `Home` function:
```
export interface SnackbarMessage {
    message: string;
    key: number;
}

export interface State {
    open: boolean;
    snackPack: readonly SnackbarMessage[];
    messageInfo?: SnackbarMessage;
}

```
Add the following lines of code at the top of the `Home` function:
```
const [snackPack, setSnackPack] = React.useState<readonly SnackbarMessage[]>([]);
const [open, setOpen] = React.useState(false);
const [messageInfo, setMessageInfo] = React.useState<SnackbarMessage | undefined>(
    undefined,
);

React.useEffect(() => {
    if (snackPack.length && !messageInfo) {
        // Set a new snack when we don't have an active one
        setMessageInfo({ ...snackPack[0] });
        setSnackPack((prev) => prev.slice(1));
        setOpen(true);
    } else if (snackPack.length && messageInfo && open) {
        // Close an active snack when a new one is added
        setOpen(false);
    }
}, [snackPack, messageInfo, open]);

const handleClick = (message: string) => () => {
    setSnackPack((prev) => [...prev, { message, key: new Date().getTime() }]);
};

const handleClose = (event: React.SyntheticEvent | Event, reason?: string) => {
    if (reason === 'clickaway') {
        return;
    }
    setOpen(false);
};

const handleExited = () => {
    setMessageInfo(undefined);
};

const action = (
    <React.Fragment>
        <IconButton
            size="small"
            aria-label="close"
            color="inherit"
            onClick={handleClose}
        >
            <CloseIcon fontSize="small" />
        </IconButton>
    </React.Fragment>
);
```
This code snippet is used to handle opening and closing the popup message that will be shown when clicking the `MacButton` components that we will add shortly.

Delete the line containing the `Typography` component and replace it with the following lines of code:
```
<Snackbar
    sx={ {paddingTop: 10} }
    open={open}
    autoHideDuration={6000}
    onClose={handleClose}
    TransitionProps={ { onExited: handleExited } }
    message={messageInfo ? messageInfo.message : undefined}
    action={action}
    anchorOrigin={ {vertical: 'top', horizontal: 'right'} }
/>
<Stack
    direction="column"
    justifyContent="space-between"
    alignItems="center"
    spacing={5}
>
    <Typography variant="h1">Hello World!</Typography>
    <Stack
        direction="row"
        justifyContent="space-between"
        alignItems="center"
        spacing={2}
    >
        <MacButton variant="contained" mainColor="primary" onClick={handleClick('Primary Button Clicked!')}>Primary</MacButton>
        <MacButton variant="contained" mainColor="secondary" onClick={handleClick('Secondary Button Clicked!')}>Secondary</MacButton>
    </Stack>
</Stack>
```
We used the MUI `Stack` component to stack the "Hello World!" message vertically on top of another `Stack` containing two buttons in a row. The `Snackbar` component is used to display the popup message shown after clicking a button. The MUI guidelines recommend displaying only one `Snackbar` message at a time. Hence, if your application has multiple `Snackbar` messages, you should dismiss the current message before displaying the next one. You can find out more about the MUI `Snackbar` component by visiting the [Snackbar official documentation](https://mui.com/material-ui/react-snackbar/).

Notice that the `mainColor` on the first button is `"primary"`, whereas the second button uses the `"secondary"` color.

Go back to your browser, your homepage should now look like this:
![buttons](assets/img/buttons.png)

Hovering over the Heritage Maroon button will turn cause it to turn dark grey:
![hover-maroon](assets/img/hover-maroon.png)
Simiarly, hovering ove the "Secondary" button will turn cause it to turn light grey:
![hover-gold](assets/img/hover-gold.png)


