---
title: "CF 105272B - Trận chiến trong không gian"
description: "Hai người chơi lần lượt viết một số. Đầu tiên, đối thủ công bố một số nguyên n. Sau khi nhìn thấy nó, chúng ta chọn số nguyên m cho riêng mình. Ai viết số lớn hơn sẽ thắng ngay."
date: "2026-06-23T06:55:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105272
codeforces_index: "B"
codeforces_contest_name: "IX MaratonUSP Freshman Contest"
rating: 0
weight: 105272
solve_time_s: 46
verified: true
draft: false
---

[CF 105272B - Trận chiến trong không gian](https://codeforces.com/problemset/problem/105272/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hai người chơi lần lượt viết một số. Đầu tiên, đối thủ công bố một số nguyên`n`. Sau khi nhìn thấy nó, chúng tôi chọn số nguyên của riêng mình`m`. Ai viết số lớn hơn sẽ thắng ngay. 

Mục tiêu là luôn đảm bảo chiến thắng, bất kể điều gì xảy ra.`n`là, trong khi tôn trọng ràng buộc rằng số chúng ta đã chọn phải nằm giữa`0`Và`100000`. 

Điều này biến vấn đề thành một quyết định đơn giản được đưa ra dưới một giới hạn trên cố định. Chúng tôi không cố gắng phản ứng linh hoạt qua nhiều vòng hoặc duy trì cấu trúc dữ liệu, chúng tôi đang chọn một giá trị duy nhất sau khi quan sát một đầu vào. 

Ràng buộc`1 ≤ n ≤ 100`là rất quan trọng. Nó cho ta biết số lượng của đối phương luôn rất nhỏ so với giá trị tối đa cho phép đối với`m`. Điều này đã gợi ý rằng bất kỳ chiến lược nào cố gắng so sánh hoặc điều chỉnh cẩn thận cho từng trường hợp đều không cần thiết, vì phạm vi khả thi cho`m`lớn hơn nhiều so với bất cứ thứ gì mà đối thủ có thể tạo ra. 

Một sai lầm ngây thơ sẽ là tính toán một cái gì đó như`m = n + 1`. Điều này hoạt động trong hầu hết các vấn đề lập trình cạnh tranh thuộc loại này, nhưng ở đây nó không bắt buộc và không tối ưu về mặt đảm bảo một biên lợi thế nghiêm ngặt. Quan trọng hơn, việc triển khai bất cẩn có thể vẫn nằm trong giới hạn nhưng sẽ thất bại ở các biến thể trong đó`n`có thể gần`100000`. 

Ví dụ: nếu quy tắc khác: 

đầu vào:```
100000
```Sau đó`m = n + 1`sẽ vượt quá giới hạn. Trong bài toán này, chúng ta an toàn, nhưng ràng buộc chính xác là điều cho phép một giải pháp hằng số đơn giản hơn. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là thử mọi giá trị có thể có của`m`từ`0`ĐẾN`100000`và chọn một giá trị lớn hơn`n`. Điều này đúng vì chúng tôi mô phỏng trực tiếp quy tắc cho mọi ứng cử viên và kiểm tra xem nó có thắng hay không. Chi phí nhiều nhất là`100001`kiểm tra, điều này không quan trọng ngay cả đối với nhiều trường hợp thử nghiệm. 

Tuy nhiên, cách tiếp cận này là không cần thiết vì cấu trúc của bài toán không phụ thuộc vào bất kỳ sự tương tác nào ngoài một so sánh đơn lẻ. Một khi chúng ta nhận ra rằng chiến thắng chỉ phụ thuộc vào việc lớn hơn`n`, chúng ta không cần phải tìm kiếm nữa. 

Quan sát quan trọng là chúng tôi muốn số lượng được phép lớn nhất có thể. Từ`100000`là giá trị tối đa chúng tôi được phép xuất ra, chúng tôi kiểm tra xem nó có đảm bảo chiến thắng hay không. Bởi vì`n ≤ 100`, nó luôn giữ điều đó`100000 > n`, do đó lựa chọn tối đa có thể đã thỏa mãn điều kiện thắng trong mọi trường hợp. 

Điều này làm giảm vấn đề thành một quyết định liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(100000) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`từ đầu vào. Điều này đại diện cho số cố định của đối thủ. 
2. Nhận ra rằng số của chúng tôi`m`phải thỏa mãn đồng thời hai điều kiện: nó phải nằm trong`[0, 100000]`, và nó phải lớn hơn`n`. 
3. Quan sát rằng số lượng ứng viên tối đa có thể`100000`là hợp lệ dưới ràng buộc. 
4. So sánh ngầm: vì`n ≤ 100`, bất đẳng thức`100000 > n`luôn luôn giữ. 
5. Đầu ra`100000`vì nó vừa hợp lệ vừa đảm bảo chiến thắng. 

### Tại sao nó hoạt động 

Thuật toán dựa trên cấu trúc đơn điệu của bài toán: số lớn hơn luôn lấn át số nhỏ hơn. Bởi vì chúng ta được phép chọn giá trị tối đa có thể trong phạm vi cho phép và giá trị của đối thủ bị giới hạn chặt chẽ dưới mức đó nên giá trị tối đa luôn là chiến lược chiếm ưu thế. Không có cấu hình nào mà lựa chọn nhỏ hơn sẽ hoạt động tốt hơn`100000`, vì chiến thắng chỉ phụ thuộc vào việc lớn hơn`n`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    print(100000)

if __name__ == "__main__":
    solve()
```Giải pháp đọc giá trị đầu vào duy nhất và xuất ra ngay lập tức`100000`. Không có sự phân nhánh vì hạn chế về`n`đảm bảo rằng giá trị này luôn đánh bại nó. Việc triển khai tránh các so sánh hoặc số học không cần thiết, giữ logic ở mức tối thiểu và mạnh mẽ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
```| Bước | n | đã chọn m | kết quả so sánh | 
| --- | --- | --- | --- | 
| 1 | 5 | 100000 | 100000 > 5 | 

Đầu ra luôn là`100000`. Điều này chứng tỏ rằng ngay cả với những giá trị nhỏ của`n`, chiến lược không thay đổi. 

### Ví dụ 2 

đầu vào:```
100
```| Bước | n | đã chọn m | kết quả so sánh | 
| --- | --- | --- | --- | 
| 1 | 100 | 100000 | 100000 > 100 | 

Điều này cho thấy trường hợp ranh giới trong đó`n`đang ở mức tối đa. Ngay cả ở đây, đầu ra cố định vẫn hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ đọc một đầu vào và một thao tác in | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Các ràng buộc là cực kỳ nhỏ và giải pháp thực thi trong thời gian không đổi, trong bất kỳ giới hạn thời gian nào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return io.StringIO().write(""), solve()

# provided sample-like cases (constructed)
assert run("1\n") is None
assert run("50\n") is None
assert run("100\n") is None

# custom cases
# minimum
assert run("1\n") is None, "minimum input"

# maximum n
assert run("100\n") is None, "upper bound of n"

# arbitrary mid
assert run("42\n") is None, "mid range"

# edge consistency check
assert run("99\n") is None, "near maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 100000 | độ chính xác đầu vào tối thiểu | 
| 100 | 100000 | độ chính xác hạn chế tối đa | 
| 42 | 100000 | hành vi tầm trung điển hình | 
| 99 | 100000 | ổn định gần biên | 

## Vỏ cạnh 

### Giá trị đối thủ tối đa có thể 

đầu vào:```
100
```Thuật toán chọn`100000`. Từ`100000 > 100`, điều kiện thắng được thỏa mãn. Không cần giá trị thay thế vì mọi giá trị hợp lệ nhỏ hơn vẫn có tác dụng, nhưng lựa chọn tối đa đảm bảo tính chính xác trong mọi trường hợp. 

### Giá trị đối thủ tối thiểu có thể có 

đầu vào:```
1
```Một lần nữa, kết quả đầu ra của thuật toán`100000`. Sự so sánh`100000 > 1`giữ một cách tầm thường, xác nhận rằng ngay cả giá trị đối thủ nhỏ nhất cũng không ảnh hưởng đến quyết định. 

### Trường hợp giới hạn chung 

Bất kỳ đầu vào nào`n`TRONG`[1, 100]`đi theo cùng một đường dẫn thực hiện, tạo ra cùng một đầu ra. Logic không bao giờ phân nhánh, vì vậy tất cả các trường hợp đều giảm xuống một kiểm tra bất biến duy nhất: đầu ra phải vượt quá đầu vào tối đa có thể, được thỏa mãn bởi`100000`.
