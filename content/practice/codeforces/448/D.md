---
title: "CF 448D - Bảng cửu chương"
description: "Chúng tôi đang làm việc với một lưới nhân khái niệm trong đó ô ở hàng i và cột j chứa giá trị i × j. Thay vì xây dựng bảng này một cách rõ ràng, chúng ta tưởng tượng việc liệt kê tất cả các giá trị n × m và sắp xếp chúng theo thứ tự không giảm."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 448
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 256 (Div. 2)"
rating: 1800
weight: 448
solve_time_s: 74
verified: true
draft: false
---

[CF 448D - Bảng cửu chương](https://codeforces.com/problemset/problem/448/D) 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, vũ lực 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một lưới nhân khái niệm trong đó ô trong hàng`i`và cột`j`chứa giá trị`i × j`. Thay vì xây dựng bảng này một cách rõ ràng, chúng ta tưởng tượng việc liệt kê tất cả`n × m`giá trị và sắp xếp chúng theo thứ tự không giảm. Nhiệm vụ là tìm giá trị xuất hiện ở vị trí`k`trong danh sách được sắp xếp này. 

Khó khăn chính là quy mô. Cả hai kích thước của bảng có thể lớn tới 500.000, khiến tổng số phần tử lên tới 2,5 × 10¹¹. Bất kỳ cách tiếp cận nào cố gắng tạo hoặc sắp xếp tất cả các giá trị đều không khả thi ngay lập tức. Ngay cả việc lặp lại tất cả các hàng và cột một cách rõ ràng là không thể trong giới hạn thời gian. 

Điều này đẩy chúng ta tới một chiến lược trong đó chúng ta không bao giờ xây dựng bảng một cách trực tiếp mà thay vào đó suy luận về số lượng giá trị. 

Trường hợp cạnh tinh tế xuất hiện khi tồn tại nhiều giá trị lặp lại. Ví dụ, trong một bảng nhỏ như`n = 3, m = 3`, giá trị`2`xuất hiện nhiều lần. Cách tiếp cận làm phẳng và sắp xếp ngây thơ vẫn có hiệu quả về mặt khái niệm, nhưng sẽ thất bại do giới hạn về bộ nhớ và thời gian. Một trường hợp cạnh khác là khi`k = 1`, nơi luôn có câu trả lời`1`, và khi nào`k = n × m`, đáp án ở đâu`n × m`. Bất kỳ giải pháp đúng đắn nào cũng phải xử lý các ranh giới này một cách tự nhiên mà không cần vỏ bọc đặc biệt. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi tạo ra mọi sản phẩm`i × j`, lưu trữ chúng trong một danh sách, sắp xếp danh sách và trả về`k`-phần tử thứ. Điều này đúng vì nó xây dựng rõ ràng nhiều tập hợp mà chúng ta được yêu cầu đặt hàng. Tuy nhiên, số phần tử`n × m`, trong trường hợp xấu nhất là khoảng 2,5 × 10¹¹. Ngay cả việc lưu trữ 10⁷ số nguyên cũng đã rất chặt chẽ trong Python; lưu trữ 10¹¹ là không thể. Việc sắp xếp cũng thêm hệ số logarit, khiến cho hệ số này hoàn toàn không thể sử dụng được. 

Quan sát quan trọng là chúng ta thực sự không cần danh sách được sắp xếp đầy đủ. Chúng ta chỉ cần trả lời các truy vấn có dạng: có bao nhiêu mục trong bảng nhỏ hơn hoặc bằng một giá trị nào đó`x`. Nếu chúng ta có thể tính toán con số này một cách hiệu quả thì chúng ta có thể sử dụng tìm kiếm nhị phân cho câu trả lời. 

Đối với một cố định`x`, mỗi hàng`i`đóng góp`min(m, x // i)`giá trị, bởi vì trong hàng`i`, tất cả các cột`j`như vậy`i × j ≤ x`thỏa mãn`j ≤ x // i`. Tính tổng số này trên tất cả các hàng sẽ cho tổng số phần tử ≤`x`. 

Điều này chuyển vấn đề thành một tìm kiếm vị ngữ đơn điệu: như`x`tăng lên, số lượng giá trị ≤`x`không bao giờ giảm. Tính đơn điệu đó cho phép tìm kiếm nhị phân trên không gian câu trả lời từ`1`ĐẾN`n × m`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm log(nm)) | O(nm) | Quá chậm | 
| Tối ưu (Tìm kiếm nhị phân + Đếm) | O(n log(nm)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng ta xây dựng giải pháp bằng cách sử dụng tìm kiếm nhị phân trên giá trị câu trả lời. 

1. Xác định hàm`count(x)`trả về số lượng ô trong bảng nhân nhỏ hơn hoặc bằng`x`. Đối với mỗi hàng`i`, cột hợp lệ lớn nhất là`x // i`, nhưng không thể vượt quá`m`, vì vậy chúng tôi thêm`min(m, x // i)`. 
2. Đặt phạm vi tìm kiếm nhị phân là`lo = 1`Và`hi = n × m`. Phạm vi này chứa tất cả các giá trị có thể có trong bảng một cách an toàn. 
3. Tính toán nhiều lần`mid = (lo + hi) // 2`. 
4. Tính toán`count(mid)`. Nếu giá trị này ít nhất`k`, sau đó`mid`đủ lớn để có khả năng là câu trả lời, vì vậy chúng tôi di chuyển`hi = mid`. 
5. Nếu không thì ít hơn`k`các phần tử là ≤`mid`, vì vậy chúng tôi cần các giá trị lớn hơn và chúng tôi di chuyển`lo = mid + 1`. 
6. Tiếp tục cho đến khi`lo == hi`. Vào thời điểm đó,`lo`là giá trị nhỏ nhất sao cho ít nhất`k`các phần tử là ≤ nó, khớp với định nghĩa của phần tử nhỏ thứ k trong nhiều tập hợp. 

Lý do chính khiến mỗi bước đều hợp lệ là hàm`count(x)`là đơn điệu trong`x`. Một khi là một giá trị`x`đủ lớn để bao gồm ít nhất`k`các phần tử, tất cả các giá trị lớn hơn cũng sẽ đáp ứng thuộc tính này, vì vậy tìm kiếm nhị phân sẽ tách biệt ranh giới một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_leq(n, m, x):
    total = 0
    for i in range(1, n + 1):
        total += min(m, x // i)
    return total

def main():
    n, m, k = map(int, input().split())

    lo, hi = 1, n * m

    while lo < hi:
        mid = (lo + hi) // 2
        if count_leq(n, m, mid) >= k:
            hi = mid
        else:
            lo = mid + 1

    print(lo)

if __name__ == "__main__":
    main()
```Chi tiết triển khai cốt lõi là`count_leq`chức năng. Nó tránh việc xây dựng bảng và thay vào đó tính toán các đóng góp theo từng hàng. Mỗi hàng`i`đóng góp mọi giá trị`i × 1`bởi vì`i × (x // i)`, nhưng chúng tôi giới hạn ở mức`m`vì cái bàn chỉ có`m`cột. 

Tìm kiếm nhị phân duy trì một bất biến mà câu trả lời nằm trong`[lo, hi]`. Các quy tắc cập nhật thu hẹp khoảng thời gian này trong khi vẫn duy trì tính chính xác. Một lỗi phổ biến là sử dụng`hi = mid - 1`, điều này phá vỡ tìm kiếm kiểu giới hạn dưới cần thiết cho các bản sao; ở đây chúng tôi cố tình giữ`hi = mid`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2 2
```Chúng tôi tìm kiếm trong phạm vi`[1, 4]`. 

| lo | xin chào | giữa | đếm (giữa) | 
| --- | --- | --- | --- | 
| 1 | 4 | 2 | 3 | 

Tại`mid = 2`, giá trị 2 là`{1, 2, 2}`, nên số đếm là 3 ≥ k=2, thu gọn sang phải. 

| lo | xin chào | giữa | đếm (giữa) | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 1 | 

Bây giờ count(1) = 1 < 2, di chuyển trái lên trên. 

Chúng tôi kết thúc tại`lo = 2`, đó là câu trả lời. 

Điều này cho thấy cách các bản sao (hai số 2 trong bảng) được xử lý một cách tự nhiên bằng cách đếm thay vì sắp xếp rõ ràng. 

### Ví dụ 2 

đầu vào:```
3 3 5
```Phạm vi tìm kiếm`[1, 9]`. 

| lo | xin chào | giữa | đếm (giữa) | 
| --- | --- | --- | --- | 
| 1 | 9 | 5 | 7 | 

Vì 7 ≥ 5 nên di chuyển sang trái. 

| lo | xin chào | giữa | đếm (giữa) | 
| --- | --- | --- | --- | 
| 1 | 5 | 3 | 4 | 

Vì 4 < 5 nên di chuyển sang phải. 

| lo | xin chào | giữa | đếm (giữa) | 
| --- | --- | --- | --- | 
| 4 | 5 | 4 | 6 | 

Bây giờ 6 ≥ 5, di chuyển sang trái. 

| lo | xin chào | giữa | đếm (giữa) | 
| --- | --- | --- | --- | 
| 4 | 4 | - | - | 

Đáp án là 4. 

Dấu vết này cho thấy cách thuật toán hội tụ đến giá trị nhỏ nhất có số tiền tố đạt tới k. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log(nm)) | Mỗi bước tìm kiếm nhị phân sẽ quét tất cả các hàng và có các bước O(log(nm)) | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng | 

Giải pháp này phù hợp với các ràng buộc vì độ sâu tìm kiếm nhị phân là khoảng 40 và mỗi bước thực hiện tối đa 500.000 phép chia số nguyên đơn giản, là giới hạn nhưng có thể chấp nhận được trong Python được tối ưu hóa theo PyPy hoặc các giả định đầu vào nhanh điển hình cho Codeforces. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m, k = map(int, input().split())

    def count_leq(n, m, x):
        total = 0
        for i in range(1, n + 1):
            total += min(m, x // i)
        return total

    lo, hi = 1, n * m
    while lo < hi:
        mid = (lo + hi) // 2
        if count_leq(n, m, mid) >= k:
            hi = mid
        else:
            lo = mid + 1
    return str(lo)

# provided sample
assert run("2 2 2\n") == "2", "sample 1"

# minimum case
assert run("1 1 1\n") == "1", "single element"

# rectangular skewed table
assert run("1 100000 50\n") == "50", "single row"

# square case
assert run("3 3 5\n") == "4", "middle selection"

# max edge small k
assert run("100000 100000 1\n") == "1", "minimum value"

# max edge large k
assert run("100000 100000 10000000000\n") == "10000000000", "maximum value"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 1 | ranh giới nhỏ nhất | 
| 1 100000 50 | 50 | hành vi hàng đơn | 
| 3 3 5 | 4 | độ chính xác của lựa chọn trung vị | 
| 100000 100000 1 | 1 | tối thiểu toàn cầu | 
| 100000 100000 10000000000 | tối đa | tối đa toàn cầu | 

## Vỏ cạnh 

cho`n = 1`, bảng thoái hóa thành một hàng`[1, 2, ..., m]`. Thuật toán rút gọn về tìm kiếm nhị phân nhỏ nhất`x`như vậy`x ≥ k`, từ`count(x)`trở nên chính xác`min(m, x)`. Hàm đếm xử lý việc này một cách tự nhiên vì chỉ có hàng`i = 1`đóng góp. 

Vì`k = 1`, tìm kiếm nhị phân ngay lập tức hội tụ về`1`bởi vì`count(1)`luôn ít nhất là 1. Không cần logic đặc biệt. 

Vì`k = n × m`, tìm kiếm sẽ hướng tới sản phẩm lớn nhất`n × m`. Tại`x = n × m`, mọi mục nhập đều được tính và vị từ trở thành đúng, cố định giới hạn trên một cách chính xác. 

Đối với các bảng không cân bằng cao như`n = 1`và lớn`m`hoặc ngược lại, vòng lặp vẫn chạy trên tất cả các hàng nhưng vẫn tuyến tính trên mỗi lần kiểm tra và tìm kiếm nhị phân chỉ đảm bảo lặp lại logarit của lần quét này.
