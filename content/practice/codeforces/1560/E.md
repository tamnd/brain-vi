---
title: "CF 1560E - Chuyển đổi Polycarp và chuỗi"
description: "Chúng ta được cấp một chuỗi t được tạo từ một số chuỗi s gốc thông qua một chuỗi thao tác cụ thể. Bắt đầu với một chuỗi trống t, Polycarp liên tục nối s hiện tại vào t và sau đó loại bỏ tất cả các lần xuất hiện của một ký tự đã chọn khỏi s."
date: "2026-06-10T12:19:07+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "implementation", "sortings", "strings"]
categories: ["algorithms"]
codeforces_contest: 1560
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 739 (Div. 3)"
rating: 1800
weight: 1560
solve_time_s: 109
verified: false
draft: false
---

[CF 1560E - Chuyển đổi Polycarp và chuỗi](https://codeforces.com/problemset/problem/1560/E) 

**Đánh giá:** 1800 
**Thẻ:** tìm kiếm nhị phân, triển khai, sắp xếp, chuỗi 
**Thời gian giải:** 1 phút 49 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một chuỗi`t`được tạo từ một số chuỗi gốc`s`thông qua một chuỗi các hoạt động cụ thể. Bắt đầu bằng một chuỗi trống`t`, Polycarp liên tục nối thêm hiện tại`s`ĐẾN`t`và sau đó loại bỏ tất cả các lần xuất hiện của một ký tự được chọn khỏi`s`. Nhiệm vụ là đảo ngược quá trình này: đã cho`t`, xây dựng lại một bản gốc có thể`s`và thứ tự các ký tự bị xóa. 

Đầu vào bao gồm nhiều trường hợp kiểm thử, mỗi trường hợp cung cấp một chuỗi`t`lên tới 500.000 ký tự. Tổng của tất cả độ dài chuỗi được giới hạn ở mức 500.000, do đó thuật toán phải tuyến tính trong tổng độ dài chuỗi. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng tất cả các lệnh loại bỏ có thể có, bởi vì số lượng các lệnh loại bỏ có thể tăng theo giai thừa với số lượng ký tự riêng biệt trong`s`. 

Các trường hợp cạnh rất tinh tế. Ví dụ, nếu`t`chứa các mẫu lặp lại không thể phân chia nhất quán theo một thứ tự xóa duy nhất, thì không hợp lệ`s`tồn tại. Một ví dụ là`t = "nowyouknowthat"`, không thể tương ứng với bất kỳ chuỗi hoạt động hợp lệ nào theo quy tắc, vì vậy đầu ra phải là`-1`. Giải pháp bất cẩn có thể hiểu sai các chữ cái lặp đi lặp lại và đề xuất sai một chuỗi`s`điều đó không phù hợp với các quy tắc loại bỏ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử mọi thứ tự có thể để loại bỏ các ký tự khỏi`s`. Đối với mỗi đơn hàng, hãy mô phỏng quy trình của Polycarp và kiểm tra xem đơn hàng được tạo có`t`phù hợp với đầu vào. Về nguyên tắc, điều này có hiệu quả, nhưng nó theo cấp số nhân về số lượng các chữ cái riêng biệt, khiến nó không khả thi khi`t`thậm chí còn chứa 26 chữ cái khác nhau. 

Cái nhìn sâu sắc quan trọng cho một giải pháp hiệu quả là thiết kế ngược quy trình bằng cách sử dụng tần số và thứ tự các ký tự trong`t`. Quan sát rằng ký tự cuối cùng bị xóa khỏi`s`phải xuất hiện đúng một lần trong đoạn cuối cùng của`t`. Tương tự, ký tự được xóa từ thứ hai đến cuối cùng sẽ xuất hiện chính xác hai lần, một lần ở phân đoạn được nối từ thứ hai đến cuối cùng và một lần ở phân đoạn cuối cùng. Bằng cách đếm ngược và đếm số lần xuất hiện, chúng ta có thể xác định thứ tự xóa và độ dài của bản gốc.`s`. 

Giải pháp tối ưu là tuyến tính vì nó tính toán tần số ký tự, suy ra thứ tự loại bỏ và cắt chuỗi trong một lần chuyển. Chúng ta không cần phải mô phỏng từng lần loại bỏ một cách rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(26!) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(26 + n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tổng số lần xuất hiện của mỗi ký tự trong`t`và lưu ý thứ tự xuất hiện lần đầu của chúng từ trái sang phải. Thứ tự này tương ứng với thứ tự xóa ký tự trong`s`. 
2. Tính số lần xuất hiện của từng ký tự trong bản gốc`s`bằng cách chia tổng số của nó cho số lần nó xuất hiện trong`t`sau khi nối thêm. Nếu bất kỳ số đếm nào không chia hết thì quá trình này không thể thực hiện được. 
3. Xác định độ dài của chuỗi gốc`s`bằng tổng số lần xuất hiện được tính ở bước 2. 
4. Trích xuất chuỗi ban đầu`s`từ đoạn đầu tiên của`t`có chiều dài này. 
5. Xác minh rằng việc bổ sung`s`và xóa ký tự theo thứ tự suy luận sẽ tạo lại`t`. Nếu việc xác minh không thành công tại bất kỳ thời điểm nào, câu trả lời là`-1`. 

Tại sao nó hoạt động: điều bất biến là tổng số ký tự trong`t`là bội số của số phân đoạn mà nó xuất hiện. Bằng cách phân chia hợp lý và xây dựng lại`s`từ phân đoạn đầu tiên, chúng tôi đảm bảo một trình tự nhất quán. Không có chuỗi nào khác có cùng độ dài có thể đáp ứng các ràng buộc về tần số và mẫu nối/xóa được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    for _ in range(int(input())):
        t = input().strip()
        total_count = {}
        order = []
        for c in t:
```
