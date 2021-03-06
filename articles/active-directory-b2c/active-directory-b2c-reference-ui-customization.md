<properties
	pageTitle="Azure Active Directory B2C preview: User interface (UI) customization | Microsoft Azure"
	description="A topic on the User interface (UI) customization features in Azure Active Directory B2C"
	services="active-directory-b2c"
	documentationCenter=""
	authors="swkrish"
	manager="mbaldwin"
	editor="bryanla"/>

<tags
	ms.service="active-directory-b2c"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/22/2015"
	ms.author="swkrish"/>

# Azure Active Directory B2C preview: How to Customize the Azure AD B2C User Interface (UI)

User experience is paramount in a consumer-facing application. It is the difference between a good application and a great one, and between merely active consumers and truly engaged ones. Azure Active Directory (AD) B2C allows you to customize consumer sign-up, sign-in (*see note below*) and profile editing pages with pixel-perfect control.

> [AZURE.NOTE]
Currently, local account sign-in pages, verification emails and self-service password reset pages are only customizable using the [company branding feature](./active-directory/active-directory-add-company-branding.md) and not by the mechanisms described in this article.

In this article, you will read about:

- Overview of the page user interface (UI) customization feature.
- A helper tool that will help you test the page UI customization feature using our sample content.
- The core UI elements in each type of page.
- Best practices when exercising this feature.

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## The Page User Interface (UI) Customization feature

The page UI customization feature allows you to customize the look-and-feel of consumer sign-up, sign-in and profile editing pages (by configuring [policies](active-directory-b2c-references.policies.md)). Your consumers will have consistent experiences when navigating between your application and pages served by the Azure AD B2C service.

Unlike other services where options are limited or are only available via APIs, Azure AD B2C uses a modern (and simpler) approach to page UI customization. Here's how it works: Azure AD B2C runs code in your consumer's browser and uses a modern approach called [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/) to load content from a URL that you specify in a policy. You can specify different URLs for different pages. The code merges Azure AD B2C's content (called UI elements) and the content loaded from your URL, and displays the page to your consumer. All you need to do is create well-formed HTML5 content with a `<div id="api"></div>` element located somewhere in the `<body>` - this is where the Azure AD B2C's content gets merged into. And host this content on an HTTPS endpoint (with CORS allowed). You can also fully style Azure AD B2C's UI elements.

## A Helper Tool that will help you Test the Page UI Customization Feature with our Sample Content

Use our helper tool (Coming soon) to try out the page UI customization feature using our sample content.

## The core UI Elements in each Type of Page

In the below sections you will find examples of HTML5 fragments (for each type of page) that Azure AD B2C merges into the <div id="api"></div> element located in your content. You can use your own style sheets to customize these UI elements. These style sheets will have to override the default style sheets that we add into these pages in the <head> fragments.

> [AZURE.IMPORTANT]
	During preview, expect the exact UI elements to change as we learn and adapt from your feedback. Always inspect the source code of default pages for the latest updates. In fact, the first change being considered is the removal of our default style sheets; this will mean that you will always have to supply your own style sheets for these UI elements in your content.

## Identity Provider Selection Page

This page contains a list of identity providers that the user can choose from during sign up or sign in. These are either social identity providers such as Facebook and Google+ or local accounts (email address- or username-based).

```HTML

<div id="api" data-name="IdpSelections">
	<div class="intro">
         <p>Sign up</p>
	</div>
	
	<div>
		<ul>
			<li>
				<button class="accountButton" id="FacebookExchange">Facebook</button>
			</li>
			<li>
				<button class="accountButton" id="GoogleExchange">Google+</button>
			</li>
			<li>
				<button class="accountButton" id="SignUpWithLogonEmailExchange">Email</button>
			</li>
		</ul>
	</div>
</div>

```

## Local Account Sign-up Page

This page contains a sign-up form that the user has to fill in when signing up using an email address- or username-based local account. The form can contain different input controls such as text input box, password entry box, radio button, single-select drop-down and multi-select check boxes.

