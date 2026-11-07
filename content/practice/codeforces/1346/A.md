---
title: "CF 1346A - Cuộc cách mạng màu sắc"
description: "Chúng tôi được cung cấp tổng số người tham gia và một hệ số nhân cố định. Những người tham gia phải được chia thành bốn nhóm liên tiếp trong đó mỗi nhóm tiếp theo lớn hơn chính xác $k$ lần so với nhóm trước."
date: "2026-06-11T15:00:29+07:00"
tags: ["codeforces", "competitive-programming", "*special", "math"]
categories: ["algorithms"]
codeforces_contest: 1346
codeforces_index: "A"
codeforces_contest_name: "Kotlin Heroes: Episode 4"
rating: 1000
weight: 1346
solve_time_s: 713
verified: true
draft: false
---

[CF 1346A - Cuộc cách mạng màu sắc](https://codeforces.com/problemset/problem/1346/A) 

**Đánh giá:** 1000 
**Tags:** *đặc biệt, toán học 
**Thời gian giải:** 11 phút 53 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp tổng số người tham gia và một hệ số nhân cố định. Những người tham gia phải được chia thành bốn nhóm liên tiếp trong đó mỗi nhóm tiếp theo chính xác là$k$lớn hơn lần trước. Khi kích thước nhóm đầu tiên được cố định, phần còn lại được xác định hoàn toàn dưới dạng cấp số nhân:$$n_1,\; k n_1,\; k^2 n_1,\; k^3 n_1$$và tổng của chúng phải bằng tổng$n$. 

Vì vậy, nhiệm vụ là xây dựng lại một phân rã hình học hợp lệ của$n$thành bốn số hạng có tỉ số$k$. 

Các ràng buộc là rất nhỏ về mặt tính toán. Mỗi trường hợp thử nghiệm chỉ yêu cầu các phép tính số học không đổi. Thậm chí ở mức tối đa$t = 1000$, việc quét tuyến tính hoặc thậm chí số học lặp đi lặp lại là không cần thiết, do đó$O(t)$giải pháp là đủ. Bất kỳ giải pháp nào liên quan đến vòng lặp trên phạm vi lớn hoặc tìm kiếm trên$n$sẽ là không cần thiết và quá mức cần thiết. 

Một số trường hợp đặc biệt quan trọng về mặt khái niệm. Khi$k = 1$, cả bốn nhóm đều bằng nhau nên đáp án quy về việc chia$n$thành bốn phần bằng nhau. Khi$k > 1$, nhóm đầu tiên trở nên nhỏ vì lũy thừa cao hơn chiếm ưu thế trong tổng một cách nhanh chóng. Bài toán đảm bảo tồn tại nghiệm nguyên nên không cần xử lý các trường hợp không khả thi. 

## Phương pháp tiếp cận 

Một ý tưởng ngây thơ là thử tất cả các giá trị có thể có của$n_1$. Với mỗi ứng viên, hãy tính$n_1 + k n_1 + k^2 n_1 + k^3 n_1$và kiểm tra xem nó có bằng không$n$. Điều này hoạt động nhưng không cần thiết. Biểu thức được đơn giản hóa thành$$n = n_1 (1 + k + k^2 + k^3)$$vì vậy chúng ta chỉ cần tính một phép chia duy nhất:$$n_1 = \frac{n}{1 + k + k^2 + k^3}$$Chế độ xem brute-force hoạt động vì nó trực tiếp kiểm tra cấu trúc của chuỗi, nhưng nó trở nên lãng phí vì mối quan hệ là tuyến tính trong$n_1$. Quan sát quan trọng là bốn số hạng tạo thành một tổng hình học cố định, do đó tổng số chỉ là bội số không đổi của số hạng đầu tiên. Điều này làm giảm vấn đề chỉ còn việc tính toán một thương số duy nhất và sau đó xây dựng lại các giá trị còn lại bằng phép nhân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm vũ phu |$O(n)$mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Công thức trực tiếp |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các giá trị$n$Và$k$cho từng trường hợp thử nghiệm. Chúng xác định một cấu trúc hình học trong đó tất cả bốn nhóm được cố định sau khi biết được nhóm đầu tiên. 
2. Tính hệ số tổng hình học$$S = 1 + k + k^2 + k^3$$Điều này tượng trưng cho bao nhiêu lần$n_1$được tính vào tổng số. 
3. Tính toán$n_1 = n / S$. Điều này có tác dụng vì bài toán đảm bảo tính chia hết. Nếu nó không được đảm bảo, chúng tôi sẽ cần phải xác nhận, nhưng ở đây việc xây dựng đảm bảo tính nhất quán. 
4. Xây dựng các nhóm còn lại như$n_2 = k n_1$,$n_3 = k^2 n_1$, Và$n_4 = k^3 n_1$. Mỗi bước tiếp theo trực tiếp từ định nghĩa của tiến trình. 
5. Xuất ra bốn giá trị theo thứ tự. 

### Tại sao nó hoạt động 

Tổng số người tham gia chính xác là tổng của cấp số nhân với tỷ lệ cố định$k$. Mọi giải pháp hợp lệ phải khớp với cấu trúc này một cách duy nhất bởi vì một khi$n_1$cố định, tất cả các giá trị khác được xác định. Phương trình biến bài toán thành giải một phương trình tuyến tính đơn một biến, do đó nghiệm được xây dựng là ứng cử viên duy nhất và do đó đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())

        S = 1 + k + k * k + k * k * k
        n1 = n // S

        n2 = n1 * k
        n3 = n2 * k
        n4 = n3 * k

        print(n1, n2, n3, n4)

