# Email Template Starter File Tested for Outlook and Gmail
This is a new email template that I am developing and testing for internal purposes but that I am keeping as generic as possible so others can take advantage of some of the formatting options. I am using tables to structure the template. This template is thoroughly tested in outlook and solves many of the formatting problems that generally occur when rendered in the Microsoft client. It will appear almost identical to the way gmail renders their emails.

<h3>Contents</h3>
<ol>
  <li><a href="#references">References</a></li>
  <li><a href="#preview-text">Preview Text</a></li>
  <li><a href="#styling">Styling</a></li>
  <li><a href="#padding-and-margin">Padding and Margin</a></li>
  <li><a href="#table-spacing">Table Spacing</a></li>
  <li><a href="#a-tag-attributes">a Tag Attributes</a></li>
  <li><a href="#images">Images</a></li>
</ol>

<h2>References</h2>

I used these 3 references to create the skeleton of the HTML. You can reference these for everything within the head tags excluding the styling:

https://www.goodemailcode.com/email-code/template

https://github.com/inline-blocksedit/starter-email-template?tab=readme-ov-file  

https://backgrounds.cm/?utm_source=emailbg.net 
https://www.caniemail.com/search/?s=display
 -> I use this for determining client compatibility

<h2>Preview Text</h2>

![image](https://github.com/user-attachments/assets/a33d19c4-711a-4bfb-8f2b-1ee0bdb0dd43)
I use all options to hide the preview text to cover all clients and screens. Display:none seems to not be respected by older versions of outlook, Yahoo, and AOL
You can check compatibility with the last link in the references I provided above. 

In order to stop clients from pulling in content from your email onto the preview text line, we need to add a lot of spaces. Clients in desktop view can support up to 100 characters so make sure to add lots of empty space. 
For mobile, you will want to design your preheader according to the character limits of mobile view to make sure it makes sense in a low character limit. I tested a few character limits in mobile view:

Gmail: 40 chars

Outlook: 32 chars

Yahoo: 42 chars

Apple mail: 50 chars

<h2>Styling</h2>
I chose to keep all my element style inline for better client support. The only time I am using internal styles are for overall styles such as html, body background color, resetting all elements with 0 padding, margin, and borders and to switch styles for mobile vs desktop viewing. Anything more specific is inline. I also limited the use of !important in the internal styling to keep things simple and neat. 

<h2>Padding and Margin</h2>
Outlook does not respect styles on a div tag. They are removed when rendered so in order to add padding and margin, they need to be places on the td tags. 

For consistency, I have styled tables only with a width and <td> tags with the padding. This allows the template to appear the same in outlook and gmail without adding too many different styles to account for each client. Both clients will render this solution the same way. I also opted to use width rather than margin as a personal preference and have kept that consistent throughout the template. 

![image](https://github.com/user-attachments/assets/8af4b61a-d2ff-4a54-b8fd-e34979e0362b)

<h2>Table Spacing</h2>
I have found that the simplest way to create 0 spacing between table cells and rows is to reset all padding and margins:
  
  
  <b>div, table, tbody, tr, td, img {
      margin: 0;
      padding: 0;
      border: 0;
  }
  table {
      border-spacing: 0;
  }</b>

Then include <b>line-height: 0px;</b> on your table tag. 

![image](https://github.com/user-attachments/assets/0cfb66cd-2234-4f0a-8e7a-f08117dc73b2)

I struggled to remove the extra space under my table for quite some time and tried setting cellpadding="0", cellspacing="0", border-collapse: "collapse", but the only thing that seemed to solve my problem was setting the line height. Without this, I was getting an extra 4px of space at the bottom of each row on my table. 


<h2>a Tag Attributes</h2>
A tags require a few additional attributes for email than for web. 
All my a tags have these attributes: href, name, target, and xt.

&lt;a href="https://google.com/" name="link name" target="_blank" xt="SPCLICKSTREAM"&gt;Link 1&lt;/a&gt;

Name attribute is used to identify the link in your email service provider. The provider will associate clicks to this name. 

xt is the tracking information. A few options include:
<ul>
  <li>SPCLICK - Regular tracking. Follows the user to the webpage</li>
  <li>SPCLICKSTREAM - Follows the user across all web pages in the same session</li>
  <li>SPNOTRACK - No tracking and does not provide any click details</li>
  <li>SPCLICKTOVIEW - Opens the email in a browser</li>
  <li>SPFORWARD - Classic forwarding</li>
</ul>

<h3>Default Link Styles</h3>
Email clients each have their own default link styles. Outlook has a blue underlined style for their links for example. I added inline styles to all my a tags to overwrite the email client's default settings. 

style="color:#0d7079; font-weight: normal; text-decoration: underline;"

<h2>Images</h2>

Outlook will by default display images at their actual size rather than respect the image size styles. In order to fix that, we can limit the table size and set the image and table max-width:XXXpx as I have done here:

![image](https://github.com/user-attachments/assets/9b55c671-cee4-4645-a774-b5755e648670)

This is beneficial to solve pixelation issues or if you have an image that should stretch across the entire mobile display (up to 480px) but only a portion of the page in full width display (say 300px). In this case you want to size your image to the max pixels it will occupy so you dont get blurry images at its max width, but you will need to limit the image width by using table dimensions when it occupies a smaller area. 

<h2>Gradient for Outlook</h2>

While a gradient background is quite simple for the gmail rendering engine, it requires VML for outlook. 

For gmail, all you need to do is add a style to the element you want to add the gradient background to. For my example I added this style to my table element:

![image](https://github.com/user-attachments/assets/aa2d0e36-44e9-42e5-b6ad-c99115b99c7c)

For outlook it is more complicated. For one, the outlook app for mobile uses a different rendering engine than the desktop version which does not support VML. I have not found a good or simple solution for this and instead add a solid fall-back color for mobile outlook users. To do this, I added a class called gradient to my table which defaults to a solid background when the screen size is smaller than 480px. I did this with media queries because outlook will default to this option in desktop if you use inline styling on your tables.

To add a gradient background compatible with Outlook (using VML), you'll need to wrap your existing structure with a VML rect element that defines the gradient.

![image](https://github.com/user-attachments/assets/565072e2-c9ee-4dc9-bc19-aad0c70c1c90)

You can copy and paste this to wrap your element:

&lt;!--[if gte mso 9]&gt;

&lt;v:rect xmlns:v="urn:schemas-microsoft-com:vml" fill="true" stroke="false" style="width:600px; height:445px"&gt;

&lt;v:fill type="gradient" color="#00344C" color2="#0D7079" angle="0" /&gt;

&lt;v:textbox style="mso-fit-shape-to-text:true" inset="0,0,0,0"&gt;

&lt;div&lt;![endif]--&gt;

[ YOUR CODE HERE ]

&lt;!--[if gte mso 9]&gt;&lt;/div&gt;&lt;/v:textbox&gt;&lt;/v:rect&gt;&lt;![endif]--&gt;


<h3>What Does the VML Mean?</h3>
<ul>
	<li>&lt;v:rect&gt; defines a rectangle with specified dimensions.</li>
		<li>style controls the width and height.</li>
		<li>fillcolor sets the background color, and strokecolor/strokeweight set the border color and width.</li>
		<li>&lt;v:fill&gt; allows for additional properties like gradients.</li>
		<li>&lt;v:textbox&gt; places text inside the shape.</li>
</ul>

<i>It should be noted that outlook does not save space for the rect element and will render the rest of the email content underneath this area unless a heigh is specified in the styles on the rect element. In my case, my table takes up 600px X 445px so I have specified exactly those dimensions. You will have to play around with these dimensions a bit.</i> 
