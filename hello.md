`// VD
var VD = {
	showPop: function(){
		var _ts = this;
		$('body').append('<div id="qr_con" class="qr_con">Loading...</div><div id="pop_con"></div>');
		$('#pop_con').width($(window).width());
		$('#pop_con').height($(window).height());
		var url = encodeURIComponent(window.location.href);
		var QR_CODE = '<img src="http://www.zhanyou8.com/index.php?m=content&c=index&a=qrcode&url='+url+'&width=200&height=200"/><p>本页面二维码</p>';

		var osetTop = $('.op_tools').offset();
		$('#qr_con').css('top', osetTop.top+30);
		setTimeout(function(){
			$('#qr_con').html(QR_CODE);
			$('#pop_con').on('click', function(){
				_ts.closePop();
			});
		},300);
	},
    showWechat: function(){
        var _ts = this;
        $('body').append('<div id="qr_con" class="qr_con">Loading...</div><div id="pop_con"></div>');
        $('#pop_con').width($(window).width());
        $('#pop_con').height($(window).height());
        var url = encodeURIComponent(window.location.href);
        var QR_CODE = '<p>'+$('#get_wechat').attr('wechat')+'</p><p>请搜索微信号</p>';

        var osetTop = $('.op_tools').offset();
        $('#qr_con').css('top', osetTop.top+30);
        setTimeout(function(){
            $('#qr_con').html(QR_CODE);
            $('#pop_con').on('click', function(){
                _ts.closePop();
            });
        },300);
    },
	closePop: function(){
		$('#qr_con').remove();
		$('#pop_con').remove();
	},
	showQrcode:function(){
		var tg = $('#get_qr_code');
		var _ts = this;
		tg.on('click', function(){
			_ts.closePop();
			//alert('qr')
			_ts.showPop();
		});
	},
    monitorWechat:function(){
        var tg = $('#get_wechat');
        var _ts = this;
        tg.on('click', function(){
            _ts.closePop();
            _ts.showWechat();
        });
    },
	slider: function(){
		var $slider = $("#slider");
		if($('#slider').size()>0){
			// guide
			var $imgCount = $slider.find("#img_count"),
				_len = $slider.find('.img_scroll li').length,
				_html = '';
			//render imgCount
			for(var i = 0 ; i < _len ; i++) {
				if(i == 0){
					_html += '<li class="cur"></li>';
				}
				else{
					_html += '<li></li>';
				}
			}
			$imgCount.html("<ul>"+_html+"</ul>");

			var li = $imgCount.find("li");
			var guide_init = function () {
			    // body...
			    if(document.getElementById('slider')){
			        var slider = new Swipe(document.getElementById('slider'), {
			            startSlide:0,
			            speed: 1000,
			            auto: 0,
			            continuous: false,
			            callback: function(index) {
			                //add class
			                for (var i = 0; i < li.length; i++) {
			                    li[i].className = '';
			                };
			                li[index].className = 'cur';
			            }
			        });
			        // document.getElementById('sd_con').style.height = document.body.scrollHeight+"px";
			        // document.getElementById('reg_menber').style.height = document.body.scrollHeight+"px";
			    }
			}
			//guide
			guide_init();
		}
	},
	initSwipe : function(){
        var img_tar = $(".img_con"),
            img_count = $(".img_count"),
            _h = '',
            _ts = this;

        for(var i = 0; i < img_tar.size(); i++){
            _h += '<li></li>';
        }
        
        //防止IOS的WebView缓存下重复渲染
        if(img_count.find('li').length === 0){
            img_count.append(_h);
        }

        // if(navigator.userAgent.match(/iphone|ipod/ig)){
        //     $(".img_scroll").addClass('ios_fix');
        // }

        Swipe_Loop($("#slider"), {
            speed : 300,
            auto : 4000,
            callback : function(pos){
                var i = img_tar.size();
                _ts.pos = pos;
                while(i--){
                    img_tar.eq(i).removeClass('cur');
                    $("li",img_count).eq(i).removeClass('active');
                }
                img_tar.eq(pos).addClass('cur');
                $("li",img_count).eq(pos).addClass('active');

            }
        });

        //bind img_con event
        var imgTag = $("#slider").find("img");
        imgTag.bind("click",function(){
        	var _index = imgTag.index($(this));
        		_ts.popSwipe(_index);
        });
    },	
    popSwipe:function(index){
    	var _html = "",
    	    imgUrl = [],
    		$popSlider = $("#pop_slider"),
    		$popFocus = $("#pop_focus"),
    		$slider = $("#slider").find("img"),
    		_w = $(window).width(),
    		_h = $(window).height(),
    		_gh =$("body").height();

    	//collect img url
    	// console.log(index+":" + $slider);
    	for(var i = 0 ; i < $slider.length ; i++ ){
    		if(i == index){
    			imgUrl.unshift($slider.eq(i).attr("src"));
    		}
    		else{
    			imgUrl.push($slider.eq(i).attr("src"));
    		}    		
    	}
    	// console.log(imgUrl);
    	//render pop_slider html
    	for(var i = 0 ; i < imgUrl.length ; i++){
    		_html += '<li class="img_con"><span style="width:'+_w+'px;height:'+_h+'px;"><img src="'+imgUrl[i]+'" /></span></li>';
    	}

    	$popSlider.html('<ul class="img_scroll">'+_html+'</ul>');

    	//set li width
    	$popFocus.css("height",_gh);
    	$popSlider.find(".img_con").width(_w);


    	//bind close event
    	$popFocus.find(".close").click(function(){
    		$popFocus.hide();
    	});

    	//resize event
    	$(window).resize(function(){
    		_w = $(window).width();
    		_h = $(window).height();
    		_gh =$(document).height();
    		//reset height width
    		$popFocus.css("height",_gh);
    		$popSlider.find(".img_con").width(_w);
    		$popSlider.find(".img_con span").css({width:_w,height:_h});
    	});

    	if(index != null){
    		
    	}
    	var img_tar = $(".img_con",$popSlider);
    	$popFocus.show();
    	Swipe_Loop($("#pop_slider"), {
            speed : 300,
            auto : 0,
            callback : function(pos){
                /*var i = img_tar.size();
                while(i--){
                    img_tar.eq(i).removeClass('cur');
                    //$("li",img_count).eq(i).removeClass('active');
                }
                img_tar.eq(pos).addClass('cur');*/
                //$("li",img_count).eq(pos).addClass('active');

            }
        });
    },
    changeTab: function(){
        $('#change_tab_hd').find('span').on('click', function(){
            var index = $(this).index();
            $('#change_tab_hd').find('span').removeClass('cur');
            $('.vd_list').find('.chg_tab').hide().eq(index).show();
            $(this).addClass('cur');
        });
    },
    init: function(){
        var _ts = this;
        VD.showQrcode();
        this.monitorWechat();
        _ts.changeTab();
        if($('#slider').size()>0){
            _ts.initSwipe();
        }
        // this.popSwipe();
    }
}
    
$(function(){
    VD.init();
});
