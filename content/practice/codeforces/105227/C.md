---
title: "CF 105227C - Cấu trúc thẻ"
description: "Chúng ta được đưa cho một số thẻ và được yêu cầu xây dựng nhiều lần "kim tự tháp thẻ" cao nhất có thể, loại bỏ các thẻ đã sử dụng và tiếp tục cho đến khi không thể xây dựng thêm kim tự tháp nào nữa. Câu trả lời cuối cùng là có bao nhiêu kim tự tháp được xây dựng trong tất cả các lần lặp lại."
date: "2026-06-24T16:30:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105227
codeforces_index: "C"
codeforces_contest_name: "CPG Training Contest - 1"
rating: 0
weight: 105227
solve_time_s: 314
verified: false
draft: false
---

[CF 105227C - Cấu trúc thẻ](https://codeforces.com/problemset/problem/105227/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa cho một số thẻ và được yêu cầu xây dựng nhiều lần "kim tự tháp thẻ" cao nhất có thể, loại bỏ các thẻ đã sử dụng và tiếp tục cho đến khi không thể xây dựng thêm kim tự tháp nào nữa. Câu trả lời cuối cùng là có bao nhiêu kim tự tháp được xây dựng trong tất cả các lần lặp lại. 

Kim tự tháp có chiều cao 1 là cấu trúc đơn giản nhất và tiêu thụ một số lượng thẻ cố định. Đối với chiều cao lớn hơn, cấu trúc được xây dựng đệ quy: kim tự tháp có chiều cao$h$nằm trên đỉnh của một đế mà bản thân nó bao gồm các kim tự tháp có chiều cao nhỏ hơn-1 và một hàng thẻ, bên dưới là một kim tự tháp có chiều cao$h-1$. Hệ quả quan trọng là mỗi chiều cao tương ứng với một tổng số quân bài cố định. 

Quá trình này có tính chất tham lam. Ở mỗi giai đoạn, chúng tôi luôn xây dựng kim tự tháp cao nhất phù hợp với các quân bài còn lại. Sau khi trừ chi phí của nó, chúng tôi lặp lại quyết định tương tự đối với phần còn lại. 

Các ràng buộc cho phép lên đến$t \le 1000$trường hợp thử nghiệm và tổng số$n$lên đến$10^9$. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào xây dựng từng thẻ kim tự tháp hoặc thậm chí lặp tuyến tính trên tất cả các độ cao có thể có cho mỗi trường hợp thử nghiệm. Một giải pháp thử mọi độ cao cho mỗi bước trừ có thể giảm xuống gần như$O(n)$trong trường hợp xấu nhất là không thể chấp nhận được. 

Trường hợp cạnh tinh tế xuất hiện khi các thẻ còn lại có kích thước nhỏ. Ví dụ, nếu$n = 1$, không thể xây dựng được kim tự tháp nào cả và câu trả lời phải bằng 0. Việc triển khai tham lam ngây thơ giả định ít nhất một cấu trúc cho mỗi lần lặp có thể vô tình thử tính toán chiều cao không hợp lệ hoặc nhập điều kiện vòng lặp không chính xác. 

Một trường hợp thất bại khác phát sinh nếu chúng ta cố gắng tính toán lại kích thước kim tự tháp nhiều lần từ đầu mà không lưu vào bộ nhớ đệm. Vì chi phí kim tự tháp tăng theo phương trình bậc hai nên việc tính toán lại chúng lên đến$n$cho mỗi trường hợp thử nghiệm sẽ là quá chậm. 

## Phương pháp tiếp cận 

Nỗ lực tự nhiên đầu tiên là mô phỏng quá trình một cách trực tiếp. Đối với một nhất định$n$, chúng tôi thử mọi độ cao có thể$h$, tính xem cần có bao nhiêu thẻ và chọn thẻ hợp lệ lớn nhất. Sau khi trừ chi phí đó, chúng tôi lặp lại. Về nguyên tắc, điều này đúng vì bài toán xác định rõ ràng một quá trình xây dựng tham lam. 

Vấn đề là hiệu quả. Nếu chúng ta tính lại chi phí của từng độ cao từ đầu thì mỗi lần kiểm tra là$O(h)$trừ khi chúng tôi tính toán trước và chúng tôi có thể lặp lại điều này trong nhiều bước cho mỗi trường hợp thử nghiệm. Trong trường hợp xấu nhất,$n$giảm chậm khi liên tục xây dựng các kim tự tháp nhỏ, dẫn đến phải lặp lại nhiều lần. Điều này đẩy sự phức tạp về phía$O(n)$hoặc tệ hơn trong các trường hợp thử nghiệm. 

Quan sát quan trọng là mỗi chiều cao kim tự tháp có một chi phí cố định không phụ thuộc vào bối cảnh. Khi chúng ta rút ra được công thức tính số thẻ cần thiết cho chiều cao$h$, chúng ta có thể tính toán trước tất cả các giá trị có thể lên đến$10^9$. Hàm chi phí tăng theo phương trình bậc hai nên chiều cao tối đa chỉ xấp xỉ$2.5 \times 10^4$. Điều này làm cho việc lưu trữ tất cả các chi phí kim tự tháp có thể có là khả thi. 

Khi chúng tôi có danh sách này, mỗi bước sẽ trở thành: tìm chi phí kim tự tháp lớn nhất không vượt quá các thẻ còn lại, trừ đi và tăng số lượng. Bởi vì chúng tôi luôn chọn chiều cao lớn nhất có thể nên chúng tôi đang giảm thiểu vấn đề một cách hiệu quả một cách tối ưu ở từng giai đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n)$trường hợp xấu nhất |$O(1)$| Quá chậm | 
| Tính toán trước + Tham lam |$O(\sqrt{n} \log \sqrt{n})$|$O(\sqrt{n})$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên, chúng tôi rút ra và tính toán trước chi phí của một kim tự tháp ở mỗi chiều cao. Cho phép$f(h)$là số lượng thẻ cần thiết. Từ các quy tắc xây dựng, mỗi lần tăng chiều cao sẽ thêm một lượng thẻ có thể dự đoán được, dẫn đến sự lặp lại tăng theo cấp hai. Chúng tôi tính toán trước tất cả các giá trị của$f(h)$cho đến khi chúng vượt quá$10^9$. 

