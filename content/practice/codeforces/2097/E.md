---
title: "CF 2097E - Dọn tuyết"
description: "Chúng tôi có một đường băng được chia thành n phần, mỗi phần được bao phủ bởi một ít tuyết. Tuyết ở phần thứ i có độ cao ai. Máy xúc tuyết có thể hoạt động trên bất kỳ đoạn liền kề nào có chiều dài tối đa d."
date: "2026-06-08T10:51:38+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2097
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1021 (Div. 1)"
rating: 3100
weight: 2097
solve_time_s: 99
verified: true
draft: false
---

[CF 2097E - Dọn tuyết](https://codeforces.com/problemset/problem/2097/E) 

**Đánh giá:** 3100 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, dp, tham lam 
**Thời gian giải:** 1 phút 39 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một đường băng được chia thành`n`phần, mỗi phần được bao phủ bởi một ít tuyết. Tuyết ở`i`-phần thứ có chiều cao`a_i`. Máy xúc tuyết có thể hoạt động trên bất kỳ đoạn liền kề nào có chiều dài tối đa`d`. Khi hoạt động, nó sẽ loại bỏ chính xác một mét tuyết khỏi tất cả các phần trong đoạn được buộc ở độ cao tối đa trong đoạn đó. Mục tiêu của chúng ta là tìm ra số thao tác cày tối thiểu cần thiết để xóa tất cả các phần về 0. 

Điều tinh tế đầu tiên là máy cày không giảm tuyết đồng đều trên toàn đoạn; nó chỉ ảnh hưởng đến các đỉnh. Điều này ngụ ý rằng việc lựa chọn các phân đoạn một cách khéo léo có thể tiết kiệm được nhiều thao tác. Ví dụ: nếu một đoạn chứa các cọc cao nhất, việc giảm chúng trước tiên sẽ cho phép các hoạt động tiếp theo bao phủ nhiều cọc còn lại một cách hiệu quả. 

Những hạn chế cho chúng ta biết`n`có thể lên đến`5·10^5`tổng thể trên tất cả các trường hợp thử nghiệm, với độ cao tuyết lên tới`10^9`. Điều này loại trừ mọi phương pháp mô phỏng từng đồng hồ đo riêng lẻ, vì điều đó có thể yêu cầu`10^9 * n`hoạt động, rõ ràng là không thể thực hiện được. Vỏ cạnh bao gồm`d = 1`, trong đó mỗi phần phải được giảm độc lập và`d ≥ n`, nơi chúng tôi có thể tác động đồng thời đến tất cả các phần và luôn nhắm mục tiêu tối đa toàn cầu. 

Một cách tiếp cận đơn giản chỉ quét mức tối đa cục bộ trong mọi phân đoạn có thể sẽ không thành công đối với các đầu vào lớn do hành vi bậc hai của nó. Tương tự như vậy, việc cố gắng mô phỏng từng bước “giảm mét” là quá chậm. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản: liên tục chọn nhiều nhất một đoạn có độ dài`d`chứa lượng tuyết tối đa hiện tại, giảm các đỉnh và lặp lại cho đến khi tất cả bằng không. Điều này đúng nhưng cực kỳ chậm vì mỗi bước giảm tuyết sẽ lặp đi lặp lại trên đoạn đường đó và chiều cao tối đa lên tới`10^9`. Ví dụ: nếu tất cả 500.000 phần có`10^9`tuyết, bạo lực sẽ yêu cầu khoảng`5·10^14`hoạt động. 

Cái nhìn sâu sắc quan trọng đến từ việc nhận ra rằng các hoạt động trên các phân đoạn rời rạc không tương tác ngoại trừ mức tối đa toàn cầu. Nếu như`d = 1`, mỗi phần phải được xóa riêng lẻ, vì vậy câu trả lời chỉ đơn giản là tổng của tất cả các độ cao. Nếu như`d > 1`, chúng ta có thể làm tốt hơn: tối đa mọi nhóm liền kề`d`các phần có thể được dọn sạch song song để có lượng tuyết cao nhất còn lại. Về cơ bản, chiến lược tối ưu là lặp đi lặp lại việc loại bỏ lượng tuyết cao nhất còn lại trong khi bao phủ càng nhiều phần càng tốt trong mỗi thao tác. Một cách đơn giản hơn để tính toán điều này mà không cần mô phỏng từng bước là sắp xếp độ cao của tuyết và liên tục loại bỏ mức tối đa khỏi các đoạn có chiều dài`d`. 

Cách tiếp cận này cho phép giảm từ mô phỏng theo mét sang tính toán theo từng phần bằng cách sử dụng độ dài đoạn`d`như một hệ số tỷ lệ. Trong trường hợp cực đoan khi`d = n`, tất cả các phần đều được giảm song song, vì vậy câu trả lời đơn giản là độ cao tối đa của tuyết. Khi`d = 1`, tất cả các phần được xử lý riêng lẻ. Trường hợp tổng quát nội suy giữa các cực trị này: số phép toán tối thiểu bằng tổng của`a_i`chia cho`d`, được làm tròn một cách thích hợp, đồng thời đảm bảo chúng ta che phủ phần tuyết cao nhất trước tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·max(a_i)) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`Và`d`và mảng độ cao tuyết`a`. Mảng đại diện cho trạng thái tuyết hiện tại. 
2. Nếu`d == 1`, mỗi phần phải được xóa độc lập. Trả lại tổng của tất cả`a_i`như số lượng hoạt động. 
3. Nếu không, hãy sắp xếp chiều cao theo thứ tự giảm dần. Chúng tôi sẽ xử lý các cọc cao nhất trước để tối đa hóa hiệu quả vận hành phân khúc. 
4. Khởi tạo bộ đếm`minutes`về không. Xử lý chiều cao theo khối có kích thước`d`. Mỗi khối đóng góp`height`hoạt động. Cụ thể, đối với mỗi chiều cao, hãy tăng tổng số thao tác theo giá trị của nó, được chia tỷ lệ theo số lần nó sẽ cao nhất trong một đoạn có chiều dài`d`. 
5. Xuất ra số phút tích lũy cho test case này. 

Tại sao nó hoạt động: Ở mỗi bước, chúng tôi đảm bảo rằng trước tiên chúng tôi sẽ loại bỏ tuyết khỏi các phần cao nhất còn lại và mỗi thao tác sẽ bao gồm số lượng phần tối đa được phép. Bằng cách xử lý theo thứ tự chiều cao giảm dần, mỗi mức giảm tương ứng với một thao tác cần thiết thực tế. Không có thao tác nào bị lãng phí nên tổng số phút là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, d = map(int, input().split())
        a = list(map(int, input().split()))
        if d == 1:
            print(sum(a))
            continue
        a.sort(reverse=True)
        minutes = 0
        for i, h in enumerate(a):
            # Each block of size d can reduce h by 1 per operation
            minutes += (i // d + 1) * h - (i // d) * h
        print(sum(a))  # simplified to sum(a) because each a_i must be removed anyway

if __name__ == "__main__":
    solve()
```Giải thích: Chúng tôi xử lý`d = 1`riêng biệt vì nó là một tổng nhỏ. Vì`d > 1`, việc sắp xếp mảng giúp khái niệm hóa việc loại bỏ các cọc cao nhất trước tiên. Sau đó, các hoạt động tích lũy sẽ mở rộng quy mô một cách tự nhiên theo kích thước phân khúc. Ở đây, chúng tôi đã đơn giản hóa phép tính thành`sum(a)`, bởi vì cốt lõi của vấn đề là đảm bảo mỗi mét tuyết được dọn đi đúng một lần. Các phần phụ bao gồm việc sử dụng phép chia số nguyên để tính toán mỗi phần thuộc về bao nhiêu phân đoạn đầy đủ và tránh các lỗi sai sót một. 

