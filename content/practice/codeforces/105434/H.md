---
title: "CF 105434H - \u7236\u5b50\u5c40"
description: "Mỗi trường hợp thử nghiệm cho một số nguyên $n$. Nhiệm vụ là chia số này thành hai số nguyên dương $x$ và $y$ khác nhau sao cho tổng của chúng bằng $n$. Nếu một cặp như vậy không thể tồn tại thì đầu ra phải là hai giá trị âm."
date: "2026-06-23T03:53:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105434
codeforces_index: "H"
codeforces_contest_name: "2024\u5e74\u201c\u6838\u6843\u676f\u201d\u6b66\u6c49\u5730\u533aACM\u840c\u65b0\u8d5b"
rating: 0
weight: 105434
solve_time_s: 57
verified: true
draft: false
---

[CF 105434H - \u7236\u5b50\u5c40](https://codeforces.com/problemset/problem/105434/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm cho một số nguyên duy nhất$n$. Nhiệm vụ là chia số này thành hai số nguyên dương khác nhau$x$Và$y$sao cho tổng của chúng bằng$n$. Nếu một cặp như vậy không thể tồn tại thì đầu ra phải là hai giá trị âm. 

Kích thước đầu vào đạt tới$10^5$các trường hợp thử nghiệm và mỗi trường hợp$n$nhiều nhất là$10^5$. Điều đó đã gợi ý rằng bất kỳ giải pháp nào cũng phải có thời gian không đổi cho mỗi trường hợp thử nghiệm, vì thậm chí$O(\sqrt{n})$mỗi bài kiểm tra sẽ bắt đầu cảm thấy không cần thiết và$O(n)$mỗi bài kiểm tra rõ ràng sẽ là quá chậm. 

Một điểm tinh tế là cả hai$x$Và$y$phải tích cực và khác biệt. Điều này ngay lập tức tạo ra một vấn đề về ranh giới cho các giá trị nhỏ nhất của$n$. Nếu như$n = 1$, không tồn tại sự phân tích thành hai số nguyên dương. Nếu như$n = 2$, cách duy nhất là$1 + 1$, nhưng các giá trị không khác nhau nên không hợp lệ. Vì$n \ge 3$, các công trình hợp lệ đột nhiên trở nên tầm thường. 

Một cách tiếp cận ngây thơ có thể thử tất cả các cặp$(x, y)$với$x + y = n$, nhưng ngay cả điều đó cũng là quá mức cần thiết vì cấu trúc buộc phải có một dạng ứng cử viên duy nhất cho bất kỳ giải pháp khả thi nào. Mối nguy hiểm duy nhất là quên ràng buộc về tính khác biệt ở các giá trị nhỏ nhất, điều này sẽ tạo ra kết quả không chính xác.$1, 1$vì$n = 2$. 

## Phương pháp tiếp cận 

Cách giải thích trực tiếp nhất là thử xây dựng hai số có tổng bằng$n$. Một phương pháp brute-force sẽ lặp lại tất cả các giá trị có thể có của$x$từ$1$ĐẾN$n-1$, bộ$y = n - x$, và kiểm tra xem$x \ne y$. Điều này luôn đúng vì nó kiểm tra toàn diện mọi phân rã nhưng lại lãng phí. Đối với mỗi trường hợp thử nghiệm, nó thực hiện$O(n)$làm việc, dẫn đến$O(Tn)$trong trường hợp xấu nhất, sẽ là khoảng$10^{10}$hoạt động và hoàn toàn không khả thi. 

Quan sát quan trọng là chúng ta thực sự không cần phải tìm kiếm. Một khi chúng tôi chọn$x = 1$, hạn chế duy nhất trở thành liệu$y = n - 1$khác với$1$. Điều này chỉ thất bại khi$n = 2$. Đối với tất cả các giá trị lớn hơn,$1$Và$n - 1$được đảm bảo là các số nguyên dương riêng biệt có tổng chính xác bằng$n$. Toàn bộ vấn đề được chuyển thành một cấu trúc có điều kiện theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng câu trả lời trực tiếp từ cấu trúc của các phân rã hợp lệ. 

1. Đọc$n$. Mục đích là biểu thị nó dưới dạng tổng của hai số nguyên dương khác nhau. 
2. Kiểm tra xem$n \le 2$. Trong phạm vi này, không có cặp hợp lệ nào tồn tại vì cặp dương khác biệt nhỏ nhất là$(1,2)$, tổng cộng là$3$. Bất kỳ tổng nhỏ hơn nào cũng buộc phải lặp lại hoặc bao gồm số 0, cả hai đều không hợp lệ. 
3. Nếu$n \ge 3$, bộ$x = 1$. Việc chọn số nguyên dương nhỏ nhất sẽ giảm thiểu các ràng buộc đối với giá trị thứ hai và đảm bảo tính dương. 
4. Tính toán$y = n - 1$. Điều này bị ép buộc bởi điều kiện tổng. 
5. Đầu ra$x$Và$y$. Chúng tự động được phân biệt bởi vì$n - 1 \ne 1$vì$n \ge 3$. 

### Tại sao nó hoạt động 

Đối với bất kỳ giải pháp hợp lệ nào, chúng tôi cần$x + y = n$với$x \ne y$và cả hai đều tích cực. Vì$n \ge 3$, cặp$(1, n-1)$thỏa mãn mọi ràng buộc. Vì việc xây dựng không dựa vào bất kỳ tài sản đặc biệt nào của$n$ngoài việc ít nhất là$3$, nó luôn tạo ra một phân tách hợp lệ bất cứ khi nào nó tồn tại. Vì$n \le 2$, các ràng buộc bất đẳng thức về tính tích cực và tính khác biệt sẽ loại bỏ mọi khả năng, do đó câu trả lời phủ định là bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        if n <= 2:
            out.append("-1 -1")
        else:
            out.append(f"1 {n-1}")
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh chính xác lý do. Mỗi trường hợp thử nghiệm được xử lý độc lập trong thời gian không đổi. Nhánh có điều kiện duy nhất kiểm tra xem có tồn tại phân tách hợp lệ hay không. Đối với trường hợp hợp lệ, việc lựa chọn$1$là có chủ ý vì nó tự động đảm bảo tính khác biệt, tránh mọi nhu cầu kiểm tra bổ sung. 

Phải cẩn thận để không vô tình xuất ra$n$Và$0$, vì số 0 không phải là số nguyên dương. Việc xây dựng$1$Và$n-1$tránh điều này hoàn toàn. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi đầu vào trong đó$n = 2, 9, 16$. 

| Bước | n | Quyết định | x | y | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | không hợp lệ (<2) | - | - | 
| 2 | 9 | hợp lệ | 1 | 8 | 
| 3 | 16 | hợp lệ | 1 | 15 | 

Vì$n = 2$, thuật toán sẽ ngay lập tức bác bỏ trường hợp này vì bất kỳ sự phân rã nào cũng sẽ buộc phải bằng 0 hoặc lặp lại. Vì$n = 9$, chọn$1$lá$8$, dương và khác biệt. Vì$n = 16$, áp dụng mô hình tương tự, tạo ra$1$Và$15$. 

Những ví dụ này xác nhận rằng cùng một cấu trúc hoạt động thống nhất trên tất cả các dữ liệu đầu vào hợp lệ mà không cần bất kỳ điều chỉnh nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T)$| Mỗi trường hợp thử nghiệm được xử lý bằng một phép trừ và kiểm tra có điều kiện duy nhất | 
| Không gian |$O(1)$| Chỉ một lượng bộ nhớ không đổi được sử dụng ngoài bộ nhớ đầu ra | 

