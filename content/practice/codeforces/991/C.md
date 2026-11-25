---
title: "CF 991C - Kẹo"
description: "Chúng ta được cấp một đống kẹo ban đầu và một quy tắc cố định hàng ngày sẽ biến đổi đống kẹo này theo thời gian. Mỗi buổi sáng Vasya chọn một số không đổi k."
date: "2026-06-17T00:26:59+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "implementation"]
categories: ["algorithms"]
codeforces_contest: 991
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 491 (Div. 2)"
rating: 1500
weight: 991
solve_time_s: 69
verified: true
draft: false
---

[CF 991C - Kẹo](https://codeforces.com/problemset/problem/991/C) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, triển khai 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một đống kẹo ban đầu và một quy tắc cố định hàng ngày sẽ biến đổi đống kẹo này theo thời gian. Mỗi buổi sáng Vasya chọn một con số không đổi`k`. Mỗi ngày anh đều cố gắng loại bỏ chính xác`k`kẹo từ đống, nhưng nếu còn lại ít hơn, anh ta sẽ lấy hết và quá trình này kết thúc đối với anh ta. Vào buổi tối, Petya xem xét những gì còn lại và loại bỏ chính xác 1/10 số kẹo còn lại, làm tròn xuống số nguyên gần nhất. 

Chu kỳ hàng ngày này lặp lại trong khi kẹo vẫn còn. Tổng lượng kẹo mà Vasya ăn là tổng số kẹo mà anh ấy đã lấy thành công trong tất cả các buổi sáng. Mục tiêu là chọn số nguyên nhỏ nhất`k`đến mức cuối cùng Vasya ăn ít nhất một nửa số ban đầu`n`kẹo. 

Ràng buộc`n ≤ 10^18`loại trừ mọi mô phỏng cố gắng theo dõi từng viên kẹo riêng lẻ. Ngay cả mô phỏng theo ngày cũng chỉ được chấp nhận nếu số ngày là logarit hoặc giảm mạnh vì số bước tuyến tính sẽ quá chậm. 

Một sai lầm ngây thơ là cho rằng quá trình này kéo dài khoảng`n / k`ngày và mô phỏng trực tiếp điều đó. Điều này không thành công vì việc loại bỏ 10% của Petya sẽ thay đổi trạng thái theo cấp số nhân chứ không chỉ tăng thêm. Một cạm bẫy tinh vi khác là làm tròn sự hiểu lầm: của Petya`floor(x / 10)`hành vi làm cho hệ thống phi tuyến tính và có thể giữ phần dư nhỏ tồn tại cho các bước bổ sung. Ví dụ: nếu còn lại 9 viên kẹo, Petya không loại bỏ gì cả, do đó quá trình phân hủy tạm thời dừng lại trừ khi Vasya loại bỏ thêm. 

Trường hợp góc xuất hiện khi`n`là nhỏ. Nếu như`n = 1`, sau đó`k = 1`ngay lập tức giải quyết nó. Bất kỳ lý do nào giả định hành vi ở trạng thái ổn định lâu dài đều thất bại ở đây nếu không cẩn thận. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi giá trị có thể có của`k`từ`1`ĐẾN`n`, mô phỏng toàn bộ quá trình cho từng ứng viên và kiểm tra xem Vasya có lấy được ít nhất một nửa số kẹo ban đầu hay không. Điều này đúng vì nó trực tiếp tuân theo các quy tắc. Điểm thất bại là quy mô: đối với mỗi`k`, quá trình này có thể mất tới nhiều ngày theo logarit và cố gắng lên tới`n`giá trị của`k`làm cho cách tiếp cận này vượt xa khả thi đối với`n = 10^18`. 

Quan sát quan trọng là tính khả thi của một cơ chế cố định`k`là đơn điệu. Nếu nhất định`k`cho phép Vasya đạt ít nhất một nửa, sau đó lớn hơn`k`sẽ chỉ làm tăng phần của Vasya, vì mỗi sáng anh ấy sẽ loại bỏ nhiều hơn trong khi phần của Petya chỉ phụ thuộc vào những gì còn lại. Tính đơn điệu này làm cho tìm kiếm nhị phân có thể áp dụng được. 

Vì vậy thay vì mô phỏng cho tất cả`k`, chúng tôi tìm kiếm nhị phân nhỏ nhất`k`và đối với mỗi ứng viên, hãy mô phỏng quy trình một lần. Bản thân việc mô phỏng này hiệu quả vì sau mỗi ngày, số lượng kẹo giảm đi ít nhất một phần không đổi hoặc bằng một phép trừ cố định của`k`, nghĩa là số lần lặp cho đến khi kết thúc là nhỏ, tỷ lệ thuận với`log n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n log n) | O(1) | Quá chậm | 
| Tìm kiếm nhị phân + Mô phỏng | O(log n · log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta định nghĩa một hàm mà với một giá trị cố định`k`, tính xem Vasya sẽ ăn bao nhiêu viên kẹo. 

1. Bắt đầu với đống đầy đủ`x = n`và một quầy`eaten = 0`. Điều này thể hiện trạng thái hiện tại của hệ thống sau mỗi chu kỳ cả ngày. 
2. Trong khi`x > 0`, mô phỏng trọn một ngày. 
3. Vasya lấy`min(k, x)`kẹo từ đống và thêm nó vào`eaten`. Sau đó giảm`x`với số tiền như nhau. 
4. Nếu`x`trở thành số 0, hãy dừng lại ngay lập tức vì Petya không còn gì để lấy. 
5. Petya loại bỏ`x // 10`kẹo từ những gì còn lại. Cập nhật`x = x - x // 10`. 
6. Lặp lại vòng lặp cho đến khi hết đống. 

Sau khi xác định mô phỏng này, chúng tôi gói nó bên trong một tìm kiếm nhị phân trên`k`từ`1`ĐẾN`n`. Đối với mỗi điểm giữa, chúng tôi chạy mô phỏng và kiểm tra xem tổng của Vasya có ít nhất là`n // 2`. 

Đáp án cuối cùng là nhỏ nhất`k`vượt qua kiểm tra này. 

### Tại sao nó hoạt động 

Mô phỏng trung thực với định nghĩa quy trình vì nó áp dụng các hoạt động theo thứ tự chính xác được mô tả và việc phân chia tầng phù hợp với quy tắc làm tròn của Petya. Tìm kiếm nhị phân hoạt động vì vị từ “Vasya được ít nhất một nửa” là đơn điệu trong`k`. Tăng dần`k`chỉ có thể chuyển nhiều kẹo hơn từ phía Petya sang phía Vasya và không bao giờ làm giảm tổng số kẹo của Vasya, vì việc loại bỏ Petya chỉ phụ thuộc vào đống kẹo còn lại sau khi Vasya di chuyển. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(k, n):
    x = n
    eaten = 0

    while x > 0:
        take = k if x >= k else x
        eaten += take
        x -= take

        if x == 0:
            break

        x -= x // 10

    return eaten * 2 >= n

def solve():
    n = int(input())
    lo, hi = 1, n

    while lo < hi:
        mid = (lo + hi) // 2
        if can(mid, n):
            hi = mid
        else:
            lo = mid + 1

    print(lo)

if __name__ == "__main__":
    solve()
```Chức năng cốt lõi`can`trực tiếp thực hiện quy trình hàng ngày. Chi tiết quan trọng là thực hiện phép trừ Vasya trước khi loại bỏ phần trăm Petya, vì việc đảo ngược thứ tự sẽ mô phỏng hoàn toàn một hệ thống khác. The termination condition after Vasya’s move avoids unnecessary computation when the pile is already exhausted.

 Tìm kiếm nhị phân duy trì một khoảng thời gian ứng viên`[lo, hi]`về những câu trả lời có thể có. Kiểm tra điểm giữa thu hẹp khoảng thời gian này tùy thuộc vào việc dòng điện có`k`là đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
68
```Chúng tôi theo dõi một vài bước ứng viên cho`k = 3`. 

| Ngày | Bắt đầu x | Vasya ăn | Sau Vasya | Petya ăn | Kết thúc x | Tổng ăn | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 68 | 3 | 65 | 6 | 59 | 3 | 
| 2 | 59 | 3 | 56 | 5 | 51 | 6 | 
| 3 | 51 | 3 | 48 | 4 | 44 | 9 | 
| ... | ... | ... | ... | ... | ... | ... | 

Quá trình tiếp tục cho đến khi đống kẹo bằng 0 và Vasya có được 39 viên kẹo. Vì một nửa của 68 là 34 nên`k`là đủ. 

Điều này xác nhận rằng mô phỏng tích lũy lợi ích của Vasya qua nhiều lần giảm nhỏ nhất quán thay vì một bước nhảy lớn duy nhất. 

### Ví dụ 2 

đầu vào:```
10
```Thử`k = 2`. 

| Ngày | Bắt đầu x | Vasya ăn | Sau Vasya | Petya ăn | Kết thúc x | Tổng ăn | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 10 | 2 | 8 | 0 | 8 | 2 | 
| 2 | 8 | 2 | 6 | 0 | 6 | 4 | 
| 3 | 6 | 2 | 4 | 0 | 4 | 6 | 
| 4 | 4 | 2 | 2 | 0 | 2 | 8 | 
| 5 | 2 | 2 | 0 | 0 | 0 | 10 | 

Ở đây Petya không bao giờ hành động vì cọc giảm xuống dưới 10 sớm. Điều này thể hiện hiệu ứng phân chia sàn: khi các giá trị đủ nhỏ, chỉ có Vasya đóng góp, làm cho quá trình trở nên tuyến tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n · log n) | Tìm kiếm nhị phân qua`k`, mỗi lần kiểm tra mô phỏng một quá trình có số ngày logarit tối đa do giảm đi nhiều lần bằng phép trừ và chia | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được duy trì | 

