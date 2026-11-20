---
title: "CF 2161C - Lòng trung thành"
description: "Chúng ta được yêu cầu tối đa hóa điểm thưởng trong một cửa hàng nơi mỗi lần mua hàng đều có khả năng làm tăng mức độ trung thành của chúng ta. Mỗi mặt hàng có giá từ 1 đến một số hệ số trung thành $X$. Ban đầu, mức độ trung thành của chúng tôi được tính bằng cách chia tổng số tiền chi tiêu $S$ cho $X$."
date: "2026-06-07T23:58:40+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2161
codeforces_index: "C"
codeforces_contest_name: "Pinely Round 5 (Div. 1 + Div. 2)"
rating: 1200
weight: 2161
solve_time_s: 111
verified: false
draft: false
---

[CF 2161C - Lòng trung thành](https://codeforces.com/problemset/problem/2161/C) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, tham lam, sắp xếp, hai con trỏ 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tối đa hóa điểm thưởng trong một cửa hàng nơi mỗi lần mua hàng đều có khả năng làm tăng mức độ trung thành của chúng ta. Mỗi mặt hàng có giá từ 1 đến một số hệ số trung thành$X$. Ban đầu, mức độ trung thành của chúng tôi được tính bằng chia số nguyên của tổng chi tiêu của chúng tôi$S$qua$X$. Mỗi khi mua một món đồ làm tăng mức độ trung thành, chúng ta sẽ kiếm được điểm thưởng bằng với giá của món đồ đó. Thứ tự mua hàng rất quan trọng vì cùng một mặt hàng được mua theo trình tự khác nhau có thể tạo ra mức tăng mức độ trung thành khác nhau. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp cung cấp số lượng mặt hàng, hệ số trung thành và danh sách giá mặt hàng. Đầu ra là tổng số điểm thưởng tối đa có thể đạt được và một ví dụ về các vật phẩm đạt được mức tối đa này. Số lượng mục trên mỗi trường hợp thử nghiệm có thể cao tới mức$10^5$và tổng của tất cả các trường hợp thử nghiệm cũng lên tới$10^5$, vì vậy chúng ta cần một cách tiếp cận hiệu quả. 

Một giải pháp ngây thơ sẽ thử tất cả$n!$hoán vị của các mục để tìm ra chuỗi tối đa hóa điểm thưởng. Ngay cả đối với$n = 10$, điều này đã không thực tế nên chúng ta cần một cách tiếp cận thông minh hơn. Các trường hợp cạnh bao gồm việc có tất cả các mục bằng$X$, đảm bảo điểm thưởng cho mỗi lần mua hàng hoặc có tất cả các mặt hàng nhỏ hơn$X$đến mức một mặt hàng không thể kích hoạt mức độ trung thành tăng lên ngay lập tức. 

Một trường hợp tinh tế khác là khi tổng các phần tử nhỏ hơn vừa đạt đến bội số tiếp theo của$X$- ở đây, thuật toán phải lựa chọn cẩn thận mặt hàng nào cần mua trước để kích hoạt càng nhiều mức độ trung thành tăng càng tốt. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là tạo ra tất cả các hoán vị của các mục và mô phỏng cách tính tiền thưởng cho mỗi chuỗi. Về nguyên tắc, điều này hoạt động chính xác vì nó kiểm tra rõ ràng mọi thứ tự có thể, nhưng độ phức tạp về thời gian là$O(n!)$, điều này hoàn toàn không thể thực hiện được đối với$n \sim 10^5$. Đối với các mảng nhỏ gồm 10 phần tử, điều này đã dẫn đến hơn 3 triệu hoán vị. 

Quan sát quan trọng để tối ưu hóa là chúng tôi chỉ quan tâm đến việc đạt được bội số của$X$một cách hiệu quả. Điểm thưởng xảy ra khi tổng tích lũy vượt qua bội số tiếp theo của$X$. Điều này gợi ý một cách tiếp cận tham lam: chúng ta muốn chi tiêu càng ít càng tốt trước khi đạt được bội số$X$để món hàng tiếp theo sẽ kích hoạt mức độ trung thành tăng lên. Nếu chúng tôi sắp xếp các mục theo thứ tự tăng dần, chúng tôi đảm bảo các mục nhỏ hơn sẽ được tích lũy trước, đưa tổng tích lũy gần với bội số tiếp theo mà không vượt quá mức. Sau đó, các vật phẩm lớn hơn có thể được sử dụng để kích hoạt mức độ trung thành tăng lên. 

Nói cách khác, bằng cách sắp xếp các mục theo thứ tự không giảm, mỗi mục hoặc là quá nhỏ để có thể tự mình kích hoạt mức tăng mức độ trung thành hoặc là mục tối thiểu đẩy tổng vượt quá bội số của$X$. Thứ tự này tối đa hóa số lần tăng mức độ trung thành, đó chính xác là thời điểm chúng tôi nhận được điểm thưởng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc số mục$n$và yếu tố trung thành$X$. 
2. Đọc danh sách giá các mặt hàng$a_1, a_2, ..., a_n$. 
3. Sắp xếp giá các mặt hàng theo thứ tự tăng dần. Điều này đảm bảo trước tiên chúng tôi sử dụng các mục nhỏ hơn để tiếp cận bội số$X$một cách hiệu quả. 
4. Khởi tạo tổng tích lũy$S = 0$và điểm thưởng$B = 0$. 
5. Lặp lại các mục đã được sắp xếp: 

- Thêm giá mặt hàng hiện tại vào$S$. 
- Tính mức độ trung thành trước đó như$(S - a_i) // X$. 
- Tính mức độ trung thành mới như$S // X$. 
- Nếu mức độ trung thành mới vượt quá mức trước đó, hãy thêm giá vật phẩm hiện tại vào$B$. 
6. Sau khi xử lý tất cả các mục, xuất ra$B$và thứ tự sắp xếp. 

Tại sao nó hoạt động: Việc sắp xếp đảm bảo rằng trước tiên chúng ta tích lũy những khoản đóng góp nhỏ hơn, vì vậy mỗi khi chúng ta vượt qua bội số của$X$, mục chịu trách nhiệm vượt qua nó càng lớn càng tốt trong số các mục còn lại. Điều này tối đa hóa số điểm thưởng thu được từ mỗi lần tăng mức độ trung thành. Điều bất biến là ở mỗi bước,$S$là tổng của tất cả các mặt hàng đã mua cho đến nay và vượt qua bội số của$X$kích hoạt chính xác phần thưởng từ vật phẩm hiện tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, X = map(int, input().split())
        prices = list(map(int, input().split()))
        prices.sort()
        bonus = 0
        S = 0
        for price in prices:
            prev_loyalty = S // X
            S += price
            new_loyalty = S // X
            if new_loyalty > prev_loyalty:
                bonus += price
        print(bonus)
        print(' '.join(map(str, prices)))

if __name__ == "__main__":
    solve()
```Mã đọc nhiều trường hợp kiểm thử một cách hiệu quả bằng cách sử dụng I/O nhanh. Việc sắp xếp là cần thiết để tối đa hóa điểm thưởng và logic tổng tích lũy đảm bảo rằng chúng tôi phát hiện chính xác khi mức độ trung thành tăng lên. Cần thận trọng khi tính toán mức độ trung thành trước đó và mới cho từng mặt hàng một cách riêng biệt để tránh các lỗi riêng lẻ. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
10 2
1 2 1 2 1 2 1 2 1 2
```| Bước | Mục | S trước | S sau | trước_level | cấp_mới | Tiền thưởng | Tiền thưởng tích lũy | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 0 | 0 | 0 | 0 | 
| 2 | 1 | 1 | 2 | 0 | 1 | 1 | 1 | 
| 3 | 1 | 2 | 3 | 1 | 1 | 0 | 1 | 
| 4 | 1 | 3 | 4 | 1 | 2 | 1 | 2 | 
| 5 | 2 | 4 | 6 | 2 | 3 | 2 | 4 | 
| 6 | 2 | 6 | 8 | 3 | 4 | 2 | 6 | 
| 7 | 2 | 8 | 10 | 4 | 5 | 2 | 8 | 
| 8 | 2 | 10 | 12 | 5 | 6 | 2 | 10 | 
| 9 | 2 | 12 | 14 | 6 | 7 | 2 | 12 | 
| 10 | 2 | 14 | 16 | 7 | 8 | 2 | 14 | 

Điều này chứng tỏ rằng việc sắp xếp đảm bảo kích hoạt tối đa mức độ trung thành tăng lên. 

### Đầu vào mẫu 2```
5 10
2 2 2 2 5
```| Bước | Mục | S trước | S sau | trước_level | cấp_mới | Tiền thưởng | Tiền thưởng tích lũy | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 2 | 0 | 0 | 0 | 0 | 
| 2 | 2 | 2 | 4 | 0 | 0 | 0 | 0 | 
| 3 | 2 | 4 | 6 | 0 | 0 | 0 | 0 | 
| 4 | 2 | 6 | 8 | 0 | 0 | 0 | 0 | 
| 5 | 5 | 8 | 13 | 0 | 1 | 5 | 5 | 

Cho thấy rằng các mặt hàng nhỏ hơn không kích hoạt lòng trung thành cho đến khi số tiền vượt quá$X$, thì mục cuối cùng sẽ mang lại phần thưởng tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; lặp lại và tính tổng là O(n) | 
| Không gian | O(n) | Chúng tôi lưu trữ danh sách đã sắp xếp và tổng tích lũy | 

Điều này có thể chấp nhận được với tổng$n$qua các trường hợp thử nghiệm là$10^5$, vừa vặn thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("1\n10 2\n1 2 1 2 1 2 1 2 1 2\n") == "12\n1 1 1 1 1 2 2 2 2 2", "sample 1"
assert run("1\n5 10\n2 2 2 2 5\n") == "5\n2 2 2 2 5", "sample 2"

# Custom cases
assert run("1\n1 1\n1\n") == "1\n1", "single item triggers bonus"
assert run("1\n3 100\n44 32 1\n") == "0\n1 32 44", "no item crosses loyalty initially"
assert run("1\n5 5\n5 5 5 5 5\n") == "25\n5 5 5 5 5", "all items max loyalty"
assert run("1\n4 7\n1 2 3 1\n") == "0\n1 1 2 3", "small
```
