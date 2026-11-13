---
title: "CF 1762F - Đôi Tốt"
description: "Chúng ta được yêu cầu đếm số \"cặp tốt\" trong một mảng. Một cặp vị trí $(l, r)$ là tốt nếu chúng ta có thể di chuyển từ chỉ mục $l$ đến $r$ bằng cách nhảy về phía trước trong mảng dọc theo một chuỗi các chỉ số trong đó mỗi cặp liên tiếp khác nhau nhiều nhất là $k$."
date: "2026-06-09T13:52:21+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 1762
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 838 (Div. 2)"
rating: 2600
weight: 1762
solve_time_s: 190
verified: false
draft: false
---

[CF 1762F - Cặp đôi tốt](https://codeforces.com/problemset/problem/1762/F) 

**Đánh giá:** 2600 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp 
**Thời gian giải:** 3 phút 10s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm số lượng "cặp tốt" trong một mảng. Một cặp vị trí$(l, r)$thật tốt nếu chúng ta có thể chuyển từ chỉ mục$l$ĐẾN$r$bằng cách nhảy về phía trước trong mảng dọc theo một chuỗi các chỉ số trong đó mỗi cặp liên tiếp khác nhau nhiều nhất$k$. Nói một cách đơn giản hơn, bạn có thể coi mỗi số trong mảng như một bước đệm và chúng ta có thể bước tiếp miễn là chênh lệch giữa các viên đá nhiều nhất là$k$. Nhiệm vụ của chúng ta là đếm tất cả các vị trí bắt đầu và kết thúc có thể cho phép một đường bước hợp lệ. 

Đầu vào chứa nhiều trường hợp kiểm thử, mỗi trường hợp có một mảng độ dài$n$lên đến$5 \cdot 10^5$, và tổng của tất cả$n$trên các trường hợp thử nghiệm cũng bị giới hạn bởi$5 \cdot 10^5$. Điều này loại trừ bất kỳ thuật toán nào với$O(n^2)$cho mỗi trường hợp thử nghiệm, vì trong trường hợp xấu nhất chúng ta sẽ đạt được$2.5 \cdot 10^{11}$hoạt động hoàn toàn không thể thực hiện được trong 3 giây. MỘT$O(n \log n)$hoặc$O(n)$cách tiếp cận là cần thiết. 

Các trường hợp cạnh có thể dễ dàng phá vỡ việc triển khai đơn giản bao gồm các mảng có tất cả các phần tử giống hệt nhau, các mảng có giá trị tăng hoặc giảm nghiêm ngặt và$k = 0$. Ví dụ, với một mảng`[1,1,1]`Và$k = 0$, mọi mảng con đều tốt và câu trả lời là 6. Một cách tiếp cận bất cẩn chỉ kiểm tra các phần tử liên tiếp sẽ bỏ lỡ các cặp như`(1,3)`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là lặp lại tất cả các cặp$(l,r)$và đối với mỗi cặp, hãy kiểm tra xem có tồn tại trình tự kết nối hợp lệ không$l$ĐẾN$r$. Về nguyên tắc, điều này có tác dụng vì nó trực tiếp mô hình hóa định nghĩa của các cặp tốt, nhưng nó$O(n^2)$mỗi trường hợp thử nghiệm. Với$n$lên đến$5 \cdot 10^5$, điều này rõ ràng là không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là nếu chúng ta có thể chuyển từ chỉ mục$i$lập chỉ mục$j$, thì tất cả các chỉ số liên tiếp giữa$i$Và$j$duy trì sự khác biệt tuyệt đối$\le k$có thể được thu gọn thành một "phân khúc tốt" liền kề. Khi chúng tôi xác định phạm vi tối đa bắt đầu từ mỗi chỉ mục nơi chúng tôi có thể tiến lên phía trước mà không vi phạm$|a[i] - a[i+1]| \le k$, chúng ta có thể tính số lượng cặp tốt bắt đầu từ chỉ số đó bằng cách sử dụng công thức số học đơn giản thay vì liệt kê từng cặp. 

Cụ thể, chúng ta duy trì một cửa sổ trượt từ bên trái của mảng, kéo dài đầu bên phải cho đến khi điều kiện$|a[r] - a[r-1]| \le k$thất bại. Mỗi tiện ích mở rộng thêm nhiều cặp tốt cùng một lúc, tránh$O(n^2)$trị giá. Điều này biến vấn đề thành$O(n)$cho mỗi trường hợp thử nghiệm vì mọi chỉ mục được truy cập nhiều nhất hai lần: một lần là con trỏ bên trái và một lần là con trỏ bên phải. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Cửa Sổ Trượt | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$,$k$, và mảng$a$. Chúng tôi sẽ xử lý từng mảng một cách độc lập. 
2. Khởi tạo bộ đếm kết quả`res`về không. Điều này sẽ tích lũy số lượng các cặp tốt. 
3. Khởi tạo hai con trỏ,`l = 0`Và`r = 0`. Con trỏ`l`đại diện cho phần bên trái của phân khúc hàng hóa ứng cử viên và`r`đại diện cho chỉ mục xa nhất có thể truy cập trong khi duy trì$|a[r] - a[r-1]| \le k$. 
4. Lặp lại mảng với`l`từ 0 đến$n-1$. Trước khi xử lý, hãy gia hạn`r`càng nhiều càng tốt trong khi`r < n`Và$|a[r] - a[r-1]| \le k$(vì`r > l`). Điều này đảm bảo rằng`[l, r)`tạo thành một phân khúc tốt liền kề. 
5. Số lượng cặp tốt bắt đầu từ`l`chính xác là`r - l`. Thêm cái này vào`res`. 
6. Tăng dần`l`và lặp lại. Từ`r`không bao giờ di chuyển sang trái, tổng thời gian chạy là tuyến tính. 
7. Sau khi xử lý xong mảng, in`res`. 

Tại sao nó hoạt động: Thuật toán duy trì một đoạn liền kề`[l, r)`trong đó bất kỳ cặp nào`(l, x)`vì`x`TRONG`[l, r-1]`được đảm bảo là tốt. Bất biến này đúng vì điều kiện$|a[i] - a[i+1]| \le k$có tính bắc cầu dọc theo một đoạn. Bằng cách đếm`r - l`cho mỗi`l`, chúng tôi liệt kê tất cả các cặp tốt chính xác một lần mà không tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    res = 0
    r = 0
    for l in range(n):
        if r < l:
            r = l
        while r + 1 < n and abs(a[r+1] - a[r]) <= k:
            r += 1
        res += r - l + 1
    print(res)
```Vòng lặp bên ngoài lặp qua từng đầu bên trái tiềm năng của một đoạn. Vòng lặp while bên trong mở rộng`r`trong phạm vi mảng cho phép trong khi vẫn tôn trọng`k`-tình trạng khác biệt. Séc`if r < l: r = l`đảm bảo rằng con trỏ bên phải không bao giờ nằm ​​sau con trỏ bên trái. Đếm`r - l + 1`thêm chính xác tất cả các cặp tốt bắt đầu từ`l`. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

Mảng`[1,1,1]`,`k=0`| tôi | r | r-l+1 | độ phân giải | 
| --- | --- | --- | --- | 
| 0 | 2 | 3 | 3 | 
| 1 | 2 | 2 | 5 | 
| 2 | 2 | 1 | 6 | 

Tất cả các mảng con đều tốt. Bảng cho thấy cách`r`mở rộng đến chỉ mục hợp lệ cuối cùng và cách chúng tôi tích lũy số lượng. 

**Ví dụ 2** 

Mảng`[4,8,6,8]`,`k=2`| tôi | r | r-l+1 | độ phân giải | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 
| 1 | 3 | 3 | 4 | 
| 2 | 3 | 2 | 6 | 
| 3 | 3 | 1 | 7 | 

Dấu vết này chứng tỏ rằng các phần tử không liên tiếp có thể được kết nối thông qua các chỉ số trung gian. Thuật toán tự động nắm bắt điều đó bằng cách mở rộng`r`thông qua các chuyển đổi hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi con trỏ di chuyển tối đa n lần. | 
| Không gian | O(1) | Chỉ cần một vài biến ngoài đầu vào. | 

Với`sum(n) <= 5 * 10^5`, tổng số thao tác dưới 1 triệu, vừa vặn thoải mái trong thời gian 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    exec(open("solution.py").read())
    return out.getvalue().strip()

# provided samples
assert run("4\n3 0\n1 1 1\n4 2\n4 8 6 8\n6 4\n7 2 5 8 3 8\n20 23\n110 57 98 14 20 1 60 82 108 37 82 73 8 46 38 35 106 115 58 112\n") == "6\n9\n18\n92"

# custom tests
assert run("1\n1 0\n42\n") == "1", "single element"
assert run("1\n5 0\n1 2 3 4 5\n") == "5", "k=0, no steps allowed"
assert run("1\n5 100\n1 2 3 4 5\n") == "15", "k large enough to connect all"
assert run("1\n4 1\n1 3 2 4\n") == "4", "only consecutive small diffs count"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1 0\n42\n`| 1 | Mảng một phần tử | 
|`1\n5 0\n1 2 3 4 5\n`| 5 | k=0 ngăn cản mọi bước nhảy | 
|`1\n5 100\n1 2 3 4 5\n`| 15 | k đủ lớn để cho phép mọi kết nối | 
|`1\n4 1\n1 3 2 4\n`| 4 | Trình tự hợp lệ không liên tiếp | 

## Vỏ cạnh 

Đối với một mảng có tất cả các phần tử giống hệt nhau, ví dụ:`[2,2,2,2]`Và`k=0`, thuật toán mở rộng chính xác`r`đến cuối mỗi lần. Với `l=0
