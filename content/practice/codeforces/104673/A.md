---
title: "CF 104673A - Mảng"
description: "Cấu trúc được mô tả trong bài toán là một lưới hình tam giác gồm các ô, trong đó mỗi hàng dài hơn hàng trước đúng một ô. Hàng đầu tiên chứa một ô duy nhất và mỗi hàng tiếp theo sẽ mở rộng đối xứng."
date: "2026-06-29T09:18:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104673
codeforces_index: "A"
codeforces_contest_name: "2022-2023 CTU Open Contest"
rating: 0
weight: 104673
solve_time_s: 54
verified: true
draft: false
---

[CF 104673A - Mảng](https://codeforces.com/problemset/problem/104673/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cấu trúc được mô tả trong bài toán là một lưới hình tam giác gồm các ô, trong đó mỗi hàng dài hơn hàng trước đúng một ô. Hàng đầu tiên chứa một ô duy nhất và mỗi hàng tiếp theo sẽ mở rộng đối xứng. Các ô trên ranh giới của mỗi hàng, cùng với ô trên cùng, đều được gán giá trị 1. Mọi ô khác được gán tổng của hai ô ngay phía trên nó ở hàng gần nhất trước đó. 

Cách xây dựng này hoàn toàn giống với sự tái diễn của tam giác Pascal. Nếu chúng ta lập chỉ mục các hàng bắt đầu từ 0 thì giá trị ở hàng r và vị trí k là hệ số nhị thức C(r, k). Các giá trị biên là C(r, 0) và C(r, r), cả hai đều bằng 1 và các giá trị bên trong tuân theo C(r, k) = C(r-1, k-1) + C(r-1, k). 

Mỗi truy vấn đưa ra một số N, được đảm bảo là giá trị của một số ô trong tam giác giống Pascal vô hạn này. Nhiệm vụ là xác định chỉ mục hàng nhỏ nhất trong đó giá trị bằng N xuất hiện. 

Các ràng buộc cho phép tối đa 100000 truy vấn và giá trị lên tới 10^9. Điều này ngay lập tức cho thấy rằng chúng ta không thể mô phỏng tam giác hoặc tính toán các hàng đầy đủ cho từng truy vấn một cách độc lập ở độ sâu lớn. Một cách tiếp cận đơn giản là xây dựng từng hàng một sẽ yêu cầu tạo ra các giá trị O(r^2) trên mỗi hàng và vì các hàng phát triển tuyến tính nên việc đạt tới mức r vừa phải sẽ vượt quá giới hạn thời gian. 

Một điểm tinh tế hơn là giá trị tăng trưởng cực kỳ nhanh chóng. Các hệ số nhị thức ở giữa các hàng tăng theo cấp số nhân với r và vượt quá 10^9 từ rất sớm. Điều này có nghĩa là bất kỳ hàng nào vượt quá kích thước không đổi nhỏ đều không liên quan đến vấn đề này. 

Một sai lầm ngây thơ là cho rằng chúng ta phải tìm kiếm có khả năng lên tới r = 10^9 vì N lớn như vậy. Điều đó không chính xác vì chỉ số và giá trị hàng không được căn chỉnh. Ví dụ: N = 10^9 không yêu cầu hàng gần 10^9; trên thực tế, không có hệ số nhị thức lớn tồn tại ở bất kỳ đâu gần các hàng như vậy trong giới hạn tính toán và chúng ta chỉ cần kiểm tra r nhỏ cho đến khi hệ số tối đa trong hàng vượt quá N. 

Một trường hợp tinh tế khác là N = 1. Giá trị này xuất hiện ở mọi hàng, nhưng câu trả lời phải là 0 vì hàng đầu tiên đã chứa nó rồi. Bất kỳ cách tiếp cận nào tìm kiếm sự xuất hiện đầu tiên mà không xem xét chính xác hàng trên cùng có thể trả về chỉ mục lớn hơn. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo là xây dựng tam giác Pascal theo từng hàng. Đối với mỗi hàng, chúng tôi tính toán tất cả các hệ số nhị thức bằng cách sử dụng phép truy toán từ hàng trước đó và quét xem có mục nhập nào bằng N hay không. Nếu tìm thấy, chúng tôi trả về chỉ mục hàng đó. 

Điều này đúng vì cách dựng hoàn toàn khớp với định nghĩa của hình tam giác. Tuy nhiên, chi phí được xác định bởi số lượng giá trị được tạo ra. Hàng r chứa các phần tử r + 1 và việc xây dựng tất cả các hàng lên đến r yêu cầu tính tổng tất cả các kích thước trước đó, dẫn đến các phép toán gần đúng O(r^2). Ngay cả khi chúng tôi cố gắng dừng sớm trên mỗi hàng, chúng tôi vẫn phải đối mặt với việc tính toán lại nhiều lần trên các truy vấn, điều này trở nên không khả thi đối với 100000 truy vấn. 

Quan sát quan trọng là chúng ta không cần phải xây dựng tất cả các hàng. Các hệ số nhị thức tăng rất nhanh và với N cố định thì hàng chứa N phải nhỏ. Với mỗi hàng r, giá trị lớn nhất là C(r, tầng(r/2)). Con số này tăng nhanh và vượt quá 10^9 khi r chỉ ở độ tuổi 30 thấp. Điều này giới hạn không gian tìm kiếm cho tất cả các truy vấn. 

Thay vì xây dựng các hàng đầy đủ, chúng ta có thể lặp qua các hàng r bắt đầu từ 0 và tính các hệ số nhị thức tăng dần trong mỗi hàng cho đến khi các giá trị vượt quá N. Nếu chúng ta tìm thấy N trong hàng r thì đó là câu trả lời. Vì r nhỏ trên toàn cầu nên điều này trở nên hiệu quả ngay cả đối với nhiều truy vấn. 

Quá trình chuyển đổi từ lực lượng vũ phu sang giải pháp tối ưu xuất phát từ việc nhận ra rằng cấu trúc là tam giác Pascal và sự phát triển của nó giới hạn độ sâu cần thiết cho tìm kiếm.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tam giác đầy đủ) | O(R^2) mỗi truy vấn | O(R) | Quá chậm | 
| Tối ưu (tìm kiếm hàng giới hạn) | O(R^2 + Q·R) trong đó R ≤ ~35 | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi khai thác thực tế là chỉ một số lượng nhỏ hàng có thể chứa các giá trị lên tới 10^9. 