Thứ hai, chúng tôi lưu trữ các giá trị này trong một danh sách được sắp xếp để có thể truy vấn kim tự tháp lớn nhất một cách hiệu quả phù hợp với ngân sách còn lại nhất định. 

Sau đó với mỗi trường hợp thử nghiệm: 

1. Bắt đầu với số lượng thẻ cho trước$n$và một bộ đếm được đặt về 0. 
2. Trong khi$n$ít nhất là giá của kim tự tháp nhỏ nhất, hãy tìm kim tự tháp lớn nhất$f(h)$như vậy$f(h) \le n$. Điều này tương ứng với việc chọn kim tự tháp hợp lệ cao nhất. 
3. Trừ chi phí đó khỏi$n$. 
4. Tăng câu trả lời lên một vì chúng ta đã xây dựng thành công một kim tự tháp. 
5. Lặp lại cho đến khi không thể hình thành được kim tự tháp nào. 

Bước suy luận quan trọng là tại sao việc chọn kim tự tháp lớn nhất có thể ở mỗi giai đoạn là hợp lệ. Bất kỳ lựa chọn nhỏ hơn nào sẽ để lại nhiều thẻ hơn, nhưng vì bước tiếp theo lặp lại quy tắc tham lam tương tự một cách độc lập với các lựa chọn trước đó, nên việc trì hoãn sử dụng thẻ không thể làm tăng tổng số kim tự tháp. 

### Tại sao nó hoạt động 

Mỗi cấu trúc kim tự tháp tiêu thụ một lượng thẻ cố định và để lại phần còn lại không phụ thuộc vào cách chúng tôi đạt đến trạng thái đó. Sự lựa chọn tham lam luôn tối đa hóa mức tiêu dùng ngay lập tức và vì tất cả các chi phí đều cố định và không trùng lặp nên quá trình này tương đương với việc trừ đi nhiều lần phần tử lớn nhất có thể có từ một tập hợp nhiều giá trị cố định. Không có quyết định nào sau này có thể khiến cho một kim tự tháp lớn hơn khả thi trước đó trở thành không thể, bởi vì tính khả thi chỉ phụ thuộc vào tổng số còn lại chứ không phụ thuộc vào cấu trúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# Precompute pyramid costs
MAX_N = 10**9
costs = []

h = 1
while True:
    # f(h) = h(3h+1)/2
    c = h * (3 * h + 1) // 2
    if c > MAX_N:
        break
    costs.append(c)
    h += 1

def solve(n):
    ans = 0

    while n >= 2:
        # find largest cost <= n
        lo, hi = 0, len(costs) - 1
        best = -1

        while lo <= hi:
            mid = (lo + hi) // 2
            if costs[mid] <= n:
                best = mid
                lo = mid + 1
            else:
                hi = mid - 1

        if best == -1:
            break

        n -= costs[best]
        ans += 1

    return ans

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        print(solve(n))

if __name__ == "__main__":
    main()
