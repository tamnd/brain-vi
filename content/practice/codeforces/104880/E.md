---
title: "CF 104880E - Dịch vụ \u7684\u6811"
description: "Chúng ta được cấp một cây có các nút được gắn nhãn $n$. Chúng tôi được phép thực hiện nhiều lần một thao tác trong đó chúng tôi chọn một nút hiện có, xóa nó và xóa tất cả các cạnh liên quan đến nó. Mỗi thao tác như vậy được tính là một bước."
date: "2026-06-28T09:21:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104880
codeforces_index: "E"
codeforces_contest_name: "The 18-th Beihang University Collegiate Programming Contest (BCPC 2023) - Preliminary"
rating: 0
weight: 104880
solve_time_s: 31
verified: false
draft: false
---

[CF 104880E - Dịch vụ \u7684\u6811](https://codeforces.com/problemset/problem/104880/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 31s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với$n$các nút được dán nhãn. Chúng tôi được phép thực hiện nhiều lần một thao tác trong đó chúng tôi chọn một nút hiện có, xóa nó và xóa tất cả các cạnh liên quan đến nó. Mỗi thao tác như vậy được tính là một bước. Sau khi thực hiện bất kỳ thao tác nào, chúng ta xem xét phần còn lại của biểu đồ: một số nút và một số cạnh giữa chúng. 

Mục tiêu là giảm thiểu chi phí kết hợp bao gồm hai phần. Đầu tiên là số lượng thao tác xóa được thực hiện. Thứ hai là số cạnh vẫn còn trong biểu đồ sau khi xóa tất cả. Nhiệm vụ là chọn những đỉnh cần loại bỏ sao cho tổng của hai đại lượng này càng nhỏ càng tốt. 

Tương tác chính là việc xóa một đỉnh sẽ loại bỏ tất cả các cạnh liên quan của nó ngay lập tức, do đó mọi cạnh đều bị loại vì ít nhất một điểm cuối bị xóa hoặc tồn tại vì cả hai điểm cuối đều được giữ lại. Do đó, các cạnh còn lại chính xác là những cạnh được tạo ra bởi tập hợp các đỉnh mà chúng ta quyết định không xóa. 

Vì đầu vào là một cái cây nên ban đầu có$n-1$cạnh và không có chu trình. Ràng buộc$n \le 5 \cdot 10^5$ngụ ý bất kỳ giải pháp nào về cơ bản phải là tuyến tính hoặc$O(n \log n)$. Bậc hai hoặc thậm chí$O(n^{1.5})$cách tiếp cận là không thể bởi vì chúng sẽ vượt quá khoảng$10^8$hoạt động trong một giây. 

Một cách giải thích ngây thơ sẽ thử giữ hoặc xóa tất cả các tập hợp con của đỉnh, tính toán các cạnh cảm ứng và đánh giá chi phí. Đó là theo cấp số nhân và ngay lập tức không thể thực hiện được. 

Một vấn đề tế nhị hơn xuất hiện khi suy nghĩ cục bộ. Ví dụ, người ta có thể cố gắng xóa các lá hoặc nút cấp cao một cách tham lam mà không xem xét cấu trúc tổng thể. Trên một con đường đơn giản như$1 - 2 - 3 - 4$, việc xóa nút giữa sẽ làm giảm các cạnh hiệu quả hơn so với xóa điểm cuối, nhưng mẫu tối ưu phụ thuộc vào cách thao tác xóa tương tác trên toàn bộ cây, do đó, các quy tắc tham lam thuần túy cục bộ có thể thất bại. 

## Phương pháp tiếp cận 

Vấn đề trở nên rõ ràng hơn nếu chúng ta diễn giải lại chi phí theo tập hợp các đỉnh được giữ cuối cùng. Giả sử chúng ta quyết định giữ một tập hợp con$S$. Khi đó số thao tác bằng$n - |S|$, vì mỗi đỉnh bị xóa đều đóng góp chính xác một thao tác. Các cạnh còn lại chính xác là những cạnh có cả hai điểm cuối trong$S$, là số cạnh trong đồ thị con cảm ứng$G[S]$. 

Vì vậy, mục tiêu trở thành:$$(n - |S|) + |E(S)| = n + (|E(S)| - |S|)$$Từ$n$đã được khắc phục, chúng tôi đang giảm thiểu:$$|E(S)| - |S|$$Bây giờ chúng ta làm việc hoàn toàn với đồ thị con cảm ứng. Đối với cây, biểu thức này có cấu trúc rất quan trọng: mỗi thành phần được kết nối trong$S$vẫn là cây (vì chỉ xóa đỉnh chứ không tạo chu trình). Nếu một thành phần có$k$đỉnh thì nó có$k-1$cạnh, đóng góp:$$(k-1) - k = -1$$Vì vậy, mọi thành phần được kết nối đều đóng góp chính xác$-1$tới mục tiêu, bất kể quy mô của nó. Do đó, tối đa hóa số lượng thành phần liên thông trong tập đỉnh đã chọn$S$giảm thiểu chi phí. 

Vấn đề giảm xuống còn việc chọn một tập hợp con các đỉnh sao cho số lượng thành phần liên thông được tạo ra trong cây là lớn nhất. 

Bây giờ hãy quan sát những gì kiểm soát khả năng kết nối: một thành phần biến mất khi việc xóa đỉnh chia tách nó. Mỗi đỉnh được giữ sẽ tạo ra các kết nối thông qua các cạnh; việc loại bỏ một đỉnh sẽ làm tăng số lượng thành phần bằng số phần tách biệt mà nó tạo ra giữa các lân cận của nó trong tập hợp được giữ lại. 

Điều này dẫn đến một công thức DP cây tiêu chuẩn trong đó chúng ta root cây và quyết định xem mỗi nút được giữ lại hay bị xóa. Nếu một nút được giữ lại, nó có thể kết nối nhiều cây con được giữ lại thành một thành phần; nếu nó bị xóa, nó sẽ tách mọi thứ bên dưới nó. 

Cấu trúc tối ưu cuối cùng bị chi phối bởi một trạng thái đơn giản trên mỗi nút: nút đó có được bao gồm hay không và có bao nhiêu “kết nối hoạt động” mà nó đóng góp trở lên. Giải pháp cuối cùng là tính toán, thông qua DP, đóng góp tốt nhất có thể đạt được của mỗi cây con theo hai lựa chọn này. 

Lực lượng vũ phu sẽ liệt kê việc giữ/xóa cho mọi nút và tính toán lại các thành phần, tính chi phí$O(2^n)$. Việc quan sát thấy mục tiêu phân rã trên các thành phần được kết nối cho phép cây DP theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Cây DP |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây ở nút 1 và tính giá trị DP cho mỗi nút. 

Chúng ta hãy xác định hai trạng thái cho mỗi nút$u$: 

1.$dp_0[u]$: đóng góp tốt nhất của cây con của$u$nếu như$u$bị xóa. 
2.$dp_1[u]$: đóng góp tốt nhất nếu$u$được giữ lại. 

Chúng tôi tính toán những điều này từ dưới lên. 

1. Khởi tạo$dp_0[u] = 0$cho tất cả các nút, bởi vì việc xóa$u$loại bỏ nó và chúng tôi chỉ kết hợp những đóng góp của trẻ em thành các bài toán con riêng biệt._
