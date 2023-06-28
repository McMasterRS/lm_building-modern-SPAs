---
layout: home
title: Introduction
nav_order: 1
---

The [McMaster Digital Brand Standards](https://brand.mcmaster.ca/app/uploads/2019/04/digital-guidelines.pdf) PDF document provides a good overview of the brand guidelines designed to ensure recognition of the McMaster University brand and consistency of its message. The design guidelines presented in this learning module were pulled from this PDF document and modified to apply to Material UI (MUI) components.

This learning module uses the Next.js framework in conjunction with the Material UI (MUI) library. If you are using a different tech stack or framework that the ones in this learning module, please feel free to book a consultation with the Research Software Development team by emailing [rsd@mcmaster.ca](mailto:rsd@mcmaster.ca).

# Introduction to Material UI (MUI)

![NextJS-MUI](assets/img/nextjs-mui.png)_Image retrieved from [itnext.io](https://itnext.io/next-js-with-material-ui-7a7f6485f671)_

Learn the basics of styling Material UI components to conform to the McMaster Digital Brand Standards. Material UI is a widely used open-source React component library that implements Google's [Material Design](https://m2.material.io/). The library includes an extensive collection of prebuilt components that are ready for use in production right out of the box. Material UI offers comprehensive styling tools that allow you to customize the design system of its components. Material UI allows you to build websites faster and enjoys a great deal of support in the React community given that it is the largest UI community in the React ecosystem. MUI also reduces the barrier to entry by providing an intuitive development experience for less technical designers and developers. The official MUI documentation includes code snippets for all its components to help developers use them effectively when creating their websites.

In this beginner learning module, participants will learn how to create a Next.js application, add the MUI library to it, and style MUI components to fit the McMaster Digital Brand Standards. We will also learn how to ensure that these components scale properly on small screen devices and how to evaluate a website for AODA compliance using the Wave tool. 
No previous experience with Next.js or MUI is required. Familiarity with TypeScript and React will be helpful but is not necessary.

## Initial Setup

Please follow these steps to install Node.js and create a Next.js application **before** starting the learning module.

If you are using Windows or MacOS, use one of the installers from the [Node.js download page](https://nodejs.org/en/download/). Be sure to install the version labeled **LTS**. Other versions have not yet been tested with `npm`.

If you are using a Linux distribution, you can install Node.js using the `apt` package manager.
1. Start by refreshing your local package index: 
~~~
sudo apt update
~~~
2. Install Node.js: 
~~~
sudo apt install nodejs
~~~
3. Verify that the installation was successful: 
~~~
node -v
~~~
  You should be presented with the version of Node.js that you just installed.
4. Install the `npm` package manager:
~~~
sudo apt install npm
~~~

### Create a Next.js Application
Once you have Node.js installed, you can create a template Next.js application using the following command:
```npx create-next-app```
You will need to pick a name for your application and specify a few preferences as shown below:
![npm-create](assets/img/npx-create.png)

### Start the Development Server
`cd` into the your project directory: 
~~~
cd lmr_mac-branding
~~~
Start the development server:
~~~
npm run dev
~~~

### View Default Landing Page
Open your browser of choice and navigate to: `localhost:3000`
You should be presented with the following page:
![next-landing](assets/img/next-js-landing.png)

### Modify `_app.tsx`
Open the `_app.tsx_` file located in the `pages` directory and **delete** the following import statement:
~~~
import '@/styles/globals.css'
~~~

### Modify `Home.module.css`
Open the `Home.module.css` file located in the `styles` directory and change the `min-height` value from `100vh` to `calc(100vh - 164px)`:
```
.main {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  padding: 6rem;
  min-height: calc(100vh - 164px);
  margin-top: auto;
}
```
This change will reduce the amount of whitespace on the main page.

### Modify the Index Page
Using the text editor or IDE of your choice, modify the `index.tsx` file located in the `pages` directory by selecting and deleting lines 2, 3, and 6 as well as lines 18-110. Next, change line 13 to `<main className={styles.main}>` 

Change the title of the application by modifying line 8.
Enter the new title between the two `title` tags: 
`<title>MacMaster Branding</title>`.

Add the following line of code under line 13: 
`<h1> Hello World!</h1>`

Your `index.tsx` file should now look like this:
```
import Head from 'next/head'  
import styles from '@/styles/Home.module.css'  
  
export default function Home() {  
	return (  
		<>  
			<Head>  
				<title>McMaster Branding</title>  
				<meta name="description" content="Generated by create next app" />  
				<meta name="viewport" content="width=device-width, initial-scale=1" />  
				<link rel="icon" href="/favicon.ico" />  
			</Head>  
			<main className={styles.main}>  
				<h1> Hello World!</h1>  
			</main>  
		</>  
	)  
}
```

Go back to your browser tab, the page should now look like this:
![hello-world](assets/img/hello-world.png)

### Add the Material UI Library to Your Application
Navigate to the your project's directory if you are not already in it: 
~~~
cd lmr_mac-branding
~~~

Run the following command to add the MUI library to your project using the `npm` package manager: 
~~~
npm install @mui/material @emotion/react @emotion/styled @mui/icons-material
~~~
You will be presented with the following message `added 59 packages, and audited 358 packages in 6s` if the installation is successful. Some of the numbers in the message may be different for you.