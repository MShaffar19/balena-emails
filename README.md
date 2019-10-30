# Balena newsletter generator

The purpose of this tool is to help with the creation of balena newsletters and other marketing emails. It uses templating principles and a responsive email framework to convert simple HTML tags into the complex table HTML required for emails, so you don't have thousands of `tables` in your markup. The main technologies used are:

- [Handlebars](https://handlebarsjs.com/) semantic HTML templates
- [MJML](https://mjml.io/) responsive email framework
- [Browsersync](https://www.browsersync.io/) Built-in synchronised server for testing
- [Mailchimp](https://mailchimp.com/) integration

## Features

- Generates the email as a responsive HTML file. 
- Mailchimp integration
  - Creates a Mailchimp campaign
  - Adds the HTML email to the campaign
  - Sends test email (will not be sent to audience, only specified tester)

## Installation

You will need [Node.js](https://nodejs.org/en/) 8 or greater.
```
git pull https://github.com/balena-io/balena-emails
cd balena-emails
npm install
```

## Usage

To use the tool you will need to:
  - specify which email template to use (see the templates section)
  - provide a JSON file with the template content (see the datafile section)

### Watch

If you want to develop a new template, edit an existing one or just see how the template looks like:

```
npm run watch -- --datafile path/to/datafile.json --template <newsletter>
```

All files except the `.json` datafile are watched so you will see the changes imediately on your browser. If you modify the datafile you will need to re-run this command.

### Build

If you just want to generate the responsive email run this command. The generated output can be located on the `dist` folder:

```
npm run build -- --datafile path/to/datafile.json --template <newsletter>
```

### Staging

If you want to generate the responsive email and publish the result to a netlify webpage you can run:

```
npm run staging -- --datafile path/to/datafile.json --template <newsletter> --netlifytoken <NETLIFY_AUTH_TOKEN>
```

[Here](https://docs.netlify.com/cli/get-started/#obtain-a-token-in-the-netlify-ui) is how you can get your Netlify token.

### Publish

If you want to generate the responsive email, publish to netlify, create the mailchimp campaign and send a test email (this will not send the campaign to the end users):

```
npm run publish -- --datafile path/to/datafile.json --template <newsletter> --netlifytoken <NETLIFY_AUTH_TOKEN> --mailchimpkey <YOUR_MAILCHIMP_API_KEY>
```

[Here](https://mailchimp.com/help/about-api-keys/) is how you can get your Mailchimp API_KEY.

## Templates

This are the templates currently available:
- newsletter

## Datafile format

The datafile must be a `JSON` file with the following parameters (there is also a sample file in the `data` folder). Parameters in **bold** are required.

### Global configuration
| parameter | type    | description                                      |
| --------- | ------- | ------------------------------------------------ |
| `configuration` | **object**  | Settings for the template |
| `configuration.mailchimp_recipients` | **string** | The target audience id. Some common ones are: `1c73cb7379` for 'Balena Users (Newsletter)', `ab92290a0b` for 'Test audience',  `79b41f7587` for 'Testing'. [Here](https://mailchimp.com/help/find-audience-id/) is how to find the recipient/audience id. Default: `ab92290a0b`|
| `configuration.mailchimp_title` | string  | The title of the campaign. Default: `Autogenerated campaign -- Default title` |
| `configuration.mailchimp_subject_line`  | string | The subject line for the campaign. Default: `Autogenerated campaign -- Default subject line` |
| `configuration.mailchimp_preview_text`  | string | The preview text for the campaign. Default: `Autogenerated campaign -- Default preview text` |
| `configuration.mailchimp_from_name`  | string | The `from` name on the campaign (not an email address). Default: `balena` |
| `configuration.mailchimp_reply_to`  | string | The reply-to email address for the campaign. Default: `hello@balena.io` |
| `configuration.mailchimp_test_emails`  | string array | An array of email addresses to send the test email to. Default: [] |
| `configuration.netlify_site_name`  | string | Name of the website on netlify. The site can be accessed on https://<netlify_site_name>.netlify.com Default: `balena-default-site` |
| `configuration.assets_folder`  | string | Path to folder with images or other media embedded on the template. Relative to root. |

### Template: Newsletter settings
| parameter | type    | description                                      |
| --------- | ------- | ------------------------------------------------ |
| `header`  | **object** | Settings for the email header |
| `header.title`  | string | The title shown on the email header (top right, eg: `October 2019`) |
| `header.image`  | url | Background image for the header. For compatibility reasons it's better if the image has a 600/380 ratio. Default: [here](https://assets.balena.io/newsletter/2019-06/hero_classic.png) |
| `header.image_height`  | numeric | Height for the image header. Default: 380px |
| `header.image_width`  | numeric | Width for the image header. Default: 600px |
| `header.color`  | string | Background color for the header. Default: `rgb(41, 80, 111)` |
| `featured`  | **object** | Settings for the featured intro section |
| `featured.text`  | string | HTML formated content to be shown on the featured section (does not need to be escaped) |
| `news`  | **object array** | Settings for the `News from balenaHQ` section. Each object is constructed as follows below. |
| `[news].title`  | **string** | News card title |
| `[news].image`  | string | Left side image on the news card. |
| `[news].button_text`  | **string** | Text to display on the button |
| `[news].link`  | **string** | URL if button/image are clicked |
| `[news].text`  | **string** | HTML formated content to be shown on the news card (does not need to be escaped) |
| `posts`  | **object array** | Settings for the `The latest from our blog` section. Each object is constructed as follows below. |
| `[posts].title`  | **string** | Blog card title |
| `[posts].image`  | string | Left side image on the posts card. |
| `[posts].button_text`  | **string** | Text to display on the button |
| `[posts].link`  | **string** | URL if button/image are clicked |
| `[posts].text`  | **string** | HTML formated content to be shown on the blog card (does not need to be escaped) |
| `projects`  | **object array** | Settings for the `Projects of the month` section. Each object is constructed as follows below. |
| `[projects].title`  | **string** | Project title |
| `[projects].link`  | string | URL if title is clicked |
| `[projects].text`  | string | HTML formated content to be shown on the project description (does not need to be escaped) |
| `jobs`  | **object** | Settings for the jobs section |
| `jobs.text`  | string | Additional text to display under the jobs section. |
| `events`  | **object array** | Settings for the `Events` section. Each object is constructed as follows below. |
| `[events].attending`  | **string** | Whether there will be balena representation on the event or not. `yes|no` |
| `[events].title`  | **string** | Name of the event |
| `[events].link`  | string | URL if title is clicked |
| `[events].location`  | **string** | Location of the event. Usually `City, Country` |
| `[events].event_date`  | **string** | Date of the event. Usually `Month datefrom - dateto, year` |



