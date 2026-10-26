---
title: "CF 106094A - Ma Trận Bel Lotus"
description: "Bài toán đưa ra một lưới 5 x 5 chứa chính xác một ô có giá trị 1 và tất cả các ô khác chứa 0. Một bước di chuyển bao gồm việc hoán đổi hai hàng lân cận hoặc hoán đổi hai cột lân cận."
date: "2026-06-25T12:02:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106094
codeforces_index: "A"
codeforces_contest_name: "SVU-HIAST CPC 2025"
rating: 0
weight: 106094
solve_time_s: 34
verified: false
draft: false
---

[CF 106094A - Ma trận Bel Lotus](https://codeforces.com/problemset/problem/106094/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Bài toán đưa ra một lưới 5 x 5 chứa chính xác một ô có giá trị`1`và tất cả các ô khác có chứa`0`. Một bước di chuyển bao gồm việc hoán đổi hai hàng lân cận hoặc hoán đổi hai cột lân cận. Mục tiêu là di chuyển các`1`vào ô trung tâm của lưới, là vị trí ở hàng 3 và cột 3 bằng cách sử dụng chỉ mục một cơ sở. Chúng tôi cần số lượng giao dịch hoán đổi tối thiểu cần thiết. 

Chi tiết quan trọng là việc hoán đổi hàng chỉ thay đổi vị trí hàng của`1`và hoán đổi cột chỉ thay đổi vị trí cột của`1`. Hai phong trào là độc lập. Nếu`1`hiện đang ở hàng`r`, nó cần chính xác`|r - 3|`hoán đổi hàng để đến hàng giữa. Tương tự, nếu nó ở cột`c`, nó cần chính xác`|c - 3|`hoán đổi cột để đến cột giữa. 

Kích thước đầu vào là cố định, ma trận luôn là 5 x 5 nên toàn bộ đầu vào chỉ chứa 25 giá trị. Điều này có nghĩa là ngay cả việc quét toàn bộ ma trận cũng không đáng kể. Chúng tôi không cần cấu trúc dữ liệu nâng cao hoặc tối ưu hóa. Bất kỳ giải pháp nào xử lý từng ô một lần đều dễ dàng nằm trong giới hạn. Ngay cả một giải pháp thực hiện một lượng công việc nhỏ không đổi trên mỗi ô cũng sẽ hoàn thành ngay lập tức. 

Các trường hợp cạnh chính đến từ vị trí của`1`và từ việc giả định rằng câu trả lời đòi hỏi phải mô phỏng các giao dịch hoán đổi. Trường hợp đầu tiên là khi ma trận đã được căn giữa. 

Ví dụ:```
0 0 0 0 0
0 0 0 0 0
0 0 1 0 0
0 0 0 0 0
0 0 0 0 0
```Đầu ra là:```
0
```Việc triển khai bất cẩn có thể thực hiện các bước di chuyển không cần thiết vì nó chỉ kiểm tra xem giá trị có tồn tại hay không chứ không phải liệu nó có ở vị trí mục tiêu hay không. 

Một trường hợp cạnh khác là khi`1`đang ở một góc. 

Ví dụ:```
1 0 0 0 0
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
```Đầu ra là:```
4
```các`1`phải di chuyển hai hàng xuống và hai cột sang phải. Nếu chỉ tính số lần di chuyển hàng hoặc chỉ tính số lần di chuyển cột thì câu trả lời sẽ không đầy đủ. 

Một lỗi phổ biến cuối cùng là trộn lẫn các tọa độ dựa trên 0 và một dựa trên. Với lập chỉ mục dựa trên 0, mục tiêu là`(2, 2)`, trong khi với lập chỉ mục dựa trên một thì nó là`(3, 3)`. Sử dụng sai mục tiêu sẽ làm thay đổi từng câu trả lời một. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng các giao dịch hoán đổi. Chúng tôi có thể xác định vị trí`1`, sau đó liên tục hoán đổi nó với hàng hoặc cột liền kề để di chuyển nó đến gần tâm hơn. Điều này sẽ hiệu quả vì mỗi lần hoán đổi sẽ thay đổi vị trí của`1`bằng chính xác một bước theo một hướng và di chuyển thẳng về phía mục tiêu luôn là tối ưu. 

Tuy nhiên, mô phỏng là không cần thiết. Vì lưới rất nhỏ nên quan sát hữu ích hơn là số lần di chuyển cần thiết chỉ phụ thuộc vào khoảng cách từ vị trí hiện tại đến tâm. Nếu`1`cách nhau ba hàng và hai cột, không có chuỗi hoán đổi nào có thể thực hiện tốt hơn năm nước đi vì mỗi nước đi chỉ thay đổi từng tọa độ một. 

Ở đây, mô phỏng lực lượng vũ phu vẫn sẽ nhanh chóng, nhưng ý tưởng tương tự sẽ trở nên không hiệu quả trên các lưới lớn hơn. Phương pháp mô phỏng tổng quát dành thời gian thực hiện các thao tác không tiết lộ thông tin mới. Vị trí của`1`đã cho chúng ta biết câu trả lời. 

Giải pháp tối ưu là quét ma trận, tìm tọa độ của`1`, và tính khoảng cách Manhattan đến tâm. Khoảng cách Manhattan hoạt động vì các hoạt động được phép chính xác là di chuyển đơn vị theo chiều dọc và chiều ngang. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(câu trả lời) | O(1) | Được chấp nhận, nhưng không cần thiết | 
| Tối ưu | O(25) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả 25 ô của ma trận và tìm hàng và cột có giá trị`1`xuất hiện. Toàn bộ vấn đề được xác định bởi vị trí duy nhất này. 
2. Tính khoảng cách từ hàng hiện tại đến hàng giữa. Sử dụng lập chỉ mục dựa trên số 0, hàng giữa là chỉ mục`2`, vậy khoảng cách này là`abs(row - 2)`. 
3. Tính khoảng cách cột từ cột hiện tại đến cột giữa. Cột giữa cũng là chỉ mục`2`, vậy khoảng cách này là`abs(col - 2)`. 
4. Cộng hai khoảng cách và in kết quả. Mỗi lần di chuyển hàng và di chuyển cột được yêu cầu đóng góp chính xác một thao tác, do đó tổng của chúng là số lượng thao tác tối thiểu. 

Tại sao nó hoạt động: 

Mỗi nước đi hợp lệ sẽ thay đổi chính xác một tọa độ của`1`bởi một. Để đến trung tâm, tọa độ hàng phải thay đổi từ giá trị hiện tại thành`2`, yêu cầu`abs(row - 2)`di chuyển. Lý do tương tự áp dụng cho cột. Vì các thao tác hàng và cột không ảnh hưởng lẫn nhau nên việc thực hiện tất cả các thao tác di chuyển hàng và cột được yêu cầu sẽ đến trung tâm với số lượng thao tác được tính toán chính xác. Không thể có số nhỏ hơn vì mỗi lần di chuyển có thể giảm tổng khoảng cách tối đa một.
