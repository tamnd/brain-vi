---
title: "CF 1215B - Số lượng sản phẩm"
description: "Chúng ta được cho một dãy các số nguyên khác 0 và được yêu cầu đếm xem có bao nhiêu mảng con tạo ra tích dương và bao nhiêu mảng con tạo ra tích âm. Một mảng con được xác định bằng cách chọn hai chỉ số $l le r$ và nhân mọi thứ từ $al$ đến $ar$."
date: "2026-06-13T17:32:07+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1215
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 585 (Div. 2)"
rating: 1400
weight: 1215
solve_time_s: 218
verified: true
draft: false
---

[CF 1215B - Số lượng sản phẩm](https://codeforces.com/problemset/problem/1215/B) 

**Đánh giá:** 1400 
**Tags:** tổ hợp, dp, triển khai 
**Thời gian giải:** 3 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các số nguyên khác 0 và được yêu cầu đếm xem có bao nhiêu mảng con tạo ra tích dương và bao nhiêu mảng con tạo ra tích âm. Một mảng con được xác định bằng cách chọn hai chỉ số$l \le r$và nhân mọi thứ từ$a_l$ĐẾN$a_r$. 

Bởi vì mọi số đều khác 0 nên dấu của bất kỳ tích nào cũng chỉ phụ thuộc vào số lượng phần tử âm bên trong mảng con. Số âm chẵn làm cho tích số dương và số lẻ làm cho tích số âm. Độ lớn không bao giờ quan trọng. 

Ràng buộc$n \le 2 \cdot 10^5$loại trừ bất kỳ phép liệt kê bậc hai nào của các mảng con. Có khoảng$n(n+1)/2$mảng con, đó là về$2 \cdot 10^{10}$ở kích thước đầu vào tối đa. Bất kỳ giải pháp nào kiểm tra rõ ràng từng phạm vi con hoặc nhân đều quá chậm. 

Một nỗ lực ngây thơ thường thất bại một cách tinh tế: ngay cả khi tránh phép nhân và chỉ theo dõi các dấu hiệu, lặp lại tất cả$l, r$cặp vẫn vượt quá giới hạn. Một lỗi phổ biến khác là tính lại tính chẵn lẻ dấu hiệu từ đầu cho mỗi$r$, dẫn đến$O(n^2)$hành vi. 

Các trường hợp cạnh quan trọng ở đây bao gồm các mảng toàn dương, trong đó mọi mảng con đều dương và các dấu hiệu xen kẽ, trong đó tính chẵn lẻ thường xuyên bị đảo ngược. Ví dụ: trong một đầu vào như$[-1, -1, -1]$, mảng con$[1,3]$có sản phẩm$-1$, Nhưng$[1,2]$là tích cực. Bất kỳ giải pháp nào cũng phải theo dõi tính chẵn lẻ một cách nhất quán thay vì tính toán lại nó. 

## Phương pháp tiếp cận 

Phương pháp brute-force xem xét mọi cặp$(l, r)$, đếm số phần tử âm bên trong và quyết định dấu của tích. Điều này đúng về mặt logic vì dấu chỉ phụ thuộc vào tính chẵn lẻ. Tuy nhiên, việc đếm số âm bên trong mỗi mảng con yêu cầu phải quét tới$O(n)$các yếu tố, tạo ra một$O(n^3)$giải pháp nếu được thực hiện trực tiếp, hoặc$O(n^2)$với tối ưu hóa tiền tố. Với$n = 2 \cdot 10^5$, thậm chí$10^10$hoạt động vượt xa thời gian chạy khả thi. 

Cái nhìn sâu sắc quan trọng là tránh nhìn vào các mảng con một cách rõ ràng. Thay vào đó, chúng tôi theo dõi có bao nhiêu mảng con kết thúc ở mỗi vị trí có số âm chẵn hoặc lẻ. Khi chúng tôi biết điều này cho từng điểm cuối$r$, chúng ta có thể tích lũy câu trả lời. 

Sự chuyển đổi xuất phát từ việc sắp xếp lại vấn đề dưới dạng tính chẵn lẻ tiền tố. Nếu chúng ta xác định một giá trị chẵn lẻ tiền tố sẽ lật bất cứ khi nào chúng ta nhìn thấy số âm, thì một mảng con có tích âm chính xác khi các điểm cuối của nó có giá trị chẵn lẻ khác nhau và tích dương khi chúng khớp. Điều này làm giảm vấn đề khi đếm các cặp trạng thái tiền tố bằng nhau hoặc khác nhau, có thể được thực hiện trong thời gian tuyến tính bằng cách duy trì số lượng tiền tố có mỗi trạng thái chẵn lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$hoặc tệ hơn |$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi vấn đề sang đếm trạng thái chẵn lẻ tiền tố. 

1. Chúng tôi xác định một biến đang chạy theo dõi tính chẵn lẻ của số phần tử âm được thấy cho đến nay. Tính chẵn lẻ này là chẵn hoặc lẻ và đảo ngược bất cứ khi nào chúng ta gặp số âm. Điều này cho chúng ta một biểu diễn nhỏ gọn của tất cả các mảng con trong quá khứ kết thúc ở mỗi chỉ mục. 
2. Chúng tôi duy trì hai bộ đếm: có bao nhiêu vị trí tiền tố có tính chẵn lẻ và bao nhiêu vị trí có tính chẵn lẻ. Ban đầu, trước khi xử lý mảng, tiền tố trống có tính chẵn lẻ, do đó số chẵn bắt đầu từ 1 và số lẻ bắt đầu từ 0. 
3. Chúng tôi lặp qua mảng và cập nhật tính chẵn lẻ hiện tại. Nếu phần tử hiện tại âm, chúng ta lật tính chẵn lẻ; nếu không, chúng tôi giữ nó không thay đổi. 
4. Tại mỗi vị trí, chúng ta quyết định có bao nhiêu mảng con kết thúc ở đây là dương hoặc âm. Nếu chẵn lẻ hiện tại là chẵn thì các mảng con kết thúc ở đây bằng tích dương tương ứng với các tiền tố chẵn trước đó và mảng con âm tương ứng với các tiền tố lẻ trước đó. Nếu tính chẵn lẻ hiện tại là số lẻ thì các vai trò sẽ được hoán đổi. 
5. Chúng tôi tích lũy những số liệu này thành các câu trả lời toàn cầu. 
6. Sau khi xử lý phần tử, chúng tôi cập nhật tần số của tính chẵn lẻ của tiền tố hiện tại để các vị trí trong tương lai có thể sử dụng nó. 

Lý do thứ tự này quan trọng là vì các mảng con kết thúc ở chỉ mục$i$phụ thuộc hoàn toàn vào thông tin tiền tố trước hoặc bao gồm$i$, vì vậy các cập nhật phải diễn ra sau khi tính các khoản đóng góp. 

### Tại sao nó hoạt động 

Mỗi mảng con$(l, r)$tương ứng với một cặp trạng thái tiền tố: tính chẵn lẻ tại$r$và sự chẵn lẻ ngay trước đó$l$. Dấu tích chỉ phụ thuộc vào việc các số chẵn lẻ này bằng nhau hay khác nhau. Bằng cách đếm xem có bao nhiêu tiền tố của mỗi loại tồn tại ở mỗi bước, chúng ta ngầm đếm tất cả các mảng con hợp lệ chính xác một lần. Không có mảng con nào bị bỏ sót vì mọi điểm cuối đều được xử lý và không có mảng con nào được tính gấp đôi vì mỗi cặp tiền tố xác định chính xác một phân đoạn. 

## Giải pháp Python```
PythonRun
```Mã này duy trì tính chẵn lẻ của các số âm và sử dụng số lượng tiền tố để xác định có bao nhiêu tiền tố trước đó kết hợp với vị trí hiện tại để tạo thành các mảng con dương hoặc âm. Điều tinh tế quan trọng là cập nhật câu trả lời trước khi tăng bộ đếm tiền tố, đảm bảo vị trí hiện tại không bị tính sai là tiền tố trước. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```

```Chúng tôi theo dõi tính chẵn lẻ và số lượng tiền tố. 

| Chỉ mục | Giá trị | Chẵn lẻ | Tính chẵn | Số lẻ | Đã thêm tích cực | Đã thêm tiêu cực | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 0 | 1 | 0 | 1 | 0 | 
| 2 | -3 | 1 | 1 | 1 | 0 | 1 | 
| 3 | 3 | 1 | 1 | 2 | 1 | 1 | 
| 4 | -1 | 0 | 2 | 2 | 2 | 2 | 
| 5 | 1 | 0 | 3 | 2 | 3 | 2 | 

Kết quả cuối cùng là âm = 8, dương = 7. 

Dấu vết này cho thấy mỗi trạng thái tiền tố đóng góp theo cách kết hợp thay vì liệt kê các mảng con riêng lẻ như thế nào. 

Một ví dụ thứ hai:```

```| Chỉ mục | Giá trị | Chẵn lẻ | Tính chẵn | Số lẻ | Đã thêm tích cực | Đã thêm tiêu cực | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | -1 | 1 | 1 | 1 | 0 | 1 | 
| 2 | -1 | 0 | 2 | 1 | 2 | 1 | 
| 3 | -1 | 1 | 2 | 2 | 2 | 2 | 

Đáp án cuối cùng: âm = 4, dương = 2. 

Điều này chứng tỏ tính chẵn lẻ luân phiên cân bằng số đếm một cách tự nhiên như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| truyền một lần qua mảng với các cập nhật liên tục | 
| Không gian |$O(1)$| chỉ các bộ đếm cho số chẵn lẻ và tiền tố | 

Quét tuyến tính phù hợp thoải mái trong các ràng buộc cho$n \le 2 \cdot 10^5$và mức sử dụng bộ nhớ là không đổi. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| toàn mảng dương | 0 10 | tính đúng đắn của tổ hợp cơ sở | 
| toàn mảng âm | 4 2 | xử lý chẵn lẻ | 
| tích cực duy nhất | 0 1 | tính đúng đắn của trường hợp cơ sở | 
| âm đơn | 1 0 | trường hợp tiêu cực tối thiểu | 

## Vỏ cạnh 

Một mảng toàn dương giữ tính chẵn lẻ cố định ở mức 0 trong suốt quá trình thực thi. Mỗi mảng con kết hợp với hai tiền tố chẵn, vì vậy mỗi phân đoạn đều đóng góp vào số dương. Thuật toán chỉ tích lũy chính xác những đóng góp dương vì bộ đếm lẻ vẫn bằng 0. 

Một mảng toàn âm xen kẽ tính chẵn lẻ ở mỗi bước. Đối với đầu vào`[-1, -1, -1]`, tính chẵn lẻ của tiền tố sẽ lật từng chỉ mục, gây ra sự trộn lẫn có hệ thống giữa số lượng tiền tố chẵn và lẻ. Thuật toán nắm bắt tất cả các cặp giữa các trạng thái chẵn lẻ khác nhau, tạo ra chính xác số lượng mảng con âm dự kiến ​​trong khi vẫn đếm các mảng dương được hình thành bởi các phân đoạn có độ dài chẵn.
