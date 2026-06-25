---
title: "CF 105239B - Chúng ta hãy cùng nhau tập hợp một danh mục đầu tư"
description: "Chúng ta được cung cấp một tập hợp các dự án và đối với mỗi dự án có một số cách loại trừ lẫn nhau để thực hiện dự án đó. Mỗi cách đều có chi phí và doanh thu. Đối với mỗi dự án, chúng ta phải chọn chính xác một trong những cách có sẵn hoặc bỏ qua toàn bộ dự án."
date: "2026-06-24T11:11:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105239
codeforces_index: "B"
codeforces_contest_name: "Dynamic Programming, SPbSU 2024, Training 1"
rating: 0
weight: 105239
solve_time_s: 44
verified: true
draft: false
---

[CF 105239B - Chúng ta hãy cùng nhau tập hợp một danh mục đầu tư](https://codeforces.com/problemset/problem/105239/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các dự án và đối với mỗi dự án có một số cách loại trừ lẫn nhau để thực hiện dự án đó. Mỗi cách đều có chi phí và doanh thu. Đối với mỗi dự án, chúng ta phải chọn chính xác một trong những cách có sẵn hoặc bỏ qua toàn bộ dự án. Tổng chi phí của tất cả các cách đã chọn không được vượt quá ngân sách toàn cầu, trong khi mục tiêu là tối đa hóa tổng doanh thu. 

Một cách hữu ích để diễn giải lại cấu trúc là mỗi dự án đóng góp một “nhóm lựa chọn” nhỏ các hạng mục. Từ mỗi nhóm, chúng tôi lấy một món đồ hoặc không lấy gì cả. Mỗi mặt hàng có trọng lượng bằng chi phí và giá trị bằng doanh thu. Ràng buộc là sức chứa ba lô duy nhất được chia sẻ cho tất cả các nhóm, nhưng với hạn chế bổ sung là chúng tôi không thể lấy nhiều hơn một vật phẩm từ mỗi nhóm. 

Các ràng buộc được cố tình không đối xứng. Số lượng dự án nhiều nhất là 20 và tổng số tùy chọn trên tất cả các dự án cũng nhiều nhất là 20. Tuy nhiên, ngân sách có thể rất lớn, lên tới 10^9, điều này ngay lập tức loại trừ bất kỳ DP nào phụ thuộc vào ngân sách làm thứ nguyên. Bất kỳ giải pháp nào cố gắng lặp lại các trạng thái chi phí sẽ thất bại cả về bộ nhớ và thời gian. 

Điều này chuyển toàn bộ khó khăn từ chiếc ba lô dựa trên sức chứa sang việc liệt kê tập hợp con trên các vật phẩm hoặc nhóm. 

Trường hợp phức tạp xuất hiện khi một dự án chỉ có những lựa chọn đắt tiền nhưng bỏ qua là tối ưu. Ví dụ: nếu một dự án có các tùy chọn (doanh thu 100, chi phí 10) và (doanh thu 80, chi phí 9) và ngân sách là 5 thì cả hai tùy chọn đều không khả thi, do đó hành vi đúng là bỏ qua hoàn toàn dự án, mang lại doanh thu bằng 0 từ dự án đó. Bất kỳ cách triển khai ngây thơ nào giả định “phải chọn một tùy chọn cho mỗi dự án” sẽ buộc phải lựa chọn vượt quá ngân sách một cách không chính xác. 

Một trường hợp khó khăn khác là khi tất cả chi phí đều bằng không. Sau đó, mọi dự án đều có thể được thực hiện một cách tự do và câu trả lời tối ưu chỉ đơn giản là tổng của phương án doanh thu tốt nhất cho mỗi dự án, vì ngân sách không liên quan. Trường hợp này cũng cho thấy việc triển khai coi các ràng buộc về chi phí là tùy chọn thay vì nghiêm ngặt. 

Cuối cùng, vì tổng các tùy chọn trên tất cả các dự án là nhỏ, nên gợi ý cấu trúc quan trọng là chúng ta có thể coi mọi tùy chọn riêng lẻ như một mục nguyên tử trong bài toán tập con toàn cục, nhưng chúng ta phải đảm bảo rằng chúng ta không bao giờ chọn hai mục từ cùng một dự án. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ là coi đây như một chiếc ba lô có nhiều lựa chọn với ngân sách là kích thước DP. Điều đó dẫn đến trạng thái như dp[i][b] đại diện cho doanh thu tốt nhất sau i dự án đầu tiên và ngân sách b. Điều này ngay lập tức là không thể vì b lên tới 10^9, do đó, ngay cả một lớp DP cũng không thể thực hiện được. 

Chúng tôi có thể thử nén ngân sách chỉ sử dụng chi phí được quan sát, nhưng vì chi phí tùy ý lên tới 10^9 và không bị giới hạn bởi tổng số tiền nên điều này vẫn không giúp ích gì. 

Quan sát quan trọng là tổng số lựa chọn chỉ là 20. Con số này đủ nhỏ để cho phép liệt kê theo cấp số nhân trên các tập hợp con của tất cả các tùy chọn có sẵn. Tuy nhiên, việc liệt kê tập hợp con ngây thơ trên 20 mục mang lại 2^20 khả năng, tức là khoảng một triệu, hoàn toàn có thể chấp nhận được. 

Điều phức tạp là hạn chế của mỗi dự án: chúng ta không thể chọn hai tùy chọn thuộc cùng một dự án. Điều này gợi ý rằng chúng ta cần mã hóa tính khả thi của một tập hợp con về mặt xung đột dự án. 

Cách rõ ràng để giải quyết vấn đề này là coi mỗi tùy chọn như một mục được gắn nhãn bởi chỉ mục dự án của nó. Sau đó, chúng tôi liệt kê các tập hợp con của tất cả các tùy chọn và đối với mỗi tập hợp con, chúng tôi kiểm tra tính hợp lệ bằng cách đảm bảo không có hai mục nào dùng chung một dự án. Nếu hợp lệ, chúng tôi sẽ tính toán tổng chi phí và doanh thu cũng như kiểm tra tính khả thi của ngân sách. 

Vì tổng số mục chỉ có 20 nên việc kiểm tra từng tập hợp con là O(20), dẫn đến tổng là O(20 * 2^20), điều này hiệu quả.

Một góc nhìn khác là DP theo từng nhóm dự án đối với các tập hợp con của dự án, nhưng vì mỗi dự án có nhiều tùy chọn nên việc làm phẳng và xác thực các tập hợp con sẽ đơn giản hơn và ít xảy ra lỗi hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Ngân sách DP vượt quá chi phí | O(n · b) | O(b) | Không thể do b lên tới 1e9 | 
| Liệt kê tập hợp con trên tất cả các tùy chọn | O(2^m · m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Thu thập tất cả các tùy chọn có sẵn vào một danh sách duy nhất, trong đó mỗi tùy chọn lưu trữ id dự án, chi phí và doanh thu. Chúng tôi coi việc bỏ qua một dự án là được bao gồm ngầm bằng cách cho phép các tập hợp con bỏ qua tất cả các tùy chọn của nó. 
2. Lặp lại tất cả các tập hợp con của các tùy chọn này bằng cách sử dụng mặt nạ bit từ 0 đến 2^m − 1, trong đó m là tổng số tùy chọn. 
3. Đối với mỗi tập hợp con, hãy theo dõi xem dự án đã được chọn bằng cách sử dụng mảng boolean hay mặt nạ bit hay chưa. Nếu chúng tôi gặp hai tùy chọn thuộc cùng một dự án, chúng tôi sẽ loại bỏ ngay tập hợp con này. Điều này ngăn chặn cấu hình bất hợp pháp. 
4. Trong khi quét một tập hợp con hợp lệ, hãy tích lũy tổng chi phí và tổng doanh thu. Tính toán gia tăng này đảm bảo mỗi đánh giá tập hợp con là O(m) thay vì tính toán lại từ đầu. 
5. Nếu tổng chi phí nằm trong ngân sách, hãy cập nhật câu trả lời với doanh thu tối đa đạt được cho đến nay. 

Lý do chúng tôi xác thực rõ ràng trong quá trình lặp lại thay vì lọc trước là do xung đột phụ thuộc vào sự kết hợp của các mục đã chọn chứ không chỉ các mục riêng lẻ. 

### Tại sao nó hoạt động 

Mỗi giải pháp khả thi đều tương ứng chính xác với một tập hợp con các phương án bao gồm nhiều nhất một phương án cho mỗi dự án và tổng chi phí của chúng không vượt quá ngân sách. Thuật toán liệt kê tất cả các tập hợp con của các tùy chọn, vì vậy nó chắc chắn liệt kê tất cả các giải pháp khả thi. Việc kiểm tra tính hợp lệ sẽ lọc ra chính xác những tập hợp con vi phạm quy tắc một tùy chọn cho mỗi dự án và việc kiểm tra ngân sách sẽ thực thi tính khả thi. Vì chúng tôi đánh giá doanh thu của mọi cấu hình hợp lệ nên giá trị tối đa được ghi lại phải bằng giải pháp tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, b = map(int, input().split())
    items = []

    for i in range(n):
        k = int(input())
        for _ in range(k):
            r, c = map(int, input().split())
            items.append((i, c, r))

    m = len(items)
    ans = 0

    for mask in range(1 << m):
        used_projects = 0
        total_cost = 0
        total_rev = 0
        ok = True

        for j in range(m):
            if mask & (1 << j):
                pid, cost, rev = items[j]

                if used_projects & (1 << pid):
                    ok = False
                    break

                used_projects |= (1 << pid)
                total_cost += cost
                total_rev += rev

        if ok and total_cost <= b:
            if total_rev > ans:
                ans = total_rev

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai làm phẳng tất cả các tùy chọn dự án thành một mảng duy nhất trong khi vẫn giữ được bản sắc dự án. Bảng liệt kê tập hợp con sử dụng mặt nạ bit trên các tùy chọn này. Đối với mỗi mặt nạ, chúng tôi duy trì bitmask sử dụng dự án để đảm bảo không có dự án nào đóng góp nhiều hơn một tùy chọn đã chọn. 

Các khoản tích lũy chi phí và doanh thu là những khoản tiền đơn giản. Việc kiểm tra ngân sách chỉ được thực hiện sau khi đảm bảo tính hợp lệ, nhằm tránh những so sánh lãng phí. 

Một chi tiết triển khai tinh tế là chúng tôi sử dụng một bitmask riêng cho việc sử dụng dự án thay vì một bộ. Điều này giúp kiểm tra O(1) cho mỗi mục và tránh chi phí Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 10
2
100 10
80 9
1
10 1
```Chúng tôi có tổng cộng ba tùy chọn: dự án 0 có hai tùy chọn, dự án 1 có một tùy chọn. 

| mặt nạ | phương án đã chọn | dự án hợp lệ | chi phí | doanh thu | hợp lệ | tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 000 | không | {} | 0 | 0 | vâng | 0 | 
| 001 | (10,1) | {1} | 1 | 10 | vâng | 10 | 
| 010 | (80,9) | {0} | 9 | 80 | vâng | 80 | 
| 011 | (80,9)+(10,1) | {0,1} | 10 | 90 | vâng | 90 | 
| 100 | (100,10) | {0} | 10 | 100 | vâng | 100 | 
| 101 | (100,10)+(10,1) | {0,1} | 11 | 110 | không | 100 | 
| 110 | không hợp lệ (hai từ dự án 0) | - | - | - | không | 100 | 
| 111 | không hợp lệ | - | - | - | không | 100 | 

Dấu vết này cho thấy giải pháp tốt nhất là chọn phương án 1 của dự án 0 cộng với phương án duy nhất của dự án 1, đạt doanh thu 110 nhưng vượt ngân sách nên bị loại. Phương án khả thi tối ưu là chọn doanh thu 100+10 với chi phí 11 cũng không khả thi, tốt nhất chỉ chọn 100 thôi. 

### Ví dụ 2 

đầu vào:```
1 5
2
50 3
40 2
```| mặt nạ | đã chọn | chi phí | doanh thu | hợp lệ | tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | không | 0 | 0 | vâng | 0 | 
| 1 | (50,3) | 3 | 50 | vâng | 50 | 
| 2 | (40,2) | 2 | 40 | vâng | 50 | 
| 3 | cả hai | 5 | 90 | không | 50 | 

Điều này cho thấy hạn chế đối với mỗi dự án cấm kết hợp cả hai tùy chọn mặc dù ngân sách sẽ cho phép điều đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m · 2^m) | Mọi tập hợp con đều được kiểm tra và mỗi lần kiểm tra sẽ quét tối đa m mục | 
| Không gian | O(m) | Lưu trữ các mặt hàng dẹt và trạng thái tạm thời | 

Tổng số tùy chọn nhiều nhất là 20, vì vậy 2^20 là khoảng một triệu. Nhân với 20 thao tác trên mỗi tập hợp con vẫn dễ dàng nằm trong giới hạn trong 2 giây trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, b = map(int, input().split())
    items = []
    for i in range(n):
        k = int(input())
        for _ in range(k):
            r, c = map(int, input().split())
            items.append((i, c, r))

    m = len(items)
    ans = 0

    for mask in range(1 << m):
        used = 0
        cost = 0
        rev = 0
        ok = True

        for j in range(m):
            if mask & (1 << j):
                pid, c, r = items[j]
                if used & (1 << pid):
                    ok = False
                    break
                used |= (1 << pid)
                cost += c
                rev += r

        if ok and cost <= b:
            ans = max(ans, rev)

    return str(ans)

# sample-like
assert run("2 10\n2\n100 10\n80 9\n1\n10 1\n") == "100"

# single project
assert run("1 5\n2\n50 3\n40 2\n") == "50"

# zero budget
assert run("1 0\n1\n10 0\n") == "10"

# all too expensive
assert run("2 1\n1\n10 5\n1\n20 6\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| giống mẫu | 100 | cắt tỉa chính xác các tập hợp con không hợp lệ | 
| dự án đơn lẻ | 50 | xử lý lựa chọn theo từng dự án | 
| ngân sách bằng không | 10 | hành vi ranh giới chi phí bằng không | 
| tất cả đều quá đắt | 0 | bỏ qua tất cả các dự án | 

## Vỏ cạnh 

Trường hợp một cạnh là khi ngân sách bằng không. Thuật toán vẫn liệt kê các tập hợp con, nhưng chỉ những tập hợp con có tổng chi phí bằng 0 mới tồn tại. Điều này mang lại chính xác doanh thu bằng 0 hoặc bất kỳ tùy chọn chi phí nào bằng 0 nếu nó tồn tại. 

Một trường hợp khác là khi một dự án có nhiều phương án giá rẻ nhưng chỉ nên chọn một phương án. Mặt nạ bit của dự án đảm bảo rằng bất kỳ tập hợp con nào chọn hai tùy chọn từ cùng một dự án sẽ bị từ chối ngay lập tức, vì vậy ngay cả khi cả hai đều có giá cả phải chăng riêng lẻ, chúng sẽ không bao giờ đóng góp cùng nhau. 

Trường hợp cuối cùng là khi tất cả các lựa chọn đều đắt tiền. Vòng lặp tập hợp con vẫn chạy, nhưng mọi tập hợp con không trống đều không thể kiểm tra ngân sách và chỉ tập hợp con trống vẫn hợp lệ, tạo ra kết quả bằng 0.
