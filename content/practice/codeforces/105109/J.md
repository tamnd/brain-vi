---
title: "CF 105109J - Ghi Bản ghi Bản ghi"
description: "Bob có một tập hợp các bản ghi được đánh số từ 1 đến n. Trong n ngày tiếp theo, mỗi ngày anh ấy nghe một số tập hợp con của những bản ghi này. Một bản ghi được coi là mới vào một ngày nhất định nếu Bob chưa bao giờ nghe nó vào bất kỳ ngày nào trước đó."
date: "2026-06-27T20:06:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105109
codeforces_index: "J"
codeforces_contest_name: "UTPC Spring 2024 Open Contest"
rating: 0
weight: 105109
solve_time_s: 65
verified: false
draft: false
---

[CF 105109J - Ghi lại bản ghi kỷ lục](https://codeforces.com/problemset/problem/105109/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Bob có một tập hợp các bản ghi được đánh số từ 1 đến n. Trong n ngày tiếp theo, mỗi ngày anh ấy nghe một số tập hợp con của những bản ghi này. Một bản ghi được coi là mới vào một ngày nhất định nếu Bob chưa bao giờ nghe nó vào bất kỳ ngày nào trước đó. 

Mỗi ngày, chúng tôi quan tâm đến số lượng bản ghi được phát vào ngày hôm đó xuất hiện lần đầu tiên trong toàn bộ lịch sử nghe của Bob. Nhiệm vụ là xác định “số lượng bản ghi mới” tối đa như vậy trong tất cả các ngày. 

Đầu vào là một chuỗi n nhật ký nghe hàng ngày. Mỗi nhật ký cung cấp một danh sách ID bản ghi được phát vào ngày đó. Các bản ghi có thể lặp lại trong vòng một ngày hoặc nhiều ngày, nhưng chỉ lần xuất hiện đầu tiên của mỗi bản ghi mới góp phần vào số lượng "mới" và chỉ vào ngày bản ghi đó xuất hiện lần đầu tiên. 

Đầu ra là một số duy nhất: số lượng lớn nhất các bản ghi chưa từng thấy trước đây xuất hiện trong một ngày bất kỳ. 

Các ràng buộc rất nhỏ: n nhiều nhất là 1000 và tổng số lượt phát được ghi nhiều nhất là 10^6 trong trường hợp xấu nhất. Điều này ngay lập tức gợi ý rằng mô phỏng O(tổng số mục) hoặc O(n^2) là đủ, vì khoảng 10^6 thao tác là an toàn trong Python dưới giới hạn 2 giây. 

Một điểm tinh tế là các bản sao trong một ngày sẽ không làm tăng số lượng. Nếu một bản ghi xuất hiện nhiều lần trong cùng một ngày, bản ghi đó vẫn chỉ đóng góp một lần là “mới” nếu đây là lần đầu tiên nó xuất hiện tổng thể. 

Điều tinh tế thứ hai là chúng tôi phải đánh dấu các bản ghi chỉ được nhìn thấy trên toàn cầu sau khi xử lý chúng trong ngày hôm đó, nếu không các bản ghi trùng lặp trong cùng ngày có thể bị loại bỏ quá sớm do nhầm lẫn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng quy trình hàng ngày trong khi vẫn duy trì một tập hợp toàn cầu gồm tất cả các bản ghi mà Bob đã nghe. Mỗi ngày, chúng tôi quét tất cả m mục và đếm xem có bao nhiêu mục không có trong tập hợp chung. Bất cứ khi nào chúng tôi gặp một bản ghi chưa được xem, chúng tôi sẽ tăng số lượng ngày và đánh dấu nó là đã xem. 

Điều này có tác dụng vì định nghĩa "mới" chỉ phụ thuộc vào việc bản ghi có xuất hiện vào bất kỳ ngày nào trước đó hay không chứ không phụ thuộc vào vị trí của nó trong ngày hiện tại. Điều cần lưu ý duy nhất là tránh đếm cùng một bản ghi nhiều lần trong một ngày nếu nó lặp lại trong danh sách của ngày đó. Điều này có thể được xử lý bằng cách đặt tạm thời mỗi ngày hoặc bằng cách đảm bảo chúng tôi chỉ đánh dấu nó một lần trên toàn cầu và bỏ qua các lần lặp lại tiếp theo trong cùng một ngày. 

Hành vi brute-force đã có cấu trúc tối ưu: mỗi bản ghi được xử lý một lần khi nó mới xuất hiện lần đầu tiên, do đó tổng số lần chèn hiệu quả vào tập hợp toàn cục được giới hạn bởi n. Chi phí hoạt động đang quét tất cả các mục, tối đa là 10^6 thao tác, nằm trong giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét với bộ toàn cầu | O(tổng m) | O(n) | Đã chấp nhận | 
| Kiểm tra lồng nhau mỗi ngày | O(n * m) tệ nhất | O(n) | Được chấp nhận nhưng chi phí không cần thiết | 

## Hướng dẫn thuật toán 

1. Khởi tạo một tập hợp trống để lưu trữ tất cả các bản ghi đã xuất hiện vào những ngày trước đó. Bộ này thể hiện lịch sử toàn cầu về hoạt động nghe của Bob. 
2. Khởi tạo một câu trả lời biến thành 0. Điều này sẽ theo dõi số lượng bản ghi mới tối đa được quan sát thấy trong bất kỳ ngày nào. 
3. Mỗi ngày, hãy đọc danh sách các bản ghi đã phát. 
4. Trong ngày đó, khởi tạo bộ đếm day_new thành 0. 
5. Lặp lại từng bản ghi trong danh sách trong ngày. Nếu bản ghi không được nhìn thấy, điều đó có nghĩa đây là lần đầu tiên nó xuất hiện. Tăng day_new lên 1 và thêm bản ghi cần xem. 
6. Nếu bản ghi đã được nhìn thấy, hãy bỏ qua nó hoàn toàn vì nó không đóng góp cho những khám phá mới. 
7. Sau khi xử lý tất cả các bản ghi trong ngày, hãy cập nhật câu trả lời ở dạng max(answer, day_new). 
8. Sau khi tất cả các ngày được xử lý, xuất ra câu trả lời.

Quyết định triển khai quan trọng là cập nhật ngay lập tức khi một bản ghi lần đầu tiên được gặp trên toàn cầu. Điều này đảm bảo rằng những ngày sau đó sẽ coi nó là cũ một cách chính xác và nó cũng ngăn chặn việc tính hai lần trong các lần quét sau. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình xử lý, tập hợp được thấy khớp chính xác với tập hợp tất cả các bản ghi riêng biệt đã xuất hiện trong những ngày trước đó. Khi chúng tôi xử lý một ngày mới, mọi bản ghi đều được phân loại chính xác dựa trên việc nó có được nhìn thấy hay không. Mỗi bản ghi đóng góp vào số lượng chính xác của một ngày, cụ thể là ngày đầu tiên nó xuất hiện. Vì chúng tôi chỉ tăng day_new khi gặp một bản ghi chưa từng thấy trước đó nên không có bản ghi nào được tính hai lần và không thể bỏ lỡ đóng góp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    seen = set()
    ans = 0

    for _ in range(n):
        arr = list(map(int, input().split()))
        m = arr[0]
        records = arr[1:]

        day_new = 0
        for r in records:
            if r not in seen:
                seen.add(r)
                day_new += 1

        ans = max(ans, day_new)

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng một tập hợp toàn cầu duy nhất để theo dõi xem một bản ghi đã được xem trước đó hay chưa. Dữ liệu đầu vào mỗi ngày được phân tích cú pháp thành một danh sách và phần tử đầu tiên là số m. Phần còn lại là ID hồ sơ. 

Chi tiết triển khai quan trọng là chúng tôi cập nhật tập hợp đã thấy ngay lập tức khi chúng tôi phát hiện một bản ghi mới. Điều này đảm bảo tính chính xác trong nhiều ngày. Chúng tôi không cần thiết lập mỗi ngày vì các bản ghi trùng lặp trong một ngày không thành vấn đề: sau khi một bản ghi được đánh dấu là đã xem, các lần xuất hiện tiếp theo trong cùng ngày sẽ bị bỏ qua một cách tự nhiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 1
2 1 3
3 1 2 3
```| Ngày | Kỷ lục | Đã từng thấy | Mới được tính | Nhìn thấy sau | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | {} | 1 | {1} | 
| 2 | 1 3 | {1} | 1 (chỉ 3) | {1,3} | 
| 3 | 1 2 3 | {1,3} | 1 (chỉ 2) | {1,2,3} | 

Số lượng bản ghi mới tối đa trong bất kỳ ngày nào là 1. Điều này phù hợp với thực tế là sau lần giới thiệu đầu tiên của mỗi bản ghi, mọi thứ sẽ trở nên cũ ngay lập tức. 

### Ví dụ 2 

đầu vào:```
4
3 1 2 3
2 3 3
4 4 5 1 1
1 2
```| Ngày | Kỷ lục | Đã từng thấy | Mới được tính | Nhìn thấy sau | 
| --- | --- | --- | --- | --- | 
| 1 | 1 2 3 | {} | 3 | {1,2,3} | 
| 2 | 3 3 | {1,2,3} | 0 | {1,2,3} | 
| 3 | 4 5 1 1 | {1,2,3} | 2 | {1,2,3,4,5} | 
| 4 | 2 | {1,2,3,4,5} | 0 | {1,2,3,4,5} | 

Đỉnh điểm xảy ra vào ngày thứ 1 với 3 kỷ lục mới, cho thấy ngày đầu tiên có thể lấn át đáp án nếu nhiều vật phẩm độc đáo xuất hiện sớm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng m) | Mỗi bản ghi được kiểm tra một lần và được chèn vào một bộ nhiều nhất một lần | 
| Không gian | O(n) | Bộ lưu trữ mỗi ID bản ghi riêng biệt nhiều nhất một lần | 

Tổng số mục nhập bản ghi trong tất cả các ngày được giới hạn bởi 10^6, do đó, quá trình quét tuyến tính với hàm băm phù hợp thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ cũng bị giới hạn bởi số lượng bản ghi riêng biệt, tối đa là n. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n = int(input())
        seen = set()
        ans = 0
        for _ in range(n):
            arr = list(map(int, input().split()))
            m = arr[0]
            records = arr[1:]
            day_new = 0
            for r in records:
                if r not in seen:
                    seen.add(r)
                    day_new += 1
            ans = max(ans, day_new)
        print(ans)

    from io import StringIO
    out = StringIO()
    backup_stdout = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = backup_stdout
    return out.getvalue().strip()

# provided sample
assert run("""3
1 1
2 1 3
3 1 2 3
""") == "1"

# minimum input
assert run("""1
1 1
""") == "1"

# all same repeats
assert run("""3
3 1 1 1
3 1 1 1
3 1 1 1
""") == "1"

# staggered unique introduction
assert run("""3
1 1
1 2
1 3
""") == "1"

# maximum new on first day
assert run("""2
3 1 2 3
2 4 5
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 ngày lặp lại một lần | 1 | trùng lặp trong ngày | 
| trường hợp 1 phần tử | 1 | ranh giới tối thiểu | 
| danh sách giống hệt nhau lặp đi lặp lại | 1 | chống trùng lặp toàn cầu | 
| khám phá tuần tự | 1 | theo dõi qua nhiều ngày | 
| ngày đầu tiên lớn | 3 | theo dõi chính xác tối đa | 

## Vỏ cạnh 

Trường hợp có các bản ghi lặp lại trong một ngày sẽ xác nhận rằng các bản ghi trùng lặp trong ngày không làm tăng số lượng. Ví dụ, đầu vào`1: [1, 1, 1]`chỉ nên tính một lần. Thuật toán xử lý vấn đề này vì khi 1 được thêm vào đã xem, các lần xuất hiện tiếp theo sẽ bị bỏ qua. 

Trường hợp một bản ghi xuất hiện muộn sau nhiều lần lặp lại trước đó sẽ xác nhận việc theo dõi toàn cầu. Ví dụ: nếu số 5 chỉ xuất hiện vào ngày cuối cùng thì nó vẫn được tính một lần và chỉ vào ngày đó. Bộ này đảm bảo nó không được tính sớm hơn. 

Trường hợp mỗi ngày chỉ lặp lại các yếu tố đã thấy trước đó xác nhận rằng câu trả lời có thể bằng 0 sau ngày đầu tiên. Sau khi nhìn thấy chứa tất cả các bản ghi, tất cả các ngày trong tương lai đều đóng góp bằng 0 và mức tối đa được bảo toàn chính xác từ tính toán trước đó.
