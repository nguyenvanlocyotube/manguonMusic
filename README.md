
<!doctype html><html lang="en"><head><meta charset="utf-8"><link rel="shortcut icon" href="/favicon.ico"><meta name="viewport" content="width=device-width,initial-scale=0.3,shrink-to-fit=no,user-scalable=yes"><meta name="theme-color" content="#000000"><meta http-equiv="X-UA-Compatible" content="IE=Edge"/><script>// 历史逻辑，处理shopeexpress->spx的域名跳转
      const hostname = window.location.hostname;
      if (hostname.indexOf('shopeexpress') !== -1 || hostname.indexOf('spx') !== -1) {
        window.region = hostname.substr(-2).toUpperCase();
      }
      // BR市场由于域名只有shopeexpress，不作跳转处理
      if (window.region !== 'BR' && window.location.origin.indexOf('shopeexpress') !== -1) {
        const origin = window.location.origin.replace('shopeexpress', 'spx');
        window.location.href = `${origin}/${window.location.hash}`;
      }</script><script>window.__APP_VERSION__ = 'v20240924-1855-VN-LIVE';
      !function(t,e){"object"==typeof exports&&"undefined"!=typeof module?e(exports):"function"==typeof define&&define.amd?define(["exports"],e):e((t="undefined"!=typeof globalThis?globalThis:t||self)["tracing-entrance-tracker"]={})}(this,(function(t){"use strict";const e=t=>localStorage.getItem(t);const a="entrance_tracker_invalid_case";class i{_reporter;_endStage="js_init_end";_reported=!1;_logger=!1;_currentStage="html_init_start";_maxWaitTimer=null;_xhrOptions={};_beforeSend;_maxWait=13e3;_visiblechangeCount=0;_startTime=performance.now();_prevStartedTime=performance.now();_prevStageTime=performance.now();_contextData={data:{invalid_case:0,visible_tab:1}};constructor(t,e){this._reporter=t;const a=e||{};this._logger=!!a.logger,this._xhrOptions=a.xhrOptions||{},this._beforeSend=a.beforeSend,this._maxWait=a.maxWait&&a.maxWait>0?a.maxWait:this._maxWait}_logInfo(...t){const e=this._logger;if(!e)return;const a="string"==typeof e&&"function"==typeof window.console[e]?e:"info";window.console[a](...t)}_getDuration(t){return+(performance.now()-t).toFixed(2)}entryStageReport(t){if(this._reported)this._logInfo("[初始化成功率上报] already reported! t: ",performance.now()-this._startTime);else try{const{data:e={},...a}=this._contextData,i={type:"numeric",key:"app_init_stage",value:this._getDuration(this._startTime),...a,data:{stage:this._currentStage,visiblechange_count:this._visiblechangeCount,...e,...t}},r=this._beforeSend?this._beforeSend(i,this.getBaseData()):i;if(!1===r)return;this._reporter.sendData(r,this._xhrOptions),this._reported=!0,this._logInfo(`[初始化成功率上报] reported! is_success: "${t.is_success}". t: `,performance.now()-this._startTime),this._logInfo("[初始化成功率上报] reported! data: time: ",performance.now()-this._startTime,r)}catch(t){console.error("[初始化成功率上报] Error: ",t)}}getNavigationType(){try{const t=window.performance.getEntriesByType("navigation"),e=Array.isArray(t)&&t.length?t[0].type:"unknown";return this._logInfo("[初始化成功率上报] navigationType: ",e),e}catch(t){return console.error("[初始化成功率上报] get navigation_type error: ",t),"unknown"}}clearMaxWaitTimer(){null!==this._maxWaitTimer&&(clearTimeout(this._maxWaitTimer),this._maxWaitTimer=null)}pauseMaxWaitTimer(){this.clearMaxWaitTimer(),this._maxWait-=+(performance.now()-this._prevStartedTime).toFixed(0)}startMaxWaitTimer(){this.clearMaxWaitTimer(),this._logInfo("[初始化成功率上报] max_wait timer started. t:",performance.now()-this._startTime);const t=()=>{this._logInfo("[初始化成功率上报] max_wait reached. t:",performance.now()-this._startTime),this._contextData.data&&this._contextData.data.invalid_case||this._reported||this.entryStageReport({is_success:0})};this._maxWait>0?(this._prevStartedTime=performance.now(),this._maxWaitTimer=setTimeout(t,this._maxWait)):t()}maxWaitHandler(t=!1){if(t&&this._visiblechangeCount++,document.hidden)this.addContextData({data:{visible_tab:0}}),this.pauseMaxWaitTimer();else{if(this._reported)return this._logInfo("[初始化成功率上报] already reported! No need for max wait. t: ",performance.now()-this._startTime),void this.clearVisibilitychange();this.startMaxWaitTimer()}}clearVisibilitychange(){document.removeEventListener("visibilitychange",this.maxWaitHandler.bind(this,!0))}checkMaxWait(){document.addEventListener("visibilitychange",this.maxWaitHandler.bind(this,!0)),this.maxWaitHandler()}getContextData(){return this._contextData}addContextData(t){const e=t||{},{data:a={},...i}=this._contextData,{data:r={},...n}=e;Object.assign(this._contextData,{...i,...n,data:{...a,...r}})}changeInitStage(t,e={}){this._currentStage=t,this._logInfo(`[初始化成功率上报] changeInitStage: ${this._currentStage}. t: `,performance.now()-this._startTime),this.addContextData({data:{...e,[`to_${this._currentStage}_duration`]:this._getDuration(this._prevStageTime)}}),t!==this._endStage||this._contextData.data.invalid_case||this.entryStageReport({is_success:1}),this._prevStageTime=performance.now()}start(t){const e=t||{};this._logger=!!e.logger||this._logger,this._xhrOptions=e.xhrOptions||this._xhrOptions,this._beforeSend=e.beforeSend||this._beforeSend,null!==this._maxWaitTimer&&(this._logInfo("[初始化成功率上报] maxWait already started, resetting."),this.clearMaxWaitTimer(),this.clearVisibilitychange()),this._maxWait=e.maxWait&&e.maxWait>0?e.maxWait:this._maxWait,this.addContextData({data:{navigation_type:this.getNavigationType(),referrer:document.referrer||""}}),this.checkMaxWait()}getReporter(){return this._reporter}getBaseData(){return this._reporter.getBaseData()}setBaseData(t){return this._reporter.setBaseData(t)}getInvalidCaseState(){return e(a)}setAsInvalidCaseState(t=!1){!function(t,e){try{const a="string"==typeof e?e:JSON.stringify(e);localStorage.setItem(t,a)}catch(t){console.error("[初始化成功率] Error when set localStorage: ",t)}}(a,"1"),this.addContextData({data:{invalid_case:1}}),t&&window.addEventListener("unload",(()=>{this.resetInvalidCaseState()}))}resetInvalidCaseState(){var t;t=a,localStorage.removeItem(t),this.addContextData({data:{invalid_case:0}})}}class r{_reportContext;_env;constructor(t){const a="xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx".replace(/[xy]/g,(t=>{const e=16*Math.random()|0;return("x"===t?e:3&e|8).toString(16)})),i=(()=>{const t=window.location.hostname;return t.includes("systems")?"XX":t.substr(-2)})(),r=i.toUpperCase();this._env=(t=>{const e=t.match(/.+\.(dev|stable|test|uat|staging)\..+$/);return e?e[1]:"live"})(window.location.hostname),this._reportContext={a:r,ct:Math.floor(Date.now()/1e3),env:this._env,na:i,pf:"pc",r:1,s:location.href,sid:a,uid:e("useremail")||e("username")||"-",ut:e("stationName")||"-",nt:navigator?.connection?.effectiveType||"unknown",...t}}getBaseData(){return this._reportContext}setBaseData(t){this._reportContext={...this._reportContext,...t}}sendData(t,e){const{type:a,...i}=t||{},r=[{...this._reportContext,dt:a||this._reportContext.dt,d:{type:a,...i}}],n=new XMLHttpRequest,s=JSON.stringify(r),o="live"===this._env?"https://autobahn.ssc.shopeemobile.com/data/":"https://autobahn.ssc.test.shopeemobile.com/data/",{withCredentials:h=!0,timeout:c=1e5}=e||{};n.withCredentials=h,n.timeout=c,n.open("POST",o,!0),n.setRequestHeader("Content-type","application/json; charset=utf-8"),n.send(s)}}t.AutobahnXhrReporter=r,t.EntranceTracker=i,t.startTracker=(t,e)=>{const a=new r(t),n=new i(a,e);return n.start(),n},Object.defineProperty(t,"__esModule",{value:!0})}));
      window.__EntryTracker = window['tracing-entrance-tracker'].startTracker({
        bt: 'SLS',
        sbt: 'NSS',
        a: 'VN',
        env: 'live',
        v: window.__APP_VERSION__,
      }, {
        beforeSend(reportData, baseData) {
          const headlessUA = navigator.userAgent.toLowerCase().includes('headless');
          const validStrValueRule = /^([\w-.]+)+$/;
          const validUrlRule = /^https?:\/\/([.\w]+)(\/)?([#\w/?=&%-.]+)?/;
          const { d, v, sid, pf, nt, s } = baseData;
          const { data, key, type } = reportData;
          const { navigation_type, stage, referrer } = data;
          const isValidStr = [v, sid, pf, nt, key, type, navigation_type, stage].every(v => validStrValueRule.test(v));
          const isValidUrl = [s, referrer].every(v => validUrlRule.test(v) || !v);

          let visit_type = 'valid_visit';
          if (headlessUA) visit_type = 'crawl_visit';
          if (!isValidStr || !isValidUrl) visit_type = 'xss_visit';
          reportData.data.visit_type = visit_type;
          return reportData;
        }
      });
      !function(){var _;if(!(null===(_=window.__MDAP_PREV_RESOURCE__)||void 0===_?void 0:_.init)){window.__MDAP_PREV_RESOURCE__={data:[],init:!0};var n=function(_){var n;null===(n=window.__MDAP_PREV_RESOURCE__.data)||void 0===n||n.push(_)};window.addEventListener("error",n,!0),window.__MDAP_PREV_RESOURCE__.removePrevListener=function(){window.removeEventListener("error",n)}}}();</script><script>const WIDTH = 1400; //设计稿最宽
      const mobileAdapter = () => {
        const scale = screen.width / WIDTH;
        const content = `width=${WIDTH}, initial-scale=${scale}, maximum-scale=${scale}, minimum-scale=${scale}, user-scalable=yes`;
        let meta = document.querySelector('meta[name=viewport]');
        if(!meta){
          meta = document.createElement('meta');
          meta.setAttribute('name', 'viewport');
          document.head.appendChild(meta);
        }
        meta.setAttribute('content', content);
      }
      mobileAdapter(); //执行函数
      window.onorientationchange = mobileAdapter; //旋转屏幕重新校正</script><meta property="og:type" content="website"/><title>SPX Tracking Website</title><script>var __initMap__;
      function mapReady(cb) {
        __initMap__ = cb;
      }
      function initMap() {
        __initMap__ ? __initMap__() : (window.__mapScriptLoaded__ = true);
      }</script><script>performance.mark('qms-css:start');</script><link rel="stylesheet" href="/loading.css"><link rel="stylesheet" href="//deo.shopeemobile.com/shopee/shopee-spx-live-vn/static/css/ssc-ui-react.547db329.css"/><link rel="stylesheet" href="//deo.shopeemobile.com/shopee/shopee-spx-live-vn/static/css/react-pro-components.066bf860.css"/><link rel="stylesheet" href="//deo.shopeemobile.com/shopee/shopee-spx-live-vn/static/css/vendors.7a8e3ffa.css"/><link rel="stylesheet" href="//deo.shopeemobile.com/shopee/shopee-spx-live-vn/static/css/app.e1266450.css"/><script>performance.mark('qms-css:end');</script><style>html, body, #root {
        height: 100%;
        margin: 0;
        padding: 0;
      }</style><script>(function(w,d,s,l,i){
      if (window.region !== 'BR') {
        w[l]=w[l]||[];w[l].push({'gtm.start':
        new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
        j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
        'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
        var gtagScript = document.createElement('script');
        gtagScript.async = true;
        gtagScript.src = 'https://www.googletagmanager.com/gtag/js?id=UA-61914165-14';
        f.parentNode.insertBefore(gtagScript,f);
      }
      })(window,document,'script','dataLayer','GTM-5B2TZNC');</script><script>document.addEventListener('DOMContentLoaded', function() {
        if (window.region !== 'BR') {
          var noScript = document.createElement('noscript');
          var iframe = document.createElement('iframe');
          iframe.src = "https://www.googletagmanager.com/ns.html?id=GTM-5B2TZNC";
          iframe.height = "0";
          iframe.width = "0";
          iframe.style.display = "none";
          iframe.style.visibility = "hidden";
          noScript.appendChild(iframe);
          var body = document.body;
          body.insertBefore(noScript, body.firstChild);
        }
      });</script><script>if (window.region === 'TH') { 
        window.dataLayer = window.dataLayer || [];
        function gtag(){dataLayer.push(arguments);}
        gtag('js', new Date());
        try {
          if (!JSON.parse(localStorage.getItem('open-spx-cookiesTypeList') || '[]').includes('performance')) {
            gtag('consent', 'default', {
              'ad_storage': 'denied',
              'analytics_storage': 'denied'
            });
            dataLayer.push({
              'event': 'default_consent'
            });
          }
        } catch (error) {console.error(error)}
      }</script></head><body><noscript>You need to enable JavaScript to run this app.</noscript><div id="root"><div class="pre-loading"><div id="spx-loading-logo" class="spx-loading-logo"></div><div class="loading"><span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span></div></div></div><script>// BR市场使用Shopee xpress logo
      if (window.region != 'BR') {
        const classList = document.querySelector('#spx-loading-logo').classList;
        classList.add('spx-loading-logo-2305');
      }</script><script>performance.mark('qms-js:start');</script><script>if (location.search.includes('preview=')) {
        const scriptTag = document.createElement('script');
        const version = new URLSearchParams(location.search).get('preview');
        scriptTag.setAttribute('src', 'https://sls.sp-cdn.shopee.com/api/v4/69067583/pgmgmt/spx-vn-pc-conf.js'.replace('.js', `-draft-${version}.js`));
        document.head.appendChild(scriptTag);
      }</script><script src="https://sls.sp-cdn.shopee.com/api/v4/69067583/pgmgmt/spx-vn-pc-conf.js"></script><script src="https://maps.googleapis.com/maps/api/js?key=AIzaSyBBUB0Wrt1xnu8qOK1_7teVZF2J7hY4Smk&libraries=places&callback=initMap"></script><script src="//deo.shopeemobile.com/shopee/shopee-spx-live-vn/static/js/ssc-ui-icons.6a6ad9d3.js" crossorigin></script><script src="//deo.shopeemobile.com/shopee/shopee-spx-live-vn/static/js/ssc-ui-react.938ce140.js" crossorigin></script><script src="//deo.shopeemobile.com/shopee/shopee-spx-live-vn/static/js/react-pro-components.71392fb2.js" crossorigin></script><script src="//deo.shopeemobile.com/shopee/shopee-spx-live-vn/static/js/react.660ac3d7.js" crossorigin></script><script src="//deo.shopeemobile.com/shopee/shopee-spx-live-vn/static/js/vendors.75e0e6df.js" crossorigin></script><script src="//deo.shopeemobile.com/shopee/shopee-spx-live-vn/static/js/app.ca07819c.js" crossorigin></script><script>performance.mark('qms-js:end');</script><script>const prefix = 'ssc-numeric';
      const _separator_ = '-:-';
      const { pathname } = location;
      const qmsFcpTIDName = `FCP_${pathname.replace(/\/|-/g, '_')}`;
      const qmsLcpTIDName = `LCP_${pathname.replace(/\/|-/g, '_')}`;
      window.qmsFcpTID = `${qmsFcpTIDName}${_separator_}${Date.now()}`;
      window.qmsLcpTID = `${qmsLcpTIDName}${_separator_}${Date.now()}`;
      performance.mark(`${prefix}-${qmsFcpTID}:start`);
      performance.mark(`${prefix}-${qmsLcpTID}:start`);</script></body></html>
