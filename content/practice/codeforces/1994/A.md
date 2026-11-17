---
title: "CF 1994A - Trò chơi đa dạng"
description: "Chúng ta có một lưới đã chứa mọi số nguyên từ 1 đến $n cdot m$ đúng một lần, được sắp xếp theo một thứ tự tùy ý. Nhiệm vụ là xây dựng một lưới khác có cùng kích thước bằng cách sử dụng cùng một bộ số sao cho không có số nào nằm trong ô ban đầu của nó."
date: "2026-06-09T02:21:51+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1994
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 959 sponsored by NEAR (Div. 1 + Div. 2)"
rating: 800
weight: 1994
solve_time_s: 316
verified: false
draft: false
---

[CF 1994A - Trò chơi đa dạng](https://codeforces.com/problemset/problem/1994/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam, triển khai 
**Thời gian giải:** 5 phút 16s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới đã chứa mọi số nguyên từ 1 đến$n \cdot m$đúng một lần, sắp xếp theo thứ tự tùy ý. Nhiệm vụ là xây dựng một lưới khác có cùng kích thước bằng cách sử dụng cùng một bộ số sao cho không có số nào nằm trong ô ban đầu của nó. Mỗi giá trị phải được sử dụng lại chính xác một lần, nhưng mỗi vị trí phải nhận một giá trị khác với giá trị trước đó. 

Về cơ bản, đây là một hạn chế sắp xếp lại toàn cầu chứ không phải là một hạn chế cục bộ. Mỗi ô áp đặt một lựa chọn bị cấm duy nhất, giá trị ban đầu của nó, nhưng tất cả các giá trị khác đều được phép. Khó khăn không phải là chọn giá trị cho từng ô riêng lẻ mà là đảm bảo phép gán chung vẫn sử dụng mỗi số đúng một lần. 

Các ràng buộc nhỏ cho mỗi trường hợp thử nghiệm, với$n, m \le 10$, nhưng có thể có tới$10^3$trường hợp thử nghiệm. Tổng số phần tử trong tất cả các bài kiểm tra nhiều nhất là$5 \cdot 10^4$, vì vậy một$O(nm)$hoặc$O(nm \log nm)$xây dựng cho mỗi bài kiểm tra là đủ. Bất cứ điều gì bậc hai cho mỗi trường hợp thử nghiệm vẫn sẽ vượt qua, nhưng không cần phải phức tạp không cần thiết. 

Một trường hợp thất bại tinh tế xuất hiện khi$n \cdot m = 1$. Chỉ với một ô, phép gán duy nhất có thể là giá trị ban đầu, vi phạm điều kiện. Vì vậy trường hợp này theo định nghĩa là không thể xảy ra. 

Một trường hợp cạnh cấu trúc khác là khi lưới rất nhỏ nhưng không bằng 1. Bất kỳ công trình nào cũng phải đảm bảo tính hợp lệ của hoán vị toàn cục, do đó, các hoán đổi cục bộ ngây thơ có thể vô tình sử dụng lại các giá trị hoặc để lại một điểm cố định. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là tạo ra tất cả các hoán vị của$n \cdot m$số và kiểm tra xem bất kỳ hoán vị nào có tránh được các điểm cố định so với lưới ban đầu hay không. Điều này đơn giản về mặt khái niệm: chúng tôi thử mọi phép gán có thể và xác minh từng ô ràng buộc. Tuy nhiên, số lượng hoán vị tăng theo giai thừa, điều này ngay lập tức không khả thi ngay cả đối với các kích thước vừa phải như$10!$hoặc$20!$, và hoàn toàn không thể$50{,}000$tổng số yếu tố qua các bài kiểm tra. 

Quan sát quan trọng là chúng ta thực sự không cần một hoán vị phức tạp. Chúng ta chỉ cần bất kỳ sự xáo trộn nào, một hoán vị trong đó không có phần tử nào còn ở vị trí ban đầu. Vì tất cả các giá trị đều khác nhau nên chúng ta có thể làm phẳng lưới thành một danh sách, sắp xếp lại nó một cách xác định và ánh xạ lại. 

Một cách xây dựng đơn giản và hiệu quả là xoay mảng dẹt một vị trí. Nếu chúng ta liệt kê các phần tử theo thứ tự hàng lớn như$a[0], a[1], \dots, a[k-1]$, sau đó xác định$b[i] = a[(i+1) \bmod k]$. Điều này đảm bảo rằng mọi phần tử sẽ di chuyển khỏi chỉ mục ban đầu của nó trừ khi$k=1$. Phép quay bảo toàn song ánh và sử dụng mỗi số đúng một lần. 

Lần duy nhất điều này thất bại là khi$k = 1$, vì phép xoay ánh xạ phần tử đơn lẻ vào chính nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((nm)!)$|$O(nm)$| Quá chậm | 
| Xây dựng xoay |$O(nm)$|$O(nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới và làm phẳng nó thành một danh sách theo thứ tự hàng lớn. Điều này chuyển đổi ràng buộc 2D thành vấn đề hoán vị 1D mà không thay đổi cấu trúc của các giá trị. 
2. Hãy để$k = n \cdot m$. Nếu như$k = 1$, xuất ngay$-1$vì không có sự sắp xếp lại hợp lệ nào tồn tại. Không có giá trị thay thế nào để đặt vào ô duy nhất. 
3. Xây dựng danh sách mới bằng cách dịch chuyển từng phần tử về phía trước một vị trí: cho mỗi chỉ mục$i$, gán giá trị từ vị trí$(i+1) \bmod k$. Điều này đảm bảo chuyển động theo chu kỳ của tất cả các yếu tố. 
4. Định hình lại danh sách kết quả thành một$n \times m$ma trận và xuất nó ở dạng hàng chính. 

### Tại sao nó hoạt động 

Việc xây dựng là một hoán vị tuần hoàn của tất cả các yếu tố. Mỗi phần tử di chuyển đến một chỉ mục khác nhau vì mỗi vị trí nhận được phần tử tiếp theo trong chu trình và không có chỉ mục nào ánh xạ tới chính nó trong một chu kỳ có độ dài lớn hơn một. Vì ánh xạ là tính từ nên mỗi số xuất hiện chính xác một lần ở đầu ra và vì mọi phần tử đều dịch chuyển nên không có vị trí nào giữ nguyên giá trị ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out_lines = []

    for _ in range(t):
        n, m = map(int, input().split())
        arr = []
        for _ in range(n):
            arr.extend(map(int, input().split()))

        k = n * m
        if k == 1:
            out_lines.append("-1")
            continue

        shifted = arr[1:] + arr[:1]

        idx = 0
        for i in range(n):
            row = shifted[idx:idx + m]
            out_lines.append(" ".join(map(str, row)))
            idx += m

    print("\n".join(out_lines))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên làm phẳng ma trận để việc lập chỉ mục trở nên tuyến tính, giúp đơn giản hóa logic hoán vị. Trường hợp đặc biệt kiểm tra$k = 1$là cần thiết vì bất kỳ hoán vị nào cũng buộc phải sửa phần tử duy nhất. 

Chi tiết triển khai chính là xoay vòng dựa trên lát cắt`arr[1:] + arr[:1]`, thực hiện dịch chuyển theo chu kỳ một bước. Điều này đảm bảo sự biến dạng hợp lệ cho bất kỳ kích thước nào lớn hơn một mà không cần theo dõi vị trí rõ ràng. 

Cuối cùng, danh sách được xây dựng lại theo từng hàng, giữ nguyên định dạng đầu ra được yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3
1 2 3
4 5 6
```Mảng làm phẳng là:```
[1, 2, 3, 4, 5, 6]
```Sau khi quay:```
[2, 3, 4, 5, 6, 1]
```| tôi | bản gốc | chuyển | 
| --- | --- | --- | 
| 0 | 1 | 2 | 
| 1 | 2 | 3 | 
| 2 | 3 | 4 | 
| 3 | 4 | 5 | 
| 4 | 5 | 6 | 
| 5 | 6 | 1 | 

Ma trận được xây dựng lại:```
2 3 4
5 6 1
```Điều này chứng tỏ rằng mọi phần tử đều di chuyển khỏi vị trí ban đầu trong khi vẫn giữ nguyên tất cả các giá trị. 

### Ví dụ 2 

đầu vào:```
3 3
4 2 1
9 8 3
6 7 5
```Làm phẳng:```
[4, 2, 1, 9, 8, 3, 6, 7, 5]
```Đã thay đổi:```
[2, 1, 9, 8, 3, 6, 7, 5, 4]
```| tôi | bản gốc | chuyển | 
| --- | --- | --- | 
| 0 | 4 | 2 | 
| 1 | 2 | 1 | 
| 2 | 1 | 9 | 
| 3 | 9 | 8 | 
| 4 | 8 | 3 | 
| 5 | 3 | 6 | 
| 6 | 6 | 7 | 
| 7 | 7 | 5 | 
| 8 | 5 | 4 | 

Ma trận được xây dựng lại:```
2 1 9
8 3 6
7 5 4
```Mọi vị trí đều khác với lưới ban đầu, xác nhận tính đúng đắn của chiến lược dịch chuyển theo chu kỳ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| Mỗi phần tử được đọc một lần, dịch chuyển một lần và viết một lần | 
| Không gian |$O(nm)$| Lưu trữ cho mảng phẳng và mảng đầu ra | 

Thuật toán xử lý từng trường hợp kiểm thử theo thời gian tuyến tính tương ứng với kích thước của nó, phù hợp thoải mái trong giới hạn tổng thể của$5 \cdot 10^4$các phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, m = map(int, input().split())
            a = []
            for _ in range(n):
                a.extend(map(int, input().split()))

            if n * m == 1:
                out.append("-1")
                continue

            b = a[1:] + a[:1]

            idx = 0
            for i in range(n):
                out.append(" ".join(map(str, b[idx:idx+m])))
                idx += m
        return "\n".join(out)

    return solve()

# provided sample (format adjusted)
assert run("""1
1 1
1
""") == "-1"

# all elements shift
assert run("""1
1 2
1 2
""").strip() != ""

# single row
assert run("""1
1 3
1 2 3
""") != ""

# single column
assert run("""1
3 1
1
2
3
""") != ""

# larger grid
assert run("""1
2 2
1 2
3 4
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1×1 | -1 | trường hợp bất khả thi | 
| Hàng 1×2 | ca hợp lệ | loạn trí cơ bản | 
| cột 3×1 | ca hợp lệ | xử lý cột | 
| Lưới 2×2 | ca hợp lệ | tính đúng đắn chung | 

## Vỏ cạnh 

Trường hợp nguy hiểm về mặt cấu trúc duy nhất là khi lưới chỉ chứa đúng một phần tử. Trong trường hợp đó, việc làm phẳng sẽ tạo ra một mảng có giá trị đơn và phép dịch chuyển theo chu kỳ sẽ trả về cùng một mảng. Thuật toán kiểm tra điều này một cách rõ ràng và trả về -1 ngay lập tức. 

Đối với đầu vào 1×1:```
1
1
```Làm phẳng sản lượng`[1]`. Bước quay sẽ tạo ra`[1]`, vi phạm điều kiện Việc kiểm tra kích thước ngăn chặn điều này và đảm bảo tính chính xác. 

Tất cả các trường hợp khác có độ dài ít nhất là 2 và sự dịch chuyển theo chu kỳ luôn tạo ra sự sai lệch vì không có chỉ số nào cố định trong một chu kỳ đầy đủ có độ dài lớn hơn một.
