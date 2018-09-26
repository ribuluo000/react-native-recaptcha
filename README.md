# react-native-recaptcha
A react native wrapper for google recaptcha v3

## Installation
```
npm install --save react-native-recaptcha-v3
```

## Usage
```
<ReCaptcha {...props} />
```

### Props

* `containerStyle` An object that specifies the display style for the reCaptcha badge.

* `siteKey` A string representing the siteKey provided in the Google reCaptcha admin console.

* `url` URL associated with the app (This is the domain url that you registered on Google Admin Console when getting a siteKey)

* `action` A string representing the ReCaptcha action (Refer to the ReCaptcha v3 document)

* `reCaptchaType`: Currently two types of reCaptchas are supported:
  * `invisible`: Invisible reCaptcha do not require the users to solve a challenge. Refer to the reCaptcha V3 documentation for further information
  * `normal`: Normal reCaptcha may often require the user to click on a "I am not a robot" checkbox and solve a challenge (reCaptcha V2) - NOTE: This is meant to be used only with the firebase projects since firebase doesn't yet support reCaptcha v3.
  
* `config`: Firebase project config found in the firebase console. This prop is only required when using the normal reCaptcha

* `onExecute` A function to handle the response of ReCaptcha. Takes in a parameter that represents the
response token from the ReCaptcha.

### Contribution

Feel like contribution to this repository? The steps are simple:
* Fork the repository
* Make the changes you'd like to see
* Create a PR and wait for it to be approved by two people before merging

#### Thank-you for using `react-native-recaptcha-v3` 😀 Feel free to also leave any feedback or change requests you may have.


### demo
```
/**
 * Created by nick.
 */
import React, { PureComponent } from "react";

import ReCaptcha from "react-native-recaptcha-v3";

export default {

  get_re_captcha: (callback = (re_captcha) => {
    console.log('get_re_captcha:', re_captcha);
  }) => {
    //https://github.com/facebook/react-native/issues/19986 webview
    const re_captcha_props = {
      url: config.re_captcha_url,
      reCaptchaType: 1,
      action: 'verify',
      siteKey: config.re_captcha_site_key,
      onExecute: (result) => {
        console.log('re_captcha_props.onExecute', result);
        callback(result);
      },
      onReady: () => {
        console.log('re_captcha_props.onReady');
      },
    };

    return (
      <ReCaptcha
        {...re_captcha_props} />
    );
  },

};


```


```
/**
 * Created by nick on 2018/7/22.
 */
import React, { PureComponent } from "react";
import PropTypes from "prop-types";
import { ScrollView, StyleSheet, Text, View } from "react-native";
import T from "src/style/T";
import y_recaptche_util from "../../src/util/y_recaptche_util";


class SimpleScreen extends PureComponent {

    constructor(props) {
        super(props);

        this.state = {
          is_getting_re_captcha:false,
        };
    }

    onPress_re_captcha_invisible = ()=>{
      this.setState({
        is_getting_re_captcha:true,
      });

    };

    on_get_re_captcha = (re_captcha)=>{
      alert(re_captcha);
      console.log('re_captcha:',re_captcha)
      this.setState({
        is_getting_re_captcha:false,
      });
    };

    render() {

        return (
            <View style={T.CS.container}>
                <ScrollView>

                    <Text style={[ styles.welcome, {
                        backgroundColor : 'blue',
                    } ]}
                          onPress={this.onPress_re_captcha_invisible}
                    >
                        谷歌验证码
                    </Text>

                    {this.state.is_getting_re_captcha &&y_recaptche_util.get_re_captcha(this.on_get_re_captcha)}

                </ScrollView>
            </View>
        );
    }

}

export default SimpleScreen;
```

#### config:
```
  re_captcha_site_key:'your key',
  re_captcha_url:'http://127.0.0.1/',
```

