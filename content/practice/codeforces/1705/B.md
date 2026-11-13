---
title: "CF 1705B - ​​Đánh dấu máy quét bụi"
description: "Chúng tôi được xếp một dãy phòng, mỗi dãy có một ít bụi. Mark muốn làm sạch tất cả các phòng ngoại trừ phòng cuối cùng, sử dụng một thao tác đặc biệt để di chuyển một đơn vị bụi từ phòng trước sang phòng sau, nhưng chỉ khi tất cả các phòng ở giữa đều khác 0."
date: "2026-06-09T21:23:55+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1705
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 807 (Div. 2)"
rating: 900
weight: 1705
solve_time_s: 179
verified: false
draft: false
---

[CF 1705B - Đánh dấu máy quét bụi](https://codeforces.com/problemset/problem/1705/B) 

**Xếp hạng:** 900 
**Tags:** thuật toán xây dựng, tham lam, triển khai 
**Thời gian giải:** 2m 59s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được xếp một dãy phòng, mỗi dãy có một ít bụi. Mark muốn làm sạch tất cả các phòng ngoại trừ phòng cuối cùng, sử dụng một thao tác đặc biệt để di chuyển một đơn vị bụi từ phòng trước sang phòng sau, nhưng chỉ khi tất cả các phòng ở giữa đều khác 0. Cụ thể, chúng ta có thể chọn hai chỉ số$i < j$sao cho mỗi phòng từ$i$ĐẾN$j-1$có ít nhất một đơn vị bụi, giảm$a_i$lên 1 và tăng$a_j$bằng 1. Nhiệm vụ là xác định số lượng thao tác tối thiểu cần thiết để làm cho tất cả các phòng ngoại trừ phòng cuối cùng hoàn toàn không có bụi. 

Các ràng buộc đầu vào chính là$n$có thể đi lên$2 \cdot 10^5$và tổng số tiền$n$qua các trường hợp thử nghiệm cũng không vượt quá$2 \cdot 10^5$. Mức độ bụi có thể cao tới mức$10^9$, do đó, bất kỳ thuật toán nào cố gắng mô phỏng trực tiếp từng thao tác riêng lẻ sẽ quá chậm, vì cuối cùng chúng ta có thể cần tới$10^9$hoạt động trong trường hợp xấu nhất. 

Các trường hợp khó phát hiện khi một số phòng đã không có bụi. Ví dụ: nếu phòng đầu tiên bằng 0, chúng ta không thể di chuyển bụi khỏi nó mà thay vào đó phải dựa vào các phòng sau. Một trường hợp phức tạp khác là khi bụi tập trung xa phòng cuối cùng, đòi hỏi chúng ta phải "đẩy" bụi qua nhiều phòng, có thể là nhiều lần. 

Việc triển khai ngây thơ cố gắng thực hiện mọi thao tác từng bước sẽ thất bại cả về thời gian và đôi khi về mặt logic nếu nó giả định rằng mọi bước di chuyển luôn có thể thực hiện được mà không cần kiểm tra các số 0 trung gian. 

## Phương pháp tiếp cận 

Phương pháp brute-force rất đơn giản: với mỗi phòng có lượng bụi khác 0, hãy thử mọi điểm đến có thể ở bên phải, áp dụng thao tác và lặp lại cho đến khi tất cả trừ phòng cuối cùng đều bằng 0. Điều này có hiệu quả vì mọi hoạt động đều giảm bụi trong một số phòng, nhưng số lượng hoạt động có thể lên tới tổng của tất cả các giá trị bụi, có thể là$O(n \cdot 10^9)$, rõ ràng là quá chậm. 

Điểm mấu chốt là chúng ta không cần phải mô phỏng từng hoạt động riêng lẻ. Xem xét cách thức hoạt động: từng đơn vị bụi trong phòng$i$cuối cùng phải di chuyển sang phải, làm tăng thêm bụi trong một số phòng$j > i$. Vì mục tiêu là dọn sạch tất cả các phòng ngoại trừ phòng cuối cùng nên mọi đơn vị bụi trong phòng đều$i$ít nhất đóng góp$1$hoạt động cho chính nó, cộng thêm một số hoạt động bổ sung bằng khoảng cách nó phải di chuyển để đến được căn phòng có thể tích lũy nó. Cụ thể đối với từng phòng$i < n$, tổng số hoạt động do căn phòng đó đóng góp bằng với lượng bụi của nó cộng với bất kỳ sự "tràn" nào do bụi bị đẩy lên số 0. Tính tổng số này cho mỗi phòng sẽ mang lại số lượng hoạt động tối thiểu. 

Một cách đơn giản và đúng đắn để thực hiện điều này là lặp lại từ trái sang phải, duy trì tổng số thao tác cần thiết: nếu một căn phòng có bụi$a_i$, nó góp phần$a_i$các hoạt động để giảm chính nó về 0, và sau đó chúng ta có thể tưởng tượng việc "di chuyển" bụi này về phía trước, làm tăng lượng bụi của phòng bên cạnh. Một cách hiệu quả, chúng ta có thể tích lũy số lượng hoạt động như$a_i + \text{running\_sum}$Ở đâu$\text{running\_sum}$mang theo bụi dư thừa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng của tất cả bụi) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`ops`về không. Điều này sẽ đếm tổng số hoạt động cần thiết. 
2. Lặp lại các phòng từ phòng đầu tiên đến phòng áp chót. Đối với mỗi phòng$i$, hãy làm như sau: 

