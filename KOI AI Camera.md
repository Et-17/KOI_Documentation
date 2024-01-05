# KOI AI Camera

The KOI AI Camera is a device produced by KittenBot that allows you to perform object, face, and vocal recognition. It also has support both for interfacing with a Micro:bit and for self-hosting MicroPython code.

## Physical Connection

It is much easier to control the KOI with a Micro:bit than with self-hosting. It will let you use a larger program space, more memory, and a more familiar programming environment. If you still wish to do self-hosting, then you just need to plug the KOI into your computer using the MicroUSB port at the bottom, then skip the rest of this section. The physical connection and wiring is very important to the health and safety of you, the KOI, and the Micro:bit that you connect it too. Negligence can and has broken KOI modules before. When you look at the serial connection with the screen pointing up and the MicroSD card facing you but to the right of the serial connection, the pins from left to right are ground, voltage, transmission, reception. Keep in mind that the color scheme changes from module to module and is therefore completely unreliable. The transmission and reception have to be connected to a pin that can be used for digital GPIO. There are a few of them, and you get more by disabling the Micro:bit screen, but the most guaranteed-to-work ones are P0, P1, and P2. The ground and voltage connections depend on whether you are powering from a computer or a breakout board. It usually doesn't matter, but it is much safer for beginners if you use a computer.

### Computer

If you are powering from a computer, then only connect the data lines. Leave the ground and voltage lines unconnected. Plug both the Micro:bit and the KOI into your computer using the MicroUSB plugs on both of them. This will ensure that both get the correct power they need without risking accidentally reversing the polarity.

### Breakout Board

Refer to Mr. Cortez. Do not attempt to wire anything on your own or else you risk completely frying it. However, for **!educational purposes only!**, I will describe it. You must connect it to 5V, meaning that you have to have a 9V battery connceted to the DC barrel. The breakouts that we use in AI class have two voltage rails, V1 and V2. On the bottom near the barrel is two jumpers that decided whether each rail is connected to 3V or 5V. You can use whichever rail you want, just ensure that the one you use has the jumper connected to 5V.

The connector furthest from the MicroSD card is ground, and therefore should be connected to a ground pin on the breakout board. The one immediately to the right of it is the 5V pin, which should be connected to the 5V rail that you selected. Connect the two data lines in the same way as the previous section.

## Programming

There are many different ways to program and control the KOI. The easiest way is to connect it with a Micro:Bit, allowing you to use your previous experience and Microsoft Makecode. However, if you're up for a bit of a challenge, you can forgo the external control entirely and run your code directly on the KOI.

### Micro:bit

