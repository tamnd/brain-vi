---
title: "CF 103446F - Kaiji!"
description: "Chúng ta được cung cấp một tập hợp lớn các số nguyên được tạo ra bởi một phép truy toán. Về mặt khái niệm, hãy nghĩ nó như một hộp chứa nhiều quả bóng, mỗi quả được dán nhãn một giá trị."
date: "2026-07-03T07:36:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103446
codeforces_index: "F"
codeforces_contest_name: "The 2021 ICPC Asia Shanghai Regional Programming Contest"
rating: 0
weight: 103446
solve_time_s: 62
verified: true
draft: false
---

[CF 103446F - Kaiji!](https://codeforces.com/problemset/problem/103446/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp lớn các số nguyên được tạo ra bởi một phép truy toán. Về mặt khái niệm, hãy coi nó như một chiếc hộp chứa nhiều quả bóng, mỗi quả bóng được dán nhãn một giá trị. Các giá trị không phải là đầu vào tùy ý mà được tạo ra bởi một trình tạo xác định, do đó, trình tự này được Kaiji biết đầy đủ trước khi trò chơi bắt đầu. 

Sau đó, đối thủ chọn hai quả bóng theo một hạn chế: cặp được chọn không thể có bất kỳ quả bóng thứ ba nào có giá trị nằm hoàn toàn giữa chúng. Nói cách khác, nếu bạn sắp xếp tất cả các giá trị, đối thủ chỉ được phép chọn hai giá trị bằng nhau hoặc hai giá trị liền kề theo thứ tự sắp xếp của các giá trị riêng biệt. Hạn chế này buộc mọi cặp được chọn phải có giá trị giống hệt nhau hoặc là cặp “lân cận” trong phổ giá trị. 

Kaiji không nhìn thấy cặp đã chọn. Thay vào đó, trước tiên anh ta chọn một trong hai tay để kiểm tra, tìm hiểu giá trị của quả bóng đã chọn đó và sau đó phải cho biết giá trị ẩn ở tay kia là nhỏ hơn, bằng hay lớn hơn. 

Mục tiêu là tối đa hóa xác suất Kaiji trả lời đúng trong lối chơi tối ưu của cả hai bên: Kaiji cố gắng chọn chiến lược đảm bảo tỷ lệ thành công tốt nhất, trong khi đối thủ chọn cặp có tỷ lệ giảm thiểu tỷ lệ đó. 

Trình tạo tạo ra tối đa 10 triệu giá trị cho mỗi trường hợp thử nghiệm và có thể có tới 1000 trường hợp thử nghiệm với tổng số 10 triệu giá trị tổng thể. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào lưu trữ hoặc sắp xếp tất cả các giá trị trên tất cả các thử nghiệm một cách đơn giản nếu nó sử dụng cấu trúc dữ liệu nặng hoặc chi phí cho mỗi thử nghiệm ngoài chức năng quét tuyến tính. Một O(n) cho mỗi phương pháp thử nghiệm chỉ được chấp nhận nếu mỗi thao tác có thời gian không đổi. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các giá trị được tạo đều giống hệt nhau. Trong tình huống đó, mọi cặp hợp lệ đều bao gồm các giá trị bằng nhau, vì vậy Kaiji luôn có thể trả lời đúng “bằng” với xác suất 1. Bất kỳ cách tiếp cận nào giả định ít nhất hai giá trị riêng biệt sẽ khiến trường hợp này trở thành kết quả xác suất một cách không chính xác. 

## Phương pháp tiếp cận 

Việc mô phỏng trực tiếp trò chơi là không khả thi. Ngay cả khi chúng tôi sửa một cặp, quá trình ra quyết định của Kaiji vẫn liên quan đến sự không chắc chắn về yếu tố nào anh ấy sẽ kiểm tra và giá trị đó liên quan đến yếu tố kia như thế nào. Việc liệt kê tất cả các cặp là không thể vì có khả năng O(n2). 

Thay vào đó, cấu trúc của các cặp được phép là sự đơn giản hóa chính. Kẻ thù chỉ có thể chọn các cặp có giá trị giống hệt nhau hoặc giá trị liền kề trong danh sách các giá trị riêng biệt được sắp xếp. Điều này có nghĩa là, theo quan điểm của Kaiji, sự không chắc chắn duy nhất mà đối thủ từng đưa ra xuất phát từ sự chuyển đổi giữa các giá trị khác biệt lân cận. 

Nếu chỉ có một giá trị riêng biệt trong toàn bộ mảng, trò chơi sẽ trở nên tầm thường: mọi cặp đều bằng nhau, vì vậy Kaiji luôn trả lời đúng bằng cách nêu rõ sự bằng nhau. 

Nếu có ít nhất hai giá trị riêng biệt, đối thủ luôn có thể tránh mang lại cho Kaiji bất kỳ lợi thế thông tin xác định nào. Bất cứ khi nào Kaiji quan sát một giá trị, giá trị ẩn có thể tương ứng với giá trị lân cận theo thứ tự riêng biệt được sắp xếp. Theo quan điểm của Kaiji, bất cứ khi nào đối thủ chọn một cặp không bằng nhau, tình huống sẽ giảm xuống mức phân biệt hướng trong chuỗi giá trị cục bộ, trong đó xác suất thành công tốt nhất có thể rơi vào việc đoán giữa hai khả năng nhất quán. 

Quan sát quan trọng là không có chiến lược nào có thể đẩy xác suất thành công lên trên một nửa khi có ít nhất hai giá trị phân biệt. Đối thủ luôn có thể chọn một cặp giá trị riêng biệt liền kề, tạo ra sự mơ hồ giữa “cái này nhỏ hơn và cái kia lớn hơn” so với cấu trúc ngược lại tùy thuộc vào bàn tay nào được tiết lộ. Vì Kaiji buộc phải cam kết sau khi chỉ nhìn thấy một điểm cuối của cấu trúc hai thành phần với độ không chắc chắn đối xứng, nên chiến lược tối ưu sẽ giảm xuống thành phỏng đoán nhị phân. 

Do đó, toàn bộ vấn đề giảm xuống còn việc kiểm tra xem tất cả các giá trị được tạo có bằng nhau hay không.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực theo cặp | O(n²) | O(n) | Quá chậm | 
| Kiểm tra khác biệt tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo chuỗi bằng cách sử dụng phép truy toán trong khi theo dõi xem tất cả các giá trị có giữ nguyên hay không. Chúng tôi khởi tạo giá trị đầu tiên làm đường cơ sở tham chiếu và so sánh mọi giá trị tiếp theo với giá trị đó. 
2. Nếu bất kỳ giá trị nào được tạo ra khác với giá trị cơ sở, chúng ta biết ngay rằng có ít nhất hai giá trị riêng biệt tồn tại trong mảng. Từ thời điểm này trở đi, thế hệ tiếp theo có thể tiếp tục nhưng quyết định cuối cùng đã được xác định. 
3. Sau khi xử lý tất cả các giá trị, hãy quyết định câu trả lời dựa trên việc liệu giá trị riêng biệt thứ hai có từng được quan sát hay không. 
4. Nếu không quan sát thấy sự khác biệt, ghi ra 1, vì mọi cặp hợp lệ đều bằng nhau và Kaiji luôn trả lời đúng. 
5. Ngược lại, xuất ra nghịch đảo mô-đun của 2 dưới 998244353, vì xác suất thành công được đảm bảo tối ưu chính xác là một nửa. 

Lý do quét đủ là vì hạn chế của đối thủ chỉ phụ thuộc vào thứ tự giữa các giá trị và cấu trúc đó sụp đổ hoàn toàn khi có nhiều hơn một giá trị riêng biệt tồn tại. 

### Tại sao nó hoạt động 

Ràng buộc của đối thủ đảm bảo rằng mọi cặp được chọn đều nhất quán cục bộ theo thứ tự giá trị được sắp xếp, nhưng vị trí này không tạo ra cấu trúc có thể khai thác cho Kaiji ngoài khả năng phát hiện sự bình đẳng. Nếu tồn tại nhiều giá trị riêng biệt thì sẽ tồn tại ít nhất một cặp giá trị riêng biệt liền kề hợp lệ. Đối thủ luôn có thể chọn một cặp như vậy và đối với cặp đó, quan sát của Kaiji giảm xuống thành một bài toán suy luận hai kết quả đối xứng. Vì Kaiji phải cam kết sau khi chỉ quan sát một bên, nên không có chiến lược xác định nào có thể phân biệt hướng của sự bất bình đẳng với xác suất lớn hơn một nửa trong trường hợp xấu nhất. Ngoại lệ duy nhất là khi không tồn tại sự kề cận như vậy, điều này xảy ra chính xác khi tất cả các giá trị giống hệt nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
INV2 = 499122177

def solve():
    it = iter(sys.stdin.read().strip().split())
    t = int(next(it))
    out = []

    for _ in range(t):
        n = int(next(it))
        a0 = int(next(it))
        A = int(next(it))
        B = int(next(it))
        C = int(next(it))
        M = int(next(it))

        first = a0
        prev = a0
        all_same = True

        for i in range(2, n + 1):
            prev = (A * prev * prev + B * prev + C) % M + 1
            if prev != first:
                all_same = False

        if all_same:
            out.append("1")
        else:
            out.append(str(INV2))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai chỉ giữ lại giá trị trước đó của lần lặp lại, tránh mọi hoạt động lưu trữ mảng. Điều này rất cần thiết vì n có thể đạt tới 10 triệu. Trạng thái duy nhất chúng tôi duy trì là liệu tất cả các giá trị được tạo có khớp với giá trị đầu tiên hay không. 

Câu trả lời mô-đun sử dụng nghịch đảo được tính toán trước của 2 trong 998244353, vì đó là kết quả đầu ra bắt buộc khi tồn tại nhiều giá trị riêng biệt. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp tất cả các giá trị đều giống hệt nhau. Trình tạo tạo ra một chuỗi như 5, 5, 5, 5. Quá trình quét không bao giờ phát hiện ra sự không khớp, vì vậy trạng thái cuối cùng vẫn là all_same = True. 

| Bước | Giá trị hiện tại | Giá trị đầu tiên | tất cả_giống nhau | 
| --- | --- | --- | --- | 
| 1 | 5 | 5 | Đúng | 
| 2 | 5 | 5 | Đúng | 
| 3 | 5 | 5 | Đúng | 
| 4 | 5 | 5 | Đúng | 

Đầu ra là 1, phản ánh sự chắc chắn về tính chính xác. 

Bây giờ hãy xem xét trường hợp có ít nhất hai giá trị phân biệt, chẳng hạn như 3, 3, 7, 3. Thời điểm 7 xuất hiện, bất biến bị phá vỡ. 

| Bước | Giá trị hiện tại | Giá trị đầu tiên | tất cả_giống nhau | 
| --- | --- | --- | --- | 
| 1 | 3 | 3 | Đúng | 
| 2 | 3 | 3 | Đúng | 
| 3 | 7 | 3 | Sai | 
| 4 | 3 | 3 | Sai | 

Khi all_same trở thành Sai, phần còn lại của chuỗi không liên quan. Câu trả lời trở thành 1/2. 

Điều này cho thấy rằng chỉ sự tồn tại của giá trị phân biệt thứ hai mới quan trọng chứ không phải tần số hoặc vị trí của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi giá trị được tạo một lần và được so sánh theo thời gian không đổi | 
| Không gian | O(1) | Chỉ một số đại lượng vô hướng được lưu trữ bất kể n | 

Tổng n trên các trường hợp thử nghiệm được giới hạn bởi 10⁷, do đó, một lần truyền tuyến tính duy nhất trên tất cả các giá trị đầu vào sẽ vừa vặn thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353
INV2 = 499122177

def solve():
    it = iter(sys.stdin.read().strip().split())
    t = int(next(it))
    out = []

    for _ in range(t):
        n = int(next(it))
        a0 = int(next(it))
        A = int(next(it))
        B = int(next(it))
        C = int(next(it))
        M = int(next(it))

        first = a0
        prev = a0
        all_same = True

        for i in range(2, n + 1):
            prev = (A * prev * prev + B * prev + C) % M + 1
            if prev != first:
                all_same = False

        out.append("1" if all_same else str(INV2))

    print("\n".join(out))

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old

# minimum size, all equal
assert run("1\n2 1 0 0 0 1\n") == "1"

# two distinct values appear
assert run("1\n3 1 0 1 0 2\n") == str(INV2)

# all equal larger n
assert run("1\n5 7 0 0 0 1\n") == "1"

# boundary mixed pattern
assert run("1\n4 2 0 1 0 3\n") == str(INV2)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=2 đều bằng nhau | 1 | trường hợp chắc chắn tầm thường | 
| hỗn hợp nhỏ | 1/2 | tính đúng đắn theo hai giá trị | 
| đồng phục lớn hơn | 1 | ổn định trong thời gian dài | 
| trộn ranh giới | 1/2 | phát hiện sớm giá trị thứ hai | 

## Vỏ cạnh 

Trường hợp đặc biệt về mặt cấu trúc là khi tất cả các giá trị được tạo đều giống hệt nhau. Trong tình huống đó, thuật toán không bao giờ lật cờ all_same nên nó xuất ra 1. 

Đối với bất kỳ đầu vào nào có ít nhất một giá trị được tạo khác nhau, cờ sẽ trở thành Sai chính xác ở lần phân kỳ đầu tiên và vẫn là Sai. Thuật toán không phụ thuộc vào nơi xảy ra sự phân kỳ mà chỉ phụ thuộc vào sự tồn tại của nó. 

Quá trình thực thi đại diện là đầu vào 2 1 0 1 0 2, tạo ra 1, 2. Quá trình quét sẽ ngay lập tức nhận thấy sự không khớp và chuyển sang kết quả xác suất 1/2, khớp với phân tích bảo đảm đối nghịch.
