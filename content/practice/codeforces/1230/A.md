---
title: "CF 1230A - Dawid và Túi Kẹo"
description: "Chúng tôi được phát bốn túi riêng biệt, mỗi túi chứa một số kẹo. Mỗi túi không thể chia được và phải được trao hoàn toàn cho đúng một trong hai người bạn."
date: "2026-06-15T05:04:31+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1230
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 588 (Div. 2)"
rating: 800
weight: 1230
solve_time_s: 213
verified: true
draft: false
---

[CF 1230A - Dawid và Túi kẹo](https://codeforces.com/problemset/problem/1230/A) 

**Đánh giá:** 800 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 3m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được phát bốn túi riêng biệt, mỗi túi chứa một số kẹo. Mỗi túi không thể chia được và phải được trao hoàn toàn cho đúng một trong hai người bạn. Nhiệm vụ là xác định xem liệu chúng ta có thể giao mỗi túi trong số bốn túi cho một trong hai người bạn sao cho cả hai người bạn đều có tổng số kẹo như nhau hay không. 

Nói cách khác, chúng ta đang phân chia một tập hợp gồm bốn số nguyên thành hai nhóm và chúng ta muốn tổng của hai nhóm bằng nhau. 

Các ràng buộc cực kỳ nhỏ: mỗi giá trị tối đa là 100 và chỉ có bốn túi. Điều này ngay lập tức loại trừ mọi mối lo ngại về hiệu quả. Bất kỳ giải pháp nào thậm chí thử tất cả các bài tập hoặc tập hợp con đều đủ nhanh vì tổng số khả năng được cố định tại$2^4 = 16$. 

Bởi vì chỉ có bốn mục nên sự tinh tế chính không phải là hiệu suất mà là tính chính xác trong việc xử lý nhất quán tất cả các phân vùng. 

Một sai lầm phổ biến là cho rằng việc ghép nối tham lam có tác dụng, chẳng hạn như kết hợp nhỏ nhất với lớn nhất hoặc chỉ kiểm tra một vài kết hợp. Điều đó không thành công vì cấu trúc không được sắp xếp theo thứ tự và các phân vùng tối ưu có thể kết hợp các thái cực theo những cách không rõ ràng. 

Ví dụ, hãy xem xét:```
1 2 3 4
```Một nỗ lực tham lam có thể ghép nối (1,4) và (2,3), hoạt động hiệu quả, nhưng nếu thay vào đó, một phương pháp phỏng đoán ghép cặp (1,2) và (3,4) thì nó sẽ thất bại ngay cả khi tồn tại một phân vùng hợp lệ. Điều này cho thấy lý luận từng phần về các cặp là không đáng tin cậy trừ khi tất cả các phân vùng đều được xem xét. 

Một trường hợp sai sót khác phát sinh khi tổng số tiền là số lẻ. Ví dụ:```
1 1 1 2
```Tổng số là 5, do đó không thể phân chia bằng nhau bất kể phép gán. Giải pháp đúng nào cũng phải ngầm bác bỏ những trường hợp như vậy. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực là thử mọi cách có thể để giao từng túi trong số bốn túi cho một trong hai người bạn. Vì mỗi túi có hai lựa chọn, điều này dẫn đến$2^4 = 16$bài tập. Với mỗi bài tập, chúng ta tính tổng số kẹo của bạn A và bạn B và kiểm tra xem chúng có khớp nhau không. 

Điều này đã là tối ưu cho kích thước đầu vào nhỏ như vậy. Tính đúng đắn xuất phát từ tính đầy đủ: mỗi phân vùng hợp lệ tương ứng với chính xác một phép gán nhị phân của bốn túi. 

Quan sát duy nhất giúp đơn giản hóa lý luận là chúng ta thực sự không cần liệt kê các bài tập một cách rõ ràng nếu thay vào đó chúng ta nghĩ theo các tập hợp con: việc chọn một tập hợp con các túi cho người bạn đầu tiên sẽ tự động xác định tập hợp con thứ hai. 

Một cách rút gọn mang tính khái niệm hơn một chút là nhận thấy rằng tổng số tiền phải chẵn và chúng ta đang tìm kiếm một tập hợp con các túi có tổng bằng một nửa tổng số. Với bốn phần tử, việc liệt kê tập hợp con là chuyện nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các bài tập) | O(2^4) | O(1) | Đã chấp nhận | 
| Kiểm tra tập hợp con (chế độ xem được tối ưu hóa) | O(2^4) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng giá trị của bốn túi. Nếu tổng là số lẻ, ngay lập tức kết luận rằng không thể chia bằng nhau vì hai số nguyên bằng nhau phải có tổng bằng tổng. 
2. Xác định tổng mục tiêu bằng một nửa tổng số. Đây là số tiền mỗi người bạn phải nhận. 
3. Lặp lại tất cả các tập con của bốn túi. Biểu thị từng tập hợp con bằng cách sử dụng mặt nạ bit từ 0 đến 15, trong đó mỗi bit cho biết liệu một túi có được gán cho người bạn đầu tiên hay không. 
4. Với mỗi tập hợp con, hãy tính tổng số túi đã chọn. Điều này tượng trưng cho số kẹo được tặng cho người bạn đầu tiên. 
5. Nếu tổng tập hợp con này bằng mục tiêu, chúng ta đã ngầm giao các túi còn lại cho người bạn thứ hai và cả hai tổng đều khớp nhau. Trả lại CÓ ngay lập tức. 
6. Nếu không có tập hợp con nào đạt được tổng mục tiêu, trả về NO. 

