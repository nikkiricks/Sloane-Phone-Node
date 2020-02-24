# Sloane-Phone-Node

- For an intro and setting up NodeBots check out my [Node_Bots repo](https://github.com/nikkiricks/Node_Bots)
- To see how I took my Sloane Phone to the next level using C++ check out my [Sloane-Phone-Arduino repo](https://github.com/nikkiricks/Sloane-Phone-Arduino)

## The Tech Inspiration

I was listening to the [Code Newbie Podcast](https://www.codenewbie.org/podcast) episode about [node bots](https://www.codenewbie.org/podcast/how-do-you-build-a-robot-in-javascript) with [Rachel White](http://rachelisaweso.me/) and was inspired as she was saying how simple it was to start building robots if you were familiar with javascript. Which felt accessible to me at the time because it was the first programming language that I picked up when learning how to code.

## The Real Life Inspiration

One day, when leaving my kids with a new babysitter, my oldest, who was 6 at the time was really nervous about it and didn’t want us to go. Before the babysitter came, I told her that we could create a secret password to pass through the babysitter without him knowing. I said that we could tell the babysitter that my partner and I were going to go grocery shopping and my daughter was going to think about the type of fruit she was wanting us pick up. But the secret password was, that if she was having a really hard time and needed us to come home she could tell the babysitter to text us saying she wanted “bananas” (a fruit my daughter doesn’t like) but if she was having a great time and didn’t need us to come home she could text us and say she wanted “kiwi” (one of my daughters favorite fruit).

Bad time = “bananas” Good time = “kiwi”

## How tech and life came together

This interaction inspired me to think, what if I could have left my daughter with a simple device that could text me an emoji how she was feeling as opposed to going through the babysitter? Maybe the two options could be :smiley: or :cry:?

So the idea of the **Sloane Phone** was born.

_By the way, Sloane wanted “kiwi’s”._

## Parts

- I purchased an [Arduino Uno](https://store.arduino.cc/usa/arduino-uno-rev3) ($22) board online, and after realizing I had no way to plug it into my computer also bought a [Freenove Starter Kit](https://www.amazon.com.au/Freenove-Processing-Oscilloscope-Voltmeter-Components/dp/B0721B8228/ref=sr_1_1?keywords=freenove+arduino+uno+starter+kit&qid=1576150765&s=electronics&sr=1-1) ($34) as well.

- I attended a saturday’s hackers session at [Connected Community Hackerspace](https://www.hackmelbourne.org/) where I learned the basics of [Ohm’s Law](https://en.wikipedia.org/wiki/Ohm%27s_law) (electricity) and saw people building and driving around machine learning car robots.
- There I did the “hello world” of arduino boards and made an led light turn on through the circuit board
- I’ve looked through the Arduino library for some examples of different projects to build but realized they’re in C++ and I’m wanting to use node.js

### Next steps:

- I understand that [Johnny-Five](http://johnny-five.io/) is a great site for documentation and the community is really friendly so I’ll try and start there as a guide
  - I join the [Gitter chat group](https://gitter.im/rwaldron/johnny-five) for more support
- I was also referred to [Node-Ardx](http://node-ardx.org/) as a place for node.bot newbies to start from people at Hackerspace Melbourne.
- Figure out all the hardware I need and what I need to purchase, I believe I need a [GSM](https://www.arduino.cc/en/Guide/ArduinoGSMShield) (\$40 (and this hobby is starting to get expensive))

## How to send an email in a push of a button (as in an actual button)

Sign up for [Send Grid](https://sendgrid.com/solutions/email-api/) and get an API key. I found the [github instructions](https://github.com/sendgrid/sendgrid-nodejs/tree/master/packages/mail) really helpful.

Use the Johnny-Five docs and set up a [button](http://johnny-five.io/examples/button/)

Read [these instructions](https://sendgrid.com/blog/how-to-send-email-with-arduino-at-ny-tech-meetup/) from Swift at SendGrid.

Here's the code that I used in my `email-button.js` to send two different emails from two different buttons:

```
var arduino = require("johnny-five")
var board = new arduino.Board()
const sgMail = require('@sendgrid/mail');
sgMail.setApiKey(process.env.SENDGRID_API_KEY);

board.on("ready", function() {
  var buttonTwo = new arduino.Button(2); // Button on pin 2
  var buttonFour = new arduino.Button(4)

  buttonTwo.on("up", function() {
    const msg = {
      to: 'ENTER_EMAIL,
      from: 'ENTER_EMAIL(can't be the same)',
      subject: 'ENTER SUBJECT',
      text: 'testing testing testing',
      html: '<strong>ENTER TEXT FOR BODY OF THE EMAIL</strong>',
    };
    sgMail.send(msg);
    console.log(msg)
  });

  buttonFour.on("up", function() {
    const msg = {
      to: 'ENTER_EMAIL',
      from: 'ENTER_EMAIL',
      subject: 'ENTER SUBJECT',
      text: 'testing testing testing',
      html: '<strong>ENTER TEXT FOR BODY OF THE EMAIL</strong>',
    };
    sgMail.send(msg);
    console.log(msg)
  });
});
```

Here's how I set up my board:

![](images/board_setup_left.JPG)
![](images/board_setup_right.JPG)

Demo of the setup

![](images/final_setup.gif)

## Challenges

- **Hardware** was both fun and frustrating. It was interesting to research the kit I purchased and see what I could do with all of the pieces but frustrating when I would purchase a piece and then needed something else to go along with it.

  - GSM was going to cost $70 and then $70 to ship to get here with just 3 days to code on it which I didn't feel comfortable with. So I decided to get a Wifi Shield instead and try and code on it that way
  - I was waiting on an NodeMCU and didn't end up getting it in time to implement into the project
  - To use the vibrate method like I considered above I would need to buy a [vibration motor](https://www.sparkfun.com/products/8449), it's only \$2 but I didn't have the time to wait for it.

- **SMS** Running into issues with Twilio - Australia has regulation issues with signing up for an account that could take up to 3 days. Because I'm an American and have no Australian documentation I wasn't verified.
  -Then tried using [TouchSMS](https://platform.touchsms.com.au/register) but they require you to sign up manually by emailing their support team? No thanks.
  -Finally I heard about [MessageMedia](https://hub.messagemedia.com/) where you can easily send a text using an email address if the number you're trying to text is "+610577355263" you could just add it to "@e2s.messagemedia.com" so it would be "610577355263@e2s.messagemedia.com". So easy!

## Resources

- [npm keypress](https://www.npmjs.com/package/keypress) is helpful to use your keyboard as a controller. For example in the [Servo Continuous tutorial](http://johnny-five.io/examples/servo-continuous/) you require keypress and use the the up and down arrows, space bar, and q to control the servo.

- Sending SMS from Arduino over internet on [instructables](https://www.instructables.com/id/Send-SMS-from-Arduino-over-the-Internet-using-ENC2/)

- [Arduino Uno projects](https://electronicsforu.com/arduino-projects-ideas) to see what’s [possible](https://howtomechatronics.com/arduino-projects/) in [general](https://circuitdigest.com/arduino-projects).
