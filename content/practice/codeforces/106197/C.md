---
title: "CF 106197C - Lưới chia"
description: "Bài toán xoay quanh cấu trúc được hình thành bởi tất cả các ước của một số nguyên cho trước khi được sắp xếp theo tính chia hết."
date: "2026-06-25T10:27:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106197
codeforces_index: "C"
codeforces_contest_name: "Rutgers University Programming Contest Fall 2025 - Open Division"
rating: 0
weight: 106197
solve_time_s: 52
verified: true
draft: false
---

[CF 106197C - Mạng chia số](https://codeforces.com/problemset/problem/106197/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán xoay quanh cấu trúc được hình thành bởi tất cả các ước của một số nguyên cho trước khi được sắp xếp theo tính chia hết. Nếu bạn lấy một số và liệt kê mọi số nguyên chia cho nó, thì các ước số này sẽ tạo thành một cấu trúc giống như đồ thị một cách tự nhiên: mỗi ước số có thể được kết nối với các ước số “lớn hơn” thu được bằng cách tăng lũy ​​thừa của một thừa số nguyên tố duy nhất. 

Cụ thể hơn, hãy coi các ước số là các điểm trong một lưới trong đó mỗi trục tương ứng với một thừa số nguyên tố của số. Mỗi ước số được biểu thị bằng số lần mỗi số nguyên tố xuất hiện trong đó. Việc chuyển từ ước số này sang ước số khác chỉ được phép khi bạn tăng số mũ của đúng một số nguyên tố lên một bước, trong khi vẫn giữ nguyên tất cả các số mũ khác. Nhiệm vụ là tính toán có bao nhiêu “sự gia tăng một bước” trực tiếp như vậy tồn tại trong cấu trúc này. 

Đầu vào bao gồm một số nguyên duy nhất, số có cấu trúc ước số mà chúng ta đang phân tích. Đầu ra là một số nguyên duy nhất biểu thị số cạnh trong biểu đồ ước số này, trong đó một cạnh kết nối hai ước số nếu một ước số có thể lấy được từ ước số kia bằng cách nhân với chính xác một thừa số nguyên tố vẫn có sẵn trong giới hạn phân tích nhân tử. 

Hàm ý hạn chế chính là bản thân số đó có thể đủ lớn để việc liệt kê tất cả các ước số một cách rõ ràng là không khả thi. Ngay cả đối với các giá trị vừa phải, số lượng ước số có thể tăng theo cấp số nhân và việc xây dựng biểu đồ ước số đầy đủ sẽ cần thời gian tỷ lệ thuận với kích thước đó. Điều này thúc đẩy chúng ta làm việc trực tiếp với việc phân tích thành thừa số nguyên tố thay vì liệt kê các ước số. 

Một số trường hợp đặc biệt bộc lộ những suy nghĩ ngây thơ không thành công. Nếu số đó là số nguyên tố, ví dụ 13, thì chỉ có hai ước số là 1 và 13 và có đúng một cạnh giữa chúng. Một cách tiếp cận ngây thơ giả định nhiều cấu trúc hơn có thể đếm không chính xác nhiều lần chuyển đổi, nhưng chỉ có thể tăng một hệ số nguyên tố duy nhất. 

Đối với một số như 16, tức là 2⁴, các ước số là 1, 2, 4, 8, 16. Cấu trúc là một chuỗi đơn giản và có chính xác bốn cạnh. Bất kỳ cách tiếp cận nào xử lý nhầm các ước số là các tổ hợp độc lập thay vì số gia số mũ sẽ tính sai cấu trúc tuyến tính. 

Đối với một số hỗn hợp như 12 = 2² · 3¹, cấu trúc ước số là hai chiều. Một phương pháp đơn giản chỉ tính các lựa chọn nhân cho mỗi ước số mà quên rằng mỗi trạng thái đóng góp nhiều cạnh đi ra sẽ bị tính thiếu. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ tạo ra tất cả các ước số của số một cách rõ ràng, sau đó với mỗi lần chia số nhân với tất cả các số nguyên tố vẫn giữ kết quả trong số ban đầu. Điều này đòi hỏi phải liệt kê các ước số trước, việc này mất thời gian O(d(n)) trong đó d(n) là số lượng ước số. Đối với mỗi ước số, việc kiểm tra chuyển đổi có thể mất tới O(log n), dẫn đến tổng chi phí khoảng O(d(n) log n). Trong trường hợp xấu nhất, chẳng hạn như các số tổng hợp cao, d(n) tăng đủ lớn đến mức điều này trở nên không khả thi. 

Cái nhìn sâu sắc về cấu trúc quan trọng là tập hợp số chia không phải là tùy ý. Khi số được viết ở dạng phân tích thừa số nguyên tố, mỗi ước số tương ứng với một lựa chọn số mũ cho mỗi số nguyên tố. Các cạnh của đồ thị chính xác là số gia của các vectơ số mũ này. Thay vì xây dựng biểu đồ, chúng ta có thể đếm các chuyển tiếp theo tổ hợp. 

Đối với thừa số nguyên tố pᵢ có số mũ aᵢ, mọi ước số có thể tăng số mũ của nó trong pᵢ từ 0 lên aᵢ − 1. Đối với mỗi lựa chọn như vậy, các số nguyên tố khác vẫn cố định, tạo thành các kết hợp độc lập. Điều này biến vấn đề thành tổng các đóng góp theo các chiều của tích Descartes, loại bỏ mọi nhu cầu liệt kê rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(d(n) log n) | O(d(n)) | Quá chậm | 
| Tối ưu | O(√n + k²) (hoặc hệ số Pollard Rho) | O(k) | Đã chấp nhận |

Ở đây k là số các thừa số nguyên tố phân biệt. 

## Hướng dẫn thuật toán 

1. Phân tích số đầu vào thành lũy thừa nguyên tố n = p₁ᵃ¹ · p₂ᵃ² · ... · p_kᵃᵏ. 

Điều này là cần thiết vì toàn bộ cấu trúc của ước số chỉ phụ thuộc vào các số mũ này. 
2. Với mỗi thừa số nguyên tố pᵢ, hãy tính đóng góp của nó vào các cạnh bằng aᵢ nhân với số cách chọn số mũ cho tất cả các số nguyên tố khác, là Π_{j ≠ i} (aⱼ + 1). 

Điều này tương ứng với việc cố định trạng thái ước số và đếm số lần chúng ta có thể tăng số mũ của pᵢ. 
3. Tích lũy tổng của các đóng góp này trên tất cả các thừa số nguyên tố. 
4. Xuất số tiền cuối cùng. 

Lý do bước 2 hợp lệ là vì mỗi cạnh tương ứng duy nhất với việc chọn một ước số và một số nguyên tố có số mũ chưa đạt mức tối đa. Mỗi lựa chọn như vậy tương ứng với chính xác một ước số liền kề hợp lệ và không có cạnh nào được tính hai lần vì số mũ nguyên tố tăng dần xác định duy nhất quá trình chuyển đổi. 

### Tại sao nó hoạt động 

Mỗi ước số được xác định đầy đủ bởi một vectơ số mũ (x₁, x₂, ..., x_k). Một cạnh tồn tại khi chúng ta di chuyển từ vectơ này sang vectơ khác bằng cách tăng chính xác một tọa độ lên 1 trong khi vẫn ở trong giới hạn. Đối với tọa độ i cố định, mọi vectơ bắt đầu hợp lệ đều có xᵢ trong [0, aᵢ − 1] và tất cả các tọa độ khác đều tự do trong phạm vi của chúng. Điều này tạo ra chính xác aᵢ · Π_{j≠i}(aⱼ+1) cạnh hợp lệ cho chiều i. Tính tổng trên tất cả các chiều sẽ bao phủ mọi cạnh có thể có chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import math

def factorize(n: int):
    res = {}
    d = 2
    while d * d <= n:
        while n % d == 0:
            res[d] = res.get(d, 0) + 1
            n //= d
        d += 1
    if n > 1:
        res[n] = res.get(n, 0) + 1
    return res

def solve():
    n = int(input().strip())
    pf = factorize(n)

    exps = list(pf.values())
    k = len(exps)

    ans = 0
    for i in range(k):
        contrib = exps[i]
        for j in range(k):
            if i != j:
                contrib *= (exps[j] + 1)
        ans += contrib

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện bắt đầu bằng cách phân tích số đó bằng cách sử dụng phép chia thử. Điều này là đủ với giả định rằng kích thước đầu vào vừa phải; đối với các ràng buộc lớn hơn, phương pháp phân tích nhân tử nhanh hơn sẽ thay thế bước này mà không thay đổi phần còn lại của logic. 

Sau khi trích xuất số mũ, mã sẽ trực tiếp thực hiện công thức tổ hợp. Đối với mỗi số mũ nguyên tố, nó tính tích của (aⱼ + 1) trên tất cả các số nguyên tố khác, sau đó nhân với aᵢ. Điều này phản ánh số cách để chọn trạng thái ước số hợp lệ trong đó số nguyên tố đó vẫn có thể tăng lên. 

Một điểm tinh tế là chúng ta không bao giờ xây dựng các ước số hoặc các cạnh một cách rõ ràng. Tất cả việc đếm xảy ra trong không gian số mũ, giúp tránh được sự bùng nổ tổ hợp. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 12 

Ở đây 12 = 2² · 3¹, vậy số mũ là [2, 1]. 

| Bước | Chỉ số chính i | Công thức đóng góp | Giá trị | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 * (1 + 1) | 4 | 
| 2 | 3 | 1 * (2 + 1) | 3 | 

Tổng số câu trả lời = 7. 

Dấu vết này cho thấy mỗi chiều nguyên tố đóng góp độc lập như thế nào và chiều kia hoạt động như một hệ số lựa chọn nhân như thế nào. 

### Ví dụ 2: n = 16 

Ở đây 16 = 2⁴, vậy số mũ là [4]. 

| Bước | Chỉ số chính i | Công thức đóng góp | Giá trị | 
| --- | --- | --- | --- | 
| 1 | 2 | 4 | 4 | 

Tổng số câu trả lời = 4. 

Điều này xác nhận trường hợp suy biến một chiều trong đó mạng chia trở thành một chuỗi đơn giản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(√n + k2) | nhân tử hóa phép chia thử nghiệm cộng với phép tính tổng số mũ theo cặp | 
| Không gian | O(k) | lưu trữ số mũ nguyên tố | 

Việc tính toán bị chi phối bởi hệ số hóa. Sau đó, tập hợp dựa trên số mũ là thời gian không đổi trên mỗi cặp thừa số nguyên tố, không đáng kể đối với các ràng buộc điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    def factorize(n: int):
        res = {}
        d = 2
        while d * d <= n:
            while n % d == 0:
                res[d] = res.get(d, 0) + 1
                n //= d
            d += 1
        if n > 1:
            res[n] = res.get(n, 0) + 1
        return res

    def solve():
        n = int(sys.stdin.readline().strip())
        pf = factorize(n)
        exps = list(pf.values())

        ans = 0
        for i in range(len(exps)):
            cur = exps[i]
            for j in range(len(exps)):
                if i != j:
                    cur *= (exps[j] + 1)
            ans += cur
        return str(ans)

    return solve()

# custom cases
assert run("1") == "0", "edge: no edges"
assert run("2") == "1", "prime case"
assert run("12") == "7", "two-dimensional lattice"
assert run("16") == "4", "single chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | đầu vào tối thiểu, không có ước số nào ngoài chính nó | 
| 2 | 1 | trường hợp cạnh thừa số nguyên tố | 
| 12 | 7 | tương tác giữa hai số nguyên tố | 
| 16 | 4 | xích dẫn điện đơn | 

## Vỏ cạnh 

Với n = 1, hệ số hóa trống, do đó không có thứ nguyên số mũ. Thuật toán tạo ra một tổng trống và trả về chính xác 0, khớp với thực tế là mạng chia có một nút duy nhất và không có cạnh. 

Đối với số nguyên tố như 17, phân tích nhân tử có một số mũ [1]. Đóng góp duy nhất là 1, tương ứng với cạnh đơn từ 1 đến 17. Vòng lặp xử lý việc này một cách rõ ràng mà không có trường hợp đặc biệt nào. 

Đối với một số như 2³ · 3², thuật toán tính rõ ràng các khoản đóng góp từ cả hai thứ nguyên. Khi tập trung vào 2, có 3 lựa chọn số mũ cho 2 chuyển đổi và 3 lựa chọn cho 3 chiều, cho 3 · 3 = 9 cạnh đóng góp bởi 2. Về mặt đối xứng, 3 chiều đóng góp 2 · 4 = 8 cạnh. Tổng này nắm bắt toàn bộ lưới chuyển tiếp mà không chồng chéo vì mỗi cạnh được xác định duy nhất theo sự thay đổi tọa độ.
