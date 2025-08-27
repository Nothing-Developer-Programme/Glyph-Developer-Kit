# Setup Development Environment 

## Normal Process
Before starting development, you need to set up the API Key. Define the API Key provided by Nothing in the `AndroidManifest.xml` file of your application, within the `<application>` tag. Here is how to do it:

```xml
<meta-data android:name="NothingKey" android:value="{API Key}"/>
```
Be sure to replace {API Key} with the actual API key that has been provided to you.

## Debug Process
If you are in the debug phase, there is no need to set up the true API Key. Instead, use a test key in your AndroidManifest.xml like this:
```xml
<meta-data android:name="NothingKey" android:value="test"/>
```

## Setup Instructions
1. Only foreground applications are allowed to be used.
2. Debugging can be enabled via adb command.
```bash
adb shell settings put global nt_glyph_interface_debug_enable 1
```
   - Debugging mode will be automatically disabled after 48 hours to prevent misuse of the SDK. 

3. Once successfully activated, you will receive a notification.

## SDK Integration
1. There are several key points to note when integrating the Glyph SDK:
   - Glyph SDK primarily operates Glyph through a system proxy service. Please ensure that GlyphManager's init is called at the beginning.
   -  The SDK needs to be registered before it can be used. Call the ```register()``` function to grant permission for SDK usage.
   -  For each use of Glyph, it is important to open a session using ```openSession()```. When not using Glyph, be sure to close the session to release resources.
2. You can refer to Example 1 as a reference, but the overall management of the SDK usage cycle can be adjusted according to your needs.
3. Limitation: This SDK only works on Nothing devices running Android version 14 or newer.

# API 
## PackageName: com.nothing.ketchum
### AndroidManifest.xml
Add two segments to AndroidManifest.xml
1. Enable the permission: com.nothing.ketchum.permission.ENABLE <br>
```xml
<uses-permission android:name="com.nothing.ketchum.permission.ENABLE"/>
```
2. Add API Key <br>
```xml
<!-- This tag should be added in application tag -->
<meta-data android:name="NothingKey" android:value="{Your APIKey}" />
```
## Common
Help to distinguish between smartphone model.

| Methods   | |
| ------------ | ------------ |
| boolean  | ```is20111()``` <br> Whether model is Phone (1)       |
| boolean  | ```is22111()``` <br> Whether model is Phone (2)       |
| boolean  | ```is23111()``` <br> Whether model is Phone (2a)      |
| boolean  | ```is23113()``` <br> Whether model is Phone (2a) Plus |
| boolean  | ```is24111()``` <br> Whether model is Phone (3a) and Phone (3a) Pro |
## Glyph
How the Glyph Interface is indexed

| Constants  | |
| ------------ | ------------ |
| String  | ```DEVICE_20111```  |
| String  | ```DEVICE_22111```  |
| String  | ```DEVICE_23111```  |
| String  | ```DEVICE_23113```  |
| String  | ```DEVICE_24111```  |

### Nothing Phone (1)
| Constants  |   | ArrayIndex  |
| ------------ | ------------ | ------------ |
| int | A1  |  ```0```  |
| int  | B1  | ```1```  |
| int  | C1 - C4  | ```2 - 5```  |
| int  | D1_1 - D1_8 <br> Where D1_1 is the bottom and D1_8 is the top  | ```7 - 14```  |
| int  | E1  | ```6``` |