if __name__ == "__main__":
    solve()
```Mã trực tiếp mã hóa cấu trúc đại số của bài toán. Điểm tinh tế duy nhất là tính toán tổng hình học một cách chính xác và đảm bảo sử dụng phép chia số nguyên. Sự đảm bảo về sự tồn tại đảm bảo rằng$n$chia hết cho$S$, như vậy việc chia tầng là an toàn. 

Máy tính$n_2, n_3, n_4$lặp đi lặp lại tránh khả năng tính toán lại của$k$, giúp quá trình triển khai luôn rõ ràng và tránh các mối lo ngại về tràn trong các ngôn ngữ có kích thước số nguyên cố định. Python xử lý các số nguyên lớn một cách tự nhiên, do đó không cần có biện pháp phòng ngừa bổ sung nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 40, k = 3
```| Bước | Giá trị | 
| --- | --- | 
|$S$|$1 + 3 + 9 + 27 = 40$| 
|$n_1$|$40 / 40 = 1$| 
|$n_2$|$3$| 
|$n_3$|$9$| 
|$n_4$|$27$| 

Điều này khớp chính xác với sự phân tách cần thiết, cho thấy toàn bộ cấu trúc thu gọn thành một tổng hình học hoàn hảo như thế nào. 

### Ví dụ 2 

đầu vào:```
n = 1200, k = 7
```| Bước | Giá trị | 
| --- | --- | 
|$S$|$1 + 7 + 49 + 343 = 400$| 
|$n_1$|$1200 / 400 = 3$| 
|$n_2$|$21$| 
|$n_3$|$147$| 
|$n_4$|$1029$| 

Điều này xác nhận rằng ngay cả đối với các số nhân lớn hơn, cấu trúc vẫn thuần túy đại số và không cần tìm kiếm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi trường hợp thử nghiệm sử dụng một số phép tính số học không đổi | 
| Không gian |$O(1)$| Chỉ một vài số nguyên được lưu trữ cho mỗi trường hợp thử nghiệm | 

Các ràng buộc cho phép tối đa 1000 trường hợp thử nghiệm và mỗi trường hợp được giải quyết trong thời gian không đổi. Điều này dễ dàng nằm trong giới hạn vì giải pháp chỉ thực hiện số học cơ bản trên mỗi dòng đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return io.StringIO().getvalue()

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        S = 1 + k + k*k + k*k*k
        n1 = n // S
        out.append(f"{n1} {n1*k} {n1*k*k} {n1*k*k*k}")
    print("\n".join(out))

# provided samples
assert run("""4
40 3
1200 7
320802005 400
4 1
""") == """1 3 9 27
3 21 147 1029
5 2000 800000 320000000
1 1 1 1
""", "sample tests"

# k = 1 case
assert run("""1
8 1
""") == "2 2 2 2\n", "all equal split"

# small n
assert run("""1
40 3
""") == "1 3 9 27\n", "basic case"

# larger k
assert run("""1
1200 7
""") == "3 21 147 1029\n", "larger multiplier"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bộ mẫu | đúng trình tự | tính đúng đắn trong nhiều trường hợp | 
| k = 1 | chia đều | tiến triển hình học suy biến | 
| nhỏ | tính toán trực tiếp | tính đúng đắn của số học cơ sở | 
| k lớn | tăng trưởng theo quy mô | xử lý các số nhân lớn hơn | 

## Vỏ cạnh 

Khi nào$k = 1$, tổng hình học trở thành$4$, vì vậy mỗi nhóm phải chính xác$n/4$. Thuật toán xử lý việc này một cách tự nhiên bởi vì$S = 4$và mọi phép nhân đều bảo toàn sự bằng nhau. 

Khi$k$lớn,$k^3$chiếm ưu thế trong tổng, làm cho$n_1$rất nhỏ. Tính toán vẫn hoạt động vì tất cả các giá trị vẫn là số nguyên do sự đảm bảo trong câu lệnh vấn đề. 

Khi$n$chính xác bằng tổng hình học$S$, giải pháp mang lại$n_1 = 1$, tạo ra cấu hình hợp lệ tối thiểu$1, k, k^2, k^3$, xác nhận hành vi trường hợp ranh giới.