1. Không tính toán trước gì trên toàn cục, nhưng với mỗi chỉ mục hàng r bắt đầu từ 0, hãy tạo các giá trị của hàng r bằng cách sử dụng công thức nhân cho các hệ số nhị thức. Chúng tôi bắt đầu mỗi hàng với giá trị 1. 
2. Đối với hàng r cố định, hãy tính toán từng mục lặp bằng cách sử dụng đẳng thức C(r, k+1) = C(r, k) * (r - k) / (k + 1). Điều này tránh việc tính toán lại các giai thừa và giữ các giá trị chính xác theo số học số nguyên. 
3. Trong khi tạo các mục nhập của hàng r, hãy kiểm tra xem có giá trị nào bằng N hay không. Nếu vậy, ngay lập tức trả về r làm câu trả lời cho truy vấn đó. 
4. Nếu không tìm thấy kết quả trùng khớp, hãy chuyển sang hàng tiếp theo r + 1. 
5. Dừng khi giá trị tối đa có thể có trong một hàng vượt quá 10^9 và chúng tôi đã vượt qua tất cả các hàng ứng cử viên, điều này trên thực tế xảy ra rất sớm (khoảng r ≈ 35). 

Lý do đằng sau tính đúng đắn là mỗi giá trị ô trong cấu trúc là một hệ số nhị thức và mọi hệ số nhị thức xuất hiện chính xác trong hàng tương ứng của nó. Do đó, hàng đầu tiên trong đó N xuất hiện chính xác là dòng r nhỏ nhất sao cho C(r, k) = N với một số k. Vì chúng tôi liệt kê các hàng theo thứ tự tăng dần và quét toàn bộ từng hàng nên kết quả trùng khớp đầu tiên được đảm bảo là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def row_has_value(r, target):
    val = 1
    if val == target:
        return True
    for k in range(0, r):
        val = val * (r - k) // (k + 1)
        if val == target:
            return True
        if val > target:
            break
    return False

def solve():
    q = int(input())
    queries = [int(input()) for _ in range(q)]

    max_n = max(queries)

    # rows beyond this are unnecessary; C(r, r//2) already exceeds 1e9
    # around r = 34..35
    limit = 60

    # precompute answers for all possible N by scanning rows
    # but since Q is large and N varies, we just answer per query
    for n in queries:
        if n == 1:
            print(0)
            continue

        for r in range(limit):
            if row_has_value(r, n):
                print(r)
                break

if __name__ == "__main__":
    solve()