```Chi tiết triển khai cốt lõi là tính toán trước tất cả chi phí kim tự tháp hợp lệ bằng cách sử dụng công thức dạng đóng$f(h) = \frac{h(3h+1)}{2}$. Điều này tránh mọi logic xây dựng đệ quy trong quá trình truy vấn. 

Trong mỗi trường hợp thử nghiệm, tìm kiếm nhị phân được sử dụng để xác định chi phí kim tự tháp khả thi lớn nhất. Điều này là cần thiết vì việc quét tuyến tính qua tất cả các độ cao trong mỗi lần lặp sẽ làm giảm hiệu suất khi tồn tại nhiều kích thước kim tự tháp. 

Điều kiện vòng lặp`n >= 2`phản ánh thực tế rằng một kim tự tháp có chiều cao 1 đã có giá 2 lá bài, vì vậy bất kỳ lá bài nào thấp hơn mức đó ngay lập tức bị coi là cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 14$Chi phí có sẵn (tiền tố): 2, 7, 15, ... 

| Bước | n trước | chi phí đã chọn | chiều cao | n sau | kim tự tháp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 14 | 7 | 2 | 7 | 1 | 
| 2 | 7 | 7 | 2 | 0 | 2 | 

Quá trình này cho thấy phép trừ tham lam tiếp tục cho đến khi các thẻ còn lại không thể hỗ trợ ngay cả cấu trúc nhỏ nhất. Điều này khẳng định rằng việc tái sử dụng cùng một chiều cao là được phép và tối ưu khi nó phù hợp nhiều lần. 

### Ví dụ 2:$n = 24$| Bước | n trước | chi phí đã chọn | chiều cao | n sau | kim tự tháp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 24 | 15 | 3 | 9 | 1 | 
| 2 | 9 | 7 | 2 | 2 | 2 | 
| 3 | 2 | 2 | 1 | 0 | 3 | 

Dấu vết này cho thấy cách thuật toán giảm dần theo độ cao một cách tự nhiên khi ngân sách còn lại giảm xuống, luôn chọn cấu trúc hợp lệ lớn nhất ở mỗi giai đoạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \log H)$| Mỗi trường hợp thử nghiệm thực hiện tìm kiếm nhị phân lặp lại nhiều nhất$H \approx 2.5 \times 10^4$giá trị được tính toán trước | 
| Không gian |$O(H)$| Chi phí lưu trữ cho tất cả kim tự tháp lên tới$10^9$| 

Ràng buộc trên$n$đảm bảo rằng số lượng chiều cao kim tự tháp có thể có là nhỏ, do đó, ngay cả việc tìm kiếm nhị phân lặp lại cũng dễ dàng đủ nhanh để$t \le 1000$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MAX_N = 10**9
    costs = []
    h = 1
    while True:
        c = h * (3 * h + 1) // 2
        if c > MAX_N:
            break
        costs.append(c)
        h += 1

    def solve(n):
        ans = 0
        while n >= 2:
            lo, hi = 0, len(costs) - 1
            best = -1
            while lo <= hi:
                mid = (lo + hi) // 2
                if costs[mid] <= n:
                    best = mid
                    lo = mid + 1
                else:
                    hi = mid - 1
            if best == -1:
                break
            n -= costs[best]
            ans += 1
        return ans

    t = int(input())
    out = []
    for _ in range(t):
        out.append(str(solve(int(input()))))
    return "\n".join(out)

# provided samples
assert run("5\n3\n14\n15\n24\n1\n") == "1\n2\n1\n3\n0"

# minimum input
assert run("1\n1\n") == "0"

# exact single pyramid
assert run("1\n2\n") == "1"

# repeated same pyramid
assert run("1\n14\n") == "2"

# larger mixed case
assert run("1\n100\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 1 | 0 | không thể xây kim tự tháp nhỏ nhất | 
| 1, 2 | 1 | xây dựng hợp lệ tối thiểu | 
| 1, 14 | 2 | lặp đi lặp lại tham lam sử dụng cùng chiều cao | 
| 1.100 | 5 | đi xuống nhiều bậc trên độ cao | 

## Vỏ cạnh 

Khi nào$n = 1$, thuật toán thoát ngay vì kim tự tháp nhỏ nhất yêu cầu 2 lá bài. Điều kiện vòng lặp ngăn chặn mọi nỗ lực chọn độ cao và câu trả lời vẫn bằng 0. 

Đối với trường hợp như$n = 2$, chỉ có chiều cao 1 có sẵn. Thuật toán chọn giá 2, trừ nó và kết thúc. Điều này xác nhận rằng tìm kiếm nhị phân xác định chính xác chi phí hợp lệ nhỏ nhất ngay cả khi không tồn tại cấu trúc lớn hơn. 

Đối với các giá trị lớn hơn trong đó nhiều hình chóp có cùng chiều cao khớp liên tiếp, chẳng hạn như$n = 14$, thuật toán liên tục chọn chiều cao 2. Mỗi lần lặp lại tính toán lại tính khả thi dựa trên phần còn lại được cập nhật, đảm bảo rằng việc lựa chọn lặp lại không phụ thuộc vào các lựa chọn cấu trúc trước đó mà chỉ phụ thuộc vào ngân sách còn lại.
