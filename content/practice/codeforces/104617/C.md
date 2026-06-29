---
title: "CF 104617C - Lựa chọn ngọt ngào"
description: "Chúng tôi được cung cấp ba bộ sưu tập dây độc lập: hương vị kem, mưa phùn và lớp phủ trên. Một món tráng miệng bao gồm chính xác hai muỗng kem, một muỗng mưa phùn và một topping."
date: "2026-06-29T17:33:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104617
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 09-22-23 Div. 2 (Beginner)"
rating: 0
weight: 104617
solve_time_s: 66
verified: true
draft: false
---

[CF 104617C - Những lựa chọn ngọt ngào](https://codeforces.com/problemset/problem/104617/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp ba bộ sưu tập dây độc lập: hương vị kem, mưa phùn và lớp phủ trên. Một món tráng miệng bao gồm chính xác hai muỗng kem, một muỗng mưa phùn và một topping. Hai muỗng được chọn từ danh sách hương vị và chúng có thể có cùng một hương vị hoặc hai hương vị khác nhau. Thứ tự của hai muỗng không quan trọng nên việc chọn A rồi B cũng giống như chọn B rồi A. 

Nhiệm vụ là đếm xem có thể tạo ra bao nhiêu món tráng miệng khác nhau theo những quy tắc này. Hai món tráng miệng được coi là khác nhau nếu bất kỳ thành phần nào được chọn khác nhau, có nghĩa là cặp muỗng, nước mưa hoặc lớp phủ đều khác nhau. 

Quan sát chính về các ràng buộc là mỗi danh sách có tối đa 100 mục. Điều này giúp cho việc xem xét tất cả các cặp hương vị một cách rõ ràng là khả thi, vì số lượng các cặp không có thứ tự lặp lại nhiều nhất là 5050. Nhân với tối đa 100 giọt mưa và 100 lớp phủ sẽ cho khoảng 50 triệu kết hợp trong trường hợp xấu nhất, vốn đã là ranh giới đối với Python nếu được triển khai một cách ngây thơ, nhưng vẫn có thể quản lý được nếu tính toán bằng cách sử dụng số học thay vì liệt kê. 

Một sai lầm ngây thơ là cố gắng xây dựng rõ ràng mọi sự kết hợp món tráng miệng dưới dạng một bộ chuỗi và lưu trữ chúng trong một bộ. Cách tiếp cận đó đúng về mặt logic nhưng có rủi ro về chi phí không cần thiết từ việc xử lý chuỗi và băm hàng triệu bộ dữ liệu. 

Cạm bẫy tinh vi thứ hai đến từ việc hiểu nhầm việc ghép cặp tin sốt dẻo. Ví dụ: với các hương vị A, B, C, các cặp hợp lệ là AA, AB, AC, BB, BC, CC. Coi AB và BA khác nhau sẽ đếm gấp đôi và thổi phồng câu trả lời không chính xác. 

Không có ràng buộc cấu trúc nào khác tồn tại, do đó vấn đề giảm xuống việc đếm các kết hợp thay vì tìm kiếm hoặc tối ưu hóa các phụ thuộc. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ sẽ lặp đi lặp lại một cách rõ ràng qua tất cả các cách để chọn hai hương vị (có thay thế), sau đó lặp lại trên tất cả các cơn mưa phùn và sau đó là tất cả các lớp phủ trên. Đối với mỗi lựa chọn, chúng tôi sẽ xây dựng một hình đại diện của món tráng miệng và chèn nó vào một bộ để đảm bảo tính độc đáo. Điều này có hiệu quả vì tập hợp này sẽ loại bỏ các bản sao một cách tự nhiên do tính đối xứng theo thứ tự muỗng. 

Lỗ hổng là phương pháp này thực hiện việc xây dựng rõ ràng mọi tổ hợp ba. Với 100 hương vị, có 100 × 100 = 10000 cặp có thứ tự, nhưng chỉ có khoảng 5050 cặp không có thứ tự. Ngay cả khi chúng tôi sửa lỗi đặt hàng, cuối cùng chúng tôi vẫn lặp lại tới 10000 cặp. Nhân với 100 giọt mưa phùn và 100 lớp phủ trên bề mặt sẽ mang lại kết quả là 100 triệu lần lặp. Mỗi lần lặp lại bao gồm việc tạo và băm bộ dữ liệu, quá trình này quá chậm trong Python. 

Cấu trúc của bài toán loại bỏ mọi tương tác giữa ba thành phần. Việc lựa chọn cặp muỗng không phụ thuộc vào lựa chọn mưa phùn và lựa chọn lớp phủ. Khả năng phân tách này có nghĩa là tổng câu trả lời là tích của các số đếm độc lập. 

Vì vậy, nhiệm vụ giảm xuống: 

Đếm các cặp hương vị không theo thứ tự với số lần lặp lại cho phép, sau đó nhân với số lượng mưa phùn và lớp phủ. 

Phần không tầm thường duy nhất là đếm chính xác các cặp hương vị. 

Gọi n là số mùi vị. Số cặp không có thứ tự hợp lệ có sự lặp lại là n(n+1)/2. Điều này xuất phát từ việc chọn hai chỉ số i ≤ j. 

Sau khi tính xong, chúng tôi nhân nó với d × t. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² · d · t) | O(n² · d · t) | Quá chậm | 
| Tối ưu | O(n + d + t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc danh sách các hương vị và đếm xem có bao nhiêu hương vị. Gọi đây là n. Điều này quan trọng vì chúng ta chỉ cần số đếm chứ không cần chuỗi thực tế. 
2. Tính số lượng các cặp hương vị không có thứ tự với sự lặp lại được phép sử dụng n × (n + 1) / 2. Điều này trực tiếp đếm tất cả các cặp (i, j) trong đó i ≤ j. 
3. Đọc danh sách các cơn mưa phùn và đếm chúng là d. 
4. Đọc danh sách các loại đồ ăn kèm và đếm chúng là t. 
5. Nhân ba lựa chọn độc lập: cặp hương vị × d × t và đưa ra kết quả. 

### Tại sao nó hoạt động 

Mỗi món tráng miệng hợp lệ được xác định duy nhất bởi ba quyết định độc lập: một bộ gồm nhiều kích cỡ gồm hai hương vị, một loại mưa phùn và một loại phủ trên. Việc lựa chọn hương vị tạo thành một tập hợp nhiều tổ hợp có kích thước hai và công thức đếm tiêu chuẩn n(n+1)/2 liệt kê từng tập hợp như vậy chính xác một lần. Vì các lựa chọn mưa phùn và lớp phủ không tương tác với nhau hoặc với việc lựa chọn hương vị nên nguyên tắc nhân áp dụng trực tiếp. Không có sự kết hợp nào được tính hai lần hoặc bị bỏ sót vì mỗi thành phần được chọn độc lập và tất cả các kết hợp đều hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    flavors = input().split()
    drizzles = input().split()
    toppings = input().split()

    n = len(flavors)
    d = len(drizzles)
    t = len(toppings)

    pairs = n * (n + 1) // 2
    print(pairs * d * t)

if __name__ == "__main__":
    main()
```Giải pháp đọc từng dòng và chuyển đổi ngay lập tức thành danh sách mã thông báo bằng cách sử dụng`split`, tránh mọi nhu cầu phân tích logic ngoài việc phân tách khoảng trắng. Chỉ có độ dài là quan trọng, do đó không cần lưu trữ ngoài các danh sách này. 

Tính toán quan trọng là`n * (n + 1) // 2`, đếm các cặp không có thứ tự với sự lặp lại. Việc sử dụng phép chia số nguyên đảm bảo tính đúng đắn cho cả n chẵn và lẻ. Phép nhân cuối cùng kết hợp các lựa chọn tổ hợp độc lập. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu```
Grass Licorice
Motor-Oil
Bone-Dust
```Ở đây có 2 vị, 1 vị mưa phùn và 1 vị topping. 

| Bước | Giá trị | 
| --- | --- | 
| hương vị (n) | 2 | 
| mưa phùn (d) | 1 | 
| lớp phủ (t) | 1 | 
| cặp hương vị | 2 × 3/2 = 3 | 
| kết quả | 3 × 1 × 1 = 3 | 

Điều này phù hợp với ba kết hợp muỗng có thể có: (Cỏ,Cỏ), (Cỏ,Cam thảo), (Cam thảo,Cam thảo). 

### Đầu vào tùy chỉnh```
A B C
X Y
Z
```| Bước | Giá trị | 
| --- | --- | 
| n | 3 | 
| d | 2 | 
| t | 1 | 
| cặp hương vị | 3×4/2 = 6 | 
| kết quả | 6 × 2 × 1 = 12 | 

Điều này xác nhận rằng mỗi cặp trong số sáu cặp muỗng có thể được kết hợp với từng cặp mưa phùn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + d + t) | Mỗi dòng được quét một lần để đếm mã thông báo, theo sau là số học theo thời gian không đổi | 
| Không gian | O(1) | Chỉ số lượng được lưu trữ; không có cấu trúc tổ hợp nào được xây dựng | 

Kích thước đầu vào nhỏ, nhưng ngay cả khi chúng ở mức tối đa, giải pháp vẫn nằm trong giới hạn tầm thường vì nó tránh được việc liệt kê hoàn toàn và giảm mọi thứ về số học theo thời gian không đổi sau khi phân tích cú pháp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# provided sample
assert run("Grass Licorice\nMotor-Oil\nBone-Dust\n") == "3"

# single flavor, single drizzle, single topping
assert run("Vanilla\nChocolate\nSprinkles\n") == "1"

# two flavors, no distinct pairing variety
assert run("A B\nX\nY Z\n") == "3"

# three flavors, all components large enough to test multiplication
assert run("A B C\nD E F\nG H\n") == "36"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mỗi vị 1 hương vị | 1 | Xử lý n = 1 đúng cách | 
| 2 hương vị | 3 | Xác thực việc đếm kết hợp | 
| thùng 3×3×2 | 36 | Kiểm tra cấu trúc nhân đầy đủ | 

## Vỏ cạnh 

Trường hợp phức tạp là khi chỉ có một hương vị. Đối với đầu vào:```
Vanilla
Chocolate
Sprinkles
```chúng ta có n = 1, vì vậy các cặp hương vị = 1 × 2/2 = 1. Lựa chọn muỗng hợp lệ duy nhất là (Vanilla, Vanilla). Thuật toán xử lý chính xác điều này mà không cần bất kỳ sự phân nhánh đặc biệt nào. 

Một trường hợp khác là khi tất cả các danh sách đều ở mức tối thiểu:```
A
B
C
```Ở đây kết quả vẫn là 1, vì mỗi danh mục có đúng một lựa chọn. Phép nhân vẫn giữ nguyên vì số cặp vẫn hợp lệ và không xảy ra hành vi chia hoặc cạnh 0. 

Cuối cùng, khi hương vị lớn hơn nhưng mưa phùn hoặc lớp phủ là danh sách một thành phần, công thức sẽ thu gọn các kích thước đó một cách tự nhiên mà không làm mất đi tính chính xác, vì nhân với 1 sẽ giữ nguyên số đếm và không yêu cầu xử lý có điều kiện.
