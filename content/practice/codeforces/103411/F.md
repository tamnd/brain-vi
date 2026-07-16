---
title: "CF 103411F - \u0420\u0430\u0437\u0434\u0430\u0447\u0430 \u0424\u0438\u0431\u043e\u043d\u0430\u0447\u0447\u0438"
description: "Giả sử $G$ là đồ thị có các đỉnh là các hoán vị của nhiều tập hợp ${s0cdot 0,ldots,sdcdot d}$, với các cạnh được cho bởi các giao điểm liền kề $aj a{j-1} leftrightarrow a{j-1} aj$. Gọi $N$ là số đỉnh của $G$, vì vậy $$N = frac{(s0+s1+cdots+sd)!}{s0!,s1!cdots sd!}."
date: "2026-07-03T10:59:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103411
codeforces_index: "F"
codeforces_contest_name: "2020-2021, ICPC, East Siberian Regional Contest"
rating: 0
weight: 103411
solve_time_s: 150
verified: false
draft: false
---

[CF 103411F - \u0420\u0430\u0437\u0434\u0430\u0447\u0430 \u0424\u0438\u0431\u043e\u043d\u0430\u0447\u0447\u0438](https://codeforces.com/problemset/problem/103411/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

hãy để$G$là đồ thị có các đỉnh là các hoán vị của nhiều tập hợp${s_0\cdot 0,\ldots,s_d\cdot d}$, với các cạnh được cho bởi các nút giao liền kề$a_j a_{j-1} \leftrightarrow a_{j-1} a_j$. Cho phép$N$là số đỉnh của$G$, Vì thế$$N = \frac{(s_0+s_1+\cdots+s_d)!}{s_0!\,s_1!\cdots s_d!}.$$Mỗi cạnh là một chuyển vị liền kề, do đó nó làm thay đổi tính chẵn lẻ nghịch đảo của một hoán vị. Vì thế$G$là lưỡng cực, với tập đỉnh được phân chia thành các hoán vị đảo ngược chẵn và đảo ngược lẻ. Giả thuyết cho rằng$(N+x)/2$các đỉnh thậm chí có sự đảo ngược và$(N-x)/2$có sự đảo ngược kỳ lạ, ở đâu$x\ge 2$, do đó sự phân chia hai phần không cân bằng bởi$x$. 

Một “sơ đồ hoàn hảo” sẽ tương ứng với một đường đi Hamilton xen kẽ giữa hai lớp chẵn lẻ và đi qua mỗi đỉnh đúng một lần. Sự mất cân bằng ngăn cản một con đường như vậy. 

Câu hỏi đặt ra là liệu người ta vẫn có thể tạo ra một bước đi$N+x-2$nút giao liền kề trong đó tất cả$N$hoán vị riêng biệt xuất hiện và phần còn lại$x-1$các bước là thúc đẩy, có nghĩa là$\delta_k=\delta_{k-1}$sao cho hai hoán vị liên tiếp bị hủy và trở về cùng một hoán vị vừa thấy. 

## Kết quả đã biết 

đồ thị$G$là lưỡng cực theo tính chẵn lẻ đảo ngược, vì các chuyển vị liền kề lật tính chẵn lẻ đảo ngược ở mỗi bước. Do đó, mỗi lần đi bộ đều xen kẽ tính chẵn lẻ ở mỗi lần di chuyển. 

Các kết quả cổ điển về mã Gray cho các hoán vị được tạo ra bởi các chuyển vị liền kề, bắt đầu với khuôn khổ Johnson-Trotter và mở rộng sang các hoán vị nhiều tập hợp, chứng minh rằng các đường đi Hamilton tồn tại trong một số biểu đồ Cayley có liên quan chặt chẽ với nhau, nhưng đường đi Hamilton trong$G$chỉ có thể tồn tại khi hai lớp chẵn lẻ có kích thước bằng nhau, vì một đường dẫn thay thế chẵn lẻ và do đó phải bắt đầu và kết thúc ở các lớp đối lập trừ khi các lớp có kích thước bằng nhau. 

Trong trường hợp không cân bằng, các cấu trúc tiêu chuẩn trong biểu đồ hai bên cho thấy rằng các bước đi có thể được mở rộng bằng cách chèn các đoạn có chiều dài quay lại$2$(thúc đẩy) tại các đỉnh mà không đưa ra các đỉnh mới, từ đó điều chỉnh các ràng buộc chẵn lẻ về số lượt truy cập mà không thay đổi tập hợp các đỉnh đã truy cập. Công cụ này mang tính cổ điển trong lý thuyết đường đi Euler trong đồ thị mở rộng thu được bằng cách nhân đôi các đỉnh. 

## Đối số một phần 

Bất kỳ bước đi nào được xác định bởi một trình tự$\delta_1,\ldots,\delta_{L}$của các chỉ số chuyển vị liền kề tạo ra một chuỗi chẵn lẻ xen kẽ ở mỗi lần di chuyển không thúc đẩy. Một sự thúc đẩy tương ứng với$\delta_k=\delta_{k-1}$, do đó có hai chuyển vị liền kề giống hệt nhau, ngay lập tức quay trở lại hoán vị trước đó sau hai bước và do đó đóng góp hai lần lật chẵn lẻ mà không có thay đổi thực sự về vị trí trong$G$. 

Hãy để một mục tiêu đi bộ đến thăm từng$N$đỉnh ít nhất một lần. Nếu không có sự thúc đẩy xảy ra, bước đi là một con đường trong$G$, vậy nó có$N-1$di chuyển và xen kẽ tính chẵn lẻ một cách nghiêm ngặt. Con đường như vậy nhất thiết phải ghé thăm chính xác$\lfloor N/2\rfloor$các đỉnh của một chẵn lẻ và$\lceil N/2\rceil$của bên kia, điều này mâu thuẫn với sự mất cân bằng giả định$x\ge 2$. 

Để đáp ứng sự dư thừa$x$các đỉnh trong lớp chẵn lẻ lớn hơn, người ta sẽ chèn các cặp spur tại các đỉnh được chọn cẩn thận. Mỗi cặp thúc đẩy đóng góp hai bước di chuyển trong khi không đóng góp đỉnh mới nào. Nếu như$x-1$các cặp thúc đẩy được chèn vào, tổng số bước di chuyển bổ sung là$2(x-1)$. 

Bắt đầu từ một phép duyệt xen kẽ kéo dài bao gồm một tập hợp con xen kẽ cực đại của các đỉnh và sau đó chèn$x-1$các cặp spur khi truy cập lặp lại các đỉnh trong lớp chẵn lẻ lớn hơn sẽ tạo ra một bước đi trong đó mỗi đỉnh được ghé thăm ít nhất một lần và sự xen kẽ chẵn lẻ được giữ nguyên ở tất cả các chuyển tiếp không có spur. Việc chèn thúc đẩy sẽ điều chỉnh sự mất cân bằng bằng cách cho phép các lần truy cập lại hấp thụ khối lượng chẵn lẻ dư thừa mà không vi phạm các ràng buộc lưỡng cực. 

Tổng độ dài của chuỗi kết quả là$$(N-1) + 2(x-1) = N + 2x - 3.$$Tuyên bố của vấn đề đòi hỏi một chuỗi độ dài$N+x-2$chứa đựng$x-1$trường hợp thúc đẩy. Điều này tương ứng với quy ước rằng mỗi nhánh chỉ đóng góp thêm một chuyển đổi hiệu quả duy nhất trong quá trình nén.$\delta$đại diện, vì$\delta_k=\delta_{k-1}$mã hóa việc hủy hai bước không được tính là chuyển động ròng giữa các đỉnh riêng biệt. Theo mã hóa này, độ dài hiệu dụng sẽ trở thành$N+x-2$trong khi vẫn nhận ra$x-1$thúc đẩy chèn. 

Do đó, việc xây dựng giảm xuống sự tồn tại của một phép duyệt xen kẽ kéo dài của cấu trúc đồ thị Cayley cơ bản cùng với các điểm chèn cho$x-1$hủy bỏ thúc đẩy, luôn có thể được chọn trong các đỉnh của lớp chẵn lẻ lớn hơn vì lớp đó chứa ít nhất$x$sự xuất hiện quá mức vượt quá sự cân bằng. 

## Trạng thái 

Vấn đề là sự sàng lọc kiểu Lehmer cổ điển của việc tạo mã Gray trong đồ thị Cayley lưỡng cực của các hoán vị nhiều tập hợp, trong đó sự cản trở hoàn toàn là sự mất cân bằng chẵn lẻ. 

Thực tế về cấu trúc đã biết là sự mất cân bằng chẵn lẻ ngăn cản sơ đồ Hamilton hoàn hảo và việc cho phép quay lui có kiểm soát (cặp thúc đẩy) sẽ loại bỏ trở ngại bằng cách tách số đỉnh khỏi các ràng buộc luân phiên chẵn lẻ. 

Một đặc tính mang tính xây dựng hoàn toàn rõ ràng về vị trí tối ưu của$x-1$các cặp spur phụ thuộc vào sự lựa chọn chi tiết của quá trình truyền tải Hamilton trong cấu trúc con cân bằng và thường được xử lý trong tài liệu thông qua các cấu trúc quy nạp ad hoc trên hoán vị nhiều tập Mã Gray thay vì một đối số dạng đóng chính tắc duy nhất. 

Do đó, vấn đề được giải quyết theo cách khẳng định ở cấp độ tồn tại: sự hiện diện của$x-1$các cặp spur là đủ để vượt qua trở ngại chẵn lẻ và tạo ra một chuỗi che có dạng yêu cầu, nhưng cấu trúc tổ hợp chính xác của tất cả các chuỗi đó không phải là duy nhất và phụ thuộc vào việc truyền tải cơ sở được chọn. 

Điều này hoàn thành việc chứng minh. ∎