```HTML

<div id="api" data-name="SelfAsserted"> 
	<div class="intro"> 
		<p>Create your account by providing the following details</p>
	</div>
	
	<div id="attributeVerification"> 
		<div class="errorText" id="passwordEntryMismatch" style="display: none;">The password entry fields do not match. Please enter the same password in both fields and try again.</div>
		<div class="errorText" id="requiredFieldMissing" style="display: none;">A required field is missing. Please fill out all required fields and try again.</div>
		<div class="errorText" id="fieldIncorrect" style="display: none;">One or more fields are filled out incorrectly. Please check your entries and try again.</div>
		<div class="errorText" id="claimVerificationServerError" style="display: none;"></div>
		<div class="attr" id="attributeList"> 
			<ul>
				<li> 
					<div class="attrEntry validate">
						<div> 
							<div class="verificationInfoText" id="email_intro" style="display: inline;">Verification is necessary. Please click Send button.</div>
							<div class="verificationInfoText" id="email_info" style="display:none">Verification code has been sent to your inbox. Please copy it to the input box below.</div>
							<div class="verificationSuccessText" id="email_success" style="display:none">E-mail address verified. You can now continue.</div>
							<div class="verificationErrorText" id="email_fail_retry" style="display:none">Incorrect code, try again.</div>
							<div class="verificationErrorText" id="email_fail_no_retry" style="display:none">Exceeded number of retries you need to send new code.</div>
							<div class="verificationErrorText" id="email_fail_server" style="display:none">Server error, please try again</div>
							<div class="verificationErrorText" id="email_incorrect_format" style="display:none">Incorect format.</div>
						</div>

					<div class="helpText show">This information is required</div>
						<label>Email</label>
						<input id="email" class="textInput" type="text" placeholder="Email" required="" autofocus=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Email address that can be used to contact you.');" class="tiny">What is this?</a>

					<div class="buttons verify" claim_id="email"> 
						<div id="email_ver_wait" class="working" style="display: none;"></div>
							<label id="email_ver_input_label" for="email_ver_input" style="display: none;">Verification code</label> 
							<input id="email_ver_input" type="text" placeholder="Verification code" style="display:none">
							<button id="email_ver_but_send" class="sendButton" type="button" style="display: inline;">Send verification code</button>
							<button id="email_ver_but_verify" class="verifyButton" type="button" style="display:none">Verify code</button>
							<button id="email_ver_but_resend" class="sendButton" type="button" style="display:none">Send new code</button>
							<button id="email_ver_but_edit" class="editButton" type="button" style="display:none">Change e-mail</button>
							<button id="email_ver_but_default" class="defaultButton" type="button" style="display:none">Default</button> 
						</div>
					</div>
				</li>
				<li> 
					<div class="attrEntry">
						<div class="helpText">8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ " ( ) ; .This information is required</div>
						<label>Enter password</label>
						<input id="password" class="textInput" type="password" placeholder="Enter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title="8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ &quot; ( ) ; ." required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Enter password');" class="tiny">What is this?</a>
					</div>
				</li>
				<li> 
					<div class="attrEntry">
						<div class="helpText"> This information is required</div>
						<label>Reenter password</label>
						<input id="reenterPassword" class="textInput" type="password" placeholder="Reenter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title=" " required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Reenter password');" class="tiny">What is this?</a>
					</div>
				</li>
				<li> 
					<div class="attrEntry">
						<div class="helpText">This information is required</div>
						<label>Name</label>
						<input id="displayName" class="textInput" type="text" placeholder="Name" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your display name.');" class="tiny">What is this?</a>
					</div>
				</li>
				<li> 
					<div class="attrEntry">
						<div class="helpText"></div>
						<label>Gender</label>
						<input id="extension_Gender_F" name="extension_Gender" type="radio" value="F" autofocus="">
						<label for="extension_Gender_F">Female</label>
						<input id="extension_Gender_M" name="extension_Gender" type="radio" value="M">
						<label for="extension_Gender_M">Male</label>
						<a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('');" class="tiny">What is this?</a>
					</div>
				</li>
				<li> 
					<div class="attrEntry">
						<div class="helpText"></div>
						<label>Loyalty number</label>
						<input id="extension_MemNum" class="textInput" type="text" placeholder="Loyalty number"><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Membership number');" class="tiny">What is this?</a>
					</div>
				</li>
				<li> 
					<div class="attrEntry">
						<div class="helpText"></div>
						<label>State</label>
						<select class="dropdown_single" id="state">
							<option style="display:none" disabled="disabled" value="placeholder" selected="">State</option>
							<option value="WA">Washington</option>
							<option value="NY">New York</option>
							<option value="CA">California</option>
						</select>
						<a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your residential state or province.');" class="tiny">What is this?</a>
					</div>
				</li>
				<li> 
					<div class="attrEntry">
						<div class="helpText">This information is required</div>
						<label>Zip code</label>
						<input id="postalCode" class="textInput" type="text" placeholder="Zip code" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('The postal code of your address.');" class="tiny">What is this?</a>
					</div>
				</li>
			</ul> 
		</div>
		<div class="buttons"> <button id="continue" disabled="">Create</button> <button id="cancel">Cancel</button></div>
	</div>
	<div class="verifying-modal"> 
		<div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
		<div id="verifying_blurb"></div>
	</div>
</div>

```