### Tại sao nó hoạt động 

Mọi phân phối hợp lệ đều tương ứng chính xác với một tập con túi được gán cho người bạn đầu tiên. Các túi còn lại tạo thành tập hợp con bổ sung. Vì chúng tôi kiểm tra tất cả các tập hợp con nên chúng tôi nhất thiết phải kiểm tra mọi phân vùng có thể. Tính đúng đắn dẫn đến thực tế là sự bằng nhau của các tổng tương đương với việc tìm một tập hợp con có tổng bằng một nửa tổng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a = list(map(int, input().split()))
    total = sum(a)
    
    if total % 2 != 0:
        print("NO")
        return
    
    target = total // 2
    
    # try all subsets of 4 elements
    for mask in range(1 << 4):
        s = 0
        for i in range(4):
            if mask & (1 << i):
                s += a[i]
        if s == target:
            print("YES")
            return
    
    print("NO")

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ đọc bốn giá trị và tính tổng của chúng. Việc kiểm tra tính chẵn lẻ sẽ ngăn chặn việc liệt kê tập hợp con không cần thiết khi không thể có được sự bằng nhau. 

Vòng lặp bitmask liệt kê tất cả các phép gán túi có thể có cho người bạn đầu tiên. Vòng lặp bên trong tích lũy tổng tập hợp con. Thời điểm tìm thấy một tập hợp con hợp lệ, hàm sẽ thoát sớm vì sự tồn tại là đủ. 

Một chi tiết triển khai tinh tế là chúng ta không cần tính toán tổng của người bạn thứ hai một cách rõ ràng. Khi tổng đầu tiên khớp với một nửa tổng số, phần bù cũng tự động khớp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 7 11 5
```Tổng cộng là 24, mục tiêu là 12. 

| mặt nạ | tập hợp con | tổng tập hợp con | 
| --- | --- | --- | 
| 0001 | [1] | 1 | 
| 0010 | [7] | 7 | 
| 0011 | [1,7] | 8 | 
| 0100 | [11] | 11 | 
| 0101 | [1,11] | 12 | 

Tại mặt nạ 0101, chúng ta đạt được tổng mục tiêu là 12, vì vậy câu trả lời là CÓ. 

Điều này xác nhận rằng một phân vùng hợp lệ tồn tại trong đó tập hợp con được chọn và phần bù của nó đều có tổng bằng 12. 

### Ví dụ 2 

đầu vào:```
1 1 1 2
```Tổng cộng là 5, thật là lẻ. 

| bước | tổng cộng | kiểm tra tính chẵn lẻ | quyết định | 
| --- | --- | --- | --- | 
| 1 | 5 | lẻ | KHÔNG | 

Vì tổng số không thể chia đều nên chúng tôi kết luận ngay rằng không có phân vùng hợp lệ nào tồn tại mà không liệt kê các tập hợp con. 

Điều này chứng tỏ rằng việc kiểm tra tính chẵn lẻ sẽ loại bỏ chính xác các trường hợp không thể xảy ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có 16 tập hợp con được kiểm tra, mỗi tập có 4 phần tử | 
| Không gian | O(1) | Không có cấu trúc phụ trợ ngoài một vài biến | 

Giới hạn trên không đổi làm cho giải pháp này trở nên tầm thường trong giới hạn. Ngay cả trong kịch bản thử nghiệm nhiều lần, khối lượng công việc vẫn không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    a = list(map(int, input().split()))
    total = sum(a)
    if total % 2 != 0:
        return "NO"
    target = total // 2
    for mask in range(1 << 4):
        s = 0
        for i in range(4):
            if mask & (1 << i):
                s += a[i]
        if s == target:
            return "YES"
    return "NO"

# provided samples
assert run("1 7 11 5") == "YES"
assert run("1 1 1 2") == "NO"

# custom cases
assert run("2 2 2 2") == "YES", "all equal"
assert run("1 2 3 6") == "YES", "exact partition exists"
assert run("1 2 4 8") == "NO", "no subset sum match"
assert run("100 1 1 98") == "YES", "boundary values"

print("ok")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 2 2 | CÓ | phân phối đối xứng | 
| 1 2 3 6 | CÓ | trận đấu tập hợp con không tầm thường | 
| 1 2 4 8 | KHÔNG | không có phân vùng hợp lệ | 
| 100 1 1 98 | CÓ | giá trị ranh giới và cực trị | 

## Vỏ cạnh 

Trường hợp cạnh chẵn lẻ được xử lý ngay từ đầu. Đối với một đầu vào như:```
1 1 1 2
```thuật toán tính tổng số 5 và ngay lập tức trả về NO. Điều này ngăn chặn việc liệt kê tập hợp con không cần thiết. 

Đối với đầu vào đối xứng như:```
2 2 2 2
```thuật toán sẽ vẫn hoạt động thông qua các tập hợp con, nhưng nó nhanh chóng tìm thấy nhiều mặt nạ hợp lệ, chẳng hạn như chọn hai túi bất kỳ có tổng bằng 4. 

Đối với các trường hợp có một phần tách hợp lệ bị ẩn giữa nhiều phần không hợp lệ, chẳng hạn như:```
100 1 1 98
```tìm kiếm tập hợp con đảm bảo rằng ngay cả các kết hợp không trực quan cũng được kiểm tra và mặt nạ chọn {100, 1} là đủ để đạt 101, khớp với mục tiêu 101.
