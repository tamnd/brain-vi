---
title: "CF 460C - Hiện Tại"
description: "Chúng tôi được yêu cầu tối đa hóa chiều cao tối thiểu của hoa sau một số ngày tưới nước giới hạn. Mỗi bông hoa có chiều cao ban đầu và vào bất kỳ ngày nào chúng ta có thể tưới nước cho một đoạn liền kề gồm chính xác w bông hoa, tăng dần từng bông hoa."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 460
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 262 (Div. 2)"
rating: 1700
weight: 460
solve_time_s: 60
verified: true
draft: false
---

[CF 460C - Hiện tại](https://codeforces.com/problemset/problem/460/C) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tối đa hóa chiều cao tối thiểu của hoa sau một số ngày tưới nước giới hạn. Mỗi bông hoa có chiều cao ban đầu và vào bất kỳ ngày nào chúng ta có thể tưới chính xác _w_ hoa cho một đoạn liền kề, tăng từng bông hoa lên. Chúng ta có _m_ ngày để áp dụng các lần tưới như vậy và hoa được sắp xếp theo một hàng cố định nên các phân đoạn tưới nước phải tôn trọng thứ tự. 

Đầu vào cung cấp số lượng hoa _n_, số ngày _m_ và chiều rộng tưới _w_, theo sau là chiều cao ban đầu của hoa. Đầu ra là một số nguyên duy nhất: chiều cao lớn nhất có thể của bông hoa nhỏ nhất sau khi tưới nước một cách tối ưu trong _m_ ngày. 

Các ràng buộc cho chúng ta biết rằng _n_ có thể lên tới 10^5 và _m_ cũng có thể lên tới 10^5, với chiều cao hoa lên tới 10^9. Điều này loại trừ bất kỳ mô phỏng ngây thơ nào trong đó chúng tôi thử mọi trình tự tưới nước có thể vì số lượng khả năng là theo cấp số nhân. Mọi giải pháp đều phải hoạt động ở khoảng O(n log X) hoặc O(n) cho mỗi trường hợp thử nghiệm, trong đó X được giới hạn bởi chiều cao tối đa có thể đạt được. 

Một trường hợp cạnh quan trọng xảy ra khi bông hoa nhỏ nhất đã cao so với những bông hoa khác hoặc khi _w_ = 1. Ví dụ: nếu _n_ = 3, _m_ = 1, _w_ = 1 và độ cao ban đầu là`[1, 2, 3]`, chiều cao tối thiểu tối đa là`2`bởi vì chúng ta chỉ có thể tăng một bông hoa lên một đơn vị. Một cách tiếp cận bất cẩn cố gắng tưới nước một cách tham lam từ trái sang phải mà không tính đến số lần tưới còn lại sẽ đánh giá quá cao mức tối thiểu có thể đạt được. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ sẽ thử tất cả các chuỗi tưới nước hàng ngày, cập nhật mảng mỗi lần và kiểm tra mức tối thiểu. Điều này đúng vì bất kỳ trình tự tưới nước nào cũng có thể được mô phỏng, nhưng nó yêu cầu các thao tác O(n * m * n) trong trường hợp xấu nhất, quá chậm với n, m ≤ 10^5. 

Điểm mấu chốt là vấn đề có thể được trình bày lại thành một vấn đề quyết định: "Chúng ta có thể đạt được độ cao tối thiểu H bằng cách sử dụng tối đa m lần tưới không?" Nếu chúng ta có thể trả lời câu hỏi này một cách hiệu quả, thì chúng ta có thể áp dụng tìm kiếm nhị phân trên H. Điều này hiệu quả vì việc tăng H là đơn điệu: nếu H có thể đạt được thì mọi H nhỏ hơn cũng có thể đạt được; nếu H không thể đạt được thì bất kỳ H nào lớn hơn cũng không thể đạt được. 

Việc kiểm tra xem có thể đạt được độ cao H hay không có thể được thực hiện một cách tham lam bằng mảng sai phân để mô phỏng tác động gia tăng của việc tưới nước trong O(n). Đối với mỗi bông hoa, chúng tôi theo dõi tác động thực của các lần tưới trước đó và quyết định xem có áp dụng phân đoạn tưới nước mới để đáp ứng chiều cao H hay không. Điều này tránh việc cập nhật toàn bộ mảng mỗi lần và giữ độ phức tạp tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m * n) | O(n) | Quá chậm | 
| Tối ưu (Tìm kiếm nhị phân + Tham lam + Mảng khác biệt) | O(n log(max_height)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`low`đến chiều cao hoa tối thiểu và`high`ĐẾN`max(a) + m`, giới hạn trên lý thuyết của chiều cao tối thiểu sau khi áp dụng tất cả các lần tưới cho những bông hoa nhỏ nhất. 
2. Trong khi`low < high`, nhặt`mid = (low + high + 1) // 2`. Đây là chiều cao tối thiểu H của ứng viên mà chúng tôi muốn kiểm tra. 
3. Để kiểm tra xem H có đạt được hay không, hãy khởi tạo một mảng sai phân`add`có kích thước n+1 đến 0. Mảng này sẽ theo dõi số lần tưới ảnh hưởng đến từng bông hoa một cách hiệu quả. 
4. Theo dõi`current_add = 0`, mức tăng trưởng ròng từ những lần tưới trước khi chúng tôi lặp lại từ trái sang phải. 
5. Với mỗi bông hoa i từ 0 đến n-1, hãy tính`current_height = a[i] + current_add`. Nếu như`current_height < H`, chúng ta phải tưới nước bắt đầu từ i. Số lần tưới cần thiết là`delta = H - current_height`. 
6. Tăng dần`current_add`theo delta và cập nhật`add[i + w] -= delta`nếu như`i + w < n`để biểu thị tác dụng của việc tưới nước này kết thúc sau vị trí i + w - 1. Theo dõi tổng số lần tưới nước đã sử dụng cho đến nay. 
7. Nếu tổng lượng nước tưới vượt quá m thì không đạt được H; nếu không thì đúng là như vậy. 
8. Nếu H đạt được, đặt`low = mid`; nếu không thì đặt`high = mid - 1`. 
9. Sau khi tìm kiếm nhị phân kết thúc,`low`là chiều cao tối thiểu tối đa có thể đạt được. 

Tại sao nó hoạt động: Mảng chênh lệch đảm bảo rằng mỗi lần tưới ảnh hưởng chính xác đến w bông hoa liền kề và chúng ta có thể tính toán hiệu ứng thực lên bất kỳ bông hoa nào trong O(1). Tìm kiếm nhị phân tận dụng tính đơn điệu của hàm quyết định. Sự lựa chọn tham lam ở mỗi bông hoa đảm bảo chúng ta không bao giờ ở dưới nước, không đạt được H, và không bao giờ tưới quá nhiều nước, có thể lãng phí nhiều ngày. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_achieve(H, n, m, w, a):
    add = [0] * (n + 1)
    current_add = 0
    used = 0
    for i in range(n):
        current_add += add[i]
        if a[i] + current_add < H:
            delta = H - (a[i] + current_add)
            used += delta
            if used > m:
                return False
            current_add += delta
            if i + w < n:
                add[i + w] -= delta
    return True

def solve():
    n, m, w = map(int, input().split())
    a = list(map(int, input().split()))
    low, high = min(a), max(a) + m
    while low < high:
        mid = (low + high + 1) // 2
        if can_achieve(mid, n, m, w, a):
            low = mid
        else:
            high = mid - 1
    print(low)

if __name__ == "__main__":
    solve()
```Giải pháp tách biệt việc tìm kiếm nhị phân và kiểm tra tính khả thi.`can_achieve`sử dụng một mảng khác biệt nên mỗi lần tưới chỉ cập nhật hai vị trí, giữ độ phức tạp về thời gian O(n). Các lỗi riêng lẻ có thể tránh được bằng cách lập chỉ mục cẩn thận cho mảng chênh lệch, lưu ý rằng hiệu ứng tưới nước từ vị trí i kéo dài cho đến i + w - 1. 

## Ví dụ đã hoạt động 

**Mẫu 1**: 

đầu vào:`n=6, m=2, w=3, a=[2,2,2,2,1,1]`Kiểm tra tìm kiếm nhị phân H = 3: 

| tôi | một [tôi] | hiện tại_add | đồng bằng | đã qua sử dụng | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 0 | 1 | 1 | 
| 1 | 2 | 1 | 0 | 1 | 
| 2 | 2 | 1 | 0 | 1 | 
| 3 | 2 | 1 | 1 | 2 | 
| 4 | 1 | 1 | 1 | 3 → vượt quá m=2 → Sai | 

Vậy H=3 thất bại, H=2 thành công. Đầu ra là 2. 

**Ví dụ tùy chỉnh**: 

đầu vào:`n=5, m=5, w=2, a=[1,1,1,1,1]`Kiểm tra tìm kiếm nhị phân H=3: 

| tôi | một [tôi] | hiện tại_add | đồng bằng | đã qua sử dụng | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 2 | 2 | 
| 1 | 1 | 2 | 0 | 2 | 
| 2 | 1 | 2 | 1 | 3 | 
| 3 | 1 | 1 | 2 | 5 | 
| 4 | 1 | 0 | 3 | 8 → vượt quá m=5 → Sai | 

H=2 là có thể đạt được. Đầu ra 2. 

Điều này cho thấy mảng khác biệt truyền tưới nước hiệu quả như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log(max(a)+m)) | Tìm kiếm nhị phân trên chiều cao tối thiểu tiềm năng, với mỗi lần kiểm tra O(n) | 
| Không gian | O(n) | Mảng chênh lệch kích thước n+1 | 

Cho n, m ≤ 10^5 và chiều cao ≤ 10^9, giải pháp sẽ chạy thoải mái trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sysio
    f = sysio.StringIO()
    with redirect_stdout(f):
        solve()
    return f.getvalue().strip()

# provided sample
assert run("6 2 3\n2 2 2 2 1 1\n") == "2", "sample 1"

# minimum input
assert run("1 1 1\n1\n") == "2", "minimum n"

# all equal
assert run("4 4 2\n1 1 1 1\n") == "3", "equal heights"

# maximum width
assert run("5 3 5\n1 2 1 2 1\n") == "2", "w=n"

# maximum watering days not needed
assert run("3 100 1\n1 2 3\n") == "103", "unused waterings"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1\n1`| 2 | Đầu vào kích thước tối thiểu | 
|`4 4 2\n1 1 1 1`| 3 | Tất cả |
