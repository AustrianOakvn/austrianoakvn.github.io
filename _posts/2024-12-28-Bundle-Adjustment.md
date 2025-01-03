---
layout: post
title:  "Bundle Adjustment (Tối ưu hóa chùm)"
date:   2024-12-18 21:21:21 +0530
tags: [sfm]
---

Bài toán xấp xỉ vị trí và hướng (extrinsic parameters) của camera và không gian 3D cũng giống như đi giải bài toán "Con gà có trước hay quả trứng có trước" 
(Mặc dù sinh học đã chứng minh trứng có trước). Phép ví von mình sài mục đích để nhấn mạnh sự phụ thuộc lẫn nhau của lời giải mà chúng ta đang xấp xỉ, một mặt thì 
chúng ta không thể tính được vị trí cũng như hướng nếu không định nghĩa ra được không gian 3D, mặt khác thì cũng không thể ước lượng không gian 3D nếu không biết vị trí 
camera. 

Đến bước này chúng ta sẽ cần sài một số giả thiết mạnh để có thể tính một cách xấp xỉ, sẽ có nhiều cách tính khác nhau, mình sẽ giới thiệu một cách sơ lược về 
các phương pháp mình biết đến.

### Xấp xỉ bằng phép đối chiếu đặc trưng (image matching)
Phần này đòi hỏi bạn sẽ cần có kiến thức nền tảng về các kỹ thuật cổ điển (kinh điển=)) trong thị giác máy tính như "feature detection", "feature description", và "image matching".

Bài viết sẽ không tập trung vào các phần này (tương lai sẽ có bài viết nếu mình rảnh =)) nên các bạn có thể tìm hiểu các nguồn khác hoặc đọc chương 11 sách Computer Vision của Szeliski.

Về bản chất, image matching sẽ đi tìm các điểm đặc trưng chung giữa 2 bức ảnh chụp cùng một không gian và tồn tại overlap giữa chúng. Để bình luận một chút về image matching 
thì mặc dù rất nhiều các kỹ thuật cả cổ điển lẫn học máy được nghiên cứu nhưng đây vẫn là một task rất khó trong thị giác máy tính. Khi mang các thuật toán image matching SOTA 
ở hiện tại ra một số bài toán trong không gian có tồn tại nhiễu như cây cối, các kiến trúc có tính đối xứng, việc matching trở thành rất khó. Quay lại với bài toán chúng ta bàn luận ta cần phải giả thiết rằng các điểm chung ta tìm được là đúng. Các điểm chung trong ảnh được gọi là 2D correspondences, và chúng được viết dưới dạng sau:

$$
z1={z_1^1, z_1^2, ..., z_1^N}, z2={z_2^1, z_2^2, ..., z_2^N}.
$$
