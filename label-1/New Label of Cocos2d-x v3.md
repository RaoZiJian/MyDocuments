#New Label of Cocos2d-x v3.0
---

##What's new of Label in Cocos2d-x v3.0?

* Deprecated `CCLabelTTF`, `CCLabelBMFont`, `CCLabelAtlas`. There is only one class `Label` since Cocos2d-x v3.0.
* GLow, shadow and outline effects supported.
* Using `libfreetype2`to render system font. It's faster and more efficient. Furthermore, fonts will look the same on all supported platforms.

##How to use new Label?

#### Create with SystemFont

`createWithSystemFont()` will call the native API by the platform-dependent code. The first 3 parameters are necessaries and the rest would have a default value if they were omitted.

```
    /** Creates a label with an initial string,font[font name or font file],font size, dimension in points, horizontal alignment and vertical alignment. **/
    
    static Label* createWithSystemFont(const std::string& text, const std::string& font, float fontSize, const Size& dimensions = Size::ZERO, TextHAlignment hAlignment = TextHAlignment::LEFT, TextVAlignment vAlignment = TextVAlignment::TOP);
```

![Mou icon](1.png)   
#### Create with TTF
`CreateWithTTF()` uses `libfreetype2` to render the fonts. It would save each char of the strings to the cache that improve the speed of rendering fonts. There are two ways to create a LabelTTF belows:

```
//First way to create a LabelTTF by TTF files
auto label = Label::createWithTTF("label test","fonts/Marker Felt.ttf",32);
label->setPosition(Point(size.width/2,size.height*0.6));
this->addChild(label);     
    
//Second way to create a LabelTTF by TTFconfig
TTFConfig label_config;
//Set the TTF file of TTFConfig, this attribute should not be omitted
label_config.fontFilePath = "fonts/Marker Felt.ttf";
label_config.fontSize = 32;
//Choose the glyhps, which includes DYNAMIC, NEHE, ASCII and CUSTOM now
label_config.glyphs = GlyphCollection::DYNAMIC;
//Set the customGlyphs to null since here we don't use the custom glyphs.
label_config.customGlyphs = nullptr;
//Open or not open distanceField
label_config.distanceFieldEnabled = false;
label_config.outlineSize = 0;
//create the LabelTTF through the label config
auto label_two = Label::createWithTTF(label_config, "label test");
label_two->setPosition(Point(size.width/2,size.height*0.5));
this->addChild(label_two);
```

To know what is `distanceField`， read this paper [Distance-field-fonts](https://github.com/libgdx/libgdx/wiki/Distance-field-fonts)

![image](distance-field-fonts.png)


#### Create with BMFont
To create a label with FNT file, we have `createWithBMFont()`

```
//Create a BMFont label, the first parameter is the fnt file, seconde is your text. The chars of your string should be included in your fnt files, or it can't display nomally.

auto bmfont = Label::createWithBMFont("fonts/gameover_score_num.fnt", "123456789");
```


#### Create with CharMap
The `createWithCharMap()` represents the `CCLabelAtlas`in the past. It has a simpler rule compare to the BMFont. It is common used in the number fonts. We have 3 overrided functions in Cocos2d-x v3.0 above to create with char map.

```  
	// 3 ways to create with char map, according to the 'CCLabel.h'
    static Label * createWithCharMap(const std::string& charMapFile, int itemWidth, int itemHeight, int startCharMap);
    static Label * createWithCharMap(Texture2D* texture, int itemWidth, int itemHeight, int startCharMap);
    static Label * createWithCharMap(const std::string& plistFile);

//create a label with char map.
//parameters: PNG picture, width, height, the first char.
auto charMap = Label::createWithCharMap("fonts/tuffy_bold_italic-charmap.png", 48, 64, ' ');
charMap->setPosition(Point(size.width/2,size.height*0.4));
charMap->setString("123456789");
this->addChild(charMap);
    
//You can also create the char map by a plist
auto charMap2 = Label::createWithCharMap("fonts/tuffy_bold_italic-charmap.plist");
charMap2->setPosition(Point(Point(size.width/2,size.height*0.3)));
charMap2->setString("123456789");
this->addChild(charMap2); 
```  
![Mou icon](3.png)   

####GLow, shadow and outline effects

```
//Outline effect, only support for TTF
label_two->enableOutline(Color4B(255,0,0,255),5);

//Glow effect,only support for TTF and the distanceFieldEnabled must be true
label_three->enableGlow(Color4B(255,0,0,255));

//Shadow effect
label_four->enableShadow(Color4B(0,0,255,255),Size(3,10),0);
```