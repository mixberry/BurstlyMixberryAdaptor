BurstlyMixberryAdaptor - iOS
=======================


###Mixberry SDK

Download the [latest SDK here](network_site.com).

###Integration

####1- Download adaptor and SDK
Download the BurstlyMixberryAdaptor “BurstlyMixberryAdaptor.a” and Mixberry SDK “MB\_SDK\_iOS.a”, then drag and drop them into the root folder in your project.

####2- Add required Frameworks
Add the following frameworks to your project:

* AudioToolbox.framework
*  CoreGraphics.framework
*	SystemConfiguration.framework
*	QuartzCore.framework
*	CoreMedia.framework
*	AVFoundation.framework

Also insure that the following frameworks are already added to your project:

*	UIKit.framework
*	Foundation.framework

***Note:**
All the above frameworks are existing frameworks provided by Apple*

####3- Request Ad configuration
Mixberry-Burstly adaptor supports only Interstitial Ad, which is represented as a full screen Ad that contains a banner (300x250) combined with an audio Ad. 
The interstitial is shown on the screen as long as the audio ad is playing. When the audio ad ends, a close button will appear on the top right corner, which the user can click to close the ad. Otherwise, the ad will close automatically after 5 seconds.

Once the two files and the frameworks have been added to the project you can create an interstitial ad:

<pre><code>self.interstitial = [[[BurstlyInterstitial alloc]
initAppId:appId zoneId:interstitialZoneId delegate:self] autorelease];
</code></pre>

where appId is the application id and interstitialZoneId is the zone id assigned to Mixberry network; both are obtained from the SkyRocket (Burstly) portal.

Finally to show the ad use the showAd method:

<pre><code>[self.interstitial showAd];</code></pre>

Burstly Interstitial Ads can also be cached by setting the *useAutomaticCaching* flag to YES and display it later by calling showAd. 

***Note:**
Please make sure that interstitial ads are shown within 15 minutes of caching*

###Custom Parameters

Parameters are divided into two categories:

**1. Network parameters**

The set of necessary parameters added when the publisher creates a new network.


* **API Key (required):**<br/>
The API key is provided during the application registration process with Mixberry Media. The key must be unique for every application registered by the Publisher.
It is a required parameter that is used to initialize the Mixberry SDK and also to request ads from Mixberry servers.



  ***Note:**
The API key used by the Mixberry Media library to identify which application requested the Ad. Please make sure to use the correct API Key for each application that you registered.*

* **Should Play in Background (optional):**<br/>
Should Play in Background is an optional parameter; its value indicates whether the player should continue playing the ad when the application enters the background. It accepts the boolean values (YES and NO), the default value for this parameter is NO.

JSON example:
<pre><code>Mixberry?{"apiKey" :"API_KEY" ,"shouldPlayInBG" :"YES"}</code></pre>
where API_KEY is string obtained upon your application approval from Mixberry Media

**2. Custom parameters**

Mixberry Custom Parameters can be set using the following BurstlyAdRequest method:
<pre><code>- (void)setCustomParamsForNetwork:(NSString*)network
withDictionary:(NSDictionary*)params;</code></pre>
where *network* is the network name and must be passed as **@"mixberry"**, and *params* is an NSDictionary that holds configuration and request parameters that can be passed to the Mixberry SDK. All parameters are passed as string keys and string values.

The set of Mixberry Custom Parameters are divided into two groups

*  **Configuration parameters**<br/>
The configuration parameters represent requested ad specifications.

  * **adPeriod** <br/>
The requested Ad can be of different lengths, up to 30 seconds. The key value of adPeriod parameter should be passed as **@"adPeriod"**. You can choose the maximum Ad length to be one of the following:
  <pre><code>Short: 10 seconds
Normal: 20 seconds
Long: 30 seconds
  </code></pre>
***Note:**
If adPeriod is not specified, then an Ad of any length will be delivered.*
  * **Genre (Optional)** <br/>
  Music app Publishers can set the music genre parameter in order to match between the played song and the requested Ad. The key value of genre parameter should be passed as **@"genre"**. To do so, we provide publishers with various types of genres such as ROCK, COUNTRY, BLUES, FOLK, ELECTRONIC, SOUL / RnB…etc. For more information, please refer to Appendix A for the full list of supported genres.

*  **Targeted parameters**<br/>
The targeted parameters target a certain group of users according to the information available about those users. The targets parameters must be passed when the publisher sets these targets. 
  * **Language**<br/>
  The publishers can target the users according to their language. The key value of language parameter should be passed as **@"language"**. For more information about the accepted language values, please refer to Appendix A.
  * **Gender**<br/>
  The publishers can target the users according to their gender. The key value of gender parameter should be passed as **@"gender"**. It accepts one of two values: (Male, Female).
  * **Age Group**<br/>
  The publishers can target the users according to their age. The key value of age parameter should be passed as **@"age"**. It accepts any value greater than zero.
  
  ***Note:**<br/> There is no need to set the targeted parameters in each request, its enough to set them once. <br/>
The SDK uses a caching mechanism for the user info to be used by any subsequent Ad request.*

  Set parameters sample:
  <pre><code>NSDictionary *param = [NSDictionary dictionaryWithObjectsAndKeys:@"26",
@"age",@"fr",@"language",@"Short",@"adPeriod",nil];
[self.interstitial.adRequest setCustomParamsForNetwork:@"mixberry" 
withDictionary: param];
  </code></pre>

### Appendix A: Parameter Values
**Gender** <br/>
Represents the user’s gender and takes one of the following values:
<pre><code>Male
Female </code></pre>

**Language** <br/>
Passed language values must be in ISO: 639-1 standard codes. The following table represents the supported language values and their ISO: 639-1 codes:

<pre><code>Arabic    ar               Greek        el                    Icelandic   is
Spanish   es               Portuguese   pt                    Malay       ms
English   en               Filipino     tl                    Urdu        ur
French    fr               Hindi        hi                    Chinese     zh
Danish    da               Finnish      fi                    Hungarian   hu
Persian   fa               Hebrew       he                    Indonesian  id
Italian   it               Japanese     ja                    Dutch       nl
Polish    pl               Russian      ru                    Norwegian   no
Korean    ko               Swedish      sv                    Zulu        zu
Thai      th
</code></pre>

**Music Genre** <br/>
The following list shows all supported music genre types:
<pre><code>ROCK                                  COUNTRY                   NEW_AGE
BLUES                                 FOLK                      ELECTRONIC
SOUL_RNB                              OCEANIA                   POP
JAZZ                                  EUROPE                    REGGAE
HIP_HOP                               AFRICA                    LATIN
CLASSICAL_EARLY_COMMON_PRACTICE       ENTERTAINMENT             METAL
CLASSICAL_MODERN_CONTEMPORARY         MIDDLE_EAST
</code></pre>

