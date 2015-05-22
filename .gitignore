<?php
class Curl{

	//curl实例
	private $ch_;
	//准备请求的url
	private $url_;
	//超时时间
	private $time_;
	//请求设置
	private $options_;
	//保存请求头信息
	private $request_header_;
	//保存响应头信息
	private $response_header_;
	//保存curl执行返回结果
	private $body_;
	//保存最新的执行信息
	private $info_;
	//cookie
	private $cookie_file_;
	//保存错误信息
	private $error_info_;

	static public $TIMEOUT = 20;
	static public $HEADER = array("Connection: Keep-Alive","Accept: text/html, application/xhtml+xml, */*", "Pragma: no-cache", "Accept-Language: zh-Hans-CN,zh-Hans;q=0.8,en-US;q=0.5,en;q=0.3","User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)");

	public function __construct(){
		if(!function_exists('curl_init') ) {
			die("不支持curl拓展");
		}
		//echo "__construct";
		$this->cookie_file_ = $cookie_file='./temp';
		$this->info_ = array();

	}
	public function getCookie(){
		return $this->cookie_file_;
	}
	public function init($url){
		$this->url_ = $url;
		$this->ch_ = curl_init($url);
		curl_setopt($this->ch_, CURLOPT_HEADER, 0);
		curl_setopt($this->ch_, CURLOPT_RETURNTRANSFER , 1 );
		curl_setopt($this->ch_, CURLOPT_HTTPHEADER, self::$HEADER);
		curl_setopt($this->ch_, CURLOPT_RETURNTRANSFER,1);
		curl_setopt($this->ch_, CURLOPT_TIMEOUT, self::$TIMEOUT);
		curl_setopt($this->ch_, CURLOPT_COOKIEFILE , $this->cookie_file_);
		curl_setopt($this->ch_, CURLOPT_COOKIEJAR , $this->cookie_file_);
	}
	public function setFollowLocation($follow = true){
		curl_setopt($this->ch_, CURLOPT_FOLLOWLOCATION,$follow);
		return $this;
	}
	public function setUrl(string $url){
		$this->init($url);
		return $this;
	}
	public function doHttpPost(array $params = array()){
		//一些设置
		if ($params) {
			curl_setopt($this->ch_, CURLOPT_POST, true);
			curl_setopt($this->ch_, CURLOPT_POSTFIELDS, http_build_query($params));
		}
		return $this->execute();
	}
	public function doHttpGet(array $params = array()){
		if($params){
			if(strpos($this->url_,'?')){
				$this->url_ .= '&'.http_build_query($params);
			}else{
				$this->url_ .= '?'.http_build_query($params);
			}
			curl_setopt($this->ch_,CURLOPT_URL,$this->url_);
		}
		if (strpos($url, 'https') === 0) {
			curl_setopt($this->ch_, CURLOPT_SSL_VERIFYHOST, 0);
			curl_setopt($this->ch_, CURLOPT_SSL_VERIFYPEER, 0);
		}
		return $this->execute();
	}
	public function execute(){
		$this->body_ = curl_exec($this->ch_);
		$this->info_ = curl_getinfo($this->ch_);
		$header_size = curl_getinfo($this->ch_, CURLINFO_HEADER_SIZE);
		$this->response_header_ = substr($this->body_, $start = 0, $offset = $header_size);
		//$this->response_header_ = http_parse_headers($this->response_header_);
		
		if($this->body_ === false){
			$this->error_info_ = curl_error($this->ch_);
			
			return false;
		}
		curl_close($this->ch_);//关闭该对象
		return $this->body_;
	}
	/**
	* @设置HEADER
	*/

	public function SetHeader($headers = null){
		if (is_array($headers) && count($headers) > 0) {
			curl_setopt($this->ch_, CURLOPT_HTTPHEADER, $headers);
		}
		return $this;
	}
	/**
	*设置Referer
	*/
	public function setReferer(string $referer){
		if(!empty($referer))
			curl_setopt($this->ch_,CURLOPT_REFERER,$referer);
		return $this;
	}
	//获取执行后的header信息
	public function getResponseHeader(){
		return $this->response_header_;
	}
	public function getInfo(){
		return $this->info_;
	}
	public function getError(){
		return $this->error_info_;
	}
}
