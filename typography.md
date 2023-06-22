---
layout: default
title: Typography
nav_order: 4
---

# Typography

McMaster recommends the use of the Roboto family of fonts on all websites associated with the university. Roboto is a neo-grotesque sans-serif typeface family developed by Google. There are multiple variants of the Roboto font that are used for different headlines, subheads and body text when designing a new webpage. In this section, we will learn how to use the MUI Typography component to define different heading styles.

### Create `theme.ts`
In the root directory of your project, create a new directory named `config`. You can create the folder using the command line (`mkdir config`) or the GUI.
Navigate to the newly created `config` directory, and create a new file called `theme.ts` in this directory.

### Define the Typography Styles
Open the `_document.tsx` file located in the `pages` directory and modify line 6 as follows by deleting `<Head />` and replacing it with:
```
<Head >  
	<link  
	rel="stylesheet"  
	href="https://fonts.googleapis.com/css?family=Roboto:300,300i,700,700i|Roboto+Condensed:400,400i,700,700i|&display=swap"  
	/>  
</Head>
```
This line of code download the needed Roboto font variants from the Google Fonts.

The `_document.tsx` file should now contain the following code:
```
import { Html, Head, Main, NextScript } from 'next/document'  
  
export default function Document() {  
	return (  
		<Html lang="en">  
			<Head >  
				<link  
					rel="stylesheet"  
					href="https://fonts.googleapis.com/css?family=Roboto:300,300i,700,700i|Roboto+Condensed:400,400i,700,700i|&display=swap"  
				/>  
			</Head>  
			<body>  
				<Main />  
				<NextScript />  
			</body>  
		</Html>  
	)  
}
```

Add the following code snippet to  `theme.ts`:
```
declare module '@mui/material/Typography' {
    interface TypographyPropsVariantOverrides {
        settingTitle: true;
    }
}

const themeOptions = {
    typography: {
        h1: {
            fontFamily: 'Roboto Condensed',
            fontSize: '50pt',
        },
        h2: {
            fontFamily: 'Roboto Condensed',
            fontSize: '28pt',
            fontWeight: 400,
        },
        h3: {
            fontFamily: 'Roboto Condensed',
            fontSize: '20pt',
        },
        h4: {
            fontFamily: 'Roboto',
            fontSize: '13pt',
            fontWeight: 900,
        },
        button: {
            fontFamily: 'Roboto Condensed',
            fontWeight: 700,
        },
        settingTitle: {  
			fontFamily: 'Roboto Condensed',  
			fontSize: '15pt',  
		},
    },
}

export default themeOptions
```

In this code snippet, we are defining the different typography variant that can be use in our application. The heading styles conform to the McMaster Digital Brand Standards. The `button` and `settingTitle` typographies define the font style to use for text located in buttons and setting titles respectively. We will cover styling buttons and setting pages in later sections of this workshop.

### Creating a Theme Provider
Open the `_app.tsx_` file located in the `pages` directory and add the following import statements:
```
import CssBaseline from '@mui/material/CssBaseline'  
import {createTheme, ThemeProvider} from '@mui/material/styles'  
import themeOptions from '@/config/theme'
```

Create the `theme` constant in the `App` function (before the `return` statement):
```
const theme = createTheme({  
...themeOptions  
});
```
Notice that the theme uses the `themeOptions` defined in and imported from `theme.ts`.

Update the return statement as shown below:
```
return <>  
	<ThemeProvider theme={theme}>  
		<CssBaseline />  
		<Component {...pageProps} />  
	</ThemeProvider>  
</>
```
Your `_app.tsx` file should now look like this:
```
import type { AppProps } from 'next/app'  
import CssBaseline from '@mui/material/CssBaseline'  
import {createTheme, ThemeProvider} from '@mui/material/styles'  
import themeOptions from '@/config/theme'  
  
export default function App({ Component, pageProps }: AppProps) {  
	const theme = createTheme({  
		...themeOptions  
	});  
	return <>  
		<ThemeProvider theme={theme}>  
			<CssBaseline />  
			<Component {...pageProps} />  
		</ThemeProvider>  
	</>  
}
```

### Using the Typography Component
Open the `index.tsx` file and add the following import statement to import the MUI Typography component:
```
import Typography from '@mui/material/Typography'
```
Delete line 15 and replace it with the following line of code:
```
<Typography variant="h1">Hello World!</Typography>
```

Go back the browser. The "Hello World" text should now use the `h1` style defined in `theme.ts`:
![styled-hello-world](assets/img/styled-hello-world.png)
You can use any the typography styles defined in `theme.ts` by specifying the variant in the `Typography` component. You can also define additional styles and use them in your website.
