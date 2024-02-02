# Setup Development Environment 
## Normal Process
Before starting development, you need to set up the API Key. Please define the API Key provided by Nothing in the AndroidManifest.xml file of your application, within the <application> tag. You can do it in the following way:
`<meta-data android:name="NothingKey" android:value="{API Key}"/>`
## Debug Process
No need to set up the true API Key. You just need to setup a test key in your AndroidManifest.xml
`<meta-data android:name="NothingKey" android:value="test"/>`

# Setup Instructions
1. Only foreground applications are allowed to be used.
2. Developers can turn on debugging through the adb command. Allow 48hrs for adjustment after enabled.
  1. adb command: adb shell settings put global nt_glyph_interface_debug_enable 1
3. After enabled, there will be a notification to remind you of the usage phase of the app.

## SDK Integration
1. There are several key points to note when integrating the Glyph SDK:
  1. The Glyph SDK primarily operates Glyph through a system proxy service. Please ensure that GlyphManager's init is called at the beginning.
  2. The SDK needs to be registered before it can be used. Call the register function to grant permission for SDK usage.
  3. For each use of Glyph, it is important to open a session using openSession. When not using Glyph, be sure to close the session to release resources.
2. You can refer to Example 1 as a reference, but the overall management of the SDK usage cycle can be adjusted according to your needs.
3. Limitation: Only can work on Android version 14 of Nothing Phone.

# API 
## PackageName: com.nothing.ketchum
### AndroidManifest.xml
Add two segments to AndroidManifest.xml
1. Enable the permission: com.nothing.ketchum.permission.ENABLE
`<uses-permission android:name="com.nothing.ketchum.permission.ENABLE"/>`
2. Add API Key
`<!-- This tag should be added in application tag -->`
`<meta-data android:name="NothingKey" android:value="{Your APIKey}" />`
## Common
Help to identify Nothing phone models.

| Constant  | |
| ------------ | ------------ |
| String  | DEVICE_20111  |
| String  | DEVICE_22111  |

| Methods   | |
| ------------ | ------------ |
| boolean  | **is20111()** <br> Whether it is Phone1 |
| boolean  | **is22111()** <br> Whether it is Phone2 |
## Glyph
How the Glyph Interface is indexed
### Phone1
| Constants  |   | ArrayIndex  |
| ------------ | ------------ | ------------ |
| int | A1  |  0  |
| int  | B1  | 1  |
| int  | C1 ~ C4  | 2 ~ 5  |
| int  | D1_?<br> ?: 1 ~ 8 <br> Where D1_1 is the bottom and D1_8 is the top  | 7 ~ 14  |
| int  | E1  | 6 |

### Phone2
| Constants  |   | ArrayIndex  |
| ------------ | ------------ | ------------ |
| int | A1  |  0  |
| int  | A2  | 1  |
| int  | B1 | 2  |
| int  | C1_? <br> ?: 1 ~ 16 <br> Where C1_1 is bottom right and C1_16 is top left of C1  | 3 ~ 18  |
| int  | C2 ~ C6  | 19 ~ 23 |
| int  | D1_? <br> ?: 1 ~ 8<br> Where D1_1 is the bottom and D1_8 is the top   |  25 ~ 32   |
| int   |  E1   |  24   |

## GlyphFrame
To indicate the Glyph to be used.

| Methods  |   |
| ------------ | ------------ |
| int  | **getPeriod()** <br> Get the duration of the GlyphFrame is to be turned on, measured in milliseconds.  |
| int  | **getCycles()** <br> Get number of cycles the GlyphFrame is to be turned on for. |
| int  | **getInterval()** <br>Get the interval between cycles.   |
| int[]  | **getChannel()** <br> Get the array of Glyph channels that are included in the GlyphFrame.  |

## GlyphFrame.Builder
To create the group of Glyphs to be used.

| Constructor   |
| ------------ |
| **GlyphFrame.Builder()** <br> Create this class. Default adapter 22111.  |
|  **GlyphFrame.Builder(String device)** <br> Create this class. Select the device to adapt. |

