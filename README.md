
<!DOCTYPE html>
<html>
<head>
  <meta content="text/html; charset=utf-8" http-equiv="Content-Type">
  <title>Bulk!</title>
  <link href="https://cdn.branch.io/assets/3af4cdb3/static/common/favicons/apple-touch-icon-144x144.png" rel="icon" type="image/x-icon">
  <meta content="width=device-width, initial-scale=1" name="viewport">
  <link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css" rel="stylesheet">
  <script src="//ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js">
  </script>
  <script src="//maxcdn.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js">
  </script>
  <style>
      body {
	margin:20px;
	padding-top:5px;
	}
	
	textarea {
    float:left;
	}
	
    #input {
	height:60px;
	font-family:"Courier New",Courier,monospace;
	}
	
	#result{
	height:60px;
	font-family:"Courier New",Courier,monospace;
	}
  </style>
</head>
<div class="container">
    <div class="panel panel-default">
      <div class="panel-body">
	  <h3>BulkURL!</h3>
	  <ul class="nav nav-pills nav-justified">
	  <li class="active"><a href="./bulkcopas1.html">Short!</a></li>
	  <li><a href="./bulkcopas2.html">Debug!</a></li>
	  </ul>
	  <br>
        <div class="alert alert-info" role="alert">
          Bulk bit.ly or j.mp
        </div>
        <textarea class="form-control input-sm u-full-width" id="accessToken" onfocus="this.select();" placeholder="52f6ec7a1c7f..." rows="1" style="resize:none;" required></textarea>
        <hr style="margin:3px;padding:3px 3px;border:0;border-bottom:0px dashed #ccc">
				<select id="option" class="form-control">
					<option value="0" selected="selected">bit.ly</option>
					<option value="1">j.mp</option>
				</select>
                <hr style="margin:3px;padding:3px 3px;border:0;border-bottom:0px dashed #ccc">
        <textarea class="form-control input-sm u-full-width" id="input" onfocus="this.select();" placeholder="https://..." rows="5" style="resize:none;" required></textarea>
		<textarea type="text" class="form-control input-sm u-full-width" id="result" autocomplete="off" placeholder="result URL" rows="5" style="resize:none;" required></textarea>
		<button type="button" data-copytarget="#result" class="btn btn-default btn-sm">
		<span class="iconify" data-icon="ic:baseline-content-copy" data-copytarget="#result">
			  </span>
	  </button>
        <hr style="margin:3px;padding:3px 3px;border:0;border-bottom:0px dashed #ccc">
        <input class="form-control btn-info" id="btn" type="submit" value="Shorter!">
        <hr style="margin:3px;padding:3px 3px;border:0;border-bottom:0px dashed #ccc">
      </div>
    </div>
  </div>
  <script>
function start() {
  var lines = $('#input').val().split(/\n/);
  var accessToken = $('#accessToken').val();
  var option = $("#option").val();
  var output = [];
  var outputText = [];
  for (var i = 0; i < lines.length; i++) {
    if (/\S/.test(lines[i])) {
      
      var params = {
        "long_url" : $.trim(lines[i])    
    };

    $.ajax({
        url: "https://api-ssl.bitly.com/v4/shorten",
        cache: false,
        dataType: "json",
        method: "POST",
        contentType: "application/json",
        beforeSend: function (xhr) {
            xhr.setRequestHeader("Authorization", "Bearer " + accessToken);
        },
        data: JSON.stringify(params)
    }).done(function(data) {
            if(option == 1) {
        var rest = data.link;
        var uh = rest.replace('bit.ly', 'j.mp');
      }
      else{
      var uh = data.link;
      }

        $("#result").append( uh + '\r\n'), $(".alert").removeClass("alert-info alert-warning").addClass("alert-success").text("Done!"), btn.button("reset")
    }).fail(function(data) {
        $("#result").append(data.link + "<br>"), $(".alert").removeClass("alert-info alert-warning").addClass("alert-success").text("Done!"), btn.button("reset")
    });

    }
  };
}

    $(document).ready(function() {
    $("#btn").click(function() {
        btn = $(this), btn.button("loading"), start(0), $(".alert").addClass("alert-warning").removeClass("alert-success").text("waiting...!");
      
    })
});

  </script>
  <script>
        (function () {
            'use strict';
            document.body.addEventListener('click', copy, true);
            function copy(e) {
                var t = e.target,
                    c = t.dataset.copytarget,
                    inp = (c ? document.querySelector(c) : null);
                if (inp && inp.select) {
                    inp.select();
                    try {
                        document.execCommand('copy');
                        inp.blur();
                        inp.focus();
                        t.classList.add('copied');
                        setTimeout(function () {
                            t.classList.remove('copied');
                        }, 1500);
                    } catch (err) {
                        swal.fire('please press Ctrl/Cmd+C to copy');
                    }
                }
            }
        })();
  </script>
  <script src="https://code.iconify.design/1/1.0.7/iconify.min.js"></script>
</body>
</html>
