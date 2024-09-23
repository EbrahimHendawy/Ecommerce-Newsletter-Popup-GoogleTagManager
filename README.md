# Ecommerce Newsletter Popup with Google Tag Manager

This project contains a customizable popup for collecting email subscriptions, designed for ecommerce websites. The popup is implemented in pure JavaScript (ES5 compatible) and CSS, making it easy to integrate with various web platforms and content management systems. It also includes data layer events for analytics tracking.

## Features

- Clean, modern design
- Customizable text and styling
- Email input field
- Subscribe button
- Option to close the popup
- Delayed appearance for better user experience
- Data layer events for popup interactions
- ES5 compatible for wide browser support and Google Tag Manager compatibility

## Setup Instructions

### 1. Add the Code to Your Website

You can add this popup to your website by including the following code in your HTML file or through a tag management system like Google Tag Manager.

```html
<script>
(function() {
  // Styles
  var styles = '' +
    '.popup-overlay {' +
      'position: fixed;' +
      'top: 0;' +
      'left: 0;' +
      'width: 100%;' +
      'height: 100%;' +
      'background: rgba(0, 0, 0, 0.5);' +
      'display: flex;' +
      'justify-content: center;' +
      'align-items: center;' +
      'z-index: 1000;' +
    '}' +
    '.popup-content {' +
      'background: white;' +
      'padding: 30px;' +
      'border-radius: 10px;' +
      'text-align: center;' +
      'max-width: 400px;' +
      'width: 90%;' +
    '}' +
    '.popup-header {' +
      'margin-bottom: 20px;' +
    '}' +
    '.popup-title {' +
      'font-size: 24px;' +
      'font-weight: bold;' +
      'margin-bottom: 10px;' +
    '}' +
    '.popup-subtitle {' +
      'font-size: 16px;' +
      'color: #666;' +
    '}' +
    '.popup-input {' +
      'width: 100%;' +
      'padding: 10px;' +
      'margin-bottom: 15px;' +
      'border: 1px solid #ddd;' +
      'border-radius: 5px;' +
    '}' +
    '.popup-button {' +
      'background: #4CAF50;' +
      'color: white;' +
      'border: none;' +
      'padding: 10px 20px;' +
      'border-radius: 5px;' +
      'cursor: pointer;' +
      'font-size: 16px;' +
      'width: 100%;' +
    '}' +
    '.popup-footer {' +
      'margin-top: 15px;' +
      'font-size: 14px;' +
      'color: #888;' +
      'cursor: pointer;' +
    '}';

  // Create and append style element
  var styleElement = document.createElement('style');
  styleElement.textContent = styles;
  document.head.appendChild(styleElement);

  // Create popup HTML
  var popupHTML = '' +
    '<div id="newsletterPopup" class="popup-overlay">' +
      '<div class="popup-content">' +
        '<div class="popup-header">' +
          '<div class="popup-title">Become an Ecommerce Expert</div>' +
          '<div class="popup-subtitle">Get weekly insights, tips & news from our team</div>' +
        '</div>' +
        '<input type="email" class="popup-input" placeholder="Enter your email">' +
        '<button class="popup-button">SUBSCRIBE</button>' +
        '<div class="popup-footer" onclick="closePopup()">No thanks, I\'ll pass</div>' +
      '</div>' +
    '</div>';

  // Inject popup HTML
  document.body.insertAdjacentHTML('beforeend', popupHTML);

  // Function to push data layer event
  function pushDataLayerEvent(eventName, eventData) {
    window.dataLayer = window.dataLayer || [];
    var pushData = {
      'event': eventName
    };
    for (var prop in eventData) {
      if (eventData.hasOwnProperty(prop)) {
        pushData[prop] = eventData[prop];
      }
    }
    window.dataLayer.push(pushData);
  }

  // Function to show popup
  window.showPopup = function() {
    document.getElementById('newsletterPopup').style.display = 'flex';
    pushDataLayerEvent('newsletter_popup_shown', {
      'popup_id': 'newsletter_subscription'
    });
  };

  // Function to close popup
  window.closePopup = function() {
    document.getElementById('newsletterPopup').style.display = 'none';
    pushDataLayerEvent('newsletter_popup_closed', {
      'popup_id': 'newsletter_subscription'
    });
  };

  // Show popup after 3 seconds (adjust as needed)
  setTimeout(showPopup, 3000);

  // Add event listener for subscribe button
  document.querySelector('.popup-button').addEventListener('click', function() {
    var email = document.querySelector('.popup-input').value;
    // Add your form submission logic here
    console.log('Subscribing email: ' + email);
    pushDataLayerEvent('newsletter_subscribed', {
      'popup_id': 'newsletter_subscription',
      'email': email // Be cautious with PII in production
    });
    // After successful submission, close the popup
    closePopup();
  });
})();
</script>
```

### 2. Customize the Popup (Optional)

You can customize the popup by modifying the following elements in the code:

- Text content: Update the title, subtitle, button text, and footer text.
- Colors: Modify the CSS properties to match your brand colors.
- Timing: Adjust the `setTimeout` value to change when the popup appears.

### 3. Data Layer Events

This popup fires the following data layer events:

1. `newsletter_popup_shown`: When the popup is displayed.
2. `newsletter_popup_closed`: When the popup is closed.
3. `newsletter_subscribed`: When a user subscribes (clicks the subscribe button).

You can use these events to trigger tags in Google Tag Manager or other analytics tools.

### 4. Implement Form Submission

The code includes a basic event listener for the subscribe button. You should replace the `console.log` statement with your actual form submission logic.

## Integration with Google Tag Manager

1. Log in to your Google Tag Manager account.
2. Create a new tag:
   - Click "New Tag"
   - Name it "Newsletter Popup"
   - Choose "Custom HTML" as the tag type
3. Copy the entire script (including the `<script>` tags) into the HTML field.
4. Set up a trigger for the tag (e.g., All Pages - Page View).
5. Save and publish your changes.

### Setting Up Event Tracking in GTM

1. Create Data Layer Variable:
   - Go to "Variables" > "New"
   - Choose "Data Layer Variable"
   - Set the Data Layer Variable Name to "popup_id"
   - Repeat for other variables you want to track (e.g., "email")

2. Create Trigger for each event:
   - Go to "Triggers" > "New"
   - Choose "Custom Event" as trigger type
   - Set the Event name to match your data layer event (e.g., "newsletter_popup_shown")

3. Create Tags for tracking:
   - Go to "Tags" > "New"
   - Choose your desired tag type (e.g., Google Analytics: GA4 Event)
   - Configure the tag to send the event data
   - Set the trigger to the corresponding Custom Event trigger you created

4. Test and Publish:
   - Use GTM's Preview mode to test your setup
   - Once confirmed working, publish your changes

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is open source and available under the [MIT License](LICENSE).
