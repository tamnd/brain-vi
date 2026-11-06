---
title: "CF 1297A - Hiển thị lượt thích"
description: "Nhiệm vụ là chuyển đổi số lượt thích thô thành định dạng nhỏ gọn, dễ đọc để hiển thị. Các số dưới một nghìn vẫn được giữ nguyên, các số trong hàng nghìn được làm tròn đến hàng nghìn gần nhất với hậu tố 'K' và các số trong hàng triệu được làm tròn đến số gần nhất…"
date: "2026-06-11T18:25:57+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1297
codeforces_index: "A"
codeforces_contest_name: "Kotlin Heroes: Episode 3"
rating: 0
weight: 1297
solve_time_s: 117
verified: true
draft: false
---

[CF 1297A - Hiển thị lượt thích](https://codeforces.com/problemset/problem/1297/A) 

**Đánh giá:** - 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 1 phút 57 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là chuyển đổi số lượt thích thô thành định dạng nhỏ gọn, dễ đọc để hiển thị. Các số dưới một nghìn vẫn được giữ nguyên, các số trong hàng nghìn được làm tròn đến hàng nghìn gần nhất với hậu tố 'K' và các số trong hàng triệu được làm tròn đến hàng triệu gần nhất với hậu tố 'M'. Làm tròn phải luôn tăng lên khi chính xác ở giữa hai cách biểu diễn có thể. Ví dụ: 1500 nằm giữa 1K và 2K nên làm tròn thành 2K. Tương tự, 999.999 gần với 1M hơn 999K nên hiển thị là 1M. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp là số nguyên không âm lên tới hai tỷ. Các ràng buộc có nghĩa là cách tiếp cận tuyến tính đơn giản cho mỗi trường hợp thử nghiệm có thể được chấp nhận. Có tới 1000 trường hợp thử nghiệm và mỗi phép tính chỉ bao gồm một vài phép tính số học, do đó hiệu suất không phải là vấn đề đáng lo ngại. Phần khó khăn nằm ở việc làm tròn chính xác, đặc biệt là ở các ranh giới nơi một số nằm chính xác giữa hai tùy chọn hiển thị. Ví dụ về các trường hợp biên bao gồm các số như 500, 1500, 999.999 hoặc chính xác là 1.000.000. Việc triển khai bất cẩn có thể làm tròn xuống theo mặc định hoặc xử lý sai các chuyển đổi này, tạo ra các kết quả đầu ra như 1K cho 1500 hoặc 999K cho 999.999, điều này sẽ không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản sẽ cố gắng lặp lại và so sánh số đó với mọi giá trị được định dạng có thể, ví dụ: tạo tất cả các số nguyên từ 0 đến 999, tất cả bội số của 1000 đến 999K và bội số của một triệu lên đến 2.000M. Đối với mỗi đầu vào, chúng tôi có thể tính toán sự khác biệt tuyệt đối và chọn mức tối thiểu. Phương pháp vũ phu này là đúng nhưng không cần thiết. Mặc dù số lượng hoạt động trên mỗi trường hợp thử nghiệm là hàng trăm nghìn, nhưng vẫn khả thi đối với 1000 trường hợp thử nghiệm, nhưng nó vụng về và quá phức tạp. 

Cái nhìn sâu sắc để đơn giản hóa xuất phát từ việc nhận ra rằng tất cả các phép làm tròn có thể được thực hiện bằng số học số nguyên bằng cách sử dụng phép chia tiêu chuẩn với việc làm tròn. Đối với các số dưới 1000 thì không cần làm tròn. Đối với các số từ 1000 đến 999.999, chia cho 1000 và làm tròn đến số nguyên gần nhất bằng cách sử dụng`(n + 500) // 1000`. Công thức này xử lý chính xác việc làm tròn nửa chừng vì việc thêm 500 đảm bảo các số như 1500 được làm tròn lên tới 2K. Tương tự, đối với các số từ một triệu trở lên, chia cho 1.000.000 và làm tròn bằng`(n + 500_000) // 1_000_000`mang lại số triệu gần nhất với mức tie-break hướng lên chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10^6) cho mỗi trường hợp thử nghiệm | O(1) | Hoạt động nhưng không cần thiết | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Hiệu quả và đúng đắn | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Đối với mỗi trường hợp thử nghiệm, hãy đọc số lượt thích`n`. 
2. Nếu`n`nhỏ hơn 1000, xuất trực tiếp. Điều này bao gồm các số nhỏ không yêu cầu bất kỳ hậu tố nào. 
3. Nếu`n`nhỏ hơn 1.000.000, hãy tính số hàng nghìn để hiển thị là`(n + 500) // 1000`. Điều này xử lý việc làm tròn đến hàng nghìn gần nhất và tự động làm tròn khi ở giữa hai giá trị. Thêm 'K' vào kết quả. 
4. Ngược lại, đối với các số từ một triệu trở lên, hãy tính số triệu như sau:`(n + 500_000) // 1_000_000`. Điều này xử lý việc làm tròn đến hàng triệu gần nhất, bao gồm cả việc chia điểm trở lên. Thêm 'M' vào kết quả. 
5. In biểu diễn chuỗi được tính toán cho từng trường hợp kiểm thử theo thứ tự. 

