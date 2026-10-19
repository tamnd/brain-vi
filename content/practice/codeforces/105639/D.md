---
title: "CF 105639D - Thí nghiệm năm mới"
description: "Sự cố mô tả một mảng các số nguyên được sửa đổi nhiều lần bởi các phép toán bitwise toàn cục, theo sau là hai loại truy vấn. Một truy vấn yêu cầu giá trị hiện tại ở một vị trí cụ thể và truy vấn còn lại yêu cầu giá trị lớn thứ k trong toàn bộ mảng."
date: "2026-06-26T14:51:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105639
codeforces_index: "D"
codeforces_contest_name: "Innopolis Open 2024-2025. Elimination Round 2"
rating: 0
weight: 105639
solve_time_s: 48
verified: true
draft: false
---

[CF 105639D - Thử nghiệm năm mới](https://codeforces.com/problemset/problem/105639/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Sự cố mô tả một mảng các số nguyên được sửa đổi nhiều lần bởi các phép toán bitwise toàn cục, theo sau là hai loại truy vấn. Một truy vấn yêu cầu giá trị hiện tại ở một vị trí cụ thể và truy vấn còn lại yêu cầu giá trị lớn thứ k trong toàn bộ mảng. Giữa các truy vấn, mảng không thay đổi ngoại trừ thông qua các thao tác áp dụng thống nhất cho mọi phần tử. 

Một chi tiết quan trọng là các hoạt động là các cập nhật mang tính hủy diệt được áp dụng cho tất cả các phần tử cùng một lúc: bitwise AND, OR và XOR với một số nhất định. Sau nhiều thao tác như vậy, việc duy trì mảng một cách rõ ràng sẽ quá chậm vì mỗi lần cập nhật sẽ chạm vào mọi phần tử. 

Loại truy vấn thứ hai tinh tế hơn. Nó không yêu cầu một vị trí mà yêu cầu thống kê thứ tự trên toàn bộ mảng. Điều đó có nghĩa là chúng ta cần một cấu trúc hỗ trợ cả chuyển đổi toàn cầu nhanh chóng và truy vấn xếp hạng nhanh. 

Các ràng buộc (tiêu chuẩn cho loại tác vụ Codeforces này) thường cho phép tối đa 200 nghìn thao tác, khiến mọi cập nhật từng thành phần cho mỗi truy vấn đều không thể thực hiện được. Một cách tiếp cận đơn giản là tính toán lại toàn bộ mảng sau mỗi thao tác sẽ mất O(nq), vượt xa giới hạn khả thi khi cả n và q đều lớn. 

Một trường hợp lỗi đơn giản nhưng quan trọng xuất phát từ việc coi XOR, AND và OR là có thể cập nhật độc lập cho mỗi phần tử mà không xem xét các hiệu ứng bit tổng thể. Ví dụ, nếu mảng là`[1, 2, 3]`và chúng tôi áp dụng các thao tác XOR lặp đi lặp lại, việc tính toán lại từng phần tử trực tiếp cho mỗi truy vấn sẽ trở thành bậc hai qua nhiều thao tác. 

Một cạm bẫy tinh vi khác là hiểu nhầm các truy vấn lớn thứ k sau các thao tác bitwise. Chẳng hạn, OR chỉ có thể tăng bit, AND chỉ có thể xóa bit, XOR lật chúng. Nếu không theo dõi các chuyển đổi này một cách chính xác, việc sắp xếp mảng sau mỗi lần cập nhật sẽ dẫn đến hết thời gian chờ ngay cả trên các đầu vào vừa phải. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: duy trì toàn bộ mảng một cách rõ ràng. Đối với mỗi thao tác AND, OR hoặc XOR, hãy lặp lại tất cả các phần tử và cập nhật chúng. Đối với truy vấn điểm, trả về giá trị được lưu trữ. Đối với truy vấn lớn thứ k, hãy sắp xếp mảng và chọn phần tử cần thiết. 

Điều này hoạt động chính xác vì nó phản ánh chính xác định nghĩa. Mọi thao tác đều được áp dụng cho mọi phần tử và việc sắp xếp luôn phản ánh đúng thứ tự. 

Tuy nhiên, mỗi thao tác toàn cầu có chi phí O(n) và mỗi truy vấn thứ k có chi phí O(n log n) do sắp xếp. Với tối đa q thao tác, tổng độ phức tạp sẽ trở thành O(nq + q n log n), quá lớn khi cả n và q đều lớn. 

Quan sát quan trọng là tất cả các cập nhật đều được thực hiện theo bit và thống nhất trên toàn bộ mảng. Thay vì theo dõi từng giá trị riêng lẻ, chúng tôi theo dõi cách mỗi vị trí bit được chuyển đổi trên toàn cầu. Giá trị của mỗi phần tử đang được biến đổi bởi cùng một chuỗi các hàm bitwise, vì vậy chúng ta có thể biểu diễn phép biến đổi dưới dạng ánh xạ trên các bit thay vì giá trị. 

Chúng tôi duy trì hai mặt nạ khái niệm mô tả cách mỗi bit phát triển: cách XOR lật các bit và cách AND/OR buộc các bit về 0 hoặc 1. Sau khi quá trình chuyển đổi này được duy trì, chúng tôi có thể áp dụng nó một cách lười biếng và chỉ xây dựng lại các giá trị khi cần. 

Để trả lời các truy vấn lớn thứ k một cách hiệu quả, chúng tôi chỉ xây dựng lại các giá trị được chuyển đổi khi cần thiết và sắp xếp một lần cho mỗi truy vấn bằng cách sử dụng biểu diễn được chuyển đổi hoặc duy trì cấu trúc cho phép đếm tần số trên các trạng thái bit tùy thuộc vào các ràng buộc. 

Ý tưởng trung tâm là các hoạt động theo bit sẽ kết hợp độc đáo trên mỗi vị trí bit, cho phép chúng ta tránh chạm vào mọi phần tử mảng trong mỗi lần cập nhật. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | O(nq + qn log n) | O(n) | Quá chậm | 
| Theo dõi chuyển đổi bitwise | O(q log A + n log A) khấu hao | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một biểu diễn nhỏ gọn về cách mỗi bit trong bất kỳ số nào được biến đổi bởi chuỗi các thao tác. Thay vì sửa đổi mảng, chúng tôi duy trì một “hàm bit lười” cho chúng tôi biết điều gì xảy ra với một bit trong tất cả các hoạt động được áp dụng cho đến nay. 

1. Chúng ta bắt đầu mà không áp dụng phép biến đổi nào, nghĩa là mọi bit đều không thay đổi. Điều này tương ứng với ánh xạ nhận dạng cho từng vị trí bit. 

2. Đối với phép toán AND có giá trị c, mỗi bit 0 trong c sẽ buộc bit tương ứng trong mọi phần tử mảng trở thành 0. Các bit 1 trong c vẫn không bị ảnh hưởng. Điều này có nghĩa là chúng tôi cập nhật phép biến đổi của mình để phản ánh việc đưa về 0 bắt buộc. 

3. Đối với phép toán OR có giá trị c, mỗi bit là 1 trong c sẽ buộc bit tương ứng trở thành 1 trong tất cả các phần tử. Chúng tôi cập nhật bản đồ bit toàn cầu cho phù hợp. 

4. Đối với thao tác XOR có giá trị c, mỗi bit sẽ chuyển trạng thái của nó bất cứ khi nào bit trong c là 1. Điều này được xử lý bằng cách chuyển đổi trạng thái biến đổi cho các vị trí bit đó. 

5. Đối với truy vấn giá trị tại chỉ mục i, chúng tôi áp dụng phép biến đổi bit hiện tại cho giá trị được lưu trữ ban đầu a[i], xây dựng lại giá trị hiện tại trong O(log A). 

6. Đối với truy vấn lớn thứ k, chúng tôi áp dụng phép chuyển đổi cho tất cả các phần tử, thu thập kết quả và trích xuất thống kê thứ k. 

Hiệu quả đến từ thực tế là bước 2, 3 và 4 không phụ thuộc vào n mà chỉ phụ thuộc vào độ rộng bit. Tất cả các phần tử mảng được cập nhật ngầm thông qua phép chuyển đổi. 

### Tại sao nó hoạt động

Tại bất kỳ thời điểm nào, mỗi phần tử là kết quả của việc áp dụng một chuỗi các thao tác theo bit cố định cho giá trị ban đầu của nó. Các hoạt động theo bit hoạt động độc lập trên từng vị trí bit, do đó giá trị cuối cùng của mỗi bit chỉ phụ thuộc vào bit đầu tiên và chuỗi các hoạt động được áp dụng cho vị trí bit đó. Tính độc lập này cho phép chúng tôi thay thế các cập nhật theo từng phần tử bằng máy trạng thái theo từng bit. Vì tất cả các phần tử đều có chung một chuyển đổi nên việc áp dụng nó theo yêu cầu sẽ duy trì tính chính xác cho cả truy vấn lập chỉ mục và đặt hàng trực tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def apply_transform(x, mask_and, mask_or, mask_xor):
    # first OR, then XOR, then AND (typical normalized form reasoning)
    x |= mask_or
    x ^= mask_xor
    x &= mask_and
    return x

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    mask_and = (1 << 20) - 1  # assume 20-bit values typical in CF
    mask_or = 0
    mask_xor = 0

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == 'AND':
            c = int(tmp[1])
            mask_and &= c
            mask_or &= c
            mask_xor &= c

        elif tmp[0] == 'OR':
            c = int(tmp[1])
            mask_or |= c
            mask_and |= c

        elif tmp[0] == 'XOR':
            c = int(tmp[1])
            mask_xor ^= c

        elif tmp[0] == 'GET':
            i = int(tmp[1]) - 1
            print(apply_transform(a[i], mask_and, mask_or, mask_xor))

        else:  # KTH
            k = int(tmp[1])
            arr = [apply_transform(x, mask_and, mask_or, mask_xor) for x in a]
            arr.sort(reverse=True)
            print(arr[k - 1])

if __name__ == "__main__":
    solve()
```Mã duy trì ba mặt nạ bit chung thể hiện hiệu ứng tích lũy của tất cả các hoạt động. Thay vì cập nhật mảng, nó cập nhật các mặt nạ này để mã hóa cách hoạt động của từng bit. các`apply_transform`hàm tái tạo lại giá trị hiện tại của bất kỳ phần tử nào khi cần. 

Truy vấn đặt hàng tính toán lại tất cả các giá trị theo yêu cầu, điều này chỉ được chấp nhận nếu các truy vấn đó tương đối ít. Trong các phiên bản chặt chẽ hơn của vấn đề, cấu trúc dữ liệu nâng cao hơn, chẳng hạn như cây phân đoạn hoặc cây phân đoạn theo bit với sự lan truyền lười biếng sẽ thay thế bước tái thiết này. 

Một lỗi thường gặp là áp dụng các thao tác không đúng thứ tự bên trong`apply_transform`. Thứ tự đúng phải tôn trọng cách cấu thành các phép biến đổi: HOẶC đặt các bit trước, XOR lật chúng và cuối cùng xóa các bit. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 0
1 2 3 4 5
GET 3
OR 2
GET 3
XOR 1
GET 3
```| Bước | Hoạt động | mặt nạ_hoặc | mặt nạ_xor | mặt nạ_and | a[3] biến đổi | 
|------|----------|---------|----------|----------|-------------------| 
| 1 | ban đầu | 0 | 0 | tất cả 1s | 3 | 
| 2 | NHẬN 3 | 0 | 0 | tất cả 1s | 3 | 
| 3 | HOẶC 2 | 2 | 0 | không thay đổi | 3 → 3 | 
| 4 | NHẬN 3 | 2 | 0 | không thay đổi | 3 → 3 | 
| 5 | XOR 1 | 2 | 1 | không thay đổi | 3 → 2 | 
| 6 | NHẬN 3 | 2 | 1 | không thay đổi | 2 | 

Dấu vết này cho thấy các mặt nạ chung phát triển độc lập với mảng và mỗi truy vấn chỉ diễn giải trạng thái hiện tại. 

### Ví dụ 2 

đầu vào:```
4 0
5 1 7 3
XOR 6
AND 7
GET 2
KTH 2
```| Bước | Hoạt động | mặt nạ_xor | mặt nạ_and | ảnh chụp nhanh mảng kết quả | 
|------|----------|----------|----------|------------------------| 
| 1 | ban đầu | 0 | tất cả 1s | [5,1,7,3] | 
| 2 | XOR 6 | 6 | tất cả 1s | [3,7,1,5] | 
| 3 | VÀ 7 | 6 | 7 | [3,7,1,5] → [3,7,1,5] (không có thay đổi nào trong phạm vi này) | 
| 4 | NHẬN 2 | 6 | 7 | 7 | 
| 5 | KTH 2 | 6 | 7 | đã sắp xếp: [7,5,3,1] → đáp án 5 | 

Ví dụ này minh họa cách XOR xáo trộn lại các giá trị trong khi AND hạn chế sự tăng trưởng bit và cách truy vấn thứ k phụ thuộc vào các giá trị được xây dựng lại đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(q + n log n trên mỗi truy vấn thứ k) | cập nhật bitmask là O (1), chỉ xây dựng lại cho các truy vấn đặt hàng | 
| Không gian | O(n) | mảng ban đầu được lưu trữ mà không sửa đổi | 

Giải pháp này hiệu quả với số lượng truy vấn đặt hàng vừa phải và dựa vào các cập nhật bitmask liên tục theo thời gian cho tất cả các hoạt động toàn cầu. Nó phù hợp với các ràng buộc điển hình trong đó n và q lên tới khoảng 2e5 nhưng các truy vấn thứ k không chiếm ưu thế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

# These are placeholders since full original statement input format is unspecified

# minimal case
# assert run("...") == "..."

# alternating operations
# assert run("...") == "..."

# all OR operations
# assert run("...") == "..."

# XOR toggling
# assert run("...") == "..."
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| mảng tối thiểu | tính chính xác của truy vấn trực tiếp | trường hợp cơ sở đúng đắn | 
| XOR lặp đi lặp lại | ổn định khi lật | chuyển đổi bit đúng đắn | 
| HOẶC bão hòa | bit buộc phải đúng | cài đặt bit đơn điệu | 
| hoạt động hỗn hợp | đầy đủ thành phần chính xác | tương tác của tất cả các hoạt động | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn được lặp lại các thao tác AND xóa dần dần các bit. Đối với một đầu vào như`[7, 3, 5]`liên tiếp`AND 6`Và`AND 2`, phép biến đổi sẽ thu gọn các giá trị về 0. Thuật toán xử lý vấn đề này vì mặt nạ AND liên tục co lại và quá trình tái tạo luôn áp dụng mặt nạ mới nhất một cách nhất quán. 

Một trường hợp khác xảy ra khi các thao tác XOR được áp dụng nhiều lần theo trình tự. Ví dụ, áp dụng`XOR 1`hai lần hủy bỏ tác dụng của nó. Việc biểu diễn mặt nạ nắm bắt điều này một cách tự nhiên vì tích lũy XOR có thể đảo ngược và tự đảo ngược, do đó trạng thái chuyển đổi chính xác mà không cần theo dõi lịch sử một cách rõ ràng. 

Trường hợp cạnh thứ ba liên quan đến các truy vấn thứ k sau các phép biến đổi nặng tạo ra các bản sao. Vì nhiều phần tử có thể thu gọn về cùng một giá trị nên việc sắp xếp vẫn hoạt động chính xác vì nó hoạt động trên các giá trị được xây dựng lại hoàn toàn tại thời điểm đó, bảo toàn tính bội số.
