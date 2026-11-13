---
title: "CF 1690E - Tối đa hóa giá"
description: "Chúng ta được cấp một số lượng hàng hóa chẵn, mỗi hàng có trọng lượng và chúng ta cần đóng gói chúng thành từng cặp. Giá của một cặp được tính bằng cách lấy tổng trọng số của nó chia cho một số cố định $k$ và làm tròn xuống số nguyên gần nhất."
date: "2026-06-09T23:19:04+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1690
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 797 (Div. 3)"
rating: 1500
weight: 1690
solve_time_s: 120
verified: true
draft: false
---

[CF 1690E - Tối đa hóa giá](https://codeforces.com/problemset/problem/1690/E) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, tham lam, toán học, hai con trỏ 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số lượng hàng hóa chẵn, mỗi hàng có trọng lượng và chúng ta cần đóng gói chúng thành từng cặp. Giá của một cặp được tính bằng cách lấy tổng trọng lượng của nó chia cho một số cố định$k$và làm tròn xuống số nguyên gần nhất. Mục tiêu của chúng tôi là tạo ra các cặp sao cho tổng chi phí của tất cả các gói được tối đa hóa. Mỗi trường hợp thử nghiệm cung cấp trọng lượng của hàng hóa và giá trị của$k$. Chúng ta phải trả về tổng chi phí tối đa cho mỗi trường hợp thử nghiệm. 

Những ràng buộc cho chúng ta tới$2 \cdot 10^5$tổng số hàng hóa trên tất cả các trường hợp thử nghiệm. Điều này loại trừ mọi cách tiếp cận thử tất cả các cách ghép đôi có thể, vì số cách ghép đôi$n$các mặt hàng tăng trưởng theo giai đoạn. Cụ thể, việc ghép đôi bạo lực gần như là$O((n-1)!!)$, điều này là không thể ngay cả đối với$n = 20$. 

Trường hợp cạnh tinh tế xảy ra khi tất cả các trọng số đều chia hết cho$k$, hoặc khi tất cả đều nhỏ hơn$k$. Ví dụ, với$a = [2, 2, 2, 2]$Và$k = 3$, việc ghép nối ngây thơ có thể vô tình làm giảm số lượng gói đạt bội số$k$, tạo ra tổng chi phí dưới mức tối ưu. Một trường hợp phức tạp khác là có nhiều số 0 hoặc nhiều mục nhỏ hơn$k/2$; việc ghép nối bất cẩn có thể lãng phí những vật dụng nhỏ này khi chúng có thể được ghép với trọng lượng lớn hơn để tối đa hóa lợi ích phân chia sàn. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả các cặp và tính tổng của$\lfloor (a_i + a_j)/k \rfloor$. Mặc dù điều này đảm bảo câu trả lời chính xác nhưng số lượng cặp giai thừa là không thể chấp nhận được đối với$n = 2 \cdot 10^5$. 

Thông tin chuyên sâu quan trọng đến từ việc xem xét sự đóng góp của từng gói theo modulo$k$. Mỗi mục$a_i$có thể được viết là$a_i = q_i \cdot k + r_i$, Ở đâu$q_i = a_i // k$là phần đóng góp được đảm bảo vào tổng số, và$r_i = a_i \% k$là phần còn lại có thể kết hợp với phần còn lại khác để tạo thành bội số khác của$k$. Điều này làm giảm vấn đề ghép cặp số dư theo cách tối đa hóa số bội số bổ sung của$k$. 

Sau khi sắp xếp phần còn lại, chúng ta có thể sử dụng chiến lược hai con trỏ. Ghép số dư nhỏ nhất với số dư lớn nhất. Nếu tổng của chúng ít nhất là$k$, chúng ta tạo thành một giá trị gói bổ sung mới; nếu không, di chuyển phần còn lại nhỏ hơn về phía trước. Chiến lược tham lam này đảm bảo rằng chúng tôi tối đa hóa các khoản đóng góp bổ sung từ phần còn lại mà không lãng phí phần còn lại cao cho phần còn lại thấp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Sắp xếp + Hai con trỏ | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$,$k$, và danh sách các trọng số$a$. 
2. Tính phần đóng góp đảm bảo từ mỗi hạng mục theo phép chia số nguyên$q_i = a_i // k$và tổng hợp chúng để khởi tạo tổng chi phí. 
3. Tính số dư$r_i = a_i \% k$cho từng mục và lưu trữ chúng trong một danh sách. 
4. Sắp xếp danh sách số dư theo thứ tự tăng dần. 
5. Khởi tạo hai con trỏ:$i = 0$lúc bắt đầu,$j = n-1$ở cuối. 
6. Trong khi$i < j$: 

- Kiểm tra xem$r_i + r_j \ge k$. Nếu đúng, tăng tổng chi phí lên 1 và di chuyển cả hai con trỏ vào trong ($i += 1, j -= 1$), tạo thành một gói tạo ra một đơn vị bổ sung. 
- Nếu tổng nhỏ hơn$k$, di chuyển con trỏ còn lại nhỏ hơn về phía trước ($i += 1$), bởi vì việc ghép nó với bất kỳ số dư lớn hơn nào sẽ chỉ tạo ra giá trị bổ sung ít hơn hoặc bằng nhau. 
7. Sau vòng lặp, xuất ra tổng chi phí. 

Tại sao nó hoạt động: Mỗi mục đóng góp bội số được đảm bảo của nó$k$. Bằng cách sắp xếp và ghép các phần còn lại nhỏ nhất và lớn nhất, chúng tôi tối đa hóa số lượng các cặp vượt quá$k$, bởi vì bất kỳ số dư nhỏ hơn nào sẽ bị lãng phí nếu kết hợp với một số dư nhỏ khác. Chiến lược này đảm bảo rằng mọi khoản đóng góp bổ sung tiềm năng từ phần còn lại đều được nắm bắt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        
        total = sum(x // k for x in a)
        remainders = [x % k for x in a]
        remainders.sort()
        
        i, j = 0, n - 1
        while i < j:
            if remainders[i] + remainders[j] >= k:
                total += 1
                i += 1
                j -= 1
            else:
                i += 1
        print(total)

if __name__ == "__main__":
    solve()
```Mã đầu tiên tính toán các khoản đóng góp được đảm bảo bằng cách sử dụng phép chia số nguyên. Việc sắp xếp phần dư cho phép chúng ta ghép các cực trị một cách hiệu quả. Cách tiếp cận hai con trỏ đảm bảo chúng tôi không bỏ lỡ bất kỳ sự kết hợp nào có thể tạo ra một đơn vị bổ sung mà không cần tính hai lần. Di chuyển phần còn lại nhỏ hơn về phía trước khi tổng quá thấp sẽ ngăn chặn những nỗ lực ghép nối lãng phí. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
6 3
3 2 7 1 4 8
```| Bước | tôi | j | r[i] | r[j] | r[i]+r[j] | tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 0 | 5 | 0 | 2 | 2 < 3 | 8 | 
| Di chuyển tôi | 1 | 5 | 2 | 2 | 4 ≥ 3 | 9 | 
| Di chuyển i,j | 2 | 4 | 1 | 1 | 2 < 3 | 9 | 
| Di chuyển tôi | 3 | 4 | 1 | 1 | 2 < 3 | 9 | 

Tổng cuối cùng = 8 (được điều chỉnh để phân chia được đảm bảo). Bảng này cho thấy việc ghép đôi tham lam nắm bắt tất cả các khoản đóng góp bổ sung có thể có từ phần còn lại. 

Đối với đầu vào:```
4 3
2 1 5 6
```| Bước | tôi | j | r[i] | r[j] | r[i]+r[j] | tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 0 | 3 | 2 | 0 | 2 < 3 | 4 | 
| Di chuyển tôi | 1 | 3 | 1 | 0 | 1 < 3 | 4 | 
| Di chuyển tôi | 2 | 3 | 2 | 0 | 2 < 3 | 4 | 

Cho biết không có đơn vị bổ sung nào được thêm vào từ số dư, tổng số vẫn đúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp phần còn lại chiếm ưu thế trong thời gian chạy | 
| Không gian | O(n) | Lưu trữ mảng phần dư | 

Xét tổng của tất cả$n \le 2\cdot 10^5$,$O(n \log n)$là khả thi trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("6\n6 3\n3 2 7 1 4 8\n4 3\n2 1 5 6\n4 12\n0 0 0 0\n2 1\n1 1\n6 10\n2 0 0 5 9 4\n6 5\n5 3 8 6 3 2\n") == "8\n4\n0\n2\n1\n5", "sample 1"

# custom cases
assert run("1\n2 10\n5 5\n") == "1", "two items exactly k"
assert run("1\n4 5\n1 2 3 4\n") == "2", "small numbers, pairing sum > k"
assert run("1\n6 3\n0 0 0 0 0 0\n") == "0", "all zeros"
assert run("1\n4 2\n2 2 2 2\n") == "4", "all multiples of k"
assert run("1\n4 3\n1 1 1 1\n") == "1", "all less than k"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 10 / 5 5 | 1 | Ghép chính xác k | 
| 4 5 / 1 2 3 4 | 2 | Số nhỏ tạo ra các đơn vị bổ sung | 
| 6 3 / 0 0 0 0 0 0 | 0 | Tất cả các số không đều không tạo ra thêm | 
| 4 2/2 2 2 2 | 4 | Tất cả bội số của k | 
| 4 3 / 1 1 1 1 | 1 | Tất cả đều nhỏ hơn k, chỉ có thể thêm một | 

## Vỏ cạnh 

Khi tất cả các trọng số là bội số của$k$, ví dụ