| Methods   |   |
| ------------ | ------------ |
| GlyphFrame.Builder  | **buildPeriod(int period)** <br> Set the duration of the GlyphFrame is to be turned on, measured in milliseconds.  |
| GlyphFrame.Builder   | **buildCycles(int cycles)** <br> Set the number of cycles the GlyphFrame is to be turned on for.  |
| GlyphFrame.Builder   | **buildInterval(int interval)** <br> Set the interval between cycles.   |
| GlyphFrame.Builder  | **buildChannel(int channel)** <br> Set the Glyph to be used. Please refer to the Glyph class above.    |
| GlyphFrame.Builder  | **buildChannel?()** <br> ?: A ~ E <br> Quickly set a zone of Glyphs to be used. Please refer to the Glyph class above.   |
| GlyphFrame  | **build()** <br> Create the instance of the GlyphFrame.   |

## GlyphManager
Used to control the Glyph Interface.

| Static Methods  |
| ------------ |
| **getInstance(Context context)** <br> Obtaining an instance of GlyphManager.   |

| Methods  |   |
| ------------ | ------------ |
| void  | **init(GlyphManager.Callback callback)** <br> Used for binding the Service. It is recommended to be created in the onCreate method.  |
| void   | **unInit()** <br> Used for unbinding the Service. It is recommended to be used in the onDestroy method.   |
| boolean   | **register()** <br> Used to verify if this application is authorized to use this SDK. If the application does not make the call, it will fail.  |
| boolean  | **register(String targetDevice)** <br> Used to verify if this application is authorized to use this SDK. If the application does not make the call, it will fail and mark the adapted device.  |
| GlyphFrame.Builder  | **getGlyphFrameBuilder()** <br> Get the GlyphFrame.Builder currently to be adapted.  |
| void   | **openSession()** <br> Used to open a session for controlling Glyph. Please call it only after successfully calling onServiceConnected(ComponentName componentName).  |
| void  | **closeSession()** <br> Used to close the session that was originally used to control Glyph.  |
|void   | **toggle(GlyphFrame frame)** <br> Used to enable/disable the channels set in the GlyphFrame.  |
|void   | **animate(GlyphFrame frame)** <br> Used for animating a breathing animation using the parameters of channels, period, and interval set in the GlyphFrame.  |
| void  | **displayProgress(GlyphFrame frame, int progress)** <br> Used to display the progress of the light strips for C1/D1. <br> Phone (1), limited to D1 only. Phone(2), limited to C1/D1.  |
| void   | **displayProgress(GlyphFrame frame, int progress, bool reverse)** <br> Used to display the progress of the light strips for C1/D1. Phone (1), limited to D1 only. Phone(2), limited to C1/D1.  |
| void   | **displayProgressAndToggle(GlyphFrame frame, int progress, boolean isReverse)** <br>Used to simultaneously turn on light strips other than C1/D1 and display the progress of C1/D1’s light strips. Phone (1), limited to D1 only. Phone(2), limited to C1/D1.  |

## GlyphManager.Callback
Used to verify if GlyphManager has successfully connected to the Service.

| Interface   |   |
| ------------ | ------------ |
| void  | **onServiceConnected(ComponentName componentName)** <br> Mainly used to inform GlyphService about a successful connection.  |
| void  | **onServiceDisconnected(ComponentName componentName)** <br> Mainly used to inform GlyphService about a failed connection or disconnection.  |

## Example 1

```
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
                if (Common.is20111()) mGM.register(Common.DEVICE_20111);
                if (Common.is22111()) mGM.register(Common.DEVICE_22111);
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
    }
}
```
## Example 2
We are trying to turn on B1 and C1_4, then A2 and C1_2. When toggling frame2, frame1 will be toggled off.
```
GlyphFrame.Builder builder = mGM.getGlyphFrameBuilder();
GlyphFrame frame1 = builder.buildChannel(Glyph.B1).buildChannel(Glyph.C1_4).build();
mGM.toggle(frame1);
GlyphFrame frame2 = builder.buildChannel(Glyph.A2).buildChannel(Glyph.C1_2).build();
mGM.toggle(frame2);
```