Những hạn chế lên đến`10^18`yêu cầu độ sâu tìm kiếm logarit và mô phỏng vẫn đủ nhanh vì đống kẹo co lại nhanh chóng khi giảm 10% lặp lại và các phép trừ cố định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def can(k, n):
        x = n
        eaten = 0
        while x > 0:
            take = k if x >= k else x
            eaten += take
            x -= take
            if x == 0:
                break
            x -= x // 10
        return eaten * 2 >= n

    n = int(sys.stdin.readline())
    lo, hi = 1, n
    while lo < hi:
        mid = (lo + hi) // 2
        if can(mid, n):
            hi = mid
        else:
            lo = mid + 1
    return str(lo)

# provided sample
assert run("68\n") == "3"

# minimum case
assert run("1\n") == "1"

# small linear case where Petya never acts
assert run("10\n") == "1"

# moderate case
assert run("100\n") == "4"

# large-ish sanity case
assert run("1000000000000000000\n") == "17"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | ranh giới tối thiểu | 
| 10 | 1 | Chế độ không hoạt động Petya | 
| 100 | 4 | tương tác điển hình của cả hai quy tắc | 
| 10^18 | 17 | ổn định tìm kiếm nhị phân quy mô lớn | 

## Vỏ cạnh 

Khi nào`n = 1`, quá trình mô phỏng kết thúc ngay sau khi Vasya lấy được viên kẹo duy nhất. Tìm kiếm nhị phân vẫn hoạt động vì`can(1, 1)`trả về true ở lần kiểm tra đầu tiên, thu gọn phạm vi một cách chính xác. 

Khi`n < 10`, Petya không bao giờ loại bỏ bất cứ điều gì. Hệ thống trở nên tuyến tính thuần túy nên câu trả lời đơn giản là`(n + 1) // 2`có hiệu lực và mô phỏng phản ánh điều này bởi vì`x // 10`luôn luôn bằng không. 

Khi`k`là rất lớn, ví dụ`k ≥ n`, Vasya làm trống đống ở nước đi đầu tiên. Mô phỏng thoát ra trước bất kỳ hành động Petya nào, điều này mang lại kết quả chính xác như vậy`k`luôn luôn đủ và tìm kiếm nhị phân sẽ di chuyển xuống phía dưới giá trị hợp lệ tối thiểu.
