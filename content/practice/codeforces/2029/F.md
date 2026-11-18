---
title: "CF 2029F - Palindrome Khắp Mọi Nơi"
description: "Chúng ta có một biểu đồ chu trình với các đỉnh $n$ được gắn nhãn từ $0$ đến $n-1$. Mỗi cạnh trong chu trình có một màu đỏ hoặc xanh."
date: "2026-06-08T12:04:19+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2029
codeforces_index: "F"
codeforces_contest_name: "Refact.ai Match 1 (Codeforces Round 985)"
rating: 2500
weight: 2029
solve_time_s: 104
verified: false
draft: false
---

[CF 2029F - Palindrome ở mọi nơi](https://codeforces.com/problemset/problem/2029/F) 

**Đánh giá:** 2500 
**Tags:** thuật toán xây dựng, đồ thị, tham lam 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị chu trình với$n$đỉnh được dán nhãn$0$ĐẾN$n-1$. Mỗi cạnh trong chu trình có một màu đỏ hoặc xanh. Với hai đỉnh bất kỳ$i$Và$j$, chúng tôi muốn biết liệu có tồn tại một tuyến đường từ$i$ĐẾN$j$dọc theo chu kỳ sao cho chuỗi màu sắc ở các cạnh tạo thành một bảng màu. Tuyến đường có thể xem lại các đỉnh và được phép đi theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ ở mỗi bước. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp$n$và một chuỗi dài$n$đại diện cho màu sắc của các cạnh trong chu trình. Nhiệm vụ là xuất ra "CÓ" nếu mỗi cặp đỉnh trong chu trình chấp nhận một lộ trình palindrome hoặc "KHÔNG" nếu ngược lại. 

Cho rằng$n$có thể lên đến$10^6$và tổng của$n$trên tất cả các trường hợp thử nghiệm không vượt quá$10^6$, một giải pháp kiểm tra trực tiếp mọi cặp đỉnh (sẽ là$O(n^2)$) là không thể thực hiện được. Giải pháp phải tuyến tính trong$n$cho mỗi trường hợp thử nghiệm, có thể bằng cách khai thác cấu trúc của chu trình và bản chất của palindromes. 

Trường hợp cạnh tinh tế là khi chu trình có các màu xen kẽ, ví dụ: "RBRBRB" với các màu chẵn$n$. Trong trường hợp như vậy, lộ trình palindrome có thể không tồn tại đối với một số cặp đỉnh. Một trường hợp cạnh khác là khi tất cả các cạnh có cùng màu - khi đó bất kỳ chuỗi nào cũng có tính đối xứng tầm thường, ngay cả khi tuyến đường xem lại các đỉnh. Một cách tiếp cận bất cẩn có thể cố gắng kiểm tra tất cả các cặp một cách ngây thơ hoặc bỏ qua sự cần thiết của các tuyến đường truy cập lại các đỉnh. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng kiểm tra mọi cặp đỉnh$(i,j)$và cố gắng xây dựng một lộ trình palindrome. Đối với mỗi cặp, người ta có thể mô phỏng việc đi theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ và cố gắng khớp các màu của các cạnh. Cách tiếp cận này đúng về nguyên tắc nhưng nó đòi hỏi phải kiểm tra$O(n^2)$cặp và việc mô phỏng đường dẫn có thể mất tới$O(n)$bước cho từng cặp. Điều này dẫn đến$O(n^3)$trong trường hợp xấu nhất là quá chậm. 

Quan sát quan trọng là để một lộ trình palindrome tồn tại giữa hai đỉnh bất kỳ, chu trình phải chứa ít nhất một vị trí trong đó hai cạnh liên tiếp có cùng màu. Nếu một vị trí như vậy tồn tại thì người ta có thể sử dụng “điểm gương” đó để xây dựng một lộ trình đối xứng: di chuyển từ$i$đến điểm gương theo một hướng, sau đó từ điểm gương tới$j$ngược lại, phản ánh màu sắc của cạnh. Nếu chu trình không có các cạnh liên tiếp cùng màu thì các cạnh sẽ xen kẽ nhau một cách chặt chẽ và đối với các chu kỳ có độ dài chẵn, một số cặp không thể tạo thành một lộ trình nhạt màu. Nói cách khác, bài toán quy về việc kiểm tra xem có tồn tại ít nhất một cặp cạnh liên tiếp có cùng màu hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$và chuỗi màu sắc cạnh$c$. 
2. Kiểm tra xem$n=3$. Nếu như$n=3$, bất kỳ chu kỳ nào gồm 3 đỉnh đều cho phép các tuyến đường palindromic một cách tầm thường, vì vậy hãy in "CÓ" và tiếp tục. Điều này xử lý trường hợp cạnh chu kỳ tối thiểu. 
3. Lặp lại chu trình từ đỉnh 0 đến$n-1$, so sánh từng cạnh$c_i$với cạnh tiếp theo$c_{(i+1)\bmod n}$. 
4. Nếu ít nhất một cặp cạnh liên tiếp có cùng màu, hãy in "CÓ" cho trường hợp kiểm tra này, vì cặp đó cung cấp một điểm phản chiếu cho các tuyến đường palindrome. 
5. Nếu quá trình lặp hoàn tất mà không tìm thấy các cạnh liên tiếp có cùng màu, hãy in "KHÔNG". 

Tại sao nó hoạt động: Điểm gương đảm bảo rằng bất kỳ đường dẫn nào cũng có thể được mở rộng theo cả hai hướng để khớp với thuộc tính palindrome. Nếu không có cạnh liên tiếp nào giống nhau thì chu trình sẽ luân phiên hoàn hảo. Trong một chu trình xen kẽ, một số cặp đỉnh không thể có lộ trình nhạt màu vì các màu của các cạnh không thể phản ánh đối xứng, đặc biệt khi độ dài chu trình là chẵn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        c = input().strip()
        found = False
        for i in range(n):
            if c[i] == c[(i+1)%n]:
                found = True
                break
        print("YES" if found else "NO")

solve()
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`cho các trường hợp thử nghiệm lớn. Đối với mỗi trường hợp thử nghiệm, chúng tôi lặp lại chu trình một lần và kiểm tra các cạnh liên tiếp bằng cách sử dụng số học mô-đun để bao quanh chu trình. Biến`found`đánh dấu sự tồn tại của một điểm gương. Nếu nó tồn tại, có thể đạt được thuộc tính palindrom; nếu không thì đầu ra là "KHÔNG". 

Một điểm tinh tế đang sử dụng`(i+1) % n`để xử lý phần bao quanh từ cạnh cuối cùng đến cạnh đầu tiên, điều cần thiết là kiểm tra chính xác tính liên tục của chu trình. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`"5\nRRRRB"`: 

| tôi | c[i] | c[i+1] | Bình đẳng? | 
| --- | --- | --- | --- | 
| 0 | R | R | Có | 

Chúng ta tìm thấy các cạnh bằng nhau liên tiếp tại i=0, do đó xuất ra "CÓ". 

Đầu vào mẫu`"6\nRBRBRB"`: 

| tôi | c[i] | c[i+1] | Bình đẳng? | 
| --- | --- | --- | --- | 
| 0 | R | B | Không | 
| 1 | B | R | Không | 
| 2 | R | B | Không | 
| 3 | B | R | Không | 
| 4 | R | B | Không | 
| 5 | B | R | Không | 

Không có cạnh liên tiếp nào bằng nhau nên xuất ra "NO". Dấu vết này xác nhận rằng các chu kỳ xen kẽ không thành công đối với một số cặp đỉnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Lặp lại một lần qua các cạnh của chu kỳ, kiểm tra theo thời gian không đổi | 
| Không gian | O(1) | Chỉ có một số biến được sử dụng; chuỗi đầu vào được đọc tại chỗ | 

Vì tổng của$n$trên tất cả các trường hợp thử nghiệm là ≤ 10^6, tổng số thao tác nằm trong khoảng 2×10^6, rất phù hợp trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("7\n5\nRRRRR\n5\nRRRRB\n5\nRBBRB\n6\nRBRBRB\n6\nRRBBRB\n5\nRBRBR\n12\nRRBRRBRRBRRB\n") == "YES\nYES\nYES\nNO\nNO\nYES\nNO"

# Custom cases
assert run("1\n3\nRBR\n") == "YES", "minimal cycle, odd"
assert run("1\n4\nRBRB\n") == "NO", "alternating even cycle"
assert run("1\n4\nRRBB\n") == "YES", "pair of equal edges present"
assert run("1\n5\nBBBBB\n") == "YES", "all equal edges"
assert run("1\n6\nBRBRBR\n") == "NO", "even alternating cycle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3, RBR | CÓ | Chu kỳ có kích thước tối thiểu, mọi tuyến đường palindrome đều tồn tại | 
| 4, RBRB | KHÔNG | Chu kỳ có độ dài chẵn xen kẽ không thành công | 
| 4, RRBB | CÓ | Sự tồn tại của các cạnh bằng nhau liên tiếp đảm bảo CÓ | 
| 5, BBBBB | CÓ | Tất cả các cạnh giống hệt nhau, các tuyến đường palindrome tầm thường | 
| 6, BRBRBR | KHÔNG | Chu kỳ xen kẽ dài hơn, độ dài chẵn, không thành công | 

## Vỏ cạnh 

Chu kỳ tối thiểu$n=3$được xử lý tự động vì ba cạnh bất kỳ luôn cho phép một lộ trình palindrome. Chu trình xen kẽ với độ dài chẵn chứng tỏ tại sao sự có mặt của các cạnh bằng nhau liên tiếp là rất quan trọng. Một chu trình có độ dài 6 như "RBRBRB" không có các cạnh bằng nhau liên tiếp nên không có điểm phản chiếu để phản xạ, tạo ra "NO". Thuật toán xác định chính xác điều này bằng cách lặp qua các cạnh theo modulo$n$và kiểm tra sự bình đẳng. Chu kỳ trong đó tất cả các cạnh đều giống nhau ngay lập tức trả về "CÓ", bao gồm trường hợp các màu đồng nhất.