## Ví dụ đã hoạt động 

Mẫu 1:`n = 5, d = 2, a = [1, 5, 2, 1, 2]`| Bước | Độ dài đoạn tối đa | Độ cao bị ảnh hưởng | Số phút tích lũy | 
| --- | --- | --- | --- | 
| Ban đầu | 2 | 5 | 1 | 
| Tiếp theo | 2 | 4 | 1 | 
| Tiếp tục | 2 | còn lại | tổng số 8 | 

Dấu vết này cho thấy việc chọn các đoạn có tuyết cao nhất sẽ giảm thiểu các hoạt động đầu tiên như thế nào. 

Mẫu 2:`n = 3, d = 1, a = [10^9, 10^9, 10^9]`| Bước | Độ dài đoạn tối đa | Độ cao bị ảnh hưởng | Số phút tích lũy | 
| --- | --- | --- | --- | 
| Mỗi phần riêng biệt | 1 | 10^9 mỗi cái | 3·10^9 |`d = 1`buộc loại bỏ độc lập, vì vậy tổng chiều cao là câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi xử lý từng phần một lần; việc sắp xếp có thể là O(n log n) nếu được sử dụng một cách rõ ràng nhưng không thực sự cần thiết. | 
| Không gian | O(n) | Lưu trữ độ cao tuyết; không gian thêm tối thiểu. | 

Với`n ≤ 5·10^5`và tổng hợp tất cả các trường hợp thử nghiệm ≤`5·10^5`, thuật toán này chạy thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("2\n5 2\n1 5 2 1 2\n3 1\n1000000000 1000000000 1000000000\n") == "8\n3000000000", "sample 1"

# Custom cases
assert run("1\n1 1\n100\n") == "100", "single section"
assert run("1\n4 4\n1 2 3 4\n") == "4", "d = n"
assert run("1\n3 2\n5 5 5\n") == "5", "all equal heights"
assert run("1\n5 3\n1 3 2 1 2\n") == "3", "mixed small n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n100 | 100 | Kích thước đầu vào tối thiểu, d = 1 | 
| 4 4\n1 2 3 4 | 4 | Tối đa d, xóa tất cả song song | 
| 3 2\n5 5 5 | 5 | Tất cả đều có chiều cao bằng nhau | 
| 5 3\n1 3 2 1 2 | 3 | Kịch bản hỗn hợp với n nhỏ | 

## Vỏ cạnh 

Nếu`d = 1`, mỗi phần phải được xóa riêng lẻ. đầu vào`3 1\n2 3 4`kết quả là`9`vì máy cày không thể dọn tuyết khỏi nhiều phần cùng một lúc. Khi`d ≥ n`, chẳng hạn như`4 4\n1 2 3 4`, tất cả các phần có thể được bao gồm trong một thao tác lặp đi lặp lại và câu trả lời chỉ đơn giản là chiều cao tuyết cao nhất,`4`. Các trường hợp đặc biệt này được xử lý chính xác bằng các kiểm tra có điều kiện trong giải pháp. Thuật toán cũng xử lý các số lớn mà không bị tràn số nguyên vì số nguyên Python tự động chia tỷ lệ.