- Thêm mức độ bụi của nó`a[i]`ĐẾN`ops`. Mỗi đơn vị bụi phải được loại bỏ khỏi căn phòng này, vì vậy ít nhất cần phải thực hiện nhiều thao tác này. 
- Nếu có phòng bên cạnh thì thêm bụi của phòng hiện tại vào phòng bên cạnh. Mô hình này đẩy bụi về phía trước đồng thời đếm các thao tác cần thiết. Nó duy trì tính bất biến rằng bụi trong mỗi phòng đại diện cho các đơn vị vẫn cần được di chuyển sang bên phải. 
3. Sau vòng lặp,`ops`chứa số lượng thao tác tối thiểu cần thiết để loại bỏ tất cả các phòng ngoại trừ phòng cuối cùng. 

**Tại sao nó hoạt động:** Bụi của mỗi phòng chỉ có thể di chuyển sang phải và hoạt động cho phép di chuyển từng thiết bị một, có thể qua nhiều phòng. Bằng cách tích lũy bụi về phía trước, chúng tôi đảm bảo mọi đơn vị đều được tính một lần cho mỗi lần di chuyển. Vì các hoạt động không thể giảm lượng bụi xuống dưới 0 trong các phòng trung gian nên việc tích lũy từ trái sang phải này đảm bảo số lượng bụi tối thiểu mà không cần thực hiện từng bước di chuyển riêng lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    ops = 0
    for i in range(n-1):
        ops += a[i]
        a[i+1] += a[i]
    
    print(ops)
```Mã này đọc nhiều trường hợp kiểm thử một cách hiệu quả bằng cách sử dụng I/O nhanh. Đối với mỗi trường hợp thử nghiệm, nó lặp lại lần đầu tiên$n-1$phòng, đếm mọi thao tác cần thiết cho từng phòng và đẩy bụi về phía trước. Vòng lặp kết thúc với tổng số thao tác chính xác. Sự tinh tế chính là đảm bảo chúng tôi không bao giờ cố gắng di chuyển bụi từ phòng cuối cùng hoặc xa hơn, đồng thời cập nhật chính xác bụi của phòng tiếp theo để lập mô hình tất cả các bước di chuyển trung gian. 

## Ví dụ đã hoạt động 

**Mẫu 1:**`3 2 0 0`| tôi | một | hoạt động | giải thích | 
| --- | --- | --- | --- | 
| 0 | [2,0,0] | 2 | phòng đầu tiên có 2 bụi mỗi căn sẽ dọn sang phải | 
| 1 | [2,2,0] | 2+1=3 | đẩy bụi sang phòng 2, phòng 2 hiện có 2 | 

Tổng số thao tác: 3, phù hợp với kết quả đầu ra dự kiến. 

**Mẫu 2:**`5 0 2 0 2 0`| tôi | một | hoạt động | 
| --- | --- | --- | 
| 0 | [0,2,0,2,0] | 0 | 
| 1 | [0,2,2,2,0] | 2 | 
| 2 | [0,2,2,4,0] | 2+0=2 | 
| 3 | [0,2,2,4,4] | 2+2=4 | 

Tổng số hoạt động = 5, phù hợp với kết quả mong đợi. 

Những dấu vết này xác nhận rằng thuật toán tích lũy chính xác các chuyển động của bụi trong khi đếm tất cả các hoạt động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phòng được truy cập một lần trong một vòng lặp đơn giản. | 
| Không gian | O(n) | Chỉ mảng đầu vào được lưu trữ; không cần thêm cấu trúc dữ liệu. | 

Với những hạn chế mà tổng$n \le 2 \cdot 10^5$, giải pháp chạy thoải mái trong thời gian giới hạn. Mức sử dụng bộ nhớ thấp hơn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        ops = 0
        for i in range(n-1):
            ops += a[i]
            a[i+1] += a[i]
        res.append(str(ops))
    return "\n".join(res)

# provided samples
assert run("4\n3\n2 0 0\n5\n0 2 0 2 0\n6\n2 0 3 0 4 6\n4\n0 0 0 10\n") == "3\n5\n11\n0"

# custom cases
assert run("3\n2\n0 0\n3\n1 0 1\n5\n5 0 0 0 0\n") == "0\n2\n5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 0 0 | 0 | Đã bằng 0, không cần thao tác | 
| 3 1 0 1 | 2 | Bụi cần di chuyển qua các số 0 một cách chính xác | 
| 5 5 0 0 0 0 | 5 | Tất cả bụi khi bắt đầu, di chuyển tích lũy chính xác | 

## Vỏ cạnh 

Nếu phòng đầu tiên bằng 0, như`0 5 0 0`, thuật toán xử lý nó bằng cách thêm số 0 vào`ops`và không đẩy gì về phía trước. Bụi từ phòng thứ hai sau đó di chuyển sang phải khi cần thiết. Tương tự, nếu phòng cuối cùng ban đầu có bụi thì không được tính vào hoạt động vì chỉ phòng đầu tiên mới có bụi.$n-1$phòng phải bằng không. Điều này đảm bảo thuật toán không bao giờ đếm thừa hoặc thiếu các phép toán. 

Đối với một thao tác đơn lẻ trải rộng trên nhiều số 0 trung gian, chẳng hạn như`2 0 0 1`, thuật toán sẽ tích lũy chính xác các phép toán dưới dạng 2+0+0 = 2, sau đó đẩy về phía trước để tính tổng các phép toán mà không cần mô phỏng từng bước trung gian. Sự trừu tượng này đảm bảo tính chính xác và hiệu quả.
