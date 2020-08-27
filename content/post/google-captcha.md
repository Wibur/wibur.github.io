+++
author = "Vancer"
title = "Google reCaptcha V3 使用"
date = "2020-07-21"
description = "安全相關"
tags = [
    "Google",
]
categories = [
    "安全",
]
series = ["Themes Guide"]
aliases = ["migrate-from-jekyl"]
+++

###### tags: `功能` `Google`

## 用途
一定程度防止機器人，V2版本為使用者熟悉的需要點選圖形驗證碼來證明自己不是機器人，但這樣會降低使用者感受;因此推出了V3版本，在網站後端紀錄使用者網站瀏覽的行為過程；模式是以評分方式，
區間為0.1~1，分數越低越可能是機器人，因此網站管理員可以針對分數門檻作驗證程序開啟。

## 使用方式
1. 註冊網站的reCAPTCHA => ( https://www.google.com/recaptcha/admin)
2. 會由Google給出特定的網站金鑰(前端用)及密鑰(後端用)
![](https://i.imgur.com/6gI8MXN.png)
3. 操控設定可以在裡面添加網域，沒添加網站會報錯喔
![](https://i.imgur.com/HygqRty.png)

4. 前端
```javascript=
<script src="https://www.google.com/recaptcha/api.js?render=[你的網站金鑰]"></script>

/* 登入前驗證(機器人 || 圖型) */
    async captcha() {
      this.loading = true
      const reCaptchaSiteKey = '你的網站金鑰'

      const captchaHasReady = () => new Promise(resolve => grecaptcha.ready(() => resolve()))

      try {
        await captchaHasReady()
        const token = await grecaptcha.execute(reCaptchaSiteKey, { action: 'contact' })
        const resp = await LoginCheck.apiReCaptcha({ 'token': token })
        if (!resp.status) {
          throw new Error(resp)
        }
        if (!resp.allow) {
          // 開啟圖形驗證
          this.getImg()
        } else {
          this.openLogin = true
        }
      } catch (resp) {
        this.$bus.$emit('showErrorMsg', resp, false)
        // 啟用倒數計時器
        this.setTimerByErrorMessage(resp.data.msg)
      } finally {
        this.loading = false
      }
    },
```
5. 後端
```php=
/**
     * Google ReCaptcha 驗證
     * @param Request $request
     * @return \Illuminate\Http\JsonResponse
     */
    public function captcha(Request $request)
    {
        $data = [];
        // 收到前端送來的token，再結合secrect key戳API
        $url = 'https://www.google.com/recaptcha/api/siteverify';
        $secrect = '前面圖的密鑰';
        $token = $request->input('token') ?? '';        
        $url .= '?secret=' . $secrect . '&response=' . $token;
        $response = file_get_contents($url);
        $response = json_decode($response);
        
        // 依據分數作相對應操作
        if ($response->success) {
            if ($response->score >= 0.9) {
                $data = [
                    'status' => $response->success,
                    'allow' => true,
                    'score' => $response->score
                ];
            } else {
                // 驗證失敗 ， 回傳前端 ， 讓其作後續驗證
                $data = [
                    'status' => $response->success,
                    'allow' => false,
                    'score' => $response->score
                ];
            }
        } else {
            $data = [
                'status' => $response->success,
                'err' => $response->error - codes,
                'score' => ''
            ];
        }
    }
```

5. 成功後，後台可以開始看到統計
![](https://i.imgur.com/Ruz2Bsk.png)
![](https://i.imgur.com/jjyDGAi.png)

## 參考資料
[reCAPTCHA v3 API文檔](https://developers.google.com/recaptcha/docs/v3)