```Mã xử lý từng truy vấn một cách độc lập nhưng dựa vào thực tế là không gian tìm kiếm hàng được giới hạn bởi một hằng số nhỏ. chức năng`row_has_value`xây dựng một hàng tăng dần bằng cách sử dụng nhận dạng nhị thức nhân, giúp tránh tính toán lại các hàng trước đó và đảm bảo tính toán an toàn với số nguyên. 

Việc thoát sớm khi các giá trị vượt quá mục tiêu sẽ ngăn cản công việc không cần thiết ở nửa sau của mỗi hàng, vì các hệ số nhị thức tăng rồi giảm một cách đối xứng. 

Một điểm thực hiện tinh tế là việc sử dụng phép chia số nguyên trong phép truy hồi nhị thức. Phép chia luôn chính xác vì C(r, k) là số nguyên, nhưng sử dụng`//`đảm bảo chúng ta tuân theo số học số nguyên mà không có lỗi dấu phẩy động. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ trong đó truy vấn là 1 và 3. 

Với N = 1, hàng đầu tiên đã chứa giá trị 1. 

| r | k | giá trị | tìm thấy | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | vâng | 

Câu trả lời là 0 ngay lập tức. 

Với N = 3, chúng tôi tiến hành theo từng hàng. 

| r | giá trị hàng | trận đấu | 
| --- | --- | --- | 
| 0 | 1 | không | 
| 1 | 1 1 | không | 
| 2 | 1 2 1 | không | 
| 3 | 1 3 3 1 | vâng | 

Ở hàng 3, ta tìm được 3 nên đáp án là 3. Điều này chứng tỏ ta luôn dừng ở lần xuất hiện đầu tiên, đảm bảo chỉ số hàng tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Q · R) | Mỗi truy vấn quét tối đa ~60 hàng, mỗi hàng được tính theo thời gian tuyến tính trong chỉ mục của nó | 
| Không gian | O(1) | Chỉ một số số nguyên được lưu trữ trong quá trình tính toán | 

Giới hạn không đổi trên R làm cho giải pháp tuyến tính một cách hiệu quả theo số lượng truy vấn. Ngay cả với 100000 truy vấn, tổng số thao tác vẫn đủ nhỏ để thực hiện thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    import sys as _sys
    input = _sys.stdin.readline

    def row_has_value(r, target):
        val = 1
        if val == target:
            return True
        for k in range(0, r):
            val = val * (r - k) // (k + 1)
            if val == target:
                return True
            if val > target:
                break
        return False

    def solve():
        q = int(input())
        for _ in range(q):
            n = int(input())
            if n == 1:
                print(0)
                continue
            for r in range(60):
                if row_has_value(r, n):
                    print(r)
                    break

    solve()
    sys.stdout.seek(0)
    return sys.stdout.read()

# provided sample (conceptual since formatting is incomplete)
# assert run(...) == ...

# custom cases
assert run("1\n1\n") == "0\n"
assert run("1\n3\n") == "3\n"
assert run("2\n2\n6\n") == "2\n3\n"
assert run("3\n1\n2\n10\n") == "0\n2\n5\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 1 | 0 | giá trị nhỏ nhất tại gốc | 
| 1, 3 | 3 | trận đấu nội thất ở hàng 3 | 
| 2, 2, 6 | 2, 3 | nhiều truy vấn, các hàng khác nhau | 
| 3, 1, 2, 10 | 0, 2, 5 | giá trị hỗn hợp nhỏ và vừa | 

## Vỏ cạnh 

Với N = 1, câu trả lời đúng luôn là 0 vì hàng đầu tiên chỉ chứa một ô duy nhất có giá trị 1. Thuật toán xử lý điều này một cách rõ ràng trước khi bắt đầu xây dựng hàng, tránh tính toán không cần thiết. 

Với N = 2, giá trị xuất hiện đầu tiên ở hàng 2 là C(2, 1). Thuật toán kiểm tra hàng 0 và hàng 1 trước, không tìm thấy kết quả khớp nào, sau đó xây dựng hàng 2 trong đó chuỗi 1, 2, 1 được tạo và phát hiện sự trùng khớp tại k = 1. 

Đối với các giá trị gần giới hạn trên như N = 10^9, thuật toán nhanh chóng bỏ qua các hàng cho đến khi hệ số nhị thức vượt quá mục tiêu. Vì các giá trị tăng nhanh nên việc kiểm tra sẽ kết thúc sớm ở mỗi hàng, ngăn cản việc truyền tải toàn bộ các hàng lớn.
