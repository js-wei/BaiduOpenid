<pre>#BaiduOpenid
简介
百度连接开放平台基于业界最先进的OAuth2.0授权协议，面向所有第三方开放百度的帐号体系、
好友关系链及相关产品线核心数据接口，基于这些开放接口所能实现的功能包括：
所有第三方网站都可以使用百度帐号登录其网站，以降低其用户的登录、注册成本，提升用户登录量和网站流量；
百度应用开放平台的开发者可以实现在其IFrame应用中获取百度登录用户的基本资料和好友关系等数据。
在接下去的一段时间内，我们还会开放百度统一的Feed系统接口，以支持第三方应用的各种动态信息推送、传播，
从而为第三方导入更多流量。后续我们还将会开放百度相关核心产品的数据接口供第三方使用，敬请期待
与百度应用开放平台所不同的是，接入到百度连接开放平台的应用都是不需要内嵌在百度页面中，该平台支持任何类型的第三方应用，
包括第三方Web/Wap网站、PC桌面客户端应用、手机客户端应用、各种浏览器插件应用以及各种开源建站系统的插件应用等，
唯一的前提是，接入的应用都必须支持使用百度帐号登录其应用，否则我们将不会成功对接。
百度应用开放平台的开发者可以直接使用百度连接开放平台，无需额外注册，反之亦然。
百度授权前准备
1、如果您已是百度用户，请您访问百度开发者中心并使用百度账号直接登录。
2、如果您还不是百度用户，注册一个百度账号。
3、由于百度的业务调整现在获取授权请到 http://zhida.baidu.com 登陆您的百度账号创建我的直达号获取。
使用BaiduOpenid在本例中使用是基于Thinkphp框架进行的。
1、下载BaiduOpenid，并解压到ThinkPHP\Library\Vendor\文件夹中。
2、在配置文件中配置如下信息
  'BaiduOpenid'=>array(
	  'clientId' => '***********************',             			//App key
	  'clientSecret' => '***********************',        			//Secret
	  'redirectUri' => 'http://www.jswei.com/Baidu/callback',		//回调地址
	  'cancelRedirectUri' => 'http://www.jswei.com/Baidu/cancel_callback',	//取消回调地址
	),
3、创建一个BaidController.class.php。
  class BaiduController extends CommonAction{
    	/**
    	 * [login 获取登陆授权地址]
    	 * @return [type] [description
    	 */
    	public function login(){
    		Vendor('BaiduOpenid.Baidu');
    		$baidu = new Baidu();
    		//获取登陆授权地址
    		$loginUrl = $baidu->getLoginUrl('', 'popup');
    		header('Location:'.$loginUrl);
    	}
    	/**
    	 * [logout 注销登录操作]
    	 * @return [type] [description]
    	 */
    	public function logout(){
    		Vendor('BaiduOpenid.Baidu');
    		$baidu = new Baidu();
    		$user = $baidu->getLoggedInUser();
    		if($user)
    			$logout = $baidu->getLogoutUrl(C('BaiduOpenid.cancelRedirectUri')
    			.urlencode(BaiduUtils::getCurrentUrl()));
    		else
    			return;
    		header('Location:'.$loginUrl);
    	}
    	/**
    	 * [callback 登陆授权成功后获取登陆用户信息]
    	 * @return function [description]<br/>
    	 */
    	public function callback(){
    		vendor('BaiduOpenid.Baidu');
    		$baidu = new Baidu();>
    		$user = $baidu->getUserInfo();
    		var_dump($user);
    		//下面你的其他操作
    		....
  	}
  	/**
    	 * [cancel_callback 接收退出登录的返回信息]
    	 * @return [type] [description]
    	 */
    	public function cancel_callback(){
    		var_dump($_REQUEST);
    		//下面你的其他操作
    		......
    	}
   }
百度OAuth其他的参考：http://developer.baidu.com/wiki/index.php?title=docs/oauth
