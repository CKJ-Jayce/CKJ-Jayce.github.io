# SYY工作室（SunYiYang Studio）荣誉出品  
*CKJ-Jayce网站/百词斩小班♛考神•觉醒♛的官方网站*  
欢迎访问我们的网站！<br>如果你来到了这里，说明你可能通过以下方式知晓并访问了该网站：  

- 🎯 百词斩
- 🔗 分享链接

## 请告诉我们你是如何知道我们的

<form>
  <input type="radio" id="baicizhan" name="source" value="baicizhan">
  <label for="baicizhan">百词斩</label><br>
  
  <input type="radio" id="share" name="source" value="share">
  <label for="share">分享链接</label><br>
  
  <div id="result" style="margin-top: 15px; padding: 10px; background: #f0f8ff; border-radius: 5px; display: none;">
    <!-- 结果将在这里显示 -->
  </div>
</form>

> ℹ️ 你的选择不会被别人知道哦 😊

<script>
  document.addEventListener('DOMContentLoaded', function() {
    const radios = document.querySelectorAll('input[name="source"]');
    const resultDiv = document.getElementById('result');
    
    radios.forEach(radio => {
      radio.addEventListener('change', function() {
        if (this.value === 'baicizhan') {
          resultDiv.innerHTML = '感谢您通过百词斩访问！<br>请返回到百词斩更改昵称为`ckj`以说明您访问了该网站';
          resultDiv.style.display = 'block';
          resultDiv.style.color = '#d9534f';
          resultDiv.style.backgroundColor = '#f2dede';
        } else if (this.value === 'share') {
          resultDiv.innerHTML = '感谢您通过分享链接访问！<br>欢迎探索SYY工作室的更多内容。';
          resultDiv.style.display = 'block';
          resultDiv.style.color = '#5cb85c';
          resultDiv.style.backgroundColor = '#dff0d8';
        }
      });
    });
  });
</script>
感谢您的支持，欢迎您持续关注SYY Studio！  
© 2025 SYY Studio. 保留所有权利
