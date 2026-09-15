---
title: "CF 104686D - Phá rừng"
description: "Chúng ta có một cây có gốc trong đó mỗi nút đại diện cho một đoạn vật lý của một cấu trúc lớn bằng gỗ. Mỗi đoạn có trọng số và có thể chia thành nhiều đoạn con ở cuối."
date: "2026-06-29T08:50:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104686
codeforces_index: "D"
codeforces_contest_name: "2022-2023 ICPC Central Europe Regional Contest (CERC 22)"
rating: 0
weight: 104686
solve_time_s: 29
verified: false
draft: false
---

[CF 104686D - Phá rừng](https://codeforces.com/problemset/problem/104686/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 29s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc trong đó mỗi nút đại diện cho một đoạn vật lý của một cấu trúc lớn bằng gỗ. Mỗi đoạn có trọng số và có thể chia thành nhiều đoạn con ở cuối. Gốc đại diện cho thân cây được kết nối với mặt đất và mỗi nút đóng góp trọng lượng của nó như một phần của toàn bộ cây. 

Chúng ta được phép chặt cây ở những điểm tùy ý dọc theo các cạnh hoặc trong các đoạn. Sau khi cắt, cây chia thành các thành phần được kết nối và mỗi thành phần có tổng trọng lượng bằng tổng trọng số của các nút của nó. Mục đích là phân chia cây thành số phần nhỏ nhất sao cho mỗi phần thu được có tổng trọng lượng tối đa là W. 

Đầu ra chính là số lượng các thành phần được kết nối sau khi thực hiện một bộ cắt tối ưu. 

Kích thước đầu vào lớn về mặt cấu trúc. Có tối đa 10^5 phân đoạn và tổng trọng số được giới hạn bởi 10^9. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại tổng cây con nhiều lần một cách ngây thơ. Cách tiếp cận bậc hai trên các nút hoặc tính toán lại DFS lặp đi lặp lại sẽ vượt quá giới hạn. 

Cấu trúc cũng ẩn chứa một sự tinh tế: việc phân nhánh không có gì đặc biệt, nó chỉ là một cái cây, nhưng các vết cắt không bị giới hạn ở các cạnh theo nghĩa chặt chẽ. Tuy nhiên, vì việc cắt bên trong một cạnh tương đương với việc cắt ở ranh giới cạnh về mặt các thành phần và trọng số được kết nối, nên vấn đề giảm xuống còn việc quyết định vị trí tách các cây con. 

Một số trường hợp đặc biệt quan trọng. 

Một nút có trọng lượng vượt quá W không thể gói thành bất kỳ mảnh nào nếu không cắt bên trong nó, nhưng vì việc cắt có thể tùy ý, nút đó phải được chia thành nhiều mảnh dọc theo cấu trúc bên trong của nó, làm tăng số lượng mảnh một cách hiệu quả ngay cả trước khi xem xét phần tử con. 

Một nút hình ngôi sao có nhiều nút con, mỗi nút nhỏ, vẫn có thể thực hiện nhiều lần cắt nếu tổng trọng số vượt quá W. 

Một chuỗi dài trong đó trọng số tích lũy hầu như không vượt quá W tại nhiều điểm là một trường hợp khác trong đó các quyết định nhóm tham lam đóng vai trò quan trọng. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là tính trọng số của cây con và cố gắng quyết định một cách tham lam cho mỗi nút về cách nhóm các nút con của nó thành các khối hợp lệ. Người ta có thể thử mô phỏng việc đóng gói các cây con vào các thùng có dung lượng W, kết hợp chúng một cách tùy ý. 

Ý tưởng brute-force là coi mỗi nút cần phân phối các nút con của nó thành các nhóm sao cho mỗi nhóm cộng với trọng số nút nằm trong W, sau đó tính toán đệ quy việc nhóm tối ưu cho mỗi nút. Điều này nhanh chóng trở thành tổ hợp vì mỗi nút có bậc d có thể yêu cầu phân chia các con của nó thành các tập con, là số mũ của d trong trường hợp xấu nhất. Với tổng số nút lên tới 10^5, điều này là không khả thi. 

Quan sát chính là về cơ bản đây là một vấn đề giống như cái ba lô, nhưng có cấu trúc đơn điệu mạnh mẽ: chúng ta không bao giờ cần xem xét các nhóm trẻ em tùy ý trên toàn cầu. Thay vào đó, sự tối ưu xuất hiện từ việc xử lý các phần tử con trong DFS và tích lũy những đóng góp của chúng một cách tham lam một cách có kiểm soát. 

Chúng ta root cây và tính tổng của cây con. Nếu một cây con vượt quá W, nó phải được phân chia nội bộ, đóng góp các phần bổ sung độc lập với cây mẹ. Nếu không, nó có thể được sáp nhập lên trên thành phần của cha mẹ. 

Tại mỗi nút, chúng tôi đang quyết định một cách hiệu quả có bao nhiêu “đoạn có thể mang theo” của cây con của nó có thể được hợp nhất lên trên mà không vượt quá W. Bất kỳ phần thừa nào sẽ trở thành một phần mới. 

Điều này làm giảm vấn đề về việc duyệt theo thứ tự sau trong đó mỗi cây con trả về một trọng số dư mà vẫn có thể được gắn lên trên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Nhóm tập hợp con ngây thơ trên mỗi nút | Hàm mũ | O(n) | Quá chậm | 
| Tập hợp tham lam sau thứ tự | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta coi cây như có gốc ở thân và tính toán DFS từ gốc.

1. Thực hiện duyệt theo thứ tự sau để tất cả các cây con con được xử lý trước cây cha của chúng. Điều này đảm bảo chúng tôi luôn biết được “trọng lượng mang theo dư” của từng trẻ trước khi