![img_v3_027r_aed999af-858f-47c0-8f92-dc3c837ba1ah](https://github.com/Nothing-Developer-Programme/Glyph-Developer-Kit/assets/56658376/16eb0b53-ce08-4275-97c4-26c82f18299f)


### Nothing Phone (2)
| Constants  |   | ArrayIndex  |
| ------------ | ------------ | ------------ |
| int | A1  |  ```0```  |
| int  | A2  | ```1```  |
| int  | B1 | ```2```  |
| int  | C1_1 - C1_16 <br> Where C1_1 is bottom right and C1_16 is top left of C1  | ```3 - 18```  |
| int  | C2 - C6  | ```19 - 23``` |
| int  | D1_1 - D1_8 <br> Where D1_1 is the bottom and D1_8 is the top   |  ```25 - 32```   |
| int   |  E1   |  ```24```   |


![img_v3_027r_a4ecc24c-68b7-454d-b78c-67b3bdc966fh](https://github.com/Nothing-Developer-Programme/Glyph-Developer-Kit/assets/56658376/f576177c-9e15-4628-bf78-906509bb3673)

### Nothing Phone (2a) and Phone (2a) Plus
| Constants  |   | ArrayIndex  |
| ------------ | ------------ | ------------ |
| int | A  |  ```25```  |
| int | B  |  ```24```  |
| int | C1 - C24 <br> Where C_1 is bottom left and C_24 is top right of C |  ```0 - 23```  |

![Frame 7](https://github.com/Nothing-Developer-Programme/Glyph-Developer-Kit/assets/56658376/8720123a-e88e-4958-96f3-2582b0dee590)

### Nothing Phone (3a) and Phone (3a) Pro
| Constants  |   | ArrayIndex  |
| ------------ | ------------ | ------------ |
| int | A1 - A11 <br> Where A_1 is top and A_11 is bottom of A |  ```20 - 30```  |
| int | B1 - B5 <br> Where B_1 is bottom right and B_5 is top left of B |  ```31 - 35```  |
| int | C1 - C20 <br> Where C_1 is bottom left and C_20 is top right of C |  ```0 - 19```  |

![image](https://github.com/user-attachments/assets/8bd3950a-8c12-4df3-bfb4-8a5d94c1b047)











## GlyphFrame
To indicate the Glyph to be used.

| Methods  |   |
| ------------ | ------------ |
| int  | ```getPeriod()``` <br> Get the duration of the GlyphFrame is to be turned on, measured in milliseconds.  |
| int  | ```getCycles()``` <br> Get number of cycles the GlyphFrame is to be turned on for. |
| int  | ```getInterval()``` <br>Get the interval between cycles.   |
| int[]  | ```getChannel()``` <br> Get the array of Glyph channels that are included in the GlyphFrame.  |

## GlyphFrame.Builder
To create the group of Glyphs to be used.

| Constructor   |
| ------------ |
| ```GlyphFrame.Builder()``` <br> Create the GlyphFrame class. Default value is 22111.  |
| ```GlyphFrame.Builder(String device)``` <br> Create the GlyphFrame class. Select between the other devices. |

| Methods   |   |
| ------------ | ------------ |
| GlyphFrame.Builder  | ```buildPeriod(int period)``` <br> Set the duration of the GlyphFrame is to be turned on, measured in milliseconds.  |
| GlyphFrame.Builder   | ```buildCycles(int cycles)``` <br> Set the number of cycles the GlyphFrame is to be turned on for.  |
| GlyphFrame.Builder   | ```buildInterval(int interval)``` <br> Set the interval between cycles.   |
| GlyphFrame.Builder  | ```buildChannel(int channel)``` <br> Set the Glyph to be used. Please refer to the Glyph class above.    |
| GlyphFrame.Builder  | ```buildChannel?()``` <br> ?: A - E <br> Quickly set a zone of Glyphs to be used. Please refer to the Glyph class above.   |
| GlyphFrame  | ```build()``` <br> Create the instance of the GlyphFrame.   |

## GlyphManager
Used to control the Glyph Interface.

| Static Methods  |
| ------------ |
| ```getInstance(Context context)``` <br> Obtaining an instance of GlyphManager.   |

| Methods  |   |
| ------------ | ------------ |
| void  | ```init(GlyphManager.Callback callback)``` <br> Used for binding the Service. It is recommended to be called in the onCreate method.  |
| void   | ```unInit()``` <br> Used for unbinding the Service. It is recommended to be called in the onDestroy method.   |
| boolean   | ```register()``` <br> Used to verify if this application is authorized to use this SDK. If the application does not make the call, it will fail.  |
| boolean  | ```register(String targetDevice)``` <br> Used to verify if this application is authorized to use this SDK. If the application does not make the call, it will fail and mark the adapted device.  |
| GlyphFrame.Builder  | ```getGlyphFrameBuilder()``` <br> Get the GlyphFrame.Builder currently to be adapted.  |
| void   | ```openSession()``` <br> Used to open a session for controlling the Glyph Interface. Only to be called after successfully calling **onServiceConnected(ComponentName componentName)**.  |
| void  | ```closeSession()``` <br> Used to close the session that was originally used to control Glyph.  |
|void   | ```toggle(GlyphFrame frame)``` <br> Used to enable/disable the channels set in the GlyphFrame.  |
|void   | ```animate(GlyphFrame frame)``` <br> Used for animating a breathing animation using the parameters of channels, period, and interval set in the GlyphFrame.  |
| void  | ```displayProgress(GlyphFrame frame, int progress)``` <br> Used to display a progress value on C1 / D1. <br> Limited to D1 only for Phone (1).  | |
| void   | ```displayProgress(GlyphFrame frame, int progress, bool reverse)``` <br> Used to display the progress value on C1 / D1. <br> Limited to D1 only for Phone (1).  ||
| void   | ```displayProgressAndToggle(GlyphFrame frame, int progress, boolean isReverse)``` <br>Used to simultaneously toggle all Glyphs except C1 / D1 and display the progress value on C1 / D1. Limited to D1 only for Phone (1).  | 
| void  | ```turnOff()``` <br> Used to turn off any showing glyph. |


## GlyphManager.Callback
Used to verify if GlyphManager has successfully connected to the Service.

| Interface   |   |
| ------------ | ------------ |
| void  | ```onServiceConnected(ComponentName componentName)``` <br> Used to inform GlyphService about a successful connection.  |
| void  | ```onServiceDisconnected(ComponentName componentName)``` <br> Used to inform GlyphService about a failed connection or disconnection.  |

## Example 1

```java
public class MainActivity extends AppCompatActivity {
    private GlyphManager mGM = null;
    private GlyphManager.Callback mCallback = null;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        init();
        mGM = GlyphManager.getInstance(getApplicationContext());
        mGM.init(mCallback);
    }

    @Override
    public void onDestroy() {
        try {
            mGM.closeSession();
        } catch (GlyphException e) {
            Log.e(TAG, e.getMessage());
        }
        mGM.unInit();
        super.onDestroy();
    }
    
    private void init() {
        mCallback = new GlyphManager.Callback() {
            @Override
            public void onServiceConnected(ComponentName componentName) {
                if (Common.is20111()) mGM.register(Glyph.DEVICE_20111);
                if (Common.is22111()) mGM.register(Glyph.DEVICE_22111);
                if (Common.is23111()) mGM.register(Glyph.DEVICE_23111);
                if (Common.is23113()) mGM.register(Glyph.DEVICE_23113);
                if (Common.is24111()) mGM.register(Glyph.DEVICE_24111);
                try {
                    mGM.openSession();
                } catch(GlyphException e) {
                    Log.e(TAG, e.getMessage());
                }
            }
            @Override
            public void onServiceDisconnected(ComponentName componentName) {
                mGM.closeSession();
            }
        };
    }
    
    private void initView() {
        channelABtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                GlyphFrame.Builder builder = mGM.getGlyphFrameBuilder();
                GlyphFrame frame = builder.buildChannelA().build();
                mGM.toggle(frame);
            }
        });
        channelBBtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                GlyphFrame.Builder builder = mGM.getGlyphFrameBuilder();
                GlyphFrame frame = builder.buildChannelB().buildInterval(10)
                        .buildCycles(2).buildPeriod(3000).build();
                mGM.animate(frame);
            }
        });
        channelCBtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                GlyphFrame.Builder builder = mGM.getGlyphFrameBuilder();
                GlyphFrame frame = builder.buildChannelC().build();
                mGM.animate(frame);
            }
        });
        channelDBtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                mGM.turnOff();
            }
        });
    }
}
```
## Example 2
We are trying to turn on B1 and C1_4, then A2 and C1_2. When toggling frame2, frame1 will be toggled off.
```java
GlyphFrame.Builder builder = mGM.getGlyphFrameBuilder();

GlyphFrame frame1 = builder.buildChannel(Glyph.B1).buildChannel(Glyph.C1_4).build();
mGM.toggle(frame1);

GlyphFrame frame2 = builder.buildChannel(Glyph.A2).buildChannel(Glyph.C1_2).build();
mGM.toggle(frame2);
```

## Engines, Programming Frameworks & Languages
#### Game Engines:
- [NothingOS.Unity](https://github.com/am1goo/NothingOS.Unity) - this SDK for Unity Engine provides native APIs for all of the essential Glyph features on Nothing Phone.
#### Frameworks
- [Nothing Glyph Interface for Flutter](https://pub.dev/packages/nothing_glyph_interface) - Flutter implementation of the Glyph Developer Kit from Nothing.