First wire it using the section "Physical Connection", then boot up [Makecode](https://makecode.microbit.org/). After you create a new project, go the extensions section and add the KOI extension, which will give a whole array of new blocks that you will use to control your KOI. You can find the official page of the extension [here](https://makecode.microbit.org/pkg/KittenBot/pxt-koi), but I recommend referring to the documentation for the extension later in this document. 

### Self-Host

While according to the official documentation you *should* be able to self-host block code using KittenBot's proprietary KittenCode software, it seems that it is currently broken. The only other option is MicroPython. You can get started quickly by plugging the KOI into your computer using the MicroUSB port at the bottom and opening it in a serial console, such as [this one](https://serial.huhn.me/) with a baud rate of 115,200. This will give you a full MicroPython REPL for quick development and prototyping. You can use whatever serial console you like, but my personal favorite is PuTTY. Unfortunately, this doesn't store the programs that you write. To do this, you can remove the SD Card from the KOI and place it in your computer. You should see a ```boot.py``` file in it. If you do not, or have to use a new MicroSD card, then create it and put the following in it:

```python
from fpioa_manager import *
from koi import *
```

Now add a file called `main.py`. When you the MicroSD card back in the KOI and then restart it, it will run whatever you have written in `main.py`. Please refer to the MicroPython section for more information on this.

## Features

Your KOI has many amazing features that can be used to design all sorts of devices. This section will give you an overview of these features, along with their pitfalls and shortcomings, to reduce redundancy in the API sections. if you want to know how to program any of these features, refer to those.

### MicroSD Card Storage

The KOI can store files on a provided MicroSD card. This includes things such as train

### WiFi

Perhaps the most intriguing is it's ability to connect to WiFi. When you plug the KOI into your computer, you will see a WiFi with the name of "ESD_\<numbers\>". If you connect to this WiFi, then go to the address of the network's gateway (usually ```https://192.168.1.1```) in your web browser, you will see a control panel for the various WiFi features. You can still view this panel when it is connected to the WiFi by finding out its IP address, and then going to that instead.  This panel allows you to connect to other WiFis, view various settings and status logs, and update the WiFi firmware. 

The KOI can only connect to WiFi networks that do not have spaces in their name due to how it sends commands to its WiFi chip: the arguments to the connection command are separated by spaces, so if there's a space in your WiFi name, then the WiFi chip will think that it is the next argument. Unfortunately, there is no way to know when you have successfully connected to WiFi, or even if your request to connect to one failed, except to repeatedly check the IP address of the chip for when it changes. When it is not connected to any network, it is `0.0.0.0`.

It also can only connect to 2.5 GHz WiFi networks. Most routers broadcast both a 2.5 GHz and a 5 GHz version of the WiFi, so make sure that you connect to the 2.5 GHz version.

There is a chance that the KOI will sucessfully connect to the WiFi, but still give timeout errors whenever yout ry to use the AI functions. We're not entirely sure why this happens, but we think it's because the servers are in China which isn't known to have the fastest internet. There is currently no fix.

### Facial Recognition

It is possible to differentiate between different faces using the KOI, but you need an internet connection that can make moderately speedy connections to China. The facial recognition algorithm used by the KOI allows it to make a guess to the emotion, gender presentation, and age of the face, though it's usually not correct. The process for doing this is very language dependent so refer to the instructions for the language you are using.

Usually only a single sample of a given face is sufficient for it to start accurately recognizing them. Known faces are placed in groups by the system, and you must tell it what group the face is from in order for it too identify it correctly.

When you restart the bit it will clear the training data, but if you have a MicroSD card in it you can store the training data on it and then load it on following boots. Ensure that you store the training data that you generate so that you don't lose it.

### Facial Detection

If you don't need to know what face it is, but just where the face is, then you can use the offline facial detection system.

### Barcode & QR Reading

If you ever need to scan barcodes or QR codes, then you can use the KOI. This functionality does not need WiFi. When you scan a QR or barcode it will show a square around it with the text it contains above it, and we have not yet found a way to stop it. It can also scan April Tags, which allow you to figure out where the KOI is relative to the April Tag.

## Micro:bit Programming

The best way that I have found to control the KOI using a Micro:bit is through [Microsoft Makecode](https://makecode.microbit.org/). Once you create a new project, you have to add the KOI extension, which is helpfully called "koi". You can use it either in either of the three modes, though it is natively programmed in Typescript.

In order to initiate the connect to the KOI, you must run the command the `KOI init Tx pin <A> Rx pin <B>`. The documentation is really confusing on exactly what it means by `Tx` and `Rx`, so just set `<A>` to one of the pins you connected the data lines to, and set `<B>` to the other. If the connection doesn't work, then swap them. Normally you put this in the `on start` block, but I guess you could do it whenever. You must initialize the connection before you can issue any commands to it from the Micro:bit

### WiFi

In order to *try* to connect to WiFi, you run the `Join Ap <ssid> <password>` block. Naturally, you put the password of the WiFi you want to connect to in the `<password>` section. SSID stands for Service Set Identifier, but it's just a fancy word for the name of the WiFi.

Just as it says in the features documentation, there is no way to directly determine if you have successfully connected to the WiFi aside from continuously polling the IP address with `Wifi Get IP`. Refer to the features documentation for more information.

The KOI does have the ability to interact with an IoT protocol called MQTT, but as of present we do not have an MQTT broker so we can't use it.

### Facial Recognition

Once you get connected to the WiFi, you can start doing facial recognition. You can make the bit scan for a face using `KOI Cloud Face Recognize`. This will freeze the bit for a few seconds, however, so keep that in mind. Once it finishes, it will trigger whatever is in your `on Recognize Face <token> <sex> <age> <mask> <expression>` blocks, which gives you the face token.

You don't actually store the facial data anywhere when you're using block code, just the face token. Inside the event handler, you can use the token variable with `add face token <token> to Group <group> with name <name>` to teach it the face. Remember, you can have the faces in different groups, but you have to tell it what group a face you want to recognize is from; the recognition is not cross-group. The name that you give the face is the name that it will return to you when it tries to recognize the face.

In order to try to recognize the face that it scans, you can use the `search face token <token> in group <group>` block inside the event handler. Just as with adding faces, you use the token variable that the handler gives you. Once it finishes trying to recognize the face, it will trigger the `on Find Face <name> <confidence>` handler. Inside this, you can do whatever you want with the result. The name variable is self-explanetory, but the confidence variable is a scale of 0-1, which shows how sure the KOI is of what face it told you. Normally, you don't have to worry about it, but you could use, for example, to have the program simply say it couldn't recognize the given face if the confidence is too low.

### TTS

In theory, you just have the run the `TTS <message>` block while on WiFi, but this is untested.

### Barcode & QR Reading

To start, run either the `KOI BAR code` or `KOI QR code` blocks. Once it detects one, it will trigger either the `on Barcode code <code>` or `on QR code <link>` handlers.

TODO: Test and then add April Tag documentation.

## MicroPython Programming

As written earlier, you can access MicroPython either through the REPL or through the SD card. Refer to the [MicroPython Website](https://MicroPython.org/) for more information on the language generally, but if you know Python you shouldn't have too much trouble.

### WiFi

You can attempt to connect to WiFi using the command `wifi.joinap(str("apname"), str("password"))`, where you replace `apname` with the name of your WiFi network and `password` with the password of it. Just as with everything else, there is no way to know if you have successfully connected to the network except continuously polling the IP address with `wifi.ipaddr()`.

### Facial Recognition

Doing facial recognition in MicroPython is kind of complicated. The first thing that you have to do, whether you are adding a new face or attempting to recognize a face, is run `baiduFace(op=1)`. This takes a second or two and eventually returns an object containing the scanned facial data, if it sucessfully found a face. In this documentation we assume that you set a variable to this returned data, and call this variable `face`, but you can call it whatever you want.

You can get the face token from the variable using `face[face_token]`. This syntax can also be used to get the rest of the data of the face, including its location, gender, expression, angle, whether it's wearing a mask, and its age by replacing `face_token` with `location`, `gender`, `expression`, `angle`, `mask`, or `age` respectively.

You add a face to a group with the command `baiduFace(op=2, token=face['face_token'], group="group", name="name")`.

You can attempt to recognize a face with `baiduFace(op=3, token=face['face_token'], group="group")`. This will return an object containing both the name and confidence of what it thinks the face is.

TODO: Explain the structure of the returned object.

### TTS

The official documentation for the MicroPython interface does not mention TTS, and we are still trying to see if it is possible.

### Barcode & QR Reading

You can scan for barcodes with `findBarcode()` and QR codes with `findQRCode()`. They will return an object containing the scanned barcode or QR code, if it finds one.

TODO: Explain the structure of the returned object.

TODO: Test and then add April Tag documentation