Giải pháp thoải mái phù hợp trong giới hạn vì ngay cả$10^5$các hoạt động rất đơn giản trong Python và không có vòng lặp$n$đang tham gia. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            if n <= 2:
                out.append("-1 -1")
            else:
                out.append(f"1 {n-1}")
        return "\n".join(out)

    return solve()

# provided samples (interpreted from statement)
assert run("3\n2\n9\n16\n") == "-1 -1\n1 8\n1 15"

# minimum edge
assert run("1\n1\n") == "-1 -1"

# smallest valid
assert run("1\n3\n") == "1 2"

# random mid
assert run("1\n10\n") == "1 9"

# larger boundary
assert run("1\n100000\n") == "1 99999"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$n=1$| -1 -1 | tối thiểu là không thể | 
|$n=2$| -1 -1 | trường hợp bình đẳng không hợp lệ | 
|$n=3$| 1 2 | phân rã hợp lệ nhỏ nhất | 
|$n=100000$| 1 99999 | độ đúng giới hạn trên | 

## Vỏ cạnh 

cho$n = 1$, thuật toán trực tiếp xuất ra$-1 -1$vì không có cách nào chia 1 thành hai số nguyên dương. Mọi nỗ lực sẽ yêu cầu giá trị 0 hoặc âm, không được phép. 

Vì$n = 2$, phân rã đại số duy nhất là$1 + 1$, nhưng yêu cầu về tính khác biệt sẽ vô hiệu hóa nó. Thuật toán sẽ loại bỏ nó một cách chính xác trước khi thử xây dựng. 

Cho tất cả$n \ge 3$, công trình luôn tạo ra$(1, n-1)$. Kể cả ở ranh giới$n = 3$, điều này mang lại$(1, 2)$, thỏa mãn tính tích cực và khác biệt mà không có ngoại lệ.
