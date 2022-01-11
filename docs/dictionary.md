<!DOCTYPE html>
<html>
<body>
    <h2 style="text-align: center;">ISL Dictionary by ISLRTC</h2>
    <p>Please type a keyword for which you would like to see the sign</p>
    <br/>
    <input id="dict_searchbar" type="text" name="search" placeholder="Search for sign.." autocomplete="off">
    <ul id='dict_result'></ul>
    <div id="yt_video">
    <iframe src="" id="content" style="position: absolute; height: 65%; width: 55%;"></iframe>
    </div>
</body>

<footer class="footer" style="position: fixed;left: 0;bottom: 0;width: 100%;
text-align: center;">
        <p>Credits: <a href="http://www.islrtc.nic.in/isl-dictionary-launch" target="_blank">ISLRTC</a><br>
</footer>

<script>
        $(document).ready(function () {
            $('iframe').each(function() {
                    if ($(this).attr('src') == '') {
                        $(this).hide();
                    }
                });

            const options = {
                isCaseSensitive: false,
                shouldSort: true,
                keys: [
                    "Title",
                ]
            };

            $.getJSON("https://raw.githubusercontent.com/Prem-kumar27/Demo/main/ISLRTC_videos.json", function (data) {
                const fuse = new Fuse(data, options);

                $('#dict_searchbar').keyup(function () {
                    let result = fuse.search($(this).val());
                    let resultdiv = $('#dict_result');

                    if (result.length === 0) {
                        resultdiv.hide();
                    }
                    else {
                        resultdiv.empty();
                        for (let item in result.slice(0, 5)) {
                            let searchitem = '<li><a href=' + result[item].item.URL + ' >' + result[item].item.Title.charAt(0).toUpperCase()+ result[item].item.Title.slice(1) + ' (' +result[item].item.Domain + ')' +'</a></li>';
                            resultdiv.append(searchitem);
                        }
                        resultdiv.show();
                    }
                });
                
                var $content = $('#content');
                $(document).on("click", "#dict_result li a" , function() {
                    $content.show();
                    $content.attr('src', $(this).attr('href').replace("watch?v=","embed/"));
                    return false;
                });

            });

        });

        

</script>

</html>