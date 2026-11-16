---
title: "CF 1919G - Cây LGM"
description: "Chúng ta được yêu cầu xây dựng lại một cái cây từ ma trận kết quả trò chơi. Mỗi mục s[i][j] cho biết liệu người chơi đầu tiên có thể giành chiến thắng hay không khi cây có rễ ở đỉnh i và đồng xu bắt đầu ở đỉnh j. Một đỉnh chỉ có thể di chuyển đồng xu đến con của nó."
date: "2026-06-08T19:36:18+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "divide-and-conquer", "games", "trees"]
categories: ["algorithms"]
codeforces_contest: 1919
codeforces_index: "G"
codeforces_contest_name: "Hello 2024"
rating: 3500
weight: 1919
solve_time_s: 66
verified: false
draft: false
---

[CF 1919G - Cây LGM](https://codeforces.com/problemset/problem/1919/G) 

**Đánh giá:** 3500 
**Tags:** thuật toán xây dựng, chia để trị, trò chơi, cây cối 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng lại một cái cây từ ma trận kết quả trò chơi. Mỗi mục`s[i][j]`cho biết liệu người chơi đầu tiên có thể giành chiến thắng hay không khi cây bị rễ ở đỉnh`i`và đồng xu bắt đầu ở đỉnh`j`. Một đỉnh chỉ có thể di chuyển đồng xu đến con của nó. Chúng ta phải xác định xem một cây có tồn tại phù hợp với tất cả các mục của`s`và, nếu vậy, hãy xây dựng một cái. 

Ma trận đầu vào có kích thước lên tới 5000×5000. Điều này rất quan trọng vì bất kỳ thuật toán nào kiểm tra từng cặp đỉnh nhiều lần đều có thể đạt tới 25 triệu thao tác mỗi lần vượt qua. Vì vậy, một$O(n^2)$thuật toán có thể chấp nhận được, nhưng bất cứ điều gì cao hơn, chẳng hạn như$O(n^3)$, thì không. 

Các trường hợp cạnh không rõ ràng bao gồm: 

- Một ma trận không nhất quán, ví dụ: một ô`s[i][i] = 1`khi một nút không có nút con nào trong cây cuối cùng. Điều này cho thấy chiến thắng bắt đầu từ lá, điều này là không thể. 
- Ràng buộc đối xứng: nếu một nút`j`là một chiếc lá trên cây có rễ ở`i`, thì bất kỳ`s[i][j]`phải là`0`vì người chơi đầu tiên không thể di chuyển. Bất kỳ cách tiếp cận bất cẩn nào bỏ qua những bất biến này đều có thể tạo ra một cây không hợp lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng thử tất cả$n^{n-2}$cây có thể, kiểm tra từng gốc và mô phỏng trò chơi cho tất cả các vị trí đặt đồng xu. Điều này là không thể ngay cả đối với nhỏ`n`do sự bùng nổ tổ hợp. 

Thông tin chi tiết quan trọng là sử dụng **số Grundy / thuộc tính trò chơi Sprague-Grundy** của trò chơi cây này. Trạng thái thắng/thua của một nút phụ thuộc hoàn toàn vào kết quả của các nút con của nó. Đặc biệt, đối với nút không có nút con, người chơi đầu tiên sẽ thua. Đối với nút có trẻ em, người chơi đầu tiên sẽ thắng nếu có ít nhất một trẻ dẫn đến vị trí thua cho người chơi thứ hai. Bằng cách sử dụng điều này, chúng ta có thể gắn nhãn đệ quy cho các đỉnh và mối quan hệ thắng/thua của chúng. Ma trận`s`có thể được hiểu là các ràng buộc về hoán vị các nút dựa trên chiều cao của chúng theo một thứ tự tuyến tính nào đó. Điều này cho phép chúng ta xây dựng cây ứng cử viên bằng cách nhóm các nút theo các hàng giống nhau trong`s`và sau đó liên kết chúng theo kiểu phân cấp. 

Điều này làm giảm vấn đề xuống còn$O(n^2)$thuật toán: so sánh các hàng, sắp xếp các nút theo mẫu hàng và liên kết từng nút với hàng nhỏ hơn tiếp theo trong chuỗi tuyến tính. Bất kỳ sự không nhất quán nào trong ma trận (ví dụ: một hàng yêu cầu cha mẹ nhưng chính nó lại là một lá) đều được phát hiện trong quá trình xây dựng này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^{n}) | O(n^2) | Quá chậm | 
| Tối ưu | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`và ma trận`s`kích thước`n×n`. Chuyển đổi mỗi hàng thành một bộ số nguyên để so sánh hiệu quả. 
2. Đối với mỗi hàng, hãy tính toán một mẫu có thể băm biểu thị chữ ký thắng/thua của nó. Các nút có hàng giống nhau sẽ tương đương trong hệ thống phân cấp cây. 
3. Sắp xếp các nút theo mẫu hàng của chúng. Điều này cho phép ứng viên sắp xếp thứ tự từ lá đến gốc, bởi vì một lá có tất cả`0`s ngoại trừ có thể ở vị trí riêng của nó. 
4. Cố gắng xây dựng cây bằng cách liên kết mỗi nút với một nút có mẫu nhỏ hơn (tức là nút cha có chữ ký phù hợp với việc có nút này khi còn nhỏ). Nếu không có nút gốc nào tồn tại cho nút không phải gốc, hãy báo cáo "KHÔNG". 
5. Nếu việc xây dựng thành công, xuất ra "YES" và danh sách các cạnh. 

**Tại sao nó hoạt động:** Trong trò chơi trên cây, trạng thái chiến thắng của người chơi đầu tiên tại một nút được xác định duy nhất bởi kết quả của các nút con của nút đó. Việc nhóm các nút theo các hàng giống hệt nhau sẽ tôn trọng sự phụ thuộc này và việc liên kết