## Social Account Sign-up Page

This page contains a sign-up form that the consumer has to fill in when signing up using an existing account from a social identity provider such as Facebook or Google+. This page is similar to the local account sign-up page (shown in the previous section) with the exception of the password entry fields.

## Multi-factor Authentication Page

This page enables users to verify their phone numbers (using text or voice) during sign up or sign in.

```HTML

<div id="api" data-name="Phonefactor">
	<div id="phonefactor_initial">
		<div class="intro">
			<p>Enter a number below that we can send a code via SMS or phone to authenticate you.</p>
		</div>
		<div class="errorText" id="errorMessage" style="display:none"></div>
		<div class="phoneEntry" id="phoneEntry">
			<div class="phoneNumber">
				<select id="countryCode" style="display:inline-block">
					<option value="+93">Afghanistan (+93)</option>
					<!-- Not all country codes listed -->
					<option value="+44">United Kingdom (+44)</option>
					<option value="+1" selected="">United States (+1)</option>
					<!-- Not all country codes listed -->
				</select>
			</div>
			<div class="phoneNumber">
				<input type="text" id="localNumber" style="display:inline-block" placeholder="Phone number">
			</div>
		</div>
		<div id="codeVerification" style="display:none">
			<div>
				<p>Enter your verification code below, or <button id="retryCode" class="textButton">send a new code</button></p>
				<input type="text" id="verificationCode" placeholder="Verification code">
			</div>
		</div>
		<div class="buttons">
			<button id="verifyCode" class="sendInitialCodeButton">Send Code</button>
			<button id="verifyPhone" style="display:inline-block">Call Me</button>
			<button id="cancel" style="display:inline-block">Cancel</button>
		</div>
	</div>
	<div class="dialing-modal">
		<div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
		<div id="dialing_blurb"></div><div id="dialing_number"></div>
	</div>
</div>

```

## Error Page


```HTML

<div id="api" class="error-page-content" data-name="GlobalException">
	<h2>Sorry, but we're having trouble signing you in.</h2>
    <div class="error-page-help">We track these errors automatically, but if the problem persists feel free to contact us. In the meantime, please try again.</div>
	<div class="error-page-messagedetails">Your administrator hasn't provided any contact details.</div>
	<div class="error-page-messagedetails">
		<div class="error-page-correlationid">Correlation ID:1c4f0397-c6e4-4afe-bf74-42f488f2f15f</div>
		<div>Timestamp:2015-09-14 23:22:35Z</div>
		<div class="error-page-detail">AADB2C90065: A B2C client-side error 'Access is denied.' has occurred requesting the remote resource.</div>
	</div>
</div>

```

## Things to remember when building your own Content

If you are planning to use the page UI customization feature, please review the following best practices:

- Don't copy over Azure AD B2C's default template and attempt to modify it. It is best to build your HTML5 content from scratch and to use the default template as reference.
- For security reasons, we don't allow you to include any JavaScript in your content. Most of what you need should be available out-of-the-box. If not, please use [User Voice](http://feedback.azure.com/forums/169401-azure-active-directory) to request new functionality.
- Supported browser versions:
	- Internet Explorer 11
	- Internet Explorer 10
	- Internet Explorer 9 (limited)
	- Internet Explorer 8 (limited)
	- Google Chrome 43.0
	- Google Chrome 42.0
	- Mozilla Firefox 38.0
	- Mozilla Firefox 37.0