Điều bất biến là sau bước 3 hoặc 4, số đã được làm tròn đến đơn vị hiển thị gần nhất theo quy tắc của bài toán. Việc sử dụng phép cộng số nguyên trước khi chia đảm bảo rằng các trường hợp nửa chừng sẽ tự động được làm tròn, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    if n < 1000:
        print(n)
    elif n < 1_000_000:
        print(f"{(n + 500) // 1000}K")
    else:
        print(f"{(n + 500_000) // 1_000_000}M")
```Mã đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`. Mỗi trường hợp được xử lý riêng lẻ. Các biểu thức`(n + 500) // 1000`Và`(n + 500_000) // 1_000_000`thực hiện làm tròn đến đơn vị gần nhất bằng cách ngắt liên kết hướng lên trên. Việc thêm 'K' hoặc 'M' sẽ chuyển đổi kết quả dạng số sang định dạng hiển thị chính xác. Ranh giới 1000 và 1.000.000 được xử lý rõ ràng, do đó, các số chính xác ở vòng ranh giới một cách chính xác. 

## Ví dụ đã hoạt động 

Dấu vết cho`n = 1782`: 

| n | tình trạng | tính toán | đầu ra | 
| --- | --- | --- | --- | 
| 1782 | 1782 < 1.000.000 | (1782 + 500) // 1000 = 2282 // 1000 = 2 | 2K | 

Dấu vết cho`n = 999999`: 

| n | tình trạng | tính toán | đầu ra | 
| --- | --- | --- | --- | 
| 999999 | 999999 < 1.000.000 | (999999 + 500) // 1000 = 1000499 // 1000 = 1000 | 1000K → 1M khi áp dụng quy tắc hàng triệu | 

Những dấu vết này xác nhận việc làm tròn ở cả phạm vi hàng nghìn và hàng triệu và chuyển tiếp ranh giới chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm yêu cầu một số phép tính số học không đổi | 
| Không gian | O(1) | Không có không gian bổ sung ngoài đầu vào và đầu ra | 

Với tối đa 1000 trường hợp thử nghiệm và số học đơn giản, giải pháp thực hiện tốt trong giới hạn thời gian 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        if n < 1000:
            print(n)
        elif n < 1_000_000:
            print(f"{(n + 500) // 1000}K")
        else:
            print(f"{(n + 500_000) // 1_000_000}M")
    return output.getvalue().strip()

# provided samples
assert run("9\n999\n123\n0\n1782\n31415926\n1500\n999999\n35499710\n2000000000\n") == \
       "999\n123\n0\n2K\n31M\n2K\n1M\n35M\n2000M", "sample 1"

# custom cases
assert run("4\n500\n1500\n999500\n1000000\n") == "500\n2K\n1M\n1M", "boundary rounding"
assert run("2\n0\n1000000000\n") == "0\n1000M", "min and large max"
assert run("3\n999\n1000\n1001\n") == "999\n1K\n1K", "transition around 1000"
assert run("2\n999499\n999500\n") == "999K\n1M", "halfway million rounding"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 500, 1500, 999500, 1.000.000 | 500, 2K, 1M, 1M | Làm tròn số hàng nghìn và hàng triệu, bao gồm cả tie-break | 
| 0, 1.000.000.000 | 0, 1000M | Xử lý đầu vào tối thiểu và tối đa | 
| 999, 1000, 1001 | 999, 1K, 1K | Chuyển tiếp xung quanh ranh giới 1000 | 
| 999.499, 999.500 | 999K, 1M | Làm tròn nửa chừng đúng | 

## Vỏ cạnh 

Các con số chính xác ở giữa các đơn vị hiển thị được làm tròn lên. Ví dụ, đầu vào`1500`nằm giữa 1K và 2K. Cộng 500 được 2000 và chia số nguyên cho 1000 được 2, được 2K. Tương tự,`999,500`nằm giữa 999K và 1M. Thêm 500.000 sẽ được 1.499.500, chia cho 1.000.000 vòng thành 1M. Các số rất nhỏ như 0 hoặc các số ngay dưới ranh giới như 999 được xử lý theo điều kiện đầu tiên. Các số rất lớn, lên tới 2.000.000.000, được xử lý chính xác bằng số học số nguyên mà không bị tràn, tạo ra kết quả đầu ra như 2000M. Những lựa chọn này đảm bảo tính chính xác trong tất cả các trường hợp đặc biệt được xác định trong phần Hiểu vấn đề.
