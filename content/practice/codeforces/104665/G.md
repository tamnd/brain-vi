---
title: "CF 104665G - Trò chơi mì Ý"
description: "Hai người chơi đang chơi trò chơi theo lượt thay đổi một số nguyên duy nhất, số sợi spaghetti hiện tại trong một đống chung. Trò chơi luôn bắt đầu từ con số 0. Lario di chuyển đầu tiên, sau đó là Muigi và họ luân phiên nhau tối đa 100 bước."
date: "2026-06-29T09:59:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104665
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 10-06-23 Div. 1 (Advanced)"
rating: 0
weight: 104665
solve_time_s: 74
verified: false
draft: false
---

[CF 104665G - Trò chơi mì spaghetti](https://codeforces.com/problemset/problem/104665/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Hai người chơi đang chơi trò chơi theo lượt thay đổi một số nguyên duy nhất, số sợi spaghetti hiện tại trong một đống chung. Trò chơi luôn bắt đầu từ con số 0. Lario di chuyển đầu tiên, sau đó là Muigi và họ luân phiên nhau tối đa 100 bước. 

Đến lượt Lario, anh ta có thể tăng số cọc bằng cách chọn một trong các kích cỡ bó được phép của mình. Đến lượt Muigi, anh ta có thể giảm cọc bằng cách chọn một trong các kích cỡ loại bỏ của mình, miễn là cọc không bị âm. Cả hai người chơi đều không được phép làm gì, nhưng vì cả hai đều có sẵn các lựa chọn tích cực nên cách chơi tối ưu sẽ không bao giờ dựa vào việc bỏ qua trừ khi bị ép buộc. 

Cách duy nhất để giành chiến thắng là Lario phải làm cho cọc đạt ít nhất một giá trị ngưỡng vào thời điểm ngay sau khi anh ta di chuyển. Nếu điều đó xảy ra trong vòng 100 vòng, trò chơi sẽ kết thúc ngay lập tức. Ngược lại Muigi được coi là người chiến thắng. 

Sự tương tác rất quan trọng vì Muigi luôn phản hồi ngay sau Lario, nghĩa là mọi lợi ích mà Lario đạt được đều có thể bị hủy bỏ một phần trước hành động tiếp theo của anh ta. Điều này khiến người ta dễ nghĩ rằng vấn đề đòi hỏi phải mô phỏng toàn bộ trò chơi. Tuy nhiên, cấu trúc này đủ đơn giản để toàn bộ quá trình có thể được rút gọn thành lý luận về những lựa chọn cực đoan. 

Các ràng buộc rất nhỏ: tất cả các kích thước gói và ngưỡng tối đa là 100 và số vòng được cố định ở 100. Điều này gợi ý rõ ràng rằng mọi giải pháp đều phải chạy trong thời gian không đổi cho mỗi thử nghiệm và chúng ta nên tránh mọi mô phỏng hoặc tìm kiếm theo chiến lược. 

Một điểm tinh tế là chiến thắng được kiểm tra ngay sau mỗi nước đi của Lario chứ không chỉ ở cuối. Điều này tạo ra vấn đề về “giá trị đỉnh cao” hơn là vấn đề về giá trị cuối cùng. 

Các trường hợp cạnh quan trọng: 

Ví dụ: nếu Lario có một gói đủ lớn để tiếp cận mục tiêu ngay lập tức`a = [20]`Và`t = 15`, sau đó Muigi không bao giờ có cơ hội đáp lại và Lario thắng ngay lập tức. Một giải pháp chỉ lý giải về trạng thái cuối cùng sau 100 vòng sẽ bỏ lỡ điều này. 

Ví dụ: nếu phần loại bỏ tốt nhất của Muigi lớn hơn phần bổ sung tốt nhất của Lario`a = [3]`Và`b = [10]`, thì bất kỳ khoản lợi nhuận nào mà Lario kiếm được sẽ ngay lập tức bị xóa đi nhiều hơn mức anh ta có thể bù đắp được, và đống tiền đó không bao giờ tăng lên một cách có ý nghĩa. Một ý tưởng “tổng hợp theo thời gian” ngây thơ sẽ gợi ý không chính xác về khả năng tăng trưởng. 

Cuối cùng, ngay cả khi nước đi tốt nhất của Lario nhỏ hơn ngưỡng, sự tích lũy lặp đi lặp lại vẫn có thể đạt được mục tiêu trước 100 vòng, vì vậy chúng ta phải xem xét các trạng thái trung gian chứ không chỉ là mức tối đa cho mỗi nước đi. 

## Phương pháp tiếp cận 

Cách diễn giải bạo lực sẽ mô phỏng mọi chuỗi hành động có thể có của cả hai người chơi. Ở mỗi lượt, Lario chọn một trong n hành động và Muigi chọn một trong m hành động, dẫn đến quá trình phân nhánh. Mặc dù độ sâu bị giới hạn bởi tổng số 200 nước đi, việc phân nhánh sẽ tạo ra số lượng trò chơi có thể thực hiện theo cấp số nhân. Điều này là không cần thiết vì cả hai người chơi rõ ràng là đối địch và mang tính quyết định về mục tiêu: Lario tối đa hóa cọc, Muigi giảm thiểu nó. 

Quan sát quan trọng là cả hai người chơi đều không có hạn chế phụ thuộc vào trạng thái ngoại trừ kích thước cọc và hành động của họ là các hằng số cộng độc lập. Do đó, lối chơi tối ưu sụp đổ thành việc luôn chọn các giá trị cực trị: Lario luôn chọn mức tối đa`a_i`, Muigi luôn chọn tối đa`b_j`. 

Khi chúng ta quy trò chơi về hai hằng số này, toàn bộ quá trình sẽ trở thành một chuỗi xác định. Câu hỏi còn lại là liệu có tồn tại một khoảnh khắc nào đó trong 100 lần Lario di chuyển đầu tiên khi cọc đạt ít nhất`t`. 

Cấu trúc của dòng thời gian cho thấy sau nước đi thứ k của Lario, trước khi Muigi phản ứng, cọc bằng:`k * A - (k - 1) * B`, Ở đâu`A = max(a_i)`Và`B = max(b_j)`. 

Chúng ta chỉ cần kiểm tra xem biểu thức này có bao giờ đạt đến`t`đối với một số người`k ≤ 100`. Vì nó tuyến tính theo k nên cực đại của nó xảy ra tại một trong các điểm cuối tùy thuộc vào dấu của`A - B`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | O(100 × nm) hoặc tệ hơn | O(1) | Quá chậm và không cần thiết | 
| Giảm cực trị tối ưu | O(n + m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm toàn bộ tương tác xuống còn hai giá trị: Mức tăng mạnh nhất có thể của Lario`A`và trận thua mạnh nhất có thể xảy ra của Muigi`B`. 

1. Tính toán`A = max(a_i)`Và`B = max(b_j)`. 

Đây là những nước đi duy nhất quan trọng trong lối chơi tối ưu vì bất kỳ nước đi nào yếu hơn đều bị thống trị nghiêm ngặt. 
2. Xem xét tình trạng sau nước đi đầu tiên của Lario. Cọc là`A`. 

Nếu điều này đã đạt đến`t`, Lario thắng ngay lập tức vì chiến thắng được kiểm tra ngay sau nước đi của anh ta. 
3. Ngược lại, hãy xem xét nước đi chung thứ k của Lario. Sau khi Lario di chuyển và trước khi Muigi phản ứng, đống đó là:`f(k) = k*A - (k-1)*B = k(A - B) + B`. 
4. Quan sát cách hàm này hoạt động trong k. 

Nếu như`A ≥ B`, hàm số tăng theo k, vì vậy cơ hội tốt nhất là tại`k = 100`. 

Nếu như`A < B`, hàm số giảm theo k, vì vậy cơ hội tốt nhất là tại`k = 1`. 
5. Đánh giá đỉnh có thể tiếp cận tốt nhất: 

Nếu`A ≥ B`, kiểm tra`100*(A - B) + B ≥ t`. 

Nếu như`A < B`, kiểm tra`A ≥ t`. 
6. Nếu một trong hai điều kiện được thỏa mãn, hãy chọn “Lario”, nếu không hãy chọn “Muigi”. 

### Tại sao nó hoạt động 

Quá trình giữa các bước di chuyển của Lario hoàn toàn tuyến tính và không cần nhớ. Mỗi chu kỳ đầy đủ đóng góp một lượng thay đổi ròng cố định của`A - B`, trong khi hành động của Muigi chỉ làm thay đổi phần bù hiện tại. Bởi vì việc kiểm tra chiến thắng duy nhất xảy ra ngay sau lượt của Lario, trò chơi giảm xuống mức tối đa hóa hàm tuyến tính trên một khoảng số nguyên giới hạn. Hàm tuyến tính trên số nguyên đạt mức tối đa tại điểm cuối, do đó chỉ cần kiểm tra vị trí có ý nghĩa đầu tiên và cuối cùng. Điều này loại bỏ mọi khả năng xảy ra cực đại trung gian ẩn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, t = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    A = max(a)
    B = max(b)

    # peak after first Lario move
    if A >= t:
        print("Lario")
        return

    if A >= B:
        be
```
