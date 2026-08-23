---
title: "CF 104285A - ATCG"
description: "Chúng ta có một chuỗi DNA được viết dưới dạng một chuỗi trên bảng chữ cái {A, T, C, G}. Sinh học cho chúng ta một quy tắc biến đổi chính xác để xây dựng chuỗi bổ sung: đầu tiên đảo ngược trình tự ban đầu vì hai chuỗi chạy ngược chiều nhau, sau đó thay thế từng chuỗi…"
date: "2026-07-01T20:54:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104285
codeforces_index: "A"
codeforces_contest_name: "PCCA Winter Camp Contest 2023"
rating: 0
weight: 104285
solve_time_s: 46
verified: true
draft: false
---

[CF 104285A - ATCG](https://codeforces.com/problemset/problem/104285/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi DNA được viết dưới dạng một chuỗi trên bảng chữ cái`{A, T, C, G}`. Sinh học cho chúng ta một quy tắc biến đổi chính xác để xây dựng chuỗi bổ sung: đầu tiên đảo ngược trình tự ban đầu vì hai chuỗi chạy ngược chiều nhau, sau đó thay thế từng nucleotide bằng quy tắc ghép cặp cố định`A ↔ T`Và`C ↔ G`. 

Nhiệm vụ là mô phỏng chính xác sự chuyển đổi đó cho từng trường hợp thử nghiệm. Đầu vào cung cấp nhiều chuỗi DNA độc lập và với mỗi chuỗi chúng ta phải xuất ra chuỗi bổ sung theo cùng hướng từ trái sang phải như khi nó xuất hiện từ đầu 5′ đến đầu 3′. 

Các ràng buộc đủ nhỏ để bất kỳ phép biến đổi tuyến tính nào cho mỗi trường hợp thử nghiệm đều đủ. Mỗi chuỗi có độ dài tối đa là 100 và có tối đa 2000 trường hợp thử nghiệm, do đó, ngay cả giải pháp O(n) cho mỗi trường hợp đơn giản cũng có thể chạy thoải mái trong giới hạn. Tổng số ký tự được xử lý tối đa là 200000, điều này không đáng kể đối với Python. 

Điểm tinh tế duy nhất có thể gây khó khăn cho việc triển khai là quên bước đảo ngược. Một giải pháp đơn giản chỉ áp dụng thay thế mà không đảo ngược sẽ tạo ra chuỗi DNA trông hợp lệ nhưng sai hướng. 

Ví dụ, nếu`s = "ATCG"`, thay thế trực tiếp cho`"TAGC"`, nhưng quy trình đúng sẽ đảo ngược trước:`"GCTA"`sau đó thay thế vào`"CGAT"`. 

Một sai lầm khác có thể xảy ra là áp dụng đảo ngược sau khi thay thế. Điều đó cũng cho kết quả sai vì bổ sung các ký hiệu thay đổi chứ không phải vị trí nên thứ tự thực hiện các thao tác là cố định. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ xây dựng chuỗi đảo ngược một cách rõ ràng và sau đó lặp lại một lần nữa áp dụng các quy tắc ánh xạ. Đây đã là thời gian tuyến tính và hoạt động vì phép biến đổi hoàn toàn mang tính cục bộ: mỗi ký tự ánh xạ độc lập khi biết vị trí của nó trong chuỗi đảo ngược. 

Chúng ta cũng có thể coi đó là một lần chuyển nếu chúng ta trực tiếp lặp lại chuỗi từ đầu đến cuối và xây dựng kết quả một cách nhanh chóng. Điều này loại bỏ sự cần thiết phải có một bản sao đảo ngược rõ ràng. 

Quan sát quan trọng là phép biến đổi phân hủy thành hai thành phần độc lập: hoán vị các chỉ số (thứ tự ngược lại) và thay thế các ký tự. Vì cả hai đều là O(1) cho mỗi phần tử và không phụ thuộc vào nhau ngoài thứ tự, nên chúng ta có thể hợp nhất chúng thành một lượt duy nhất. 

Phương pháp brute-force thực hiện hai lần quét tuyến tính trên mỗi chuỗi. Phiên bản được tối ưu hóa thực hiện một, nhưng tiệm cận cả hai đều là O(n). Sự cải tiến chủ yếu là đơn giản hóa hệ số không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (đảo ngược + bản đồ) | O(n) | O(n) | Đã chấp nhận | 
| Tối ưu (lặp ngược đơn) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc chuỗi`s`. Chuỗi này đại diện cho một chuỗi DNA từ đầu 5′ đến đầu 3′ của nó. 
2. Lập bản đồ giữa các nucleotit:`A → T`,`T → A`,`C → G`,`G → C`. Điều này mã hóa quy tắc ghép nối sinh học và được áp dụng độc lập cho từng ký tự. 
3. Duyệt chuỗi từ ký tự cuối cùng đến ký tự đầu tiên. Điều này ngầm thực hiện bước đảo ngược mà không cần xây dựng chuỗi đảo ngược mới. 
4. Đối với mỗi ký tự gặp phải trong quá trình duyệt ngược, hãy thay thế nó bằng cách sử dụng ánh xạ và nối kết quả vào bộ đệm đầu ra. 
5. Sau khi xử lý tất cả các ký tự, xuất ra chuỗi đã tạo. 

Lý do chúng tôi lặp lại theo thứ tự ngược lại thay vì đảo ngược một cách rõ ràng là vì nó tránh việc phân bổ thêm bộ nhớ trong khi vẫn duy trì tính chính xác. Vì mỗi vị trí là độc lập sau khi đảo ngược nên việc truyền trực tiếp quá trình chuyển đổi là tương đương. 

### Tại sao nó hoạt động 

Sự biến đổi DNA là sự kết hợp của hai chức năng: hoán vị`P`đảo ngược các chỉ số và thay thế`C`ánh xạ từng nhân vật. Bởi vì`C`hành động theo từng điểm và không phụ thuộc vào các nước láng giềng hoặc cấu trúc toàn cầu, thành phần`C(P(s))`có thể được tính bằng cách áp dụng cả hai thao tác trong một lần truyền tải duy nhất theo thứ tự chỉ mục được hoán vị. Điều này đảm bảo mỗi ký tự được chuyển đổi chính xác một lần ở đúng vị trí cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    mp = {
        'A': 'T',
        'T': 'A',
        'C': 'G',
        'G': 'C'
    }

    out = []
    for _ in range(t):
        n = int(input())
        s = input().strip()

        res = []
        for i in range(n - 1, -1, -1):
            res.append(mp[s[i]])
        out.append("".join(res))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng ánh xạ ký tự trực tiếp và xử lý từng chuỗi từ phải sang trái. Vòng lặp kết thúc`i = n-1 ... 0`ngầm thực hiện việc đảo ngược. Mỗi ký tự được chuyển đổi một lần bằng cách tra cứu từ điển, đó là O(1). Kết quả được tích lũy trong một danh sách để đảm bảo tính hiệu quả và được nối một lần cho mỗi trường hợp thử nghiệm. 

Việc sử dụng`strip()`đảm bảo không có dòng mới nào cản trở việc lập chỉ mục. Đầu ra được đệm để tránh ghi lặp lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chuỗi đầu vào:`ACGT`Chúng tôi xử lý nó từ cuối: 

| Bước | Chỉ mục | Nhân vật | Đã ánh xạ | 
| --- | --- | --- | --- | 
| 1 | 3 | T | A | 
| 2 | 2 | G | C | 
| 3 | 1 | C | G | 
| 4 | 0 | A | T | 

Kết quả xây dựng như`A C G T`, cho`"ACGT"`. 

Ví dụ này đối xứng theo phần bù và phần đảo ngược, điều này xác nhận tính đúng đắn khi chuỗi gốc bằng cấu trúc phần bù ngược của chính nó. 

### Ví dụ 2 

Chuỗi đầu vào:`ATGGCT`| Bước | Chỉ mục | Nhân vật | Đã ánh xạ | 
| --- | --- | --- | --- | 
| 1 | 5 | T | A | 
| 2 | 4 | C | G | 
| 3 | 3 | G | C | 
| 4 | 2 | G | C | 
| 5 | 1 | T | A | 
| 6 | 0 | A | T | 

Đầu ra cuối cùng:`"AGCCAT"`Điều này chứng tỏ rằng cả sự đảo ngược và thay thế đều cần thiết. Sự thay thế trực tiếp sẽ tạo ra`TACCG A`theo thứ tự ban đầu là không chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi ký tự được truy cập đúng một lần theo thứ tự ngược lại | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Lưu trữ chuỗi đầu ra cho mỗi chuỗi được chuyển đổi | 

Tổng công việc trên tất cả các trường hợp thử nghiệm tỷ lệ thuận với tổng kích thước đầu vào, tối đa là 200000 ký tự. Điều này nằm trong giới hạn thời gian thông thường đối với Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdout = old_stdout
    return out

# provided sample
assert run("""1
4
ACGT
""") == "ACGT"

# reverse check
assert run("""1
4
ATCG
""") == "CGAT"

# single character
assert run("""1
1
A
""") == "T"

# all same type pattern
assert run("""1
3
AAA
""") == "TTT"

# mixed case
assert run("""1
6
ATGGCT
""") == "AGCCAT"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ACGT | ACGT | trường hợp đối xứng | 
| ATCG | CGAT | sự đảo ngược đúng đắn | 
| A | T | kích thước tối thiểu | 
| AAA | TTT | sự ổn định lập bản đồ lặp đi lặp lại | 
| ATGGCT | AGCCAT | sự chuyển đổi đầy đủ đúng đắn | 

## Vỏ cạnh 

Đối với đầu vào một ký tự như`A`, thuật toán đọc chỉ số 0, ánh xạ trực tiếp tới`T`, và tạo ra`"T"`. Vì sự đảo ngược không ảnh hưởng đến một phần tử đơn lẻ nên tính chính xác chỉ phụ thuộc vào bảng ánh xạ, bảng này vẫn hợp lệ. 

Đối với một đầu vào như`ATCG`, thứ tự duyệt trở thành`G → C → T → A`. Mỗi ký tự được ánh xạ độc lập, tạo ra`C → G`,`G → C`,`T → A`,`A → T`, nối với`"CGAT"`. Điều này xác nhận rằng việc đảo ngược được xử lý hoàn toàn theo hướng lặp và không yêu cầu một mảng riêng biệt.
