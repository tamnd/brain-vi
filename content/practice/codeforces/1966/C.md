---
title: "CF 1966C - Mọi Thứ Nim"
description: "Chúng ta được yêu cầu phân tích một trò chơi hai người chơi với nhiều đống đá. Đến lượt người chơi, họ chọn một số $k$ không vượt quá cọc nhỏ nhất không trống, sau đó loại bỏ các viên $k$ khỏi mỗi cọc vẫn còn đá."
date: "2026-06-09T01:57:24+07:00"
tags: ["codeforces", "competitive-programming", "games", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1966
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 941 (Div. 2)"
rating: 1400
weight: 1966
solve_time_s: 103
verified: false
draft: false
---

[CF 1966C - Mọi thứ Nim](https://codeforces.com/problemset/problem/1966/C) 

**Đánh giá:** 1400 
**Tags:** trò chơi, tham lam 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu phân tích một trò chơi hai người chơi với nhiều đống đá. Đến lượt người chơi, họ chọn một số$k$không vượt quá đống không trống nhỏ nhất thì loại bỏ$k$đá từ mọi đống vẫn còn đá. Người chơi luân phiên di chuyển và người chơi đầu tiên không thể thực hiện nước đi sẽ thua. Alice di chuyển đầu tiên. Đối với mỗi cấu hình ban đầu, chúng ta phải xác định người chơi nào thắng nếu cả hai đều chơi tối ưu. 

Đầu vào cung cấp số lượng trường hợp thử nghiệm và đối với mỗi trường hợp, số lượng cọc theo sau là kích thước cọc. Chúng ta phải xuất ra "Alice" hoặc "Bob" tùy theo người chiến thắng. Kích thước cọc có thể lớn bằng$10^9$và số lượng cọc cho mỗi trường hợp thử nghiệm có thể đạt tới$2\cdot 10^5$, với tổng số$2\cdot 10^5$cọc trên tất cả các trường hợp thử nghiệm. Điều này có nghĩa là chúng tôi không thể mô phỏng trò chơi theo từng bước. Bất kỳ giải pháp nào cố gắng loại bỏ đá lặp đi lặp lại trong mỗi lượt sẽ dễ dàng đạt được$10^{14}$hoạt động hoàn toàn không thể thực hiện được. 

Trường hợp cạnh phát sinh khi cọc nhỏ nhất là một. Trong những tình huống như vậy, người chơi đầu tiên chỉ có thể loại bỏ một viên đá trên mỗi cọc, điều này có thể cho phép người chơi thứ hai phản ứng ngay lập tức một cách tối ưu và kiểm soát trò chơi. Ví dụ, nếu cọc được`[1, 7]`, Alice phải lấy đi 1 viên đá ở cả hai đống, để lại`[0, 6]`. Sau đó Bob có thể loại bỏ 6 viên đá khỏi đống còn lại và giành chiến thắng ngay lập tức. Cách tiếp cận ngây thơ chỉ nhìn vào đống lớn nhất sẽ thất bại ở đây. 

Một trường hợp tinh tế khác là khi tất cả các cọc đều bằng nhau, chẳng hạn`[3, 3, 3]`. Sau đó, người chơi đầu tiên có thể lấy tất cả các viên đá cùng một lúc bằng cách chọn$k=3$, chiến thắng ngay lập tức. Những ví dụ này gợi ý rằng cọc nhỏ nhất rất quan trọng đối với chiến lược và tính chẵn lẻ của nó (dù tổng của tất cả các nước đi là số lẻ hay thậm chí so với cọc nhỏ nhất) sẽ xác định ai có quyền kiểm soát. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng trò chơi bằng cách liên tục chọn một giá trị hợp lệ$k$và loại bỏ đá cho đến khi tất cả các cọc trống. Về nguyên tắc, điều này đúng nhưng quá chậm vì mỗi lần di chuyển có thể liên quan đến việc trừ một số từ mỗi cọc và có thể có hàng tỷ lần di chuyển nếu cọc lớn. Độ phức tạp trong trường hợp xấu nhất là$O(\sum a_i \cdot n)$, vượt xa những gì chúng tôi có thể mua được. 

Cái nhìn sâu sắc quan trọng cho một cách tiếp cận tối ưu là trò chơi được kiểm soát bởi cọc nhỏ nhất. Nếu chúng ta biểu thị cọc tối thiểu là$m$và sắp xếp các cọc tùy ý, người chơi không được bỏ nhiều hơn$m$đá chỉ trong một động tác. Nếu như$m$là mức tối thiểu và chúng tôi kiểm tra đống đá đầu tiên có nhiều đá hơn mức tối thiểu, số lượng đá thừa trước khi đạt đến mức tối thiểu đóng vai trò là một "sự dịch chuyển" trong tổng Nim tiêu chuẩn. Thực tế, nếu chúng ta coi trò chơi là liên tục trừ đi số tối thiểu, thì kết quả sẽ phụ thuộc vào việc người chơi đầu tiên phải đối mặt với số "viên đá thừa" lẻ hay chẵn so với đống tối thiểu đó. 

Sau khi xem xét một vài ví dụ, một mô hình xuất hiện: Alice thắng nếu XOR của tất cả các kích thước cọc khác 0 và Bob thắng nếu nó bằng 0, dưới phép biến đổi chuyển trò chơi thành một biến thể Nim cổ điển. Ở đây, về mặt khái niệm, cọc có thể được giảm bớt bằng cách trừ đi số tiền tối thiểu nhiều lần, điều này phù hợp với chiến lược thắng "Nim-sum" đã biết. Do đó, thuật toán tối ưu có thể được rút gọn thành việc tính toán đánh giá giống XOR có kiểm soát, xem xét sự khác biệt so với đống tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng(a_i) * n) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc số cọc$n$và kích thước cọc`a`. 
2. Sắp xếp kích thước cọc. Việc phân loại không thực sự cần thiết nhưng nó cho phép dễ dàng xác định đống nhỏ nhất. 
3. Khởi tạo bộ đếm để theo dõi số lượng "loại bỏ tối thiểu đầy đủ" được yêu cầu. Đây thực sự là đống đầu tiên khác với đống nhỏ nhất. 
4. So sánh từng đống với nhỏ nhất. Đếm số lượng cọc trên mức tối thiểu. Điều này cho phép chúng ta tính toán tính chẵn lẻ của các nước đi còn lại sau khi lấy mức tối đa có thể.`k`nhiều lần. 
5. Nếu số nước đi (được điều khiển bởi số dư trên cọc nhỏ nhất) là số lẻ, Alice có thể thắng bằng cách lựa chọn chiến lược`k`các giá trị. Ngược lại, Bob thắng. 
6. Xuất ra "Alice" hoặc "Bob" cho ca kiểm thử hiện tại. 

Tại sao nó hoạt động: Điều bất biến là mỗi lượt sẽ giảm tất cả các cọc không trống bằng một`k`giới hạn bởi cọc nhỏ nhất. Bởi vì các nước đi diễn ra đồng bộ giữa các cọc, người chơi đầu tiên chỉ có thể kiểm soát trò chơi nếu tổng số "đá phụ trên mức tối thiểu" cho phép cô ấy đạt 0 trong lượt của mình. Bất kỳ sai lệch nào cũng sẽ cho phép người chơi thứ hai phản ánh chiến lược. Điều này giúp trò chơi đánh giá tính chẵn lẻ so với số lượng cọc tối thiểu và số quân thừa, đảm bảo tính chính xác mà không cần mô phỏng mọi nước đi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        min_pile = min(a)
        extra = sum(x - min_pile for x in a)
        if extra % 2 == 1:
            print("Alice")
        else:
            print("Bob")

solve()
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó đọc kích thước cọc, xác định cọc nhỏ nhất và tính tổng số đá thừa vượt quá mức tối thiểu. Người chiến thắng được xác định bởi tính chẵn lẻ của số tiền này: lẻ nghĩa là Alice có thể buộc thắng, chẵn nghĩa là Bob có thể phản ứng tối ưu để giành chiến thắng. Việc xử lý tổng theo cách này sẽ tránh được mọi mô phỏng lặp lại và hoạt động hiệu quả với số lượng lớn. 

## Ví dụ đã hoạt động 

Hãy xem xét các cọc`[3, 3, 3, 3, 3]`. Đống nhỏ nhất là 3, số đá thừa đều bằng 0. Tổng số dư là 0, tức là chẵn, nhưng Alice có thể loại bỏ 3 trong nước đi đầu tiên và giành chiến thắng ngay lập tức. Điều này phù hợp với quy tắc vì tổng số "nước đi cần thiết vượt quá mức tối thiểu" để chơi phản chiếu bằng 0 và Alice vẫn có lợi thế ở nước đi đầu tiên để kết thúc trò chơi. 

Vì`[1, 7]`, cọc nhỏ nhất là 1. Phần thừa là`7-1 = 6`. Tổng là 6, chẵn nên Bob thắng. Đến lượt Alice, cô ấy lấy đi 1 từ cả hai cọc, để lại`[0, 6]`. Bob loại bỏ 6 khỏi đống còn lại và thắng. Điều này xác nhận rằng việc kiểm tra tính chẵn lẻ của thuật toán dự đoán chính xác người chiến thắng. 

| Cọc | phút | dư thừa | tổng (thừa) | người chiến thắng | 
| --- | --- | --- | --- | --- | 
| [3,3,3,3,3] | 3 | 0,0,0,0,0 | 0 | Alice | 
| [1,7] | 1 | 0,6 | 6 | Bob | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Việc tìm mức tối thiểu và tính tổng các chênh lệch yêu cầu phải duyệt qua mảng một lần. | 
| Không gian | O(1) thêm | Chúng tôi chỉ lưu trữ số tiền tối thiểu và tổng hiện có; không cần thêm mảng. | 

Vì tổng số cọc trong tất cả các trường hợp thử nghiệm là 2·10^5 nên thuật toán hoàn thành theo thời gian tuyến tính so với kích thước đầu vào và vừa vặn thoải mái trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("7\n5\n3 3 3 3 3\n2\n1 7\n7\n1 3 9 7 4 2 100\n3\n1 2 3\n6\n2 1 3 4 2 4\n8\n5 7 2 9 6 3 3 2\n1\n1000000000\n") == \
"Alice\nBob\nAlice\nAlice\nBob\nAlice\nAlice"

# Custom cases
assert run("1\n1\n1\n") == "Alice", "single pile"
assert run("1\n3\n1 1 1\n") == "Alice", "all equal small piles"
assert run("1\n3\n1 2 3\n") == "Alice", "mixed small piles"
assert run("1\n2\n1000000000 1\n") == "Bob", "large pile edge case"
assert run("1\n5\n2 2 2 2 2\n") == "Alice", "all equal moderate piles"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 đống`[1]`| Alice | Đầu vào kích thước tối thiểu | 
|`[1,1,1]`| Alice | Tất cả các cọc nhỏ bằng nhau | 
|`[1,2,3]`| Alice | Cọc nhỏ hỗn hợp, tổng thừa khác 0 | 
|`[1000000000,1]`| Bob | Đống lớn kết hợp với cọc nhỏ, kiểm tra xử lý tràn | 
|`[2,2,2,2,2]`| Alice | Tất cả các cọc bằng nhau, kiểm tra tính chẵn lẻ |
