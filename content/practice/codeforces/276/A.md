---
title: "CF 276A - Bữa trưa vội vã"
description: "Chúng tôi được cung cấp một danh sách các nhà hàng, trong đó mỗi nhà hàng có hai giá trị: điểm thưởng thức cơ bản và thời gian cần thiết để ăn ở đó. Huấn luyện viên chỉ cho phép một thời gian ăn trưa cố định."
date: "2026-06-05T02:15:33+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 276
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 169 (Div. 2)"
rating: 900
weight: 276
solve_time_s: 79
verified: true
draft: false
---

[CF 276A - Bữa trưa vội vàng](https://codeforces.com/problemset/problem/276/A) 

**Xếp hạng:** 900 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách các nhà hàng, trong đó mỗi nhà hàng có hai giá trị: điểm thưởng thức cơ bản và thời gian cần thiết để ăn ở đó. Huấn luyện viên chỉ cho phép một thời gian ăn trưa cố định. Nếu một nhà hàng có thể hoàn thành trong thời gian cho phép, đội sẽ nhận được toàn bộ giá trị thưởng thức. Nếu nhà hàng mất nhiều thời gian hơn thời gian cho phép, sự thích thú sẽ giảm đi đúng bằng lượng thời gian vượt quá giới hạn. 

Nhiệm vụ là chọn chính xác một nhà hàng theo cách tối đa hóa cảm giác thích thú sau khi áp dụng quy tắc này. 

Kích thước đầu vào lên tới mười nghìn nhà hàng, điều đó có nghĩa là giải pháp tính toán lại logic phức tạp cho từng nhà hàng một cách độc lập vẫn có thể chấp nhận được, nhưng bất kỳ phương pháp bậc hai nào có xử lý lồng nhau trên các cấu trúc dữ liệu lớn sẽ là chi phí không cần thiết. Một lần truyền dữ liệu dễ dàng đủ nhanh trong những ràng buộc này vì chúng tôi chỉ đánh giá một công thức đơn giản cho mỗi nhà hàng. 

Có một trường hợp tế nhị thường gây ra sai sót. Khi một nhà hàng mất ít hoặc bằng thời gian cho phép, câu trả lời chỉ đơn giản là sự thích thú cơ bản. Khi vượt quá thời hạn, chúng ta chỉ phải trừ phần vượt quá chứ không phải toàn thời gian. Ví dụ: nếu giới hạn là 5, một nhà hàng có mức độ thích thú là 10 và thời gian 8 sẽ cho kết quả 10 − (8 − 5) = 7. Một lỗi phổ biến là trừ toàn bộ thời gian thay vì chỉ trừ phần tràn, kết quả sẽ là 2 một cách không chính xác. 

Một trường hợp khác xuất hiện khi tất cả các nhà hàng vượt quá giới hạn thời gian và có giá trị cơ bản nhỏ. Công thức đã sửa có thể cho kết quả âm tính và câu trả lời đúng có thể âm tính. Điều này có nghĩa là chúng tôi không thể mặc định về 0 hoặc giả sử kết quả không âm. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tính toán mức độ thích thú cho từng nhà hàng một cách độc lập bằng cách sử dụng quy tắc nhất định và theo dõi mức tối đa. Đối với mỗi nhà hàng, chúng tôi kiểm tra xem thời gian của nhà hàng đó có nằm trong giới hạn cho phép hay không. Nếu vậy, chúng tôi lấy giá trị cơ bản; nếu không chúng tôi sẽ trừ đi thời gian dư thừa. Điều này mang lại một lượng công việc không đổi cho mỗi nhà hàng. 

Điều này có hiệu quả vì sự cố không gây ra sự phụ thuộc giữa các nhà hàng. Mỗi tùy chọn được đánh giá riêng biệt, do đó không cần sắp xếp, lập trình động hoặc xử lý tiền tố. Yêu cầu duy nhất là quét tất cả các ứng viên và tính giá trị điều chỉnh của chúng. 

Cách giải thích ngây thơ có thể gợi ý việc xử lý các trường hợp khác nhau một cách riêng biệt theo những cách phức tạp hơn, nhưng không có cấu trúc nào cho phép tái sử dụng các kết quả trung gian. Mỗi nhà hàng phải được đánh giá ít nhất một lần, vì vậy giải pháp tối ưu đã có tính tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến để lưu trữ cảm giác thích thú nhất từ trước đến nay, bắt đầu từ một số rất nhỏ. Điều này đảm bảo ngay cả kết quả tiêu cực cũng được xử lý chính xác. 
2. Lặp lại từng nhà hàng một. 
3. Với mỗi nhà hàng, hãy tính mức độ hưởng thụ hiệu quả của nó. Nếu thời gian của nó nhỏ hơn hoặc bằng giới hạn cho phép thì hãy trực tiếp sử dụng cơ sở hưởng thụ của nó. Ngược lại hãy trừ phần tràn`(t_i - k)`từ giá trị cơ sở của nó. Điều này trực tiếp phù hợp với quy tắc của vấn đề. 
4. So sánh giá trị được tính toán này với giá trị tốt nhất hiện tại và cập nhật giá trị tốt nhất nếu giá trị mới lớn hơn. 
5. Sau khi xử lý tất cả các nhà hàng, xuất ra giá trị tốt nhất tìm được. 

### Tại sao nó hoạt động 

Mỗi nhà hàng đóng góp chính xác một giá trị ứng cử viên chỉ phụ thuộc vào các thông số riêng của nó và hằng số toàn cục`k`. Vì không có sự tương tác giữa các lựa chọn nên giải pháp tối ưu phải là giá trị lớn nhất trên tất cả các giá trị điều chỉnh được tính toán riêng lẻ. Thuật toán duy trì tính bất biến rằng sau khi xử lý nhà hàng thứ i, giá trị tốt nhất được lưu trữ bằng mức độ thích thú được điều chỉnh tối đa trong số i nhà hàng đầu tiên. Bất biến này đảm bảo tính đúng đắn vì nó được bảo toàn bằng cách so sánh ứng viên hiện tại với giá trị tối đa được lưu trữ ở mỗi bước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    best = -10**18

    for _ in range(n):
        f, t = map(int, input().split())
        if t <= k:
            val = f
        else:
            val = f - (t - k)
        if val > best:
            best = val

    print(best)

if __name__ == "__main__":
    solve()
```Việc thực hiện theo thuật toán trực tiếp. Chi tiết quan trọng là sử dụng giá trị ban đầu rất nhỏ cho`best`, vì các câu trả lời hợp lệ có thể phủ định và chúng ta không được phép kẹp kết quả. Mỗi lần lặp lại sẽ tính toán mức độ tận hưởng được điều chỉnh chính xác như đã xác định, với sự phân chia có điều kiện dựa trên việc thời gian có vượt quá giới hạn hay không. 

Phép trừ được cấu trúc cẩn thận như`f - (t - k)`còn hơn là`f - t + k`chỉ để làm rõ, mặc dù cả hai đều tương đương nhau. Điều này làm giảm nguy cơ mắc lỗi ký tên trong quá trình thực hiện. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng mẫu được cung cấp và một trường hợp được xây dựng bổ sung. 

### Mẫu 1 

đầu vào:```
2 5
3 3
4 5
```| Nhà hàng | f | t | Tính toán | Giá trị | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | trong giới hạn | 3 | 3 | 
| 2 | 4 | 5 | trong giới hạn | 4 | 4 | 

Nhà hàng thứ hai cho cảm giác thích thú cao hơn nên đáp án là 4. 

Dấu vết này cho thấy rõ tính bất biến: sau mỗi bước, chúng tôi duy trì giá trị tốt nhất trong số các nhà hàng chế biến. 

### Mẫu 2 (đã thi công) 

đầu vào:```
3 4
10 6
7 3
5 10
```| Nhà hàng | f | t | Tính toán | Giá trị | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 10 | 6 | 10 - (6 - 4) = 8 | 8 | 8 | 
| 2 | 7 | 3 | trong giới hạn | 7 | 8 | 
| 3 | 5 | 10 | 5 - (10 - 4) = -1 | -1 | 8 | 

Nhà hàng đầu tiên đặt giá trị điều chỉnh cao và các mục sau không vượt quá giá trị đó. Điều này xác nhận rằng ngay cả những kết quả tiêu cực cũng được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nhà hàng được xử lý một lần với số học theo thời gian không đổi | 
| Không gian | O(1) | Chỉ một biến duy nhất được sử dụng để theo dõi mức tối đa | 

Các ràng buộc cho phép lên tới mười nghìn nhà hàng và một đường chuyền tuyến tính duy nhất với số học đơn giản phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve() if solve() is not None else "").strip()

# provided sample
assert run("2 5\n3 3\n4 5\n") == "4", "sample 1"

# all within limit
assert run("3 10\n5 1\n6 2\n7 3\n") == "7", "all within limit"

# all exceed limit
assert run("2 5\n10 10\n8 9\n") == "5", "penalized values"

# mixed positive and negative results
assert run("3 4\n1 10\n2 3\n3 20\n") == "2", "mixed case"

# single element
assert run("1 5\n100 10\n") == "95", "single restaurant"

# large k trivial case
assert run("2 100\n1 50\n2 60\n") == "2", "large k"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều trong giới hạn | 7 | lựa chọn tối đa cơ bản | 
| tất cả đều vượt quá giới hạn | 5 | xử lý phạt đền | 
| giá trị hỗn hợp | 2 | trộn âm và dương | 
| nhà hàng đơn | 95 | độ đúng cơ sở | 
| k lớn | 2 | không bị phạt khi k lớn | 

## Vỏ cạnh 

Khi tất cả các nhà hàng vượt quá giới hạn thời gian, thuật toán vẫn hoạt động vì nó tính toán giá trị giảm cho từng tùy chọn và so sánh chúng trực tiếp. Ví dụ, với`k = 5`và một nhà hàng`(f=10, t=10)`, giá trị trở thành`10 - 5 = 5`. Giá trị tối đa trên các giá trị được chuyển đổi như vậy vẫn được chọn chính xác. 

Khi một số giá trị điều chỉnh trở thành âm, việc khởi tạo`best`vì một con số rất nhỏ đảm bảo những điều này vẫn được xem xét. Ví dụ: với đầu vào:```
2 3
1 10
2 10
```các giá trị được tính toán là`1 - 7 = -6`Và`2 - 7 = -5`. Thuật toán trả về chính xác`-5`, vì nó theo dõi mức tối đa ngay cả trong số các số âm.